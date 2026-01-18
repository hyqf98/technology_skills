---
name: yolo
description: Ultralytics YOLO（You Only Look Once）- 最新的实时目标检测和图像分割模型。包括 YOLO11、预测、训练、检测、分割、分类、姿态估计、目标跟踪和 SAM（Segment Anything Model）。
---

# YOLO 技能文档

基于官方文档生成的 Ultralytics YOLO 开发综合指南。

## 何时使用此技能

在以下场景中使用此技能：
- 开发计算机视觉应用（目标检测、图像分割等）
- 训练自定义 YOLO 模型
- 使用 YOLO 进行实时推理
- 学习 YOLO 最佳实践
- 调试 YOLO 代码

## 技术概述

**YOLO（You Only Look Once）** 是最流行的实时目标检测算法系列。Ultralytics YOLO11 是最新版本，提供统一框架用于目标检测、实例分割、姿态估计、目标跟踪和图像分类。

### 核心特性

- **实时性能**：最快的推理速度，适合边缘设备部署
- **高精度**：在多个基准测试中达到最先进的结果
- **统一框架**：一个模型支持多种任务
- **易于使用**：简单的 Python API，几行代码即可使用
- **跨平台支持**：支持导出到 ONNX、CoreML、TensorFlow 等格式
- **活跃社区**：丰富的文档、示例和社区支持

### 支持的任务

| 任务 | 描述 | 输出 |
|------|------|------|
| **目标检测** | 识别图像中的物体并定位边界框 | 类别 + 边界框 + 置信度 |
| **实例分割** | 检测物体并精确分割像素级轮廓 | 类别 + 边界框 + 分割掩码 |
| **姿态估计** | 检测人体关键点（如手肘、膝盖等） | 关键点坐标 + 连线 |
| **目标分类** | 对图像中的主要对象进行分类 | 类别 + 置信度 |
| **目标跟踪** | 在视频序列中跟踪多个目标 | 目标 ID + 轨迹 |
| **定向边界框** | 检测旋转的物体 | 旋转边界框 + 角度 |

## 快速参考

### 安装与配置

```bash
# 使用 pip 安装
pip install ultralytics

# 或使用 conda
conda install -c conda-forge ultralytics

# 安装可选依赖（用于导出）
pip install ultralytics[export]

# 从 GitHub 安装最新开发版本
pip install git+https://github.com/ultralytics/ultralytics.git
```

### 常见配置模式

#### 模式 1：目标检测推理

```python
from ultralytics import YOLO

# 加载预训练模型
model = YOLO('yolo11n.pt')  # nano 版本，速度最快
# model = YOLO('yolo11s.pt')  # small 版本
# model = YOLO('yolo11m.pt')  # medium 版本
# model = YOLO('yolo11l.pt')  # large 版本
# model = YOLO('yolo11x.pt')  # xlarge 版本，精度最高

# 对图像进行推理
results = model('path/to/image.jpg')

# 查看结果
for r in results:
    print(r.boxes)  # 打印边界框

# 可视化结果
results[0].show()

# 保存结果
results[0].save('output.jpg')
```

#### 模式 2：实例分割

```python
from ultralytics import YOLO

# 加载分割模型
model = YOLO('yolo11n-seg.pt')

# 执行推理
results = model('path/to/image.jpg')

# 查看分割掩码
for r in results:
    print(r.masks)  # 打印分割掩码

# 可视化
results[0].show()
```

#### 模式 3：姿态估计

```python
from ultralytics import YOLO

# 加载姿态估计模型
model = YOLO('yolo11n-pose.pt')

# 执行推理
results = model('path/to/image.jpg')

# 查看关键点
for r in results:
    print(r.keypoints)  # 打印关键点坐标

# 可视化姿态
results[0].show()
```

#### 模式 4：目标跟踪

```python
from ultralytics import YOLO

# 加载检测模型
model = YOLO('yolo11n.pt')

# 对视频进行目标跟踪
results = model.track('path/to/video.mp4', persist=True)

# 查看跟踪结果
for r in results:
    print(r.boxes.id)  # 打印目标 ID

# 保存带跟踪框的视频
model.track('video.mp4', save=True)
```

#### 模式 5：模型训练

