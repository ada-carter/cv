# 3.15 Live Inference with PyQt

## What Live Inference Is and When You Need It

Live inference means running a detection or classification model on frames coming from a camera in real time, displaying the annotated output with minimal delay. It is the difference between reviewing footage after the fact and watching a model work in front of you. For marine science, this matters in several settings: a researcher monitoring a fish tank in a lab wants to see detection boxes appear and disappear as fish move through frame; a hatchery technician running a counting station needs an immediate visual check that the system is tracking correctly before the fish have passed; and a museum exhibit needs something that responds to what visitors are actually pointing at.

PyQt5 is a Python binding for the Qt application framework. It handles the GUI layer: windows, widgets, buttons, and the event loop that keeps all of it responsive. The key architectural challenge in any live inference application is that camera capture and model inference are slow, blocking operations that must not run on the main GUI thread. If they do, the interface freezes while the model is processing, which is both unpleasant and technically incorrect. PyQt5 solves this with QThread, a class that runs code in a background thread while communicating results back to the main thread via signals and slots.

This lesson builds a minimal working live inference application using PyQt5, OpenCV, and Ultralytics YOLO. It then describes the customizations that turn a skeleton into a useful tool.

---

## Architecture Overview

The application has two threads. The main thread owns all Qt widgets and handles all GUI updates. Qt enforces this strictly: you cannot update a widget from a background thread without risking a crash or a corrupted display.

The background thread, implemented as a subclass of QThread, owns the OpenCV VideoCapture object and the YOLO model. On each iteration of its loop it reads a frame from the camera, passes it through the model, draws the prediction boxes onto the frame using the built-in `results[0].plot()` method, and emits the annotated frame via a custom signal. The main thread connects that signal to a slot that converts the NumPy array to a QImage, wraps it in a QPixmap, and assigns it to a QLabel that fills the window. The QLabel is updated fast enough that the display appears as continuous video.

The signal-slot mechanism is Qt's way of crossing thread boundaries safely. A `pyqtSignal` declared on the thread class is emitted in the background thread but delivered to its connected slots in the main thread. Qt handles the queuing automatically.

---

## Setup and Dependencies

Install the required packages. Using pip:

```bash
pip install PyQt5 opencv-python ultralytics
```

Using conda:

```bash
conda install -c conda-forge pyqt opencv
pip install ultralytics
```

On Linux (including Jetson), PyQt5 installation via pip may require system Qt libraries. If you encounter import errors, install `python3-pyqt5` through your system package manager first.

---

## Core Implementation

The following is a minimal, complete skeleton. Read through it before running it, because understanding each component will help you extend it.

```python
import sys
import cv2
from PyQt5.QtWidgets import QApplication, QLabel, QMainWindow
from PyQt5.QtCore import QThread, pyqtSignal, Qt
from PyQt5.QtGui import QImage, QPixmap
from ultralytics import YOLO


class InferenceThread(QThread):
    frame_ready = pyqtSignal(object)

    def __init__(self, model_path, camera_index=0):
        super().__init__()
        self.model = YOLO(model_path)
        self.cap = cv2.VideoCapture(camera_index)
        self.running = True

    def run(self):
        while self.running:
            ret, frame = self.cap.read()
            if not ret:
                break
            results = self.model(frame, verbose=False)
            annotated = results[0].plot()
            self.frame_ready.emit(annotated)

    def stop(self):
        self.running = False
        self.cap.release()


class MainWindow(QMainWindow):
    def __init__(self, model_path):
        super().__init__()
        self.setWindowTitle('Live Marine CV Inference')
        self.label = QLabel(self)
        self.setCentralWidget(self.label)
        self.thread = InferenceThread(model_path)
        self.thread.frame_ready.connect(self.update_frame)
        self.thread.start()

    def update_frame(self, frame):
        rgb = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
        h, w, ch = rgb.shape
        img = QImage(rgb.data, w, h, ch * w, QImage.Format_RGB888)
        self.label.setPixmap(QPixmap.fromImage(img))

    def closeEvent(self, event):
        self.thread.stop()
        self.thread.wait()
        super().closeEvent(event)


if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = MainWindow('best.pt')
    window.show()
    sys.exit(app.exec_())
```

