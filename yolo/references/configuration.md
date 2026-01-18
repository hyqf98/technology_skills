# Yolo - Configuration (配置文档)

**页数:** 3

---

## 概述

YOLO11 配置系统提供了灵活而强大的方式来定制模型训练、推理和部署过程。本文档详细介绍 YOLO11 的配置选项、参数设置和最佳实践。

---

## YOLO11 配置文件详解

### 配置文件结构

YOLO11 使用 YAML 格式的配置文件来管理模型和数据集设置。

#### 默认配置文件位置
```
ultralytics/cfg/default.yaml
ultralytics/cfg/datasets/  # 数据集配置
ultralytics/cfg/models/    # 模型架构配置
```

### 配置参数详解

#### 核心训练参数

| 参数名称 | 类型 | 默认值 | 说明 | 最佳实践 |
|---------|------|--------|------|----------|
| `model` | str | "yolo11n.pt" | 预训练模型路径 | 优先使用官方预训练模型 |
| `data` | str | None | 数据集 YAML 文件路径 | 确保路径正确且格式规范 |
| `epochs` | int | 100 | 训练轮数 | 小数据集可增加至 200-300 |
| `batch` | int | 16 | 批次大小 | 根据显存调整 (8/16/32/64) |
| `imgsz` | int | 640 | 输入图像尺寸 | 可选: 320/640/1280 |
| `lr0` | float | 0.01 | 初始学习率 | 微调时降低至 0.001-0.0001 |
| `lrf` | float | 0.01 | 最终学习率因子 | 通常设为 lr0 的 0.01 倍 |
| `momentum` | float | 0.937 | SGD 动量 | 保持默认值 |
| `weight_decay` | float | 0.0005 | 权重衰减 (L2正则化) | 防止过拟合 |
| `warmup_epochs` | int | 3 | 预热轮数 | 小数据集可增至 5 |
| `warmup_momentum` | float | 0.8 | 预热初始动量 | 保持默认值 |
| `warmup_bias_lr` | float | 0.1 | 预热偏置学习率 | 保持默认值 |

#### 数据增强参数

| 参数名称 | 类型 | 默认值 | 说明 | 推荐范围 |
|---------|------|--------|------|---------|
| `hsv_h` | float | 0.015 | 色调增强幅度 | 0.0-0.05 |
| `hsv_s` | float | 0.7 | 饱和度增强幅度 | 0.5-0.9 |
| `hsv_v` | float | 0.4 | 明度增强幅度 | 0.3-0.6 |
| `degrees` | float | 0.0 | 旋转角度范围 | 0.0-180.0 |
| `translate` | float | 0.1 | 平移比例 | 0.0-0.2 |
| `scale` | float | 0.5 | 缩放比例范围 | 0.5-0.9 |
| `shear` | float | 0.0 | 剪切角度 | 0.0-10.0 |
| `perspective` | float | 0.0 | 透视变换强度 | 0.0-0.001 |
| `flipud` | float | 0.0 | 上下翻转概率 | 0.0-0.5 |
| `fliplr` | float | 0.5 | 左右翻转概率 | 0.0-1.0 |
| `mosaic` | float | 1.0 | Mosaic 增强概率 | 0.0-1.0 |
| `mixup` | float | 0.0 | MixUp 增强概率 | 0.0-0.2 |

#### 优化器参数

| 参数名称 | 类型 | 默认值 | 说明 | 选择建议 |
|---------|------|--------|------|---------|
| `optimizer` | str | "auto" | 优化器类型 | 可选: SGD/Adam/AdamW |
| `cos_lr` | bool | False | 余弦学习率调度 | 推荐启用 |
| `lr_scheduler` | str | "linear" | 学习率调度器 | 可选: linear/cosine |
| `amp` | bool | True | 自动混合精度训练 | 推荐启用以加速 |

---

## YOLO11 完整配置示例

### 1. 快速训练配置 (快速验证)

```yaml
# quick_train.yaml
model: yolo11n.pt
data: coco8.yaml
epochs: 10
batch: 16
imgsz: 640
optimizer: AdamW
lr0: 0.001
weight_decay: 0.0005
hsv_h: 0.01
hsv_s: 0.5
hsv_v: 0.3
mosaic: 1.0
```

**使用场景:** 快速验证代码、调试数据集

