# Deep Learning for Edge Computing in Indian Smart Cities

Real-time traffic analytics on low-power edge devices for smart city mobility monitoring.

This project demonstrates an edge–cloud traffic monitoring pipeline that detects and counts vehicles from video streams, sends processed information to a cloud server, and compares the delay between raw image transmission and edge-processed data transmission.

## Overview

The system combines:

- A Python desktop GUI for interaction and visualization
- YOLO-based vehicle detection using OpenCV and deep learning inference
- Edge-side traffic counting and tracking logic
- TCP communication between edge/client and cloud services
- Latency comparison for cloud versus edge transmission

This is designed as a practical research/demo project for intelligent transportation systems, especially in the context of Indian smart cities and low-power edge computing.

## Key Features

- Vehicle detection and counting from traffic camera video
- Real-time bounding box visualization over frames
- Edge-based processing to reduce raw data transmission
- Cloud server to receive either:
  - raw images, or
  - processed vehicle-count messages
- Delay comparison graph between cloud and edge communication
- Tkinter-based GUI for running the main workflow
- Support for Python 3.7 environment setup with Conda

## Project Architecture

The repository contains a compact edge/cloud architecture:

- `Main.py` — desktop GUI application and workflow orchestration
- `yolo_traffic.py` — YOLO detection, tracking, frame processing, and counting logic
- `CloudServer.py` — cloud server that receives data from edge devices
- `EdgeServer.py` — sample edge communication test/server sender
- `environment.yml` — Conda environment definition
- `requirements.txt` — package installation list
- `run.bat` and `runCloudServer.bat` — Windows startup shortcuts

## How It Works

1. The user launches the GUI from `Main.py`.
2. A video file is selected for traffic analysis.
3. The detection pipeline uses YOLO to identify vehicles in the frames.
4. Vehicles are tracked across frames and counted when crossing a reference line.
5. The processed result is either:
   - sent as raw image data to the cloud, or
   - sent as compact edge-processed metadata
6. The application measures transmission delay and displays a comparison graph.

## Repository Structure

```text
.
├── CloudServer.py
├── EdgeServer.py
├── Main.py
├── PROJECT_EXPLANATION.md
├── README_RUN_ENV.md
├── README.md
├── requirements.txt
├── environment.yml
├── run.bat
├── runCloudServer.bat
├── yolo_traffic.py
├── SCREENS.docx
├── commit
└── ... runtime-generated folders such as TrafficImages/ and model folders may appear during execution
```

## Technologies Used

- Python 3.7
- OpenCV
- TensorFlow / Keras
- PyTorch
- NumPy, Pandas, SciPy
- Matplotlib
- scikit-learn
- Tkinter
- Socket programming for TCP communication

## Installation

### Option 1: Conda Environment (recommended)

```bash
conda env create -f environment.yml
conda activate smartcity-py37
```

### Option 2: Pip Install

```bash
pip install -r requirements.txt
```

## Run the Project

### 1) Start the Cloud Server

On Windows:

```bat
runCloudServer.bat
```

Or directly:

```bash
python CloudServer.py
```

### 2) Run the Main GUI

```bat
run.bat
```

Or directly:

```bash
python Main.py
```

### 3) Use the Interface

From the GUI, you can:

- Train and load the deep learning algorithm
- Run traffic detection and vehicle counting
- Send raw image data to the cloud
- Send processed data using the edge server
- View delay comparison graphs

## Configuration Notes

- The project is configured for local host communication on port `2222`.
- Cloud and edge modules communicate using `pickle`-serialized messages over TCP.
- The application expects Python 3.7 compatibility due to TensorFlow 1.14 and related legacy dependencies.
- If there are dependency issues on Windows, install Visual C++ Build Tools and retry.

## Important Notes

- This project is primarily a research and demonstration prototype.
- It is optimized for experimentation and learning rather than production-grade deployment.
- Communication is intentionally simple and local for demonstration purposes.

## Future Improvements

- Upgrade to modern YOLO versions and improved tracking models
- Deploy optimized edge inference with TensorRT or TFLite
- Add secure communication and authentication for cloud services
- Expand dataset coverage and fine-tune for local traffic conditions
- Improve robustness with multi-object tracking algorithms like SORT/DeepSORT

## License

This project does not currently include a formal license file. Please check with the repository owner or original author before using it for commercial or public distribution.

## Acknowledgements

This project was developed for intelligent transportation and edge-computing research, demonstrating how deep learning traffic analytics can be implemented on low-power systems while comparing edge and cloud communication latency.

## Related files

- `PROJECT_EXPLANATION.md` — project description and architecture notes
- `README_RUN_ENV.md` — environment setup instructions
- `requirements.txt` — package list
- `environment.yml` — Conda environment setup

## Quick Start Summary

```bash
conda env create -f environment.yml
conda activate smartcity-py37
python CloudServer.py
python Main.py
```

Make sure the cloud server is running before using cloud/edge transmission features.

