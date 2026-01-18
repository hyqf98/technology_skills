# YOLO11 实例分割完全指南

**页面数:** 完整版

本文档全面介绍 YOLO11 的实例分割功能，包括数据增强、训练技巧、模型优化和实际应用。所有代码示例均使用 YOLO11 模型。

---

## 目录

1. [实例分割概述](#1-实例分割概述)
2. [数据增强技术](#2-数据增强技术)
3. [训练配置与参数](#3-训练配置与参数)
4. [模型评估与优化](#4-模型评估与优化)
5. [实际应用案例](#5-实际应用案例)
6. [高级技巧](#6-高级技巧)
7. [故障排除](#7-故障排除)

---

## 1. 实例分割概述

### 1.1 什么是实例分割？

实例分割是计算机视觉中的一项高级任务，它不仅要检测图像中的对象，还要为每个对象生成精确的像素级掩码。与语义分割不同，实例分割可以区分同一类别的不同实例。

**任务对比:**

| 任务类型 | 目标 | 输出 | 难度 |
|---------|------|------|------|
| **目标检测** | 定位对象 | 边界框 | ⭐⭐ |
| **语义分割** | 像素分类 | 像素级标签 | ⭐⭐⭐ |
| **实例分割** | 实例级分割 | 掩码 + 边界框 | ⭐⭐⭐⭐⭐ |

**YOLO11 实例分割的优势:**

1. **实时性能**: 在保持高精度的同时实现实时推理
2. **端到端训练**: 单阶段检测器，简化训练流程
3. **多尺度特征**: 有效处理不同大小的对象
4. **灵活部署**: 支持多种导出格式和平台

### 1.2 YOLO11 分割架构

```python
# YOLO11 分割模型结构示例
from ultralytics import YOLO

# 加载预训练的实例分割模型
model = YOLO("yolo11n-seg.pt")  # nano 版本
# model = YOLO("yolo11s-seg.pt")  # small 版本
# model = YOLO("yolo11m-seg.pt")  # medium 版本
# model = YOLO("yolo11l-seg.pt")  # large 版本
# model = YOLO("yolo11x-seg.pt")  # xlarge 版本

# 查看模型结构
print(model.info())
print(model.model)
```

**模型组件:**

```python
def analyze_yolo11_seg_architecture():
    """分析 YOLO11 分割模型的架构"""
    model = YOLO("yolo11n-seg.pt")

    # 核心组件
    components = {
        "Backbone": "CSPDarknet 提取特征",
        "Neck": "PANet 特征融合",
        "Detect Head": "边界框预测",
        "Segment Head": "掩码预测",
        "Proto": "原型网络生成掩码系数"
    }

    print("YOLO11-Seg 模型组件:")
    for name, description in components.items():
        print(f"  {name}: {description}")

    # 参数统计
    print(f"\n总参数量: {sum(p.numel() for p in model.model.parameters()):,}")
    print(f"可训练参数: {sum(p.numel() for p in model.model.parameters() if p.requires_grad):,}")

analyze_yolo11_seg_architecture()
```

### 1.3 快速开始

```python
# 示例 1: 基本的实例分割推理
from ultralytics import YOLO
import cv2

# 加载模型
model = YOLO("yolo11n-seg.pt")

# 对图像进行推理
results = model("test_image.jpg")

# 可视化结果
for r in results:
    # 获取检测到的对象
    boxes = r.boxes  # 边界框
    masks = r.masks  # 分割掩码

    print(f"检测到 {len(boxes)} 个对象")

    # 绘制结果
    annotated = r.plot()  # 自动绘制边界框和掩码
    cv2.imwrite("result.jpg", annotated)

    # 访问单个掩码
    if masks:
        for i, mask in enumerate(masks):
            # mask.data: [1, H, W] 的张量
            mask_array = mask.data.cpu().numpy()[0]
            print(f"掩码 {i} 形状: {mask_array.shape}")
```

```python
# 示例 2: 批量处理图像
from ultralytics import YOLO
from pathlib import Path

def batch_segmentation(image_dir, output_dir):
    """批量实例分割"""
    model = YOLO("yolo11n-seg.pt")

    image_dir = Path(image_dir)
    output_dir = Path(output_dir)
    output_dir.mkdir(parents=True, exist_ok=True)

    # 处理所有图像
    image_files = list(image_dir.glob("*.jpg")) + list(image_dir.glob("*.png"))

    print(f"处理 {len(image_files)} 张图像...")

    for i, img_path in enumerate(image_files, 1):
        # 推理
        results = model(str(img_path))

        # 保存结果
        for r in results:
            r.save(str(output_dir / f"result_{img_path.name}"))

        print(f"[{i}/{len(image_files)}] 完成: {img_path.name}")

# 使用示例
batch_segmentation("input_images", "segmentation_results")
```

---

## 2. 数据增强技术

### 2.1 基础数据增强

YOLO11 提供了丰富的数据增强技术，专门针对实例分割任务优化。

```python
# 示例 1: Mosaic 增强（4 图拼接）
from ultralytics import YOLO

def train_with_mosaic():
    """使用 Mosaic 增强训练"""
    model = YOLO("yolo11n-seg.pt")

    results = model.train(
        data="coco8-seg.yaml",  # 分割数据集配置
        epochs=100,
        imgsz=640,

        # Mosaic 增强
        mosaic=1.0,  # Mosaic 概率 (0.0-1.0)
        close_mosaic=10,  # 最后 10 个 epoch 关闭 Mosaic

        # 其他增强参数
        hsv_h=0.015,  # HSV 色调增强
        hsv_s=0.7,    # HSV 饱和度增强
        hsv_v=0.4,    # HSV 明度增强

        degrees=0.0,  # 旋转角度
        translate=0.1,  # 平移
        scale=0.5,    # 缩放
        shear=0.0,    # 剪切
        perspective=0.0,  # 透视变换
        flipud=0.0,  # 上下翻转概率
        fliplr=0.5,  # 左右翻转概率
    )

    return results

train_with_mosaic()
```

**Mosaic 增强说明:**

| 参数 | 范围 | 说明 | 推荐值 |
|------|------|------|--------|
| `mosaic` | 0.0-1.0 | 启用 Mosaic 的概率 | 1.0 |
| `close_mosaic` | int | 最后 N 个 epoch 关闭 | 10-15 |
| `mixup` | 0.0-1.0 | MixUp 增强 | 0.0-0.15 |

```python
# 示例 2: MixUp 增强
def train_with_mixup():
    """使用 MixUp 增强训练"""
    model = YOLO("yolo11s-seg.pt")

    results = model.train(
        data="coco8-seg.yaml",
        epochs=100,
        imgsz=640,

        # MixUp 增强
        mixup=0.15,  # MixUp 概率

        # 配合使用 Copy-Paste
        copy_paste=0.5,  # Copy-Paste 概率

        # 注意: MixUp 和 Copy-Paste 主要用于分割任务
    )

    return results

train_with_mixup()
```

### 2.2 Copy-Paste 增强

Copy-Paste 是专门为实例分割设计的增强技术。

```python
# 示例 3: Copy-Paste 增强详解
def train_with_copy_paste():
    """使用 Copy-Paste 增强训练"""
    model = YOLO("yolo11n-seg.pt")

    results = model.train(
        data="custom-seg.yaml",
        epochs=150,
        imgsz=640,

        # Copy-Paste 配置
        copy_paste=0.5,  # Copy-Paste 概率

        # Copy-Paste 适合的场景
        # - 小数据集 (< 1000 张图像)
        # - 对象密集的场景
        # - 需要增加对象多样性

        # 注意事项
        # - Copy-Paste 会增加训练时间
        # - 确保掩码标注准确
        # - 仅在实例分割任务中有效
    )

    return results

train_with_copy_paste()
```

**Copy-Paste 工作原理:**

```python
# 示例 4: 理解 Copy-Paste 机制
from ultralytics.data.augment import CopyPaste
import cv2
import numpy as np

def demonstrate_copy_paste():
    """演示 Copy-Paste 增强原理"""

    # Copy-Paste 步骤:
    # 1. 从图像 A 中选择一个对象及其掩码
    # 2. 从图像 B 中随机选择位置
    # 3. 将图像 A 的对象粘贴到图像 B
    # 4. 更新图像 B 的标注

    print("Copy-Paste 增强步骤:")
    print("1. 从源图像提取对象 + 精确掩码")
    print("2. 在目标图像中选择随机位置")
    print("3. 使用掩码将对象粘贴到目标图像")
    print("4. 混合边界以实现自然融合")
    print("5. 更新标注: 添加新的边界框和掩码")

    # 实际应用由 YOLO 自动处理
    model = YOLO("yolo11n-seg.pt")
    model.train(
        data="coco8-seg.yaml",
        epochs=10,
        copy_paste=0.5
    )

demonstrate_copy_paste()
```

### 2.3 几何变换

```python
# 示例 5: 几何变换增强
def train_with_geometric_augmentation():
    """使用几何变换增强训练"""
    model = YOLO("yolo11m-seg.pt")

    results = model.train(
        data="coco8-seg.yaml",
        epochs=100,
        imgsz=640,

        # 旋转和翻转
        degrees=10.0,  # 旋转 ±10 度
        flipud=0.5,    # 上下翻转 50% 概率
        fliplr=0.5,    # 左右翻转 50% 概率

        # 缩放和平移
        scale=0.5,     # 缩放 ±50%
        translate=0.1, # 平移 ±10%

        # 剪切和透视
        shear=2.0,     # 剪切 ±2 度
        perspective=0.001,  # 透视变换

        # 注意: 几何变换会同时作用于图像、边界框和掩码
    )

    return results

train_with_geometric_augmentation()
```

### 2.4 颜色增强

```python
# 示例 6: HSV 颜色增强
def train_with_color_augmentation():
    """使用颜色增强训练"""
    model = YOLO("yolo11n-seg.pt")

    results = model.train(
        data="coco8-seg.yaml",
        epochs=100,
        imgsz=640,

        # HSV 增强
        hsv_h=0.015,  # 色调变化范围 (0.0-1.0)
        hsv_s=0.7,    # 饱和度变化范围 (0.0-1.0)
        hsv_v=0.4,    # 明度变化范围 (0.0-1.0)

        # 颜色增强建议:
        # - hsv_h: 通常使用较小值 (0.01-0.02)
        # - hsv_s: 中等值 (0.5-0.8)
        # - hsv_v: 中等值 (0.3-0.5)
    )

    return results

train_with_color_augmentation()
```

**HSV 增强参数建议:**

| 任务 | hsv_h | hsv_s | hsv_v |
|------|-------|-------|-------|
| **室内场景** | 0.01 | 0.5 | 0.3 |
| **室外场景** | 0.015 | 0.7 | 0.4 |
| **医学图像** | 0.005 | 0.3 | 0.2 |
| **卫星图像** | 0.02 | 0.8 | 0.5 |

---

## 3. 训练配置与参数

### 3.1 数据集准备

```python
# 示例 7: 创建分割数据集配置文件
def create_segmentation_dataset_yaml():
    """创建实例分割数据集配置"""

    yaml_content = """
# 数据集路径
path: ../datasets/custom_segmentation  # 数据集根目录
train: images/train  # 训练图像路径
val: images/val  # 验证图像路径
test: images/test  # 测试图像路径

# 类别信息
names:
  0: person
  1: car
  2: dog
  3: cat
  4: bicycle

# 数据集统计
nc: 5  # 类别数量

# 可选: 关键点（如果是姿态分割任务）
# kpt_shape: [17, 3]  # 关键点数量和维度
"""

    # 保存配置文件
    with open("custom-seg.yaml", "w") as f:
        f.write(yaml_content)

    print("✓ 数据集配置文件已创建: custom-seg.yaml")

create_segmentation_dataset_yaml()
```

**数据集目录结构:**

```
datasets/custom_segmentation/
├── data.yaml
├── images/
│   ├── train/
│   │   ├── img001.jpg
│   │   ├── img002.jpg
│   │   └── ...
│   ├── val/
│   └── test/
└── labels/
    ├── train/
    │   ├── img001.txt  # 包含边界框和掩码信息
    │   ├── img002.txt
    │   └── ...
    ├── val/
    └── test/
```

**标注格式说明:**

```python
# 示例 8: 理解分割标注格式
def explain_segmentation_label_format():
    """解释实例分割的标注格式"""

    # YOLO 分割格式: 每行一个对象
    # 格式: <class_id> <x1> <y1> <x2> <y2> ... <xn> <yn>
    # 其中 (x1, y1), (x2, y2), ..., (xn, yn) 是掩码的多边形顶点

    example_label = """
0 0.5 0.5 0.6 0.5 0.6 0.6 0.5 0.6  # 类别 0 的掩码（4 个顶点）
1 0.3 0.3 0.4 0.3 0.4 0.4 0.3 0.4 0.32 0.32  # 类别 1 的掩码（5 个顶点）
"""

    print("YOLO 分割标注格式:")
    print(example_label)

    # 坐标是相对于图像尺寸的归一化坐标 (0.0-1.0)

    # Python 解析示例
    def parse_segmentation_label(label_line):
        """解析分割标注行"""
        parts = list(map(float, label_line.strip().split()))
        class_id = int(parts[0])
        polygon_coords = parts[1:]

        # 将坐标重组为点对
        points = []
        for i in range(0, len(polygon_coords), 2):
            x, y = polygon_coords[i], polygon_coords[i+1]
            points.append((x, y))

        return class_id, points

    # 测试解析
    class_id, points = parse_segmentation_label("0 0.5 0.5 0.6 0.5 0.6 0.6 0.5 0.6")
    print(f"\n解析结果:")
    print(f"  类别 ID: {class_id}")
    print(f"  多边形顶点: {points}")

explain_segmentation_label_format()
```

### 3.2 训练参数详解

```python
# 示例 9: 完整的训练配置
def complete_segmentation_training():
    """完整的实例分割训练配置"""
    model = YOLO("yolo11n-seg.pt")

    results = model.train(
        # ========== 数据配置 ==========
        data="coco8-seg.yaml",  # 数据集配置文件
        imgsz=640,              # 输入图像尺寸
        batch=16,               # 批次大小
        rect=False,             # 矩形训练（保持不同宽高比）
        cache=False,            # 缓存图像（True: ram, False: disk, 'disk': disk缓存）

        # ========== 训练轮数 ==========
        epochs=100,             # 训练轮数
        patience=50,            # 早停耐心值
        save_period=10,         # 每 N 轮保存一次

        # ========== 优化器配置 ==========
        optimizer="SGD",        # 优化器: SGD, Adam, AdamW
        lr0=0.01,               # 初始学习率
        lrf=0.01,               # 最终学习率 (lr0 * lrf)
        momentum=0.937,         # SGD 动量
        weight_decay=0.0005,    # 权重衰减
        warmup_epochs=3.0,      # 预热轮数
        warmup_momentum=0.8,    # 预热初始动量
        warmup_bias_lr=0.1,     # 预热偏置学习率

        # ========== 学习率调度 ==========
        scheduler="cosine",     # 学习率调度器

        # ========== 数据增强 ==========
        mosaic=1.0,             # Mosaic 增强
        mixup=0.0,              # MixUp 增强
        copy_paste=0.0,         # Copy-Paste 增强
        degrees=0.0,            # 旋转
        translate=0.1,          # 平移
        scale=0.5,              # 缩放
        shear=0.0,              # 剪切
        perspective=0.0,        # 透视
        flipud=0.0,             # 上下翻转
        fliplr=0.5,             # 左右翻转
        hsv_h=0.015,            # HSV 色调
        hsv_s=0.7,              # HSV 饱和度
        hsv_v=0.4,              # HSV 明度

        # ========== 损失函数权重 ==========
        box=7.5,                # 边界框损失权重
        cls=0.5,                # 分类损失权重
        dfl=1.5,                # 分布焦点损失权重
        pose=12.0,              # 姿态损失权重（如果适用）
        kobj=1.0,               # 关键点对象性损失
        # segmentation mask loss
        mask=1.0,               # 掩码损失权重

        # ========== 其他参数 ==========
        workers=8,              # 数据加载线程数
        project="runs/train",   # 项目目录
        name="seg_exp",         # 实验名称
        exist_ok=False,         # 是否覆盖已存在的实验
        pretrained=True,        # 使用预训练权重
        amp=True,               # 自动混合精度训练
        device=0,               # 设备 (0: GPU 0, cpu: CPU)
        verbose=True,           # 详细输出
        seed=0,                 # 随机种子
        deterministic=True,     # 确定性训练
        single_cls=False,       # 单类别训练
        image_weights=False,    # 图像权重（难例挖掘）
        close_mosaic=10,        # 最后 N 轮关闭 Mosaic
        resume=False,           # 恢复训练
        fraction=1.0,           # 使用数据集的比例
        profile=False,          # 分析速度/内存
        overlap_mask=True,      # 掩码重叠训练
        mask_ratio=4,           # 掩码下采样比例
    )

    return results

complete_segmentation_training()
```

### 3.3 针对不同场景的配置

```python
# 示例 10: 小数据集训练配置
def train_small_dataset():
    """小数据集训练配置"""
    model = YOLO("yolo11n-seg.pt")

    results = model.train(
        data="small-dataset-seg.yaml",
        epochs=300,             # 更多轮数
        batch=8,                # 较小批次

        # 强数据增强
        mosaic=1.0,
        mixup=0.15,
        copy_paste=0.5,

        # 更大的颜色变化
        hsv_h=0.02,
        hsv_s=0.8,
        hsv_v=0.5,

        # 更多几何变换
        degrees=15.0,
        translate=0.2,
        scale=0.7,
        shear=5.0,
    )

    return results

# 示例 11: 大数据集训练配置
def train_large_dataset():
    """大数据集训练配置"""
    model = YOLO("yolo11x-seg.pt")

    results = model.train(
        data="large-dataset-seg.yaml",
        epochs=100,             # 较少轮数
        batch=32,               # 较大批次
        imgsz=1280,             # 更大图像尺寸

        # 适度的数据增强
        mosaic=1.0,
        mixup=0.0,
        copy_paste=0.0,

        # 较小的颜色变化
        hsv_h=0.01,
        hsv_s=0.5,
        hsv_v=0.3,
    )

    return results
```

---

## 4. 模型评估与优化

### 4.1 评估指标

```python
# 示例 12: 模型验证与指标计算
from ultralytics import YOLO

def evaluate_segmentation_model():
    """评估实例分割模型"""
    model = YOLO("yolo11n-seg.pt")

    # 验证模型
    metrics = model.val(
        data="coco8-seg.yaml",
        batch=16,
        imgsz=640,
        conf=0.25,              # 置信度阈值
        iou=0.6,                # NMS IoU 阈值
        max_det=300,            # 每张图像最大检测数
        plots=True,             # 生成图表
        save_json=True,         # 保存 JSON 结果
        project="runs/val",
        name="seg_eval",
    )

    # 主要指标
    print("=" * 60)
    print("实例分割评估指标")
    print("=" * 60)

    # 边界框指标
    print(f"\n边界框指标:")
    print(f"  mAP50: {metrics.box.map50:.4f}")        # IoU=0.5 时的 mAP
    print(f"  mAP50-95: {metrics.box.map:.4f}")      # IoU=0.5:0.95 时的 mAP
    print(f"  mAP75: {metrics.box.map75:.4f}")       # IoU=0.75 时的 mAP
    print(f"  精确率: {metrics.box.mp:.4f}")          # 平均精确率
    print(f"  召回率: {metrics.box.mr:.4f}")          # 平均召回率

    # 掩码指标
    print(f"\n掩码指标:")
    print(f"  Mask mAP50: {metrics.mask.map50:.4f}")
    print(f"  Mask mAP50-95: {metrics.mask.map:.4f}")
    print(f"  Mask mAP75: {metrics.mask.map75:.4f}")

    # 各类别指标
    print(f"\n各类别指标:")
    for i, class_name in enumerate(metrics.names.values()):
        if i < len(metrics.box.maps):
            print(f"  {class_name}:")
            print(f"    mAP50: {metrics.box.maps[i]:.4f}")

    return metrics

metrics = evaluate_segmentation_model()
```

### 4.2 掩码质量分析

```python
# 示例 13: 分析掩码质量
def analyze_mask_quality(model_path, image_dir):
    """分析分割掩码的质量"""
    model = YOLO(model_path)

    from pathlib import Path
    import cv2
    import numpy as np

    image_dir = Path(image_dir)
    images = list(image_dir.glob("*.jpg"))[:10]  # 分析前 10 张

    for img_path in images:
        results = model(str(img_path))

        for r in results:
            if r.masks:
                masks = r.masks.data.cpu().numpy()  # [N, H, W]

                for i, mask in enumerate(masks):
                    # 计算掩码质量指标
                    mask_area = np.sum(mask > 0.5)
                    mask_ratio = mask_area / (mask.shape[0] * mask.shape[1])

                    # 边缘平滑度
                    mask_binary = (mask > 0.5).astype(np.uint8)
                    contours, _ = cv2.findContours(
                        mask_binary, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE
                    )

                    if contours:
                        perimeter = cv2.arcLength(contours[0], True)
                        area = cv2.contourArea(contours[0])
                        circularity = 4 * np.pi * area / (perimeter ** 2) if perimeter > 0 else 0

                        print(f"{img_path.name} - 掩码 {i}:")
                        print(f"  面积比例: {mask_ratio:.2%}")
                        print(f"  圆形度: {circularity:.2f}")

analyze_mask_quality("yolo11n-seg.pt", "test_images")
```

### 4.3 模型优化技巧

```python
# 示例 14: 后处理优化
def optimize_post_processing():
    """优化后处理参数"""
    model = YOLO("yolo11n-seg.pt")

    # 使用不同的后处理参数进行推理
    results = model(
        "test_image.jpg",
        conf=0.25,      # 置信度阈值 (0.0-1.0)
        iou=0.5,        # NMS IoU 阈值
        max_det=100,    # 最大检测数
        agnostic_nms=False,  # 类别无关 NMS
        classes=None,   # 过滤类别 [0, 1, 2]
        retina_masks=True,   # 使用高分辨率掩码
    )

    # 掩码后处理
    for r in results:
        if r.masks:
            # 获取原始掩码
            masks = r.masks.data  # [N, H, W]

            # 二值化掩码
            binary_masks = (masks > 0.5).cpu().numpy()

            # 形态学操作（去噪）
            kernel = cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (5, 5))
            for i in range(len(binary_masks)):
                mask = binary_masks[i].astype(np.uint8)
                mask = cv2.morphologyEx(mask, cv2.MORPH_CLOSE, kernel)
                mask = cv2.morphologyEx(mask, cv2.MORPH_OPEN, kernel)
                binary_masks[i] = mask

            print(f"优化后处理完成，处理了 {len(binary_masks)} 个掩码")

    return results

optimize_post_processing()
```

### 4.4 模型蒸馏

```python
# 示例 15: 知识蒸馏
def distill_segmentation_model():
    """将大模型知识蒸馏到小模型"""
    from ultralytics import YOLO

    # 教师模型（大模型）
    teacher = YOLO("yolo11x-seg.pt")
    teacher.eval()

    # 学生模型（小模型）
    student = YOLO("yolo11n-seg.pt")

    # 训练学生模型，使用教师模型的软标签
    results = student.train(
        data="coco8-seg.yaml",
        epochs=100,
        batch=16,

        # 蒸馏参数（需要在训练循环中实现）
        # temperature=5.0,        # 温度参数
        # alpha=0.7,             # 软标签权重
        # teacher=teacher,       # 教师模型
    )

    return results
```

---

## 5. 实际应用案例

### 5.1 医学图像分割

```python
# 示例 16: 医学图像细胞分割
def medical_image_segmentation():
    """医学图像中的细胞分割"""

    # 加载模型
    model = YOLO("yolo11n-seg.pt")

    # 训练配置（医学图像特点）
    results = model.train(
        data="medical-cells-seg.yaml",
        epochs=200,
        imgsz=1024,            # 高分辨率

        # 医学图像增强
        mosaic=0.5,            # 适度的 Mosaic
        mixup=0.0,             # 不使用 MixUp
        copy_paste=0.3,        # 使用 Copy-Paste

        # 颜色增强（医学图像通常颜色变化小）
        hsv_h=0.005,           # 很小的色调变化
        hsv_s=0.3,             # 适度的饱和度
        hsv_v=0.3,             # 适度的明度变化

        # 几何变换
        degrees=180,           # 全方向旋转
        scale=0.2,             # 较小的缩放
        fliplr=0.5,            # 水平翻转
        flipud=0.5,            # 垂直翻转
    )

    return results

medical_image_segmentation()
```

### 5.2 自动驾驶场景分割

```python
# 示例 17: 自动驾驶道路分割
def autonomous_driving_segmentation():
    """自动驾驶场景的道路分割"""
    model = YOLO("yolo11m-seg.pt")

    # 训练配置
    results = model.train(
        data="autonomous-driving-seg.yaml",
        epochs=150,
        imgsz=1280,            # 高分辨率

        # 针对道路场景的增强
        mosaic=1.0,            # 使用 Mosaic
        mixup=0.1,
        copy_paste=0.0,

        # 颜色增强（不同天气/光照）
        hsv_h=0.01,
        hsv_s=0.6,
        hsv_v=0.4,

        # 几何增强（不同视角）
        degrees=5.0,           # 小角度旋转
        translate=0.1,
        scale=0.3,
        perspective=0.001,     # 透视变换
    )

    return results

autonomous_driving_segmentation()
```

### 5.3 工业质检

```python
# 示例 18: 工业缺陷检测
def industrial_defect_detection():
    """工业产品缺陷检测与分割"""
    model = YOLO("yolo11s-seg.pt")

    results = model.train(
        data="industrial-defects-seg.yaml",
        epochs=300,
        imgsz=640,

        # 缺陷检测需要强增强
        mosaic=1.0,
        mixup=0.15,
        copy_paste=0.5,

        # 颜色增强
        hsv_h=0.02,
        hsv_s=0.7,
        hsv_v=0.5,

        # 几何增强
        degrees=15.0,
        scale=0.5,
        flipud=0.5,
        fliplr=0.5,

        # 工业图像可能有特定的光照条件
        # 可以添加自定义增强
    )

    return results

industrial_defect_detection()
```

### 5.4 视频对象分割

```python
# 示例 19: 视频中的对象分割
def video_object_segmentation(video_path, output_path):
    """视频中的对象分割"""
    model = YOLO("yolo11n-seg.pt")

    # 处理视频
    results = model(
        video_path,
        stream=True,         # 使用生成器模式
        conf=0.25,
        iou=0.5,
    )

    # 保存结果
    import cv2

    cap = cv2.VideoCapture(video_path)
    fps = cap.get(cv2.CAP_PROP_FPS)
    width = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
    height = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))

    fourcc = cv2.VideoWriter_fourcc(*'mp4v')
    out = cv2.VideoWriter(output_path, fourcc, fps, (width, height))

    frame_count = 0
    for r in results:
        # 绘制结果
        annotated_frame = r.plot()

        # 写入视频
        out.write(annotated_frame)
        frame_count += 1

        print(f"处理帧 {frame_count}", end='\r')

    cap.release()
    out.release()

    print(f"\n视频处理完成！共 {frame_count} 帧")

video_object_segmentation("input_video.mp4", "output_video.mp4")
```

---

## 6. 高级技巧

### 6.1 自定义损失函数

```python
# 示例 20: 自定义掩码损失
def custom_mask_loss():
    """实现自定义的掩码损失函数"""

    # Dice Loss
    def dice_loss(pred, target, smooth=1.0):
        """计算 Dice 损失"""
        pred_flat = pred.view(-1)
        target_flat = target.view(-1)

        intersection = (pred_flat * target_flat).sum()

        return 1 - (2. * intersection + smooth) / (
            pred_flat.sum() + target_flat.sum() + smooth
        )

    # Focal Loss
    def focal_loss(pred, target, alpha=0.25, gamma=2.0):
        """计算 Focal 损失"""
        bce = nn.functional.binary_cross_entropy_with_logits(pred, target, reduction='none')
        pt = torch.exp(-bce)
        focal_loss = alpha * (1 - pt) ** gamma * bce
        return focal_loss.mean()

    # 组合损失
    def combined_loss(pred_masks, target_masks):
        """组合 Dice Loss 和 Focal Loss"""
        dice = dice_loss(pred_masks, target_masks)
        focal = focal_loss(pred_masks, target_masks)
        return dice + focal

    print("自定义损失函数已定义")
    print("  - Dice Loss: 处理类别不平衡")
    print("  - Focal Loss: 关注难分样本")
    print("  - Combined: 结合两者优势")

custom_mask_loss()
```

### 6.2 多尺度训练

```python
# 示例 21: 多尺度训练策略
def multi_scale_training():
    """多尺度训练提高模型泛化能力"""
    model = YOLO("yolo11n-seg.pt")

    # 在训练过程中动态调整图像尺寸
    results = model.train(
        data="coco8-seg.yaml",
        epochs=100,
        imgsz=640,             # 基础尺寸

        # 多尺度训练参数
        scale=0.5,             # 允许的缩放范围 ±50%

        # YOLO 会在每个 epoch 随机选择:
        # - 320x320 (0.5x)
        # - 480x480 (0.75x)
        # - 640x640 (1.0x, 基础)
        # - 960x960 (1.5x)
        # - 1280x1280 (2.0x)

        # 注意: 多尺度训练需要更多内存
        batch=8,               # 可能需要减小批次
    )

    return results

multi_scale_training()
```

### 6.3 测试时增强 (TTA)

```python
# 示例 22: 测试时增强
def test_time_augmentation(model_path, image_path):
    """使用测试时增强提高精度"""
    model = YOLO(model_path)

    # 原始预测
    original_results = model(image_path, conf=0.25)

    # TTA 预测（多个增强版本的集成）
    tta_results = model(
        image_path,
        conf=0.25,
        augment=True,  # 启用 TTA
        # 包括:
        # - 原始图像
        # - 水平翻转
        # - 垂直翻转
        # - 对角线翻转
    )

    # 合并结果
    print(f"原始检测: {len(original_results[0].boxes)} 个对象")
    print(f"TTA 检测: {len(tta_results[0].boxes)} 个对象")

    return tta_results

test_time_augmentation("yolo11n-seg.pt", "test_image.jpg")
```

### 6.4 模型集成

```python
# 示例 23: 模型集成
def model_ensemble(image_path):
    """集成多个模型提高性能"""
    from ultralytics import YOLO

    # 加载多个模型
    models = [
        YOLO("yolo11n-seg.pt"),
        YOLO("yolo11s-seg.pt"),
        YOLO("yolo11m-seg.pt"),
    ]

    # 收集所有模型的预测
    all_detections = []

    for model in models:
        results = model(image_path, conf=0.25)
        all_detections.append(results[0])

    # 简单的 NMS 集成
    # 实际应用中可以使用更复杂的集成方法
    # 例如: 加权平均、投票机制等

    print(f"集成 {len(models)} 个模型的预测结果")

    return all_detections

model_ensemble("test_image.jpg")
```

---

## 7. 故障排除

### 7.1 常见问题

**问题 1: 掩码不准确**

```python
# 解决方案: 调整后处理参数
def fix_inaccurate_masks():
    """解决掩码不准确的问题"""
    model = YOLO("yolo11n-seg.pt")

    # 方法 1: 调整置信度阈值
    results = model("test_image.jpg", conf=0.5)  # 提高阈值

    # 方法 2: 使用高分辨率掩码
    results = model("test_image.jpg", retina_masks=True)

    # 方法 3: 调整掩码后处理
    for r in results:
        if r.masks:
            # 形态学操作平滑掩码
            pass

    return results
```

**问题 2: 小对象检测效果差**

```python
# 解决方案: 针对小对象的配置
def fix_small_object_detection():
    """解决小对象检测问题"""
    model = YOLO("yolo11n-seg.pt")

    results = model.train(
        data="coco8-seg.yaml",
        imgsz=1280,            # 提高输入分辨率

        # 针对小对象的增强
        mosaic=1.0,
        scale=0.9,             # 增加缩放范围

        # 调整检测头
        # ... (需要修改模型配置)
    )

    return results
```

**问题 3: 类别不平衡**

```python
# 解决方案: 处理类别不平衡
def fix_class_imbalance():
    """解决类别不平衡问题"""
    model = YOLO("yolo11n-seg.pt")

    # 方法 1: 使用类别权重
    # 在数据集中为不同类别设置权重

    # 方法 2: 过采样少数类
    # 方法 3: 欠采样多数类
    # 方法 4: 使用 Focal Loss

    results = model.train(
        data="imbalanced-seg.yaml",
        epochs=200,

        # 调整损失权重
        cls=1.0,  # 增加分类损失权重
    )

    return results
```

### 7.2 性能优化

```python
# 示例 24: 优化推理速度
def optimize_inference_speed():
    """优化推理速度"""
    model = YOLO("yolo11n-seg.pt")

    # 方法 1: 减少输入分辨率
    results = model("test_image.jpg", imgsz=320)

    # 方法 2: 使用更小的模型
    # model = YOLO("yolo11n-seg.pt")  # 最快

    # 方法 3: 导出为优化格式
    model.export(format="onnx")  # ONNX 格式
    model.export(format="engine")  # TensorRT 格式（最快）

    # 方法 4: 使用半精度
    results = model("test_image.jpg", half=True)

    # 方法 5: 批量推理
    results = model(["img1.jpg", "img2.jpg", "img3.jpg"], batch=8)

optimize_inference_speed()
```

### 7.3 调试技巧

```python
# 示例 25: 可视化训练过程
def debug_training():
    """调试训练过程"""
    model = YOLO("yolo11n-seg.pt")

    # 启用详细日志
    results = model.train(
        data="coco8-seg.yaml",
        epochs=10,
        verbose=True,

        # 保存频繁的检查点
        save_period=1,

        # 绘制训练曲线
        plots=True,

        # 分析速度和内存
        profile=True,
    )

    # 可视化预测结果
    results = model.val(
        data="coco8-seg.yaml",
        plots=True,  # 生成 PR 曲线、混淆矩阵等
    )

    return results

debug_training()
```

---

## 8. 总结

本文档全面介绍了 YOLO11 实例分割功能，包括:

### 核心要点

1. **数据增强**: Mosaic、MixUp、Copy-Paste 等增强技术
2. **训练配置**: 针对不同场景的参数设置
3. **模型评估**: mAP、精确率、召回率等指标
4. **实际应用**: 医学图像、自动驾驶、工业质检等
5. **高级技巧**: 自定义损失、多尺度训练、模型集成
6. **故障排除**: 常见问题及解决方案

### 最佳实践

- **数据准备**: 确保标注质量，使用适当的数据增强
- **模型选择**: 根据精度/速度需求选择合适的模型大小
- **训练策略**: 小数据集使用强增强，大数据集适度增强
- **评估**: 使用多个指标全面评估模型性能
- **优化**: 针对具体应用场景调优模型

### 相关资源

- **YOLO 文档**: https://docs.ultralytics.com
- **GitHub 仓库**: https://github.com/ultralytics/ultralytics
- **COCO 数据集**: https://cocodataset.org
- **实例分割论文**: "YOLACT: Real-time Instance Segmentation"

---

**文档版本:** YOLO11-Seg v1.0
**最后更新:** 2026-01-18
**作者:** YOLO 技术团队
