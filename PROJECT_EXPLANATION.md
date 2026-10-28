# Deep Learning for Edge Computing in Indian Smart Cities — Project Explanation

## Overview
- Goal: Real-time traffic analytics on low-power edge devices for Indian smart cities.
- Approach: Run vehicle detection and counting at the edge; send either raw images or processed metadata to a cloud server; measure and compare latency.
- Outcome: A working edge–cloud pipeline with a GUI to train/evaluate a model, run detection, transmit data, and visualize delays.

## Key Objectives
- Detect and count vehicles in traffic video streams using YOLOv7 via OpenCV DNN.
- Minimize bandwidth and latency by sending processed results from the edge when beneficial.
- Provide a simple GUI to run the pipeline and visualize performance metrics.

## System Architecture
- Edge component: Local inference and counting on device; GUI for operations and visualization.
- Cloud component: TCP server receives data and persists raw frames; acknowledges requests.
- Communication: Lightweight pickle-serialized messages over TCP on localhost:2222.
- References:
  - Main application: [Main.py](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Main.py)
  - Cloud server: [CloudServer.py](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/CloudServer.py)
  - YOLO detection: [yolo_traffic.py](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/yolo_traffic.py)

## Data Sources and Artifacts
- Input videos for testing: [Videos](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Videos).
- Cloud persists images with timestamp filenames: [TrafficImages](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/TrafficImages).
- Model files and labels: [models](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/models).

## Detection and Counting Methodology
- Model and labels:
  - COCO labels: [coco.names](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/models/coco.names)
  - YOLOv7 cfg/weights: [yolov7.cfg](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/models/yolov7.cfg), [yolov7.weights](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/models/yolov7.weights).
- Inference:
  - OpenCV DNN loads Darknet cfg/weights and forwards blobs for detection.
  - Confidence and NMS thresholds tuned for speed on edge devices.
- Box drawing:
  - Bounding boxes and labels are drawn per detection, with color per class.
- Tracking and counting:
  - Mid-point of each box tracked across last N frames using KDTree nearest-neighbor matching.
  - Unique IDs maintained; counts incremented when new objects are observed crossing the midline.
- Key implementation references:
  - YOLO execution: [runYolo](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/yolo_traffic.py#L147-L238)
  - Drawing boxes: [drawDetectionBoxes](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/yolo_traffic.py#L62-L79)
  - Counting/tracking: [count_vehicles](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/yolo_traffic.py#L110-L145), [boxInPreviousFrames](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/yolo_traffic.py#L88-L109)

## Edge–Cloud Communication and Latency
- Protocol:
  - Payloads: `[request_type, msg]` pickled and sent over a TCP socket.
  - Request types:
    - `"cloud"`: edge sends raw image; cloud saves and acknowledges.
    - `"edge"`: edge sends processed text (vehicle count + timestamp); cloud acknowledges.
- Latency measurement:
  - Round-trip time measured at the client side per request and displayed in the GUI.
- Cloud server behavior:
  - Listens on `localhost:2222`, handles multiple connections with threads.
  - Saves incoming images to `TrafficImages/` with timestamp filenames.
- References:
  - Client send/measure: [postData](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Main.py#L51-L64), [sendImage](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Main.py#L66-L79), [sendData](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Main.py#L81-L94)
  - Cloud server: [CloudServer.py](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/CloudServer.py#L31-L45)
  - Delay chart: [delayGraph](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Main.py#L36-L49)

## Training and Evaluation
- Data: Preprocessed arrays `X.txt.npy`, `Y.txt.npy`, and `bb.txt.npy` for images, labels, and bounding boxes.
- Model: Pre-saved YOLOv7 Keras HDF5 model used for inference (classification head).
- Metrics: Accuracy, Precision, Recall, and F1 computed and printed to GUI.
- References:
  - Training/eval: [trainDL](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Main.py#L138-L167)
  - Model artifacts: [models folder](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/models)

## GUI and Visualization
- Built with Tkinter for Windows; buttons trigger training, detection, sending to cloud/edge, and plotting delay comparison.
- Video frames are shown in a resizable window updated via callback; annotated with boxes and vehicle counts.
- References:
- GUI buttons and text area: [Main.py](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Main.py#L169-L207)

## Setup and Run
- Recommended environment (Windows, Python 3.7):
  - Create and activate the conda environment, then run the app:
    - See [README_RUN_ENV.md](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/README_RUN_ENV.md) and [environment.yml](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/environment.yml).
- Alternate run helpers:
  - Start cloud server: [runCloudServer.bat](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/runCloudServer.bat)
  - Run main GUI: [run.bat](file:///c:/Final%20Year%20main%20project/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/Deep%20Learning%20for%20Edge%20Computing%20in%20Indian%20Smart%20Cities%20Real%20Time%20Analytics%20on%20Low%20Power%20Devices/run.bat)

## Demonstration Flow
- Start the cloud server.
- Open the GUI and load a video from `Videos/`.
- Run detection; observe boxes and live vehicle count.
- Send raw image to cloud (measure cloud delay).
- Send processed metadata to cloud (measure edge delay).
- Plot the delay comparison chart; inspect saved images in `TrafficImages/`.

## Assumptions and Constraints
- Localhost communication; both edge and cloud run on the same machine.
- Python 3.7 with specific package versions (TensorFlow 1.14, Keras 2.3.1, OpenCV 4.x).
- Designed for low-power devices; parameters favor speed over maximal accuracy.

## Limitations and Future Work
- Tracking robustness can be improved (e.g., SORT/DeepSORT).
- Model deployment optimizations (TensorRT, TFLite, quantization).
- Network transport beyond localhost; resilience, retries, and security hardening.
- Broader dataset coverage and model fine-tuning for local traffic patterns.

## Security and Privacy Notes
- No secrets stored in code; data sent over plain TCP (non-encrypted) for demo purposes.
- Consider encryption and access controls for production deployments.