### 2. 高精度训练配置

```yaml
# high_accuracy.yaml
model: yolo11x.pt
data: your_dataset.yaml
epochs: 300
batch: 8  # 大模型需要较小批次
imgsz: 1280  # 高分辨率
optimizer: AdamW
lr0: 0.0001
lrf: 0.01
momentum: 0.937
weight_decay: 0.0005
warmup_epochs: 5

# 数据增强
hsv_h: 0.015
hsv_s: 0.7
hsv_v: 0.4
degrees: 15.0
translate: 0.1
scale: 0.9
shear: 2.0
perspective: 0.0001
fliplr: 0.5
mosaic: 1.0
mixup: 0.15

# 高级选项
amp: True
cos_lr: True
close_mosaic: 10  # 最后10个epoch关闭mosaic
```

**使用场景:** 竞赛、高精度要求场景

### 3. 边缘设备部署配置

```yaml
# edge_deployment.yaml
model: yolo11n.pt  # Nano模型
data: your_dataset.yaml
epochs: 100
batch: 32
imgsz: 320  # 小尺寸适合边缘设备
optimizer: SGD
lr0: 0.01
momentum: 0.937
weight_decay: 0.0005

# 简化数据增强
hsv_h: 0.01
hsv_s: 0.5
hsv_v: 0.3
degrees: 5.0
translate: 0.05
scale: 0.7
fliplr: 0.5
mosaic: 1.0

# 量化准备
int8: True  # INT8量化
```

**使用场景:** 移动设备、嵌入式系统

### 4. 实时检测配置

```yaml
# realtime_detection.yaml
model: yolo11s.pt  # Small模型平衡速度和精度
data: your_dataset.yaml
epochs: 150
batch: 24
imgsz: 640
optimizer: AdamW
lr0: 0.002
momentum: 0.937
weight_decay: 0.0005

# 平衡的数据增强
hsv_h: 0.015
hsv_s: 0.6
hsv_v: 0.4
degrees: 10.0
translate: 0.1
scale: 0.8
fliplr: 0.5
mosaic: 1.0
mixup: 0.1

# 速度优化
amp: True
workers: 8
```

**使用场景:** 视频流处理、实时监控

---

## Python API 配置示例

### 基础配置

```python
from ultralytics import YOLO

# 加载模型
model = YOLO("yolo11n.pt")

# 配置训练参数
config = {
    "data": "coco8.yaml",
    "epochs": 100,
    "batch": 16,
    "imgsz": 640,
    "lr0": 0.01,
    "lrf": 0.01,
    "optimizer": "AdamW",
    "weight_decay": 0.0005,
    "hsv_h": 0.015,
    "hsv_s": 0.7,
    "hsv_v": 0.4,
    "degrees": 15.0,
    "translate": 0.1,
    "scale": 0.9,
    "fliplr": 0.5,
    "mosaic": 1.0,
    "mixup": 0.15,
    "amp": True,
    "cos_lr": True,
}

# 开始训练
results = model.train(**config)
```

### 高级配置回调

```python
from ultralytics import YOLO
from ultralytics.callbacks import on_train_start, on_train_end

# 自定义回调函数
def on_train_start_callback(trainer):
    """训练开始时的回调"""
    print(f"开始训练: {trainer.args.model}")
    print(f"数据集: {trainer.args.data}")
    print(f"设备: {trainer.device}")
    # 可以在这里添加自定义初始化代码

def on_train_end_callback(trainer):
    """训练结束时的回调"""
    print(f"训练完成!")
    print(f"最佳 mAP50: {trainer.best_metric}")
    # 可以在这里添加模型保存、日志记录等

model = YOLO("yolo11n.pt")

# 添加回调
model.add_callback("on_train_start", on_train_start_callback)
model.add_callback("on_train_end", on_train_end_callback)

# 训练模型
results = model.train(
    data="coco8.yaml",
    epochs=100,
    batch=16,
    imgsz=640,
)
```

---

## 命令行配置示例

### 基础训练命令

