
# SmartBin-Vision

**Real-Time Trash Monitoring and Classification for Sustainable Waste Sorting**

SmartBin-Vision is a real-time computer vision pipeline that automatically detects, tracks, and classifies waste objects as they approach a recycling bin. The system is designed to support **sustainable waste management** by distinguishing **metal vs plastic items before disposal**, enabling smarter recycling workflows.

The pipeline integrates **object detection, multi-object tracking, pose-based hand filtering, and a custom material classifier** to ensure that only relevant trash objects are classified when they enter the bin region.

This modular architecture allows SmartBin-Vision to run efficiently and is suitable for **edge deployment in smart recycling systems, public waste stations, or smart cities infrastructure.**

---

# System Overview

The SmartBin-Vision pipeline combines multiple computer vision modules:

1. **Object Detection (YOLOv8)**
   Detects objects approaching the bin in real time.

2. **Multi-Object Tracking (ByteTrack)**
   Maintains stable identities for objects across frames.

3. **Hand Filtering (YOLO Pose)**
   Detects hands and filters them out to avoid classifying human interaction as trash.

4. **ROI Filtering**
   Only objects entering the **bin region** are considered for classification.

5. **Material Classification**
   A fine-tuned classifier determines whether an object is:

   * **Metal (0)**
   * **Plastic (1)**

6. **Majority Voting Across Frames**
   Each tracked object accumulates predictions across frames, and the final label is determined via **majority voting** for robustness.

---

# Pipeline Architecture

```
Video Stream
      │
      ▼
YOLOv8 Object Detection
      │
      ▼
ByteTrack Multi-Object Tracking
      │
      ▼
YOLO Pose Hand Detection
      │
      ▼
Hand Filtering
      │
      ▼
ROI Check (Is object entering bin?)
      │
      ▼
Material Classifier
      │
      ▼
Frame-wise Predictions
      │
      ▼
Majority Voting
      │
      ▼
Final Classification
(Metal or Plastic)
```

---

# Key Features

* Real-time object detection and tracking
* Robust classification via multi-frame voting
* Hand detection to avoid false classifications
* Region-of-interest filtering to detect only relevant trash
* Lightweight pipeline suitable for **edge deployment**
* Modular design allowing easy replacement of models

---

# Technologies Used

| Component               | Technology                 |
| ----------------------- | -------------------------- |
| Object Detection        | YOLOv8                     |
| Object Tracking         | ByteTrack                  |
| Pose Detection          | YOLO Pose                  |
| Material Classification | Custom fine-tuned CNN      |
| Framework               | PyTorch                    |
| Computer Vision         | OpenCV                     |
| Deployment              | Edge-friendly architecture |

---

# Repository Structure

```
SmartBin-Vision
│
├── models/
│   ├── yolo_detector.pt
│   ├── pose_model.pt
│   └── material_classifier.pt
│
├── src/
│   ├── detection.py
│   ├── tracking.py
│   ├── pose_filter.py
│   ├── classifier.py
│   └── pipeline.py
│
├── data/
│   ├── sample_videos/
│   └── dataset/
│
├── results/
│   ├── demo_output.mp4
│   └── evaluation_metrics.txt
│
├── notebooks/
│   └── training_classifier.ipynb
│
├── requirements.txt
└── README.md
```

---

# Installation

Clone the repository:

```bash
git clone https://github.com/yourusername/SmartBin-Vision.git
cd SmartBin-Vision
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Example requirements:

```
torch
ultralytics
opencv-python
numpy
bytetrack
```

---

# Running the System

Run the main pipeline:

```bash
python src/pipeline.py --video input_video.mp4
```

Optional parameters:

```
--video            Input video path
--roi              Bin region coordinates
--confidence       Detection threshold
--tracker          Enable ByteTrack
--display          Show live output
```

Example:

```bash
python src/pipeline.py --video demo.mp4 --display
```

---

# Region of Interest (ROI)

To ensure that only objects entering the bin are classified, a **Region of Interest (ROI)** is defined.

The ROI configuration is dynamically defined to only capture the center 70% of the frame

Objects are classified **only when their bounding box center enters this region.**

---

# Material Classifier

The classifier is trained to distinguish between:

| Label | Material |
| ----- | -------- |
| 0     | Metal    |
| 1     | Plastic  |

Training involves:

* Dataset of labeled waste items
* Transfer learning from a pretrained CNN
* Data augmentation for robustness

Example training command:

```bash
python train_classifier.py
```

---

# Majority Voting Strategy

To reduce classification noise:

1. Each object tracked by ByteTrack receives predictions across frames.
2. Predictions are stored for each track ID.
3. The final class is determined by **majority voting**.

Example:

```
Frame predictions for Track ID 12:
[Plastic, Plastic, Plastic, Metal, Plastic]

Final classification:
Plastic
```

This improves reliability when objects are partially occluded or moving quickly.

---

# Example Output

The system produces:

* Bounding boxes for detected objects
* Track IDs for each object
* Final material classification
* Real-time visualization

Example overlay:

```
Track ID: 7
Material: Plastic
Confidence: 0.93
```

---

# Applications

SmartBin-Vision can be used in:

* Smart recycling bins
* Public waste sorting stations
* Smart cities infrastructure
* Campus sustainability initiatives
* Automated waste audit systems

---

# Future Improvements

Planned improvements include:

* Multi-class classification (glass, paper, organic)
* Real-time edge deployment on Jetson Nano / Raspberry Pi
* Waste drop detection
* Integration with IoT smart bins
* Recycling contamination detection
* Cloud analytics dashboard

---

# Results

We have uploaded:

* Demo videos
* Performance metrics
* Training dataset details

---

# Contributors

SmartBin-Vision is developed as part of a **computer vision project exploring AI-driven sustainability solutions.**

Contributions, issues, and suggestions are welcome.

---