```python
from ultralytics import YOLO

# 加载预训练模型
model = YOLO('yolo11n.pt')

# 训练模型
results = model.train(
    data='coco8.yaml',           # 数据集配置文件
    epochs=100,                   # 训练轮数
    imgsz=640,                    # 图像大小
    batch=16,                     # 批次大小
    name='yolo11_custom',         # 实验名称
    device=0,                     # GPU 设备（0 表示第一个 GPU）
    workers=8,                    # 数据加载工作线程数
    patience=50,                  # 早停耐心值
    save=True,                    # 保存检查点
    project='runs/detect',        # 项目目录
    exist_ok=True,                # 允许覆盖
    pretrained=True,              # 使用预训练权重
    optimizer='AdamW',            # 优化器
    lr0=0.01,                     # 初始学习率
    lrf=0.01,                     # 最终学习率因子
    momentum=0.937,               # SGD 动量
    weight_decay=0.0005,          # 权重衰减
    warmup_epochs=3.0,            # 预热轮数
    warmup_momentum=0.8,          # 预热动量
    warmup_bias_lr=0.1,           # 预热偏置学习率
    box=7.5,                      # box 损失权重
    cls=0.5,                      # cls 损失权重
    dfl=1.5,                      # dfl 损失权重
    mosaic=1.0,                   # 数据增强 - 马赛克
    mixup=0.0,                    # 数据增强 - 混合
)

# 训练完成后，最佳模型保存在 runs/detect/yolo11_custom/weights/best.pt
```

#### 模式 6：自定义数据集训练

```yaml
# custom_data.yaml
path: ../datasets/custom  # 数据集根目录
train: images/train       # 训练图像目录
val: images/val           # 验证图像目录
test: images/test         # 测试图像目录（可选）

# 类别
names:
  0: person
  1: car
  2: dog
  3: cat

# 类别数量
nc: 4
```

```python
from ultralytics import YOLO

# 加载模型
model = YOLO('yolo11n.pt')

# 使用自定义数据集训练
model.train(
    data='custom_data.yaml',
    epochs=200,
    imgsz=640,
    batch=16,
    device=0
)
```

#### 模式 7：模型验证

```python
from ultralytics import YOLO

# 加载训练好的模型
model = YOLO('runs/detect/yolo11_custom/weights/best.pt')

# 验证模型
metrics = model.val(
    data='coco8.yaml',     # 验证数据集
    split='val',           # 验证集分割
    batch=16,              # 批次大小
    imgsz=640,             # 图像大小
    device=0,              # GPU 设备
    plots=True,            # 生成验证图表
    save_json=True,        # 保存 JSON 结果
    conf=0.25,             # 置信度阈值
    iou=0.6,               # NMS IOU 阈值
    max_det=300            # 每张图像最大检测数
)

# 打印指标
print(f"mAP50: {metrics.box.map50}")
print(f"mAP50-95: {metrics.box.map}")
print(f"Precision: {metrics.box.mp}")
print(f"Recall: {metrics.box.mr}")
```

#### 模式 8：模型导出

```python
from ultralytics import YOLO

# 加载模型
model = YOLO('yolo11n.pt')

# 导出到 ONNX 格式
model.export(
    format='onnx',          # 导出格式
    imgsz=640,              # 图像大小
    dynamic=False,          # 动态轴
    simplify=True,          # 简化模型
    opset=12,               # ONNX opset 版本
)

# 导出到其他格式
# model.export(format='torchscript')  # TorchScript
# model.export(format='coreml')       # CoreML (macOS)
# model.export(format='tflite')       # TensorFlow Lite
# model.export(format='engine')       # TensorRT
# model.export(format='pb')           # TensorFlow SavedModel
```

### 代码示例模式

#### 示例 1：批量图像推理

```python
from ultralytics import YOLO

model = YOLO('yolo11n.pt')

# 推理多张图像
results = model(['image1.jpg', 'image2.jpg', 'image3.jpg'])

# 或使用通配符
results = model('path/to/images/*.jpg')

# 处理结果
for result in results:
    result.show()
```

#### 示例 2：视频流推理

```python
from ultralytics import YOLO

model = YOLO('yolo11n.pt')

# 处理视频文件
results = model('video.mp4', save=True, device=0)

# 处理网络摄像头
# results = model(source=0, show=True)  # 0 是默认摄像头

# 处理视频流
# results = model('rtsp://example.com/stream.mp4')
```

#### 示例 3：获取详细结果

