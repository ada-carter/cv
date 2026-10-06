# 3.14 Edge Deployment Optimization

## Why Edge Deployment Matters

Training a model in the cloud is straightforward. Running it in the field is a different problem entirely. Marine science deployments rarely have the luxury of reliable internet: an ROV hovering over a coral reef at 30 meters depth is not uploading frames to a cloud API, a wave-powered buoy in the open Pacific is not phoning home to a GPU cluster, and a shipboard system in international waters may have satellite bandwidth measured in kilobytes per second. If your model cannot run locally, it does not run at all.

Edge deployment means taking a trained model and executing it on constrained hardware at the point of data collection. "Constrained" here covers compute, memory, power draw, and physical form factor. A Jetson module bolted inside an ROV pressure housing has maybe 15 watts to spare. A Raspberry Pi attached to a tide gauge sensor runs off a solar panel and a small LiFePO4 battery. Fitting a useful model into these envelopes requires deliberate optimization, not just shrinking the weight file.

This lesson covers the major hardware targets you are likely to encounter in marine science deployments, the optimization techniques used to fit models onto them, and practical workflows using Ultralytics YOLO as a concrete example.

---

## Hardware Targets

**NVIDIA Jetson family.** The Jetson line is the most capable edge platform for vision inference. The Orin Nano (8 GB variant) delivers roughly 40 TOPS of AI performance at around 10-15 W and fits into enclosures small enough for a BlueROV2 or a deployable buoy payload bay. The AGX Orin at the high end offers 275 TOPS and suits shipboard rack systems or shore-side processing stations where power is less constrained. All Jetson modules run JetPack, which bundles CUDA, cuDNN, and TensorRT. TensorRT is the reason to choose Jetson for inference: it performs layer fusion and kernel auto-tuning at compile time, squeezing substantially more throughput out of a given model than a naive CUDA implementation.

**Raspberry Pi 5 with Hailo-8.** The Pi 5 itself is not a strong inference platform, but paired with the Hailo-8 M.2 AI accelerator (available as an M.2 HAT+), it reaches 26 TOPS at well under 5 W total system power. This combination suits shallow-water sensor nodes, time-lapse camera traps monitoring intertidal zones, and any application where cost and power are the binding constraints. The tradeoff is that Hailo requires its own compilation toolchain (the Hailo Dataflow Compiler), which is more friction than TensorRT.

**Intel NUC.** A NUC running an 11th or 12th generation Intel Core processor with integrated Iris Xe graphics is a reasonable choice for shipboard analysis stations that have 19-inch rack space and shore power available. OpenVINO, Intel's inference optimization toolkit, converts ONNX models to run efficiently on Iris Xe and on Intel's Neural Processing Units in newer silicon. NUCs lack the raw throughput of Jetson AGX but are familiar x86 hardware that runs standard Ubuntu with no specialized OS layer.

---

## Model Optimization Techniques

Getting a model to fit and run fast on these platforms typically involves some combination of the following techniques.

**Quantization** converts the 32-bit floating-point weights and activations stored during training into lower-precision formats, most commonly int8 or float16. A float16 model is half the memory footprint of float32, with negligible accuracy loss on most vision tasks. Int8 quantization cuts memory by a further factor of two and enables integer math units on Jetson, which are faster and more power-efficient than floating-point units. Post-training quantization (PTQ) requires only a small calibration dataset to determine the scale factors that map float ranges to int8 ranges. Quantization-aware training (QAT) folds the quantization error into the training loop for better accuracy at the cost of a longer training run.

