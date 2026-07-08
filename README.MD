# Rixing Bawanli: RDK X5 Cable Defect Inspection System

This project is a ROS2-based cable defect inspection system for the Horizon RDK X5 board. It combines camera capture, YOLO26/BPU inference, stepper motor control, a web control console, infrared and impedance-assisted inspection, dataset management, training/export tooling, and an LLM-powered assistant.

## Features

- Real-time camera capture and web video streaming
- YOLO26 cable defect detection accelerated by the Horizon BPU
- Motor control, snapshot capture, and defect logging when defects are detected
- Infrared temperature reading and impedance data fusion for inspection reports
- False-positive samples, review sample pools, dataset upload, and model lifecycle tools
- Training, ONNX export, BIN conversion, and staged model deployment workflow
- OpenAI-compatible assistant endpoint for operator support

## Hardware And Runtime

- Board: Horizon RDK X5
- OS/runtime: Ubuntu with ROS2 Humble
- Inference: hobot_dnn / Horizon BPU
- Camera: `/dev/video0` by default
- Motor GPIO: BOARD mode
- Web service: `http://<board-ip>:8010`

## Project Layout

```text
cable1/
├── src/
│   ├── camera_pkg/          # Camera node
│   ├── detection_pkg/       # Visual and infrared detection nodes
│   ├── image_saver_pkg/     # Image saver node
│   └── motor_control_pkg/   # Motor control and web backend
├── launch/                  # ROS2 launch files
├── config/                  # Runtime configuration and model state
├── scripts/                 # Training-to-conversion pipeline scripts
├── tools/                   # YOLO26 export and mapper tools
├── ad5933_cable_analyzer/   # Impedance analysis module
└── agentic_aiops_clean_sidebar.html  # Web frontend
```

## Quick Start

```bash
cd /home/sunrise/cable1
source /opt/ros/humble/setup.bash
colcon build
source install/setup.bash
ros2 launch launch/cable_detection.launch.py
```

Then open the web console from a browser on the same network:

```text
http://<board-ip>:8010
```

## API Key Handling

Real API keys are not committed to this repository. The runtime keeps compatibility with existing deployments by reading credentials in this order:

1. Environment variables such as `DASHSCOPE_API_KEY` and `LLM_API_KEY`
2. Local hidden files on the board, such as `/home/sunrise/cable1/.dashscope_key`

For a fresh deployment:

```bash
cp .dashscope_key.example .dashscope_key
chmod 600 .dashscope_key
```

Put the real key in `.dashscope_key`. The file is ignored by git and should stay local to the board.

## Models And Data

Large or generated assets are intentionally excluded from the repository, including model weights, BPU binaries, datasets, training runs, captured images, inspection reports, and ROS build outputs. Place them in the deployment environment as needed:

- Models: `models/`
- Datasets: `datasets/`
- Reports: `inspection_reports/`
- Training outputs: `runs/`

## NodeHub Notes

This repository includes both `README_cn.md` and `README.MD` for NodeHub page ingestion. The project demonstrates industrial visual inspection, edge AI inference, and multimodal inspection workflows on the Horizon RDK X5 platform.