```python
from ultralytics import YOLO
import cv2

model = YOLO('yolo11n.pt')
results = model('image.jpg')

# 获取第一张图像的结果
r = results[0]

# 获取边界框
boxes = r.boxes
for box in boxes:
    # 获取类别
    class_id = int(box.cls[0])
    class_name = model.names[class_id]

    # 获取置信度
    confidence = float(box.conf[0])

    # 获取边界框坐标 [x1, y1, x2, y2]
    bbox = box.xyxy[0].tolist()

    print(f"类别: {class_name}, 置信度: {confidence:.2f}, 边界框: {bbox}")

# 获取分割掩码（如果有）
if r.masks:
    masks = r.masks
    for mask in masks:
        # 获取掩码数组
        mask_array = mask.data.cpu().numpy()
        print(f"掩码形状: {mask_array.shape}")

# 在图像上绘制结果
annotated_frame = r.plot()
cv2.imwrite('output.jpg', annotated_frame)
```

#### 示例 4：使用 Python API 进行检测

```python
from ultralytics import YOLO
import numpy as np

model = YOLO('yolo11n.pt')

# 使用 NumPy 数组
img = np.zeros((640, 640, 3), dtype=np.uint8)
results = model(img)

# 使用 PIL Image
from PIL import Image
img = Image.new('RGB', (640, 640))
results = model(img)

# 使用 OpenCV
import cv2
img = cv2.imread('image.jpg')
results = model(img)
```

#### 示例 5：自定义推理参数

```python
from ultralytics import YOLO

model = YOLO('yolo11n.pt')

# 自定义推理参数
results = model(
    'image.jpg',
    conf=0.25,          # 置信度阈值（0-1）
    iou=0.45,           # NMS IOU 阈值
    max_det=100,        # 每张图像最大检测数
    device='cpu',       # 设备（cpu, 0, 1, 2, ...）
    view_img=False,     # 显示结果
    save=False,         # 保存结果
    classes=None,       # 类别过滤（例如 [0, 1]）
    agnostic_nms=False, # 类别不可知的 NMS
    augment=False,      # 增强推理
    visualize=False,    # 可视化模型特征
    imgsz=640,          # 推理图像大小
    half=False,         # 使用 FP16
    dnn=False,          # 使用 OpenCV DNN
    vid_stride=1,       # 视频帧步长
)
```

## 模型变体对比

| 模型 | mAPval-TP | Speed(T4 Triton) | 参数量 | FLOPs |
|------|-----------|------------------|--------|-------|
| **YOLO11n** | 37.1 | 1.05ms | 2.6M | 6.1B |
| **YOLO11s** | 44.3 | 1.67ms | 9.4M | 21.5B |
| **YOLO11m** | 51.1 | 3.08ms | 20.1M | 68.0B |
| **YOLO11l** | 53.4 | 5.48ms | 25.3M | 86.7B |
| **YOLO11x** | 54.7 | 10.3ms | 56.9M | 194.3B |

**选择建议：**
- **边缘设备/移动端**：使用 YOLO11n 或 YOLO11s
- **平衡精度和速度**：使用 YOLO11m
- **高精度需求**：使用 YOLO11l 或 YOLO11x

## 数据集格式

### 目标检测数据集

```
datasets/custom/
├── images/
│   ├── train/
│   │   ├── image1.jpg
│   │   └── image2.jpg
│   ├── val/
│   └── test/
├── labels/
│   ├── train/
│   │   ├── image1.txt
│   │   └── image2.txt
│   ├── val/
│   └── test/
└── data.yaml
```

**标签格式 (image1.txt):**
```
<class_id> <x_center> <y_center> <width> <height>
0 0.5 0.5 0.3 0.4
1 0.2 0.3 0.1 0.2
```

坐标是相对于图像宽高的归一化值（0-1）。

### 实例分割数据集

分割数据集使用相同的目录结构，但标签文件包含额外的多边形坐标：
```
<class_id> <x1> <y1> <x2> <y2> ... <xn> <yn> <x1> <y1> <x2> <y2> ...
0 0.1 0.1 0.2 0.1 0.2 0.2 0.1 0.2
```

## 最佳实践

### 1. 训练优化

```python
# 使用混合精度训练加速
model.train(amp=True)

# 使用缓存加速数据加载
model.train(cache='ram')  # 或 'disk'

# 使用多 GPU 训练
model.train(device=[0, 1, 2, 3])

# 使用自动批次大小
model.train(batch=-1)  # 自动批次
```

