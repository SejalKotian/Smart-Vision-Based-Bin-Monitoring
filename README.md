# Smart-Vision-Based-Bin-Monitoring
SmartBin-Vision is a real-time trash monitoring system designed to automatically detect, track, and classify objects as they approach a recycling bin. The goal is to support sustainable waste sorting by identifying METAL vs PLASTIC items before they are discarded.
The system combines:

  YOLOv8 for object detection
  
  ByteTrack for stable multi-object tracking
  
  YOLO Pose for hand filtering
  
  A custom fine-tuned classifier (Metal = 0, Plastic = 1)
  
  ROI filtering to only classify trash entering the bin
  
  Live majority voting for robust final classification

This pipeline is lightweight, modular, and suitable for edge deployment

We will upload the code and results shortly!