A few points worth understanding in this skeleton:

`verbose=False` suppresses the per-frame console output that YOLO prints by default. Without this, the terminal becomes unreadable during a live session.

`results[0].plot()` returns a NumPy array with detection boxes, class labels, and confidence scores already drawn. This is the annotated frame emitted to the main thread.

The `update_frame` slot converts BGR (OpenCV's native format) to RGB (Qt's expected format) before constructing the QImage. Skipping this step produces a blue-tinted display because the red and blue channels are swapped.

`closeEvent` calls `thread.stop()` and then `thread.wait()`. The `wait()` call blocks until the thread's `run()` method has returned. Without it, the application can crash on exit if the thread tries to access Qt objects after the main window has been destroyed.

---

## Customization

The skeleton is functional but bare. Here are the most common extensions for marine science applications.

**Confidence threshold slider.** Add a `QSlider` to the window and pass its value to the inference thread. In the thread's `run()` loop, use the `conf` argument:

```python
results = self.model(frame, verbose=False, conf=self.conf_threshold)
```

Connect the slider's `valueChanged` signal to a method on the thread that updates `self.conf_threshold`. This allows an operator to reduce false positives in cluttered scenes by raising the threshold, or to catch rare detections by lowering it, without restarting the application.

**Class filter dropdown.** Ultralytics supports the `classes` argument to restrict inference to a subset of class indices:

```python
results = self.model(frame, verbose=False, classes=self.active_classes)
```

A `QComboBox` or a list of `QCheckBox` widgets lets the user select which species or object types to display. This is particularly useful when a model covers many species but the current session is focused on one or two.

**FPS counter overlay.** Track the timestamp of each emitted frame in the inference thread and compute a rolling average:

```python
import time

fps = 1.0 / (time.perf_counter() - self.last_time)
self.last_time = time.perf_counter()
cv2.putText(annotated, f'{fps:.1f} fps', (10, 30),
            cv2.FONT_HERSHEY_SIMPLEX, 1.0, (0, 255, 0), 2)
```

This gives the operator immediate feedback on whether the system is meeting its real-time target.

---

## Marine Science Applications

**Research tank monitoring.** A lab studying zebrafish or small reef fish can mount a camera above a tank and run this application continuously. The YOLO model detects individual fish; the bounding box count per frame gives an occupancy estimate. Logged to CSV alongside a timestamp, this becomes a behavioral time series with no manual annotation effort.

**Hatchery outfall counting.** Juvenile salmon smolts passing through a counting weir can be detected in real time on a camera feed. A live display allows the hatchery technician on duty to verify that the system is tracking correctly, catching failures like a dirty lens or an occlusion from floating debris, before they corrupt a full day of count data.

**Marine science center exhibit.** A kiosk display at an aquarium or visitor center can run live inference on a camera pointed at a touch tank or a large exhibit tank. Detection overlays with common names displayed on screen give visitors immediate feedback on what they are looking at. The application requires no network connection, runs on a compact NUC or a Jetson under the display, and can be left running unattended.

---

## Limitations and Alternatives

The main latency source in this architecture is the GPU-to-CPU memory transfer that happens when `results[0].plot()` returns a NumPy array. On a system with a discrete GPU, this transfer adds a few milliseconds per frame. On a Jetson with unified memory (CPU and GPU share the same physical memory), this cost is smaller but not zero. If sub-20 ms end-to-end latency is a hard requirement, consider keeping the annotated frame in GPU memory and rendering it directly to the display, which requires a more complex OpenGL-based rendering pipeline.

For applications where a full desktop GUI is unnecessary, Streamlit and Gradio are simpler alternatives. Both can display a live video feed in a browser tab with a dozen lines of code. The tradeoff is higher latency (the frame round-trips to a browser over HTTP) and less control over the interface layout. For a lab-bench demonstration or a quick prototype shown to a collaborator over a local network, Streamlit is often the right choice. For a deployed system that must respond in real time and run unattended without a browser, PyQt5 is the more appropriate tool.