```bash
# 使用默认配置训练
yolo detect train data=coco8.yaml model=yolo11n.pt epochs=100

# 指定图像尺寸和批次大小
yolo detect train data=coco8.yaml model=yolo11n.pt epochs=100 imgsz=640 batch=16

# 使用自定义配置文件
yolo detect train data=coco8.yaml model=yolo11n.pt cfg=custom_config.yaml

# 指定设备 (GPU/CPU)
yolo detect train data=coco8.yaml model=yolo11n.pt epochs=100 device=0  # 使用第一块GPU
yolo detect train data=coco8.yaml model=yolo11n.pt epochs=100 device=cpu  # 使用CPU

# 多GPU训练
yolo detect train data=coco8.yaml model=yolo11n.pt epochs=100 device=[0,1]
```

### 高级训练命令

```bash
# 完整参数训练
yolo detect train \
    data=coco8.yaml \
    model=yolo11n.pt \
    epochs=300 \
    batch=16 \
    imgsz=640 \
    lr0=0.001 \
    lrf=0.01 \
    momentum=0.937 \
    weight_decay=0.0005 \
    warmup_epochs=5 \
    optimizer=AdamW \
    amp=True \
    cos_lr=True \
    hsv_h=0.015 \
    hsv_s=0.7 \
    hsv_v=0.4 \
    degrees=15.0 \
    translate=0.1 \
    scale=0.9 \
    fliplr=0.5 \
    mosaic=1.0 \
    mixup=0.15

# 恢复训练
yolo detect train resume model=runs/detect/train/weights/last.pt

# 验证模型
yolo detect val model=runs/detect/train/weights/best.pt data=coco8.yaml

# 导出模型
yolo detect export model=runs/detect/train/weights/best.pt format=onnx
```

---

## 配置最佳实践

### 1. 学习率调整策略

```python
from ultralytics import YOLO

model = YOLO("yolo11n.pt")

# 从头训练 - 使用较高学习率
model.train(
    data="coco8.yaml",
    epochs=100,
    lr0=0.01,  # 标准学习率
    cos_lr=True  # 余弦退火
)

# 微调预训练模型 - 使用较低学习率
model.train(
    data="custom_dataset.yaml",
    epochs=50,
    lr0=0.0001,  # 降低学习率10倍
    lrf=0.01,  # 最终学习率
    cos_lr=True
)
```

### 2. 批次大小选择

```python
# 根据显存大小选择合适的批次
import torch

# 检查可用显存
if torch.cuda.is_available():
    gpu_memory = torch.cuda.get_device_properties(0).total_memory / 1024**3  # GB
    print(f"GPU显存: {gpu_memory:.2f} GB")

    if gpu_memory >= 24:
        batch_size = 64
    elif gpu_memory >= 12:
        batch_size = 32
    elif gpu_memory >= 8:
        batch_size = 16
    else:
        batch_size = 8

    model = YOLO("yolo11n.pt")
    model.train(data="coco8.yaml", epochs=100, batch=batch_size)
```

### 3. 数据集配置

```yaml
# dataset_config.yaml
path: /path/to/dataset  # 数据集根目录
train: images/train  # 训练图像路径 (相对于path)
val: images/val      # 验证图像路径
test: images/test    # 测试图像路径 (可选)

# 类别名称映射
names:
  0: person
  1: car
  2: dog
  # ... 添加更多类别

# 数据集统计信息
nc: 3  # 类别数量

# 下载脚本 (可选)
download: https://github.com/ultralytics/assets/releases/download/v0.0.0/dataset.zip
```

### 4. 多尺度训练

```python
from ultralytics import YOLO

model = YOLO("yolo11n.pt")

# 使用多个图像尺寸进行训练
model.train(
    data="coco8.yaml",
    epochs=100,
    imgsz=640,  # 基础尺寸
    scale=0.5,  # 允许0.5倍缩放 (实际: 320-640范围)
    mosaic=True  # Mosaic增强自动处理多尺度
)
```

---

## 配置文件参考

### Reference for hub_sdk/helpers/logger.py

**URL:** https://docs.ultralytics.com/zh/hub/sdk/reference/helpers/logger/

**类: hub_sdk.helpers.logger.Logger**

用于处理日志消息的日志记录器配置类。

```python
from hub_sdk.helpers.logger import Logger

# 初始化日志记录器
logger = Logger(
    logger_name="my_app",
    log_format="%(asctime)s - %(name)s - %(levelname)s - %(message)s",
    log_level="INFO"
)

# 获取日志记录器实例
log = logger.get_logger()
log.info("这是一条信息日志")
log.warning("这是一条警告日志")
```