### 2. 推理加速

```python
# 使用半精度 FP16
model = YOLO('yolo11n.pt')
results = model('image.jpg', half=True)

# 使用 TensorRT 加速（需要先导出）
model.export(format='engine')
engine_model = YOLO('yolo11n.engine')
results = engine_model('image.jpg')
```

### 3. 数据增强

```python
# 训练时使用数据增强
model.train(
    hsv_h=0.015,        # 色调增强
    hsv_s=0.7,          # 饱和度增强
    hsv_v=0.4,          # 明度增强
    degrees=0.0,        # 旋转（±deg）
    translate=0.1,      # 平移（±fraction）
    scale=0.5,          # 缩放（gain）
    shear=0.0,          # 剪切（±deg）
    perspective=0.0,    # 透视变换（±fraction）
    flipud=0.5,         # 上下翻转概率
    fliplr=0.5,         # 左右翻转概率
    mosaic=1.0,         # 马赛克增强
    mixup=0.0,          # 混合增强
)
```

## 常见问题解决

| 问题 | 解决方案 |
|------|---------|
| CUDA 内存不足 | 减小 `batch` 或 `imgsz` 参数 |
| 训练速度慢 | 使用 `cache=True` 或减小图像尺寸 |
| 精度不理想 | 增加训练轮数、使用更大的模型或增加数据增强 |
| 导出失败 | 更新 `ultralytics` 和相关依赖 |
| 检测框不准确 | 调整 `conf` 和 `iou` 阈值 |

## 参考文档

此技能在 `references/` 目录中包含全面的文档：

| 文档 | 描述 |
|------|------|
| **advanced.md** | 高级功能文档 |
| **classification.md** | 分类任务文档 |
| **configuration.md** | 配置文档 |
| **datasets.md** | 数据集文档 |
| **deployment.md** | 部署指南 |
| **detection.md** | 检测任务文档 |
| **getting_started.md** | 入门指南 |
| **guides.md** | 指南文档 |
| **models.md** | 模型文档 |
| **other.md** | 其他文档 |
| **prediction.md** | 预测文档 |
| **segmentation.md** | 分割任务文档 |
| **tracking.md** | 跟踪任务文档 |
| **training.md** | 训练文档 |
| **utils.md** | 工具函数文档 |

## 使用指南

### 对于初学者
1. 从 `getting_started.md` 开始了解基础
2. 安装 `ultralytics` 库
3. 使用预训练模型进行推理
4. 了解数据集格式
5. 尝试在自定义数据集上训练

### 对于特定任务
- **目标检测**：参考 `detection.md`
- **实例分割**：参考 `segmentation.md`
- **姿态估计**：参考 `guides.md`
- **目标跟踪**：参考 `tracking.md`

### 对于部署
- 导出到目标格式（ONNX、TensorRT 等）
- 参考部署相关文档
- 优化模型大小和速度

## 资源

### references/
从官方来源提取的组织化文档，包含：
- 详细的功能说明
- 带语言标注的代码示例
- 原始文档链接
- 快速导航目录

### scripts/
添加用于常见自动化任务的辅助脚本。

### assets/
添加模板、样板代码或示例项目。

## 注意事项

- 此技能从官方文档自动生成
- 确保有足够的 GPU 内存用于训练
- 训练前准备好正确格式的数据集
- 推理速度和精度需要根据应用场景权衡

## 更新说明

刷新此技能的文档：
1. 使用相同配置重新运行爬虫
2. 技能将使用最新信息重建

## 相关资源

### 官方文档
- [Ultralytics YOLO 官方文档](https://docs.ultralytics.com/)
- [Ultralytics YOLO GitHub](https://github.com/ultralytics/ultralytics)
- [YOLO11 使用教程](https://www.ultralytics.com/blog/how-to-use-ultralytics-yolo11-for-object-detection)

### 教程与指南
- [Getting Started with YOLO11](https://pyimagesearch.com/2025/01/13/getting-started-with-yolo11/)
- [YOLO11: Redefining Real-Time Object Detection](https://learnopencv.com/yolo11/)
- [How to Train YOLO11 Instance Segmentation on Custom Data](https://blog.roboflow.com/train-yolo11-segmentation/)
- [YOLO11: Step-by-Step Training on Custom Data](https://datature.com/blog/yolo11-step-by-step-training-on-custom-data-and-comparison-with-yolov8)
