# Writing a Model Card

A model card provides essential information about a machine learning model, including its intended use, performance metrics, limitations, and potential biases. It promotes transparency and helps users understand whether the model is suitable for their specific use case. Here’s a guide on how to write an effective model card, using the NOAA AFSC Marine Mammal Lab YOLOv11 Ice Seal Object Detection Model as an example.

## Model Card Structure

### 1. Model Overview

*   **Model Name:** NOAA AFSC Marine Mammal Lab YOLOv11 Ice Seal Object Detection Model
*   **Description:**
    *   Accurate identification of ice-associated seals in aerial imagery is essential for monitoring population dynamics and assessing ecological trends in Arctic and sub-Arctic environments. To facilitate the fast and accurate detection of these seals, I have developed a deep learning-based object detection model utilizing YOLOV11n, a lightweight neural network architecture optimized for high-performance image analysis. The model, comprising 319 layers and 2,591,400 parameters, was trained on a diverse dataset containing 7,671 high-resolution images, which were tiled into 30,684 smaller images to enhance feature recognition.

### 2. Intended Use

*   **Primary Use Case:** Object detection of ice-associated seals in aerial imagery.
*   **Specific Applications:**
    *   Monitoring population dynamics.
    *   Assessing ecological trends in Arctic and sub-Arctic environments.
    *   ROI detector for imaging surveys.

### 3. Factors

*   **Relevant Factors:**
    *   Species: Bearded, ribbon, ringed, and spotted seals (both pup and adult stages).
    *   Imagery Conditions: High-resolution aerial images.

### 4. Metrics

*   **Overall mAP:** 0.4833
*   **Class-Specific mAP:**
    *   Bearded Seals: 0.9
    *   Ringed Seals: 0.833
    *   Ribbon Seals: 0.464
*   **Precision:** 0.76783
*   **Recall:** 0.43806

### 5. Evaluation

*   **Dataset Details:**
    *   7,671 high-resolution images tiled into 30,684 smaller images.
    *   8,948 total annotations.
*   **Training Configuration:**
    *   Image Size: 1024 × 1024
    *   Batch Size: 113
    *   Optimizer: SGD (lr=0.01, momentum=0.9)
    *   Hardware: NVIDIA A100 GPU
*   **Augmentations:**
    *   Blur, MedianBlur, ToGray, CLAHE

### 6. Recommendations

*   **Optimal Performance:** Detecting ringed, spotted, and bearded seals.
*   **Underrepresented Classes:** Misclassification is more common, though false negatives remain rare.
*   **Review Pipeline:** Intended for manual verification in certain workflows.
*   **Downstream Tasks:** Clustering, two-shot fine-tuning, or in-depth analytics.
*   **Real-Time Inference:** Best to use the YOLO model.

### 7. Caveats and Limitations

*   **Pup Classes:** Consistently underperformed due to smaller size and limited representation.
*   **Misclassification:** Underrepresented classes tend to be misclassified rather than completely missed.

### 8. Bias, Risks, and Harms

*   **Potential Biases:**
    *   Imbalanced class distribution may affect performance on underrepresented classes.
*   **Mitigation Strategies:**
    *   Rebalance Train/Val splits.
    *   Ensure enough representative classes in the dataset.

### 9. Environmental Impact

*   **Compute Location:** Google Cloud Platform (GCP), northamerica-northeast1 region
*   **Carbon Efficiency:** 0.03 kgCO₂eq/kWh
*   **Hardware:** A100 PCIe 40/80GB (TDP of 250W)
*   **Compute Duration:** ~10 hours
*   **Total Emissions:** ~0.07 kgCO₂eq

### 10. How to Use

*   **Inference Code Example:**

    ```python
    from ultralytics import YOLO

    # Load your custom-trained weights
    model = YOLO("path/to/NOAA_AFSC_MML_Iceseals_31K.pt")

    # Inference on a still image
    results_image = model.predict(
        source="path/to/your_image.jpg",
        conf=0.01,   # Confidence threshold
        iou=0.45,    # IoU threshold
        device=0     # Use GPU 0 if available; set to 'cpu' if no GPU
    )
    ```

### 11. Dataset and Preprocessing Details

*   **Source Imagery:**
    *   Original resolution: 6048 × 4032
    *   Total source images: 7,671 (including 156 null examples)
    *   Resolution range: ~12.21 MP to ~24.39 MP
*   **Tiling:**
    *   Each source image was split into 4 tiles (2 rows × 2 columns) at 1024 × 1024.
    *   Post-tiling: 30,684 images.
*   **Annotations:**
    *   8,948 total annotations.
    *   ~1.2 annotations per image on average.
*   **Class Distribution:**

    | Class Name    | Total Count | Training Count | Validation Count | Test Count |
    | ------------- | ----------- | -------------- | ---------------- | ---------- |
    | ringed\_seal  | 3180        | 2190           | 674              | 316        |
    | bearded\_seal | 1922        | 1392           | 359              | 171        |
    | spotted\_seal | 1662        | 1142           | 344              | 176        |
    | unknown\_seal | 812         | 585            | 153              | 74         |
    | bearded\_pup  | 420         | 300            | 77               | 43         |
    | unknown\_pup  | 392         | 275            | 63               | 54         |
    | spotted\_pup  | 238         | 174            | 46               | 18         |
    | ribbon\_seal  | 232         | 154            | 45               | 33         |
    | ringed\_pup   | 54          | 37             | 14               | 3          |
    | ribbon\_pup   | 36          | 28             | 5                | 3          |

### 12. Training Configuration Details

*   **Parameters:**
    *   Layers: 319
    *   Parameters: 2,591,400
    *   Gradients: 2,591,384
    *   GFLOPs: 6.4

### 13. Metrics (Epoch 64)

| epoch | time    | train/box\_loss | train/cls\_loss | train/dfl\_loss | metrics/precision(B) | metrics/recall(B) | metrics/mAP50(B) | metrics/mAP50-95(B) | val/box\_loss | val/cls\_loss | val/dfl\_loss | lr/pg0      | lr/pg1      | lr/pg2      |
| ----- | ------- | --------------- | --------------- | --------------- | -------------------- | ----------------- | ---------------- | ------------------- | ------------- | ------------- | ------------- | ----------- | ----------- | ----------- |
| 64    | 23230.9 | 1.34616         | 1.18894         | 0.89475         | 0.76783              | 0.43806           | 0.4671           | 0.30454             | 1.37059       | 1.77372       | 0.90735       | 0.00993763  | 0.00993763  | 0.00993763  |

### Key Considerations

*   **Transparency:** Provide as much detail as possible about the model and its development process.
*   **Accessibility:** Use clear and straightforward language to make the model card accessible to a broad audience.
*   **Completeness:** Cover all relevant aspects of the model, including its intended use, performance, limitations, and ethical considerations.

By following this structure, you can create a comprehensive model card that promotes transparency and enables users to make informed decisions about using your model.
