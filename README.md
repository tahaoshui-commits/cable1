# Cable Defect Detection System

基于ROS2的电缆缺陷检测系统，运行在地平线RDK X5开发板上。

## 功能特性

- ✅ 实时摄像头图像采集
- ✅ 基于地平线BPU的YOLO缺陷检测
- ✅ GPIO控制步进电机
- ✅ 缺陷在中央时自动停止电机
- ✅ 检测到缺陷立即拍照
- ✅ 每0.5秒定时保存图像
- ✅ 远程实时查看摄像头画面

## 系统架构

```
摄像头 → 检测节点 → 电机控制
              ↓
         图像保存节点
```

## 快速开始

### 1. 在RDK X5板子上构建

```bash
# 进入工作空间
cd cable_defect_detection

# 源ROS2环境
source /opt/ros/humble/setup.bash

# 构建
colcon build

# 源工作空间
source install/setup.bash
```

### 2. 准备模型文件

将模型文件复制到板子：
```bash
# 确保模型文件在: /home/sunrise/yolo26cable_bayese_640x640_nv12.bin
```

### 3. 启动系统

```bash
ros2 launch launch/cable_detection.launch.py
```

### 4. 在电脑上查看实时画面

```bash
# 在你的电脑上（需要与板子在同一网络）
source /opt/ros/humble/setup.bash
ros2 run rqt_image_view rqt_image_view /detection/annotated_image
```

## 项目结构

```
cable_defect_detection/
├── src/
│   ├── camera_pkg/          # 摄像头节点
│   ├── detection_pkg/       # 缺陷检测节点
│   ├── motor_control_pkg/   # 电机控制节点
│   └── image_saver_pkg/     # 图像保存节点
├── launch/                  # 启动文件
├── config/                  # 配置文件
├── models/                  # 模型文件目录
└── saved_images/           # 保存的图像
```

## 配置说明

编辑 `launch/cable_detection.launch.py` 修改参数：

- **摄像头**: `camera_id` (0或1)
- **模型路径**: `model_path`
- **中央阈值**: `center_threshold` (像素)
- **电机速度**: `step_delay` (秒)
- **保存间隔**: `save_interval` (秒)

## 依赖项

- ROS2 Humble
- hobot_dnn (地平线BPU推理库)
- Hobot.GPIO (GPIO控制库)
- OpenCV
- NumPy

## 硬件连接

- **摄像头**: /dev/video0
- **电机GPIO引脚** (BOARD模式):
  - DIR: 31
  - STEP: 32
  - EN: 36

## 详细文档

查看 [CLAUDE.md](CLAUDE.md) 获取完整的技术文档。
