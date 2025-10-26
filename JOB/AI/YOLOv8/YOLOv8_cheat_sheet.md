# YOLOv8 Cheat Sheet

## 1. What is YOLOv8?

YOLOv8 is the latest version of the "You Only Look Once" (YOLO) real-time object detection models. It's known for its state-of-the-art performance in object detection, instance segmentation, image classification, and pose estimation.

### Key Features

*   **Versatile:** Supports object detection, segmentation, classification, and pose estimation.
*   **Accurate and Fast:** Improved accuracy and speed over previous versions.
*   **Anchor-Free:** Directly predicts an object's center, improving generalization.
*   **Decoupled Head:** Separates classification and regression tasks for better performance.
*   **C2f Module:** Replaces C3 modules for improved efficiency.
*   **Mosaic Data Augmentation:** Mixes four images for better context during training.

## 2. Installation

```bash
pip install ultralytics
```

## 3. Model Variants

YOLOv8 comes in five sizes, from `n` (nano, smallest and fastest) to `x` (extra large, most accurate but slowest).

*   YOLOv8n
*   YOLOv8s
*   YOLOv8m
*   YOLOv8l
*   YOLOv8x

## 4. Common CLI Commands

*   **Train a model:**
    ```bash
    yolo train model=yolov8n.pt data=data.yaml epochs=100 imgsz=640
    ```
*   **Predict on an image/video:**
    ```bash
    yolo predict model=yolov8n.pt source='path/to/image.jpg'
    ```
*   **Validate a model:**
    ```bash
    yolo val model=yolov8n.pt data=data.yaml
    ```
*   **Export a model:**
    ```bash
    yolo export model=yolov8n.pt format=onnx
    ```

## 5. Custom Training Data Preparation

*   **Dataset Structure:**
    '''
    dataset/
    ├── train/
    │   ├── images/
    │   └── labels/
    ├── val/
    │   ├── images/
    │   └── labels/
    └── data.yaml
    '''
*   **Annotation Files (`.txt`):**
    *   One `.txt` file per image.
    *   Each line represents an object: `class_id center_x center_y width height` (normalized coordinates).
*   **`data.yaml` File:**
    '''yaml
    train: ../dataset/train/images
    val: ../dataset/val/images

    nc: 2 # number of classes
    names: ['class1', 'class2']
    '''