**参数说明:**

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| logger_name | str | None | 日志记录器名称 |
| log_format | str | None | 日志格式 |
| log_level | str | None | 日志级别 |

### Reference for ultralytics/solutions/config.py

**URL:** https://docs.ultralytics.com/zh/reference/solutions/config/

**类: ultralytics.solutions.config.SolutionConfig**

管理 Ultralytics Vision AI 解决方案的配置参数。

```python
from ultralytics.solutions.config import SolutionConfig

# 创建配置
cfg = SolutionConfig(
    model="yolo11n.pt",
    region=[(0, 0), (100, 0), (100, 100), (0, 100)]
)

# 更新配置
cfg.update(show=False, conf=0.3)

# 访问配置
print(cfg.model)
print(cfg.region)
```

**配置参数:**

| 参数 | 类型 | 说明 |
|------|------|------|
| model | str | 模型路径 |
| region | list | 区域坐标点 |
| show | bool | 是否显示结果 |
| conf | float | 置信度阈值 |

---

## 常见问题

### Q1: 如何选择合适的图像尺寸?

**A:**
- **320px**: 边缘设备、实时检测
- **640px**: 标准配置、平衡速度和精度
- **1280px**: 高精度要求、大目标检测
- **自定义**: 根据目标尺寸调整 (建议为32的倍数)

```python
# 小目标检测 - 使用更大尺寸
model.train(data="dataset.yaml", imgsz=1280)

# 实时检测 - 使用较小尺寸
model.train(data="dataset.yaml", imgsz=320)
```

### Q2: 如何设置学习率?

**A:**
```python
# 从头训练
lr0 = 0.01  # 标准学习率

# 微调预训练模型
lr0 = 0.0001  # 降低100倍

# 小数据集
lr0 = 0.001  # 降低10倍

# 使用学习率调度器
model.train(data="dataset.yaml", lr0=0.01, cos_lr=True)
```

### Q3: 如何调整批次大小?

**A:**
```python
# 根据显存调整
batch_size = 16  # 8GB显存
batch_size = 32  # 12GB显存
batch_size = 64  # 24GB显存

# 梯度累积模拟大批次
model.train(
    data="dataset.yaml",
    batch=16,      # 实际批次
    accumulate=4,  # 累积4步 = 有效批次64
    epochs=100
)
```

### Q4: 如何配置数据增强?

**A:**
```python
# 轻度增强 - 小数据集
model.train(
    data="dataset.yaml",
    hsv_h=0.01,    # 色调
    hsv_s=0.5,     # 饱和度
    hsv_v=0.3,     # 明度
    degrees=5.0,   # 旋转
    mosaic=1.0     # Mosaic
)

# 重度增强 - 防止过拟合
model.train(
    data="dataset.yaml",
    hsv_h=0.015,
    hsv_s=0.7,
    hsv_v=0.4,
    degrees=15.0,
    translate=0.1,
    scale=0.9,
    shear=2.0,
    fliplr=0.5,
    mosaic=1.0,
    mixup=0.15
)
```

---

## 技术说明

### 配置文件优先级

1. **命令行参数** (最高优先级)
2. **配置文件**
3. **默认值** (最低优先级)

```bash
# 命令行参数会覆盖配置文件
yolo train data=custom.yaml epochs=200 lr0=0.001
```

### 环境变量配置

```python
import os

# 设置缓存目录
os.environ["ULTRALYTICS_CACHE"] = "/path/to/cache"

# 设置日志级别
os.environ["LOGGER_LEVEL"] = "INFO"

# 设置模型下载目录
os.environ["MODEL_DIR"] = "/path/to/models"
```

### 动态配置更新

```python
from ultralytics import YOLO

model = YOLO("yolo11n.pt")

# 方法1: 使用update方法
model.train(data="coco8.yaml", epochs=100, batch=16)
model.train(data="coco8.yaml", epochs=200, batch=16)  # 更新epochs

# 方法2: 使用配置字典
config = {"data": "coco8.yaml", "epochs": 100, "batch": 16}
model.train(**config)

# 方法3: 链式配置
model = YOLO("yolo11n.pt")
model.train(data="coco8.yaml", epochs=100).val(data="coco8.yaml")
```

