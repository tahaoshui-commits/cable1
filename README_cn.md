# 日行八万里：RDK X5 电缆缺陷智能检测系统

这是一个运行在地平线 RDK X5 开发板上的 ROS2 电缆缺陷检测项目。系统集成摄像头采集、YOLO26/BPU 缺陷识别、步进电机控制、Web 控制台、红外/阻抗辅助检测、数据集管理、训练转换流程和大模型智能助手，面向电缆巡检、教学演示和边缘 AI 应用验证。

## 核心功能

- 实时采集摄像头画面并推送到 Web 控制台
- 使用地平线 BPU 加速 YOLO26 电缆缺陷检测
- 检测到缺陷后联动电机控制、拍照留存和检测记录
- 支持红外图像温度分析和阻抗检测数据融合
- 提供检测报告、误报样本、复核样本池和数据集管理能力
- 支持上传 YOLO 数据集、训练、ONNX 导出、BIN 转换和模型暂存部署
- 提供右侧智能助手接口，可接入 OpenAI 兼容的大模型服务

## 硬件与运行环境

- 开发板：地平线 RDK X5
- 系统：Ubuntu + ROS2 Humble
- 推理：hobot_dnn / 地平线 BPU
- 摄像头：默认 `/dev/video0`
- 电机控制：GPIO BOARD 模式
- Web 服务：默认 `http://<board-ip>:8010`

## 项目结构

```text
cable1/
├── src/
│   ├── camera_pkg/          # 摄像头节点
│   ├── detection_pkg/       # 视觉/红外检测节点
│   ├── image_saver_pkg/     # 图像保存节点
│   └── motor_control_pkg/   # 电机控制与 Web 后端
├── launch/                  # ROS2 启动文件
├── config/                  # 运行参数和当前模型状态
├── scripts/                 # 训练到转换流水线脚本
├── tools/                   # YOLO26 导出和 mapper 工具
├── ad5933_cable_analyzer/   # 阻抗检测模块
└── agentic_aiops_clean_sidebar.html  # Web 前端页面
```

## 快速启动

```bash
cd /home/sunrise/cable1
source /opt/ros/humble/setup.bash
colcon build
source install/setup.bash
ros2 launch launch/cable_detection.launch.py
```

启动后在同一网络的浏览器访问：

```text
http://<board-ip>:8010
```

## API 密钥配置

仓库不会提交真实 API 密钥。当前代码按以下顺序读取密钥：

1. 优先读取环境变量，例如 `DASHSCOPE_API_KEY`、`LLM_API_KEY`
2. 如果没有环境变量，读取板子本地隐藏文件，例如 `/home/sunrise/cable1/.dashscope_key`

首次部署时可复制示例文件：

```bash
cp .dashscope_key.example .dashscope_key
chmod 600 .dashscope_key
```

然后把真实密钥写入 `.dashscope_key`。该文件已被 `.gitignore` 排除，不会进入 GitHub 仓库。

## 模型与数据

模型文件、训练输出、检测图片、报告、数据集和构建产物通常体积较大，默认不提交到仓库。建议按实际部署环境放置在：

- 模型目录：`models/`
- 数据集目录：`datasets/`
- 检测报告：`inspection_reports/`
- 训练输出：`runs/`

## 说明

本项目适合地瓜开发者社区 NodeHub 展示 RDK X5 在工业视觉巡检、边缘 AI 推理和多模态检测中的应用。发布仓库时已隐藏本地 API 密钥，同时保留环境变量和本地隐藏文件两种配置方式，不影响板端继续运行。

