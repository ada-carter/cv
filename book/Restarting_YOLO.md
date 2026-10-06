---
title: Restarting YOLO
---

*Placeholder content - original file not found in upstream repository.*
# 3.11 Restarting an Interrupted YOLO26 Training Session
{: #lesson-restarting-yolo}

Sometimes, long training runs can be interrupted due to runtime disconnects, power outages, or manual stops. Fortunately, YOLO26 automatically saves your training progress after every epoch to the `runs/detect/train/weights/last.pt` file.

You can easily resume an interrupted training session right where it left off.

## How to Resume Training

When you initialize your YOLO26 model, instead of loading the base model (e.g., `yolo26n.pt`), you simply load your **last saved weights**. Then, you call the `train()` method with the `resume=True` argument.

### Python Code Snippet

```python
from ultralytics import YOLO

# 1. Load the last saved checkpoint from your runs directory
# (Make sure the path matches your specific run folder)
model = YOLO("runs/detect/train/weights/last.pt")

# 2. Resume training
# YOLO26 automatically remembers your epochs, batch size, and dataset from the checkpoint
results = model.train(resume=True)
```

> [!TIP]
> If you have run multiple training sessions, your weights might be saved in a different folder like `runs/detect/train2/weights/last.pt` or `runs/segment/train/weights/last.pt`. Always double-check your file path before resuming!

Once executed, YOLO26 will seamlessly restore the optimizer state, learning rates, and current epoch, and continue training until the original epoch limit is reached.