---

## 实际应用场景

### 场景1: 工业缺陷检测

```python
from ultralytics import YOLO

# 高精度配置
model = YOLO("yolo11x.pt")

model.train(
    data="defect_detection.yaml",
    epochs=300,
    batch=8,
    imgsz=1280,
    lr0=0.0001,
    optimizer="AdamW",
    weight_decay=0.0005,

    # 精细数据增强
    hsv_h=0.01,
    hsv_s=0.5,
    hsv_v=0.3,
    degrees=10.0,
    translate=0.05,
    scale=0.8,
    fliplr=0.5,

    # 高级选项
    amp=True,
    cos_lr=True,
    close_mosaic=10
)
```

### 场景2: 自动驾驶

```python
from ultralytics import YOLO

# 实时检测配置
model = YOLO("yolo11s.pt")

model.train(
    data="autonomous_driving.yaml",
    epochs=200,
    batch=24,
    imgsz=640,
    lr0=0.002,
    optimizer="AdamW",

    # 平衡增强
    hsv_h=0.015,
    hsv_s=0.6,
    hsv_v=0.4,
    degrees=10.0,
    translate=0.1,
    scale=0.8,
    fliplr=0.5,
    mosaic=1.0,

    # 速度优化
    amp=True,
    workers=8
)
```

### 场景3: 医疗影像分析

```python
from ultralytics import YOLO

# 超高精度配置
model = YOLO("yolo11x.pt")

model.train(
    data="medical_imaging.yaml",
    epochs=500,
    batch=4,
    imgsz=1280,
    lr0=0.00005,  # 极低学习率
    lrf=0.001,
    optimizer="AdamW",
    weight_decay=0.0001,

    # 轻度增强 - 保持医疗图像真实性
    hsv_h=0.005,
    hsv_s=0.3,
    hsv_v=0.2,
    degrees=5.0,
    translate=0.05,
    scale=0.9,
    fliplr=0.5,

    # 高级技术
    amp=True,
    cos_lr=True,
    patience=50  # 早停耐心值
)
```

---

## 配置文件模板

### 通用训练模板

```yaml
# universal_train.yaml
# 适用于大多数检测任务的通用配置

# 模型配置
model: yolo11n.pt

# 数据配置
data: null  # 需要用户指定
imgsz: 640
rect: false  # 矩形训练

# 训练配置
epochs: 100
batch: 16
workers: 8
patience: 50  # 早停耐心值

# 优化器配置
optimizer: AdamW
lr0: 0.01
lrf: 0.01
momentum: 0.937
weight_decay: 0.0005
warmup_epochs: 3
warmup_momentum: 0.8
warmup_bias_lr: 0.1

# 学习率调度
cos_lr: false
lr_scheduler: linear

# 数据增强
hsv_h: 0.015
hsv_s: 0.7
hsv_v: 0.4
degrees: 0.0
translate: 0.1
scale: 0.5
shear: 0.0
perspective: 0.0
flipud: 0.0
fliplr: 0.5
mosaic: 1.0
mixup: 0.0

# 高级选项
amp: true
cache: false
single_cls: false
overlap_mask: true
mask_ratio: 4
dropout: 0.0
val: true
plots: true
save: true
save_json: false
save_hybrid: false
project: runs/detect
name: exp
exist_ok: false
pretrained: true
resume: false
device: 0
```

---

## 总结

本文档详细介绍了 YOLO11 的配置系统，包括:

1. **核心参数**: 训练、优化、数据增强参数详解
2. **配置示例**: 快速训练、高精度、边缘部署等场景配置
3. **API 使用**: Python API 和命令行配置方法
4. **最佳实践**: 学习率、批次大小、数据集配置建议
5. **实际应用**: 工业、自动驾驶、医疗等场景配置
6. **技术说明**: 配置优先级、环境变量、动态更新

合理配置这些参数可以显著提升模型性能，加快训练速度，并适应不同的应用场景。

**参考资源:**
- [Ultralytics 官方文档](https://docs.ultralytics.com)
- [YOLO11 GitHub 仓库](https://github.com/ultralytics/ultralytics)
- [配置文件参考](https://github.com/ultralytics/ultralytics/blob/main/ultralytics/cfg)