**Pruning** identifies weights with magnitudes close to zero (weights that contribute little to the output) and sets them to exactly zero, producing a sparse model. Structured pruning removes entire filters or channels, which reduces the computation graph in a way that standard dense matrix operations can exploit. Unstructured pruning produces irregular sparsity that requires sparse matrix libraries to yield actual speedups. For marine science applications, pruning is most useful when you have a very specific, limited set of target classes (five reef fish species rather than COCO's eighty classes) and can afford a fine-tuning pass after pruning.

**Knowledge distillation** trains a small "student" model to replicate the output probability distributions of a large "teacher" model. The teacher's soft probability outputs contain more information than hard one-hot labels, giving the student a richer learning signal. This approach is well-suited to cases where you want a model small enough to run on a Hailo-8 but you have already invested in training a high-accuracy YOLOv8x. You train the student (say, YOLOv8n) against both the ground-truth labels and the teacher's logits.

**ONNX export** converts a PyTorch model to the Open Neural Network Exchange format, which is a hardware-agnostic intermediate representation. ONNX is the common currency of model deployment: TensorRT, OpenVINO, and Hailo's compiler all accept it as input. Exporting to ONNX is therefore the first step in almost every edge deployment pipeline, regardless of the final target hardware.

**TensorRT** takes an ONNX model and compiles it for a specific Jetson GPU, fusing adjacent layers (for example, convolution plus batch normalization plus ReLU into a single kernel), selecting the fastest kernel implementation for each operator on that specific device, and optionally running int8 inference with the calibration data you provide. The compiled artifact is a serialized "engine" file that loads quickly at runtime and runs with minimal CPU overhead.

---

## Exporting a YOLO Model

Ultralytics makes ONNX and TensorRT export straightforward. Run the following on the machine where training completed (ONNX export works anywhere; TensorRT engine compilation must happen on the target device or an x86 machine with TensorRT installed):

```python
from ultralytics import YOLO

model = YOLO('best.pt')
model.export(format='onnx')
model.export(format='engine')  # TensorRT; requires Jetson or x86 with TRT installed
```

The ONNX export produces `best.onnx` in the same directory. The TensorRT export produces `best.engine`. The engine file is device-specific: an engine compiled on an Orin Nano will not run on an AGX Orin, and vice versa. Compile on the device you plan to deploy on.

---

## Running Inference with the Exported Engine

Once the engine file is on the Jetson, inference looks nearly identical to the standard PyTorch workflow:

```python
from ultralytics import YOLO

model = YOLO('best.engine')
results = model('frame.jpg', imgsz=640)
```

The first call warms up the TensorRT runtime, which may take a few seconds. Subsequent calls run at full optimized speed. For a video loop, initialize the model once outside the frame loop.

---

## Benchmarking Latency

The right way to measure edge performance is on the target device under realistic conditions: camera capturing, storage writing, any other processes that share CPU and memory. A simple benchmarking loop looks like this:

```python
import time
from ultralytics import YOLO

model = YOLO('best.engine')
frames = ['frame_{:04d}.jpg'.format(i) for i in range(100)]

start = time.perf_counter()
for f in frames:
    model(f, imgsz=640)
elapsed = time.perf_counter() - start

fps = len(frames) / elapsed
print(f'Throughput: {fps:.1f} fps')
print(f'Latency per frame: {1000 / fps:.1f} ms')
```

Report both throughput (frames per second) and per-frame latency. For real-time video, the relevant constraint is whether per-frame latency is less than the frame period of your camera. At 30 fps, you have about 33 ms per frame.

---

## Practical Considerations

**Thermal throttling.** Jetson modules reduce clock speeds when the SoC temperature exceeds a threshold, typically around 80-85 degrees Celsius. Inside a sealed pressure housing, passive cooling may be insufficient. Use the largest heatsink that fits your enclosure, apply thermal interface material carefully, and test your deployed system at ambient temperatures representative of your field conditions. Monitor temperature during deployment with `tegrastats`.

**Power budgets.** Profile power draw under inference load before finalizing your battery or power supply sizing. An Orin Nano running inference at full speed draws closer to 15 W than to its nominal 10 W figure. Over a six-hour dive, that difference matters.

**Batching strategy.** TensorRT can process multiple frames in a single batch, which can increase throughput for offline or near-real-time processing at the cost of increased latency. For real-time video where latency is the constraint, batch size 1 is usually correct. For post-dive analysis of recorded footage, larger batches may improve overall throughput.

---

## Marine Science Application: Coral Reef Fish Detection at 30 fps

Consider a shallow-water ROV equipped with a downward-facing 1080p camera and a Jetson Orin Nano in a 4-inch pressure tube. The goal is to detect and count fish in real time during transect surveys, replacing manual video review that previously required hours of analyst time per dive hour.

A YOLOv8n model trained on 6,000 annotated frames of reef fish achieves 0.78 mAP50 at float32. After TensorRT int8 compilation on the Orin Nano, inference runs at 34 fps on 640x640 crops, comfortably meeting the 30 fps requirement. The model logs detection timestamps, class labels, and bounding box coordinates to a CSV on an onboard NVMe drive. Post-dive, the CSV is transferred over Ethernet during the ROV recovery procedure. The analyst reviews only flagged frames rather than the full video record, reducing review time by roughly 70 percent in field tests.

This is the practical promise of edge deployment: not exotic technology for its own sake, but making real-time machine intelligence available where the data actually lives.
