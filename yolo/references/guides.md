# Yolo - Guides (实用指南)

**页数:** 6

---

## 概述

本指南提供了使用 Ultralytics YOLO11 进行计算机视觉项目的实用建议、最佳实践和常见问题解决方案。涵盖了从项目规划到部署的完整工作流程。

---

## 目录

1. [项目目标定义](#项目目标定义)
2. [数据收集与标注策略](#数据收集与标注策略)
3. [区域对象计数](#区域对象计数)
4. [语义图像搜索](#语义图像搜索)
5. [实例分割与追踪](#实例分割与追踪)
6. [锻炼监控](#锻炼监控)
7. [距离计算](#距离计算)
8. [VisionEye 视图映射](#visioneye-视图映射)
9. [数据分析与可视化](#数据分析与可视化)
10. [Neural Magic 部署](#neural-magic-部署)
11. [终端结果查看](#终端结果查看)

---

## 项目目标定义

### 为什么定义项目目标很重要?

清晰的 project 目标是成功的基石。它有助于:

- **选择合适的模型架构**
- **确定数据集需求**
- **评估项目成功标准**
- **优化资源配置**

### SMART 目标设定原则

```python
# 示例: 车辆速度估计项目

# ❌ 不明确的目标
目标: "检测车辆"

# ✅ SMART 目标
目标 = {
    "specific": "在高速公路场景中检测车辆并估计速度",
    "measurable": "达到 95% 的检测准确率，速度误差 < 5 km/h",
    "achievable": "使用 YOLO11 + 对象跟踪，10,000 张训练图像",
    "relevant": "解决现有雷达系统的低效问题",
    "time_bound": "6个月内完成开发和测试"
}
```

### 问题陈述模板

```markdown
## 项目问题陈述

### 当前问题
描述现有系统或方法的局限性

### 目标用户
- 主要用户: [列出主要利益相关者]
- 次要用户: [列出次要利益相关者]

### 成功标准
1. [可量化指标 1]
2. [可量化指标 2]
3. [可量化指标 3]

### 技术约束
- 数据可用性: [描述数据来源]
- 计算资源: [描述硬件限制]
- 时间限制: [项目时间表]
- 法规要求: [隐私和安全考虑]
```

### 计算机视觉任务选择指南

| 任务类型 | 适用场景 | YOLO11 模型 | 数据需求 |
|---------|---------|------------|---------|
| **目标检测** | 定位和分类对象 | yolo11n.pt | 每类 100+ 图像 |
| **实例分割** | 像素级对象轮廓 | yolo11n-seg.pt | 每类 500+ 图像 |
| **姿态估计** | 人体关键点检测 | yolo11n-pose.pt | 每类 1000+ 图像 |
| **目标跟踪** | 视频序列跟踪 | yolo11n.pt + tracker | 视频数据, 30+ 帧 |
| **OBB 检测** | 旋转边界框 | yolo11n-obb.pt | 每类 200+ 图像 |

### 实际应用示例

#### 场景 1: 交通监控

```python
from ultralytics import YOLO

# 问题: 估计高速公路上的车辆速度
# 任务: 目标跟踪
# 模型: yolo11n.pt
# 数据集: 自定义交通视频数据集

model = YOLO("yolo11n.pt")

# 训练模型
model.train(
    data="traffic_dataset.yaml",
    epochs=150,
    imgsz=640,
    device=0
)

# 部署速度估计
from ultralytics import solutions

speed_estimator = solutions.SpeedEstimator(
    model="yolo11n.pt",
    meter_per_pixel=0.04,  # 相机标定参数
    max_speed=120
)
```

#### 场景 2: 零售库存管理

```python
from ultralytics import YOLO

# 问题: 自动统计货架上的商品数量
# 任务: 对象检测 + 计数
# 模型: yolo11s.pt
# 数据集: 产品图像数据集

model = YOLO("yolo11s.pt")

# 训练
model.train(
    data="products_dataset.yaml",
    epochs=200,
    imgsz=640,
    batch=16
)

# 部署计数系统
counter = solutions.ObjectCounter(
    model="yolo11s.pt",
    region=[(0, 0), (640, 0), (640, 480), (0, 480)]
)
```

---

## 数据收集与标注策略

### 数据收集最佳实践

#### 1. 确定类别数量

```python
# ✅ 从具体类别开始
categories = {
    "vehicle": ["car", "truck", "bus", "motorcycle", "bicycle"],
    "person": ["adult", "child"],
    "traffic_sign": ["stop", "yield", "speed_limit"]
}

# ❌ 避免过于宽泛的类别
categories = {
    "object": ["everything"]  # 太宽泛,效果差
}
```

#### 2. 数据来源选择

| 来源类型 | 优点 | 缺点 | 适用场景 |
|---------|------|------|---------|
| **公共数据集** | 标注完善,多样性高 | 可能不匹配特定场景 | 快速原型开发 |
| **自定义收集** | 高度定制化 | 需要标注,成本高 | 生产环境部署 |
| **合成数据** | 完全可控 | 与真实数据有差距 | 数据增强 |
| **网络爬取** | 数据量丰富 | 版权问题,质量参差 | 研究项目 |

#### 3. 避免数据偏差

```python
# ✅ 均衡的数据集收集策略
balanced_dataset = {
    "天气": ["晴天", "阴天", "雨天", "雪天"],
    "时间": ["早晨", "中午", "傍晚", "夜晚"],
    "角度": ["正面", "侧面", "背面", "俯视"],
    "光照": ["强光", "正常", "弱光", "背光"],
    "背景": ["城市", "郊区", "室内", "自然"]
}

# 检查数据集偏差
from collections import Counter

def check_dataset_balance(labels):
    """检查类别分布是否均衡"""
    counts = Counter(labels)
    total = sum(counts.values())

    for cls, count in counts.items():
        ratio = count / total
        if ratio < 0.05:  # 少于 5%
            print(f"警告: {cls} 类别比例过低 ({ratio:.2%})")
        elif ratio > 0.5:  # 超过 50%
            print(f"警告: {cls} 类别比例过高 ({ratio:.2%})")
```

### 数据标注指南

#### 标注类型选择

| 任务类型 | 推荐格式 | 工具 | 复杂度 |
|---------|---------|------|--------|
| **目标检测** | YOLO txt | LabelImg, CVAT | 低 |
| **实例分割** | COCO JSON | LabelMe, CVAT | 中 |
| **姿态估计** | JSON (关键点) | CVAT, 自定义工具 | 高 |
| **OBB 检测** | YOLO-OBB txt | ROLabelImg, CVAT | 中 |

#### 标注质量控制

```python
# 标注一致性检查
import json

def validate_annotations(annotation_file):
    """验证标注文件的正确性"""
    with open(annotation_file) as f:
        annotations = json.load(f)

    errors = []

    for ann in annotations:
        # 检查边界框坐标
        bbox = ann.get("bbox", [])
        if len(bbox) != 4:
            errors.append(f"无效的边界框: {bbox}")

        # 检查类别 ID
        cls_id = ann.get("category_id")
        if not isinstance(cls_id, int) or cls_id < 0:
            errors.append(f"无效的类别 ID: {cls_id}")

        # 检查图像 ID
        img_id = ann.get("image_id")
        if img_id is None:
            errors.append(f"缺少图像 ID")

    if errors:
        print("发现标注错误:")
        for error in errors:
            print(f"  - {error}")
    else:
        print("标注验证通过!")

    return len(errors) == 0

# 标注者间一致性
def calculate_consistency(annotations_1, annotations_2):
    """计算两个标注者之间的一致性"""
    from sklearn.metrics import cohen_kappa_score

    labels_1 = [ann["category_id"] for ann in annotations_1]
    labels_2 = [ann["category_id"] for ann in annotations_2]

    kappa = cohen_kappa_score(labels_1, labels_2)

    if kappa > 0.8:
        print(f"一致性: 优秀 (κ={kappa:.3f})")
    elif kappa > 0.6:
        print(f"一致性: 良好 (κ={kappa:.3f})")
    else:
        print(f"一致性: 需要改进 (κ={kappa:.3f})")

    return kappa
```

#### 推荐标注工具

**1. CVAT (Computer Vision Annotation Tool)**

```bash
# 安装 CVAT
docker pull cvat/server
docker pull cvat/ui

# 启动服务
docker-compose up -d

# 访问 http://localhost:8080
```

**2. LabelImg**

```bash
# 安装
pip install labelImg

# 使用
labelImg data/images data/labels classes.txt
```

**3. LabelMe**

```bash
# 安装
pip install labelme

# 使用
labelme data/images --labels data/classes.txt --output data/annotations
```

### 数据增强策略

```python
from ultralytics import YOLO

# YOLO11 内置数据增强
model = YOLO("yolo11n.pt")

model.train(
    data="custom_dataset.yaml",
    epochs=100,

    # 颜色增强
    hsv_h=0.015,    # 色调变化
    hsv_s=0.7,      # 饱和度变化
    hsv_v=0.4,      # 明度变化

    # 几何增强
    degrees=15.0,   # 旋转角度
    translate=0.1,  # 平移比例
    scale=0.9,      # 缩放比例
    shear=2.0,      # 剪切角度
    perspective=0.0001,  # 透视变换
    fliplr=0.5,     # 左右翻转概率
    flipud=0.0,     # 上下翻转概率

    # 高级增强
    mosaic=1.0,     # Mosaic 增强
    mixup=0.15,     # MixUp 增强

    # 增强强度调度
    copy_paste=0.0  # Copy-Paste 增强
)
```

---

## 区域对象计数

### 什么是区域对象计数?

区域对象计数是指在视频帧的指定区域内精确统计对象数量的技术。与全帧计数相比,它提供了更精确的空间控制和灵活性。

### RegionCounter 参数说明

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `model` | str | "yolo11n.pt" | 检测模型路径 |
| `region` | list/dict | None | 区域坐标点 |
| `show` | bool | True | 是否显示结果 |
| `line_width` | int | 2 | 绘制线宽 |
| `classes` | list | None | 要计数的类别 |

### 实现代码示例

```python
import cv2
from ultralytics import solutions

# 初始化视频捕获
cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "无法读取视频文件"

# 获取视频属性
w, h, fps = (int(cap.get(x)) for x in (
    cv2.CAP_PROP_FRAME_WIDTH,
    cv2.CAP_PROP_FRAME_HEIGHT,
    cv2.CAP_PROP_FPS
))

# 创建视频写入器
video_writer = cv2.VideoWriter(
    "region_counting.avi",
    cv2.VideoWriter_fourcc(*"mp4v"),
    fps,
    (w, h)
)

# 方法1: 使用单个区域
region_points = [(20, 400), (1080, 400), (1080, 360), (20, 360)]

region_counter = solutions.RegionCounter(
    show=True,
    region=region_points,
    model="yolo11n.pt",
    classes=[0, 2]  # 只计数人和汽车
)

# 方法2: 使用多个区域
region_points = {
    "region-01": [(50, 50), (250, 50), (250, 250), (50, 250)],
    "region-02": [(640, 640), (780, 640), (780, 720), (640, 720)],
}

region_counter = solutions.RegionCounter(
    show=True,
    region=region_points,
    model="yolo11n.pt"
)

# 处理视频
while cap.isOpened():
    success, im0 = cap.read()

    if not success:
        print("视频帧为空或处理完成")
        break

    # 处理帧
    results = region_counter(im0)

    # 访问计数结果
    # print(results)  # 查看输出

    # 保存处理后的帧
    video_writer.write(results.plot_im)

cap.release()
video_writer.release()
cv2.destroyAllWindows()
```

### 命令行使用

```bash
# 基础使用
yolo solutions isegment show=True

# 指定视频源
yolo solutions isegment source="path/to/video.mp4"

# 监控特定类别
yolo solutions isegment classes="[0, 5]"
```

### 实际应用场景

#### 1. 零售店客流量统计

```python
import cv2
from ultralytics import solutions

# 定义门口区域
door_region = [(100, 200), (300, 200), (300, 400), (100, 400)]

counter = solutions.RegionCounter(
    model="yolo11n.pt",
    region=door_region,
    classes=[0],  # 只计数人
    show=True
)

# 实时计数
cap = cv2.VideoCapture(0)  # 使用摄像头
while True:
    ret, frame = cap.read()
    if not ret:
        break

    results = counter(frame)
    print(f"当前人数: {results.total_tracks}")

    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
```

#### 2. 停车场车辆计数

```python
import cv2
from ultralytics import solutions

# 定义停车位区域
parking_spots = {
    "zone_A": [(0, 0), (200, 0), (200, 300), (0, 300)],
    "zone_B": [(200, 0), (400, 0), (400, 300), (200, 300)],
    "zone_C": [(400, 0), (600, 0), (600, 300), (400, 300)],
}

counter = solutions.RegionCounter(
    model="yolo11n.pt",
    region=parking_spots,
    classes=[2, 3, 5, 7],  # car, motorcycle, bus, truck
    show=True
)

# 处理停车场监控视频
cap = cv2.VideoCapture("parking_lot.mp4")
while True:
    ret, frame = cap.read()
    if not ret:
        break

    results = counter(frame)

    # 统计各区域停车数
    for region_name, count in results.region_counts.items():
        print(f"{region_name}: {count} 辆车")

cap.release()
```

---

## 语义图像搜索

### 什么是语义图像搜索?

语义图像搜索使用自然语言查询在图像数据集中查找相似的图像。它结合了 OpenAI CLIP 和 Meta FAISS,实现零样式的跨模态搜索。

### VisualAISearch 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `data` | str | None | 图像目录路径 |
| `device` | str | "cpu" | 计算设备 (cpu/cuda) |

### 实现代码

```python
from ultralytics import solutions

# 创建搜索引擎
searcher = solutions.VisualAISearch(
    data="path/to/images",  # 使用自己的图像
    device="cuda"  # 使用 GPU 加速
)

# 执行搜索
results = searcher("a dog sitting on a bench")

# 查看结果
# Ranked Results:
#   - image_001.jpg | Similarity: 0.3269
#   - image_002.jpg | Similarity: 0.2899
#   - image_003.jpg | Similarity: 0.2761
```

### Web 应用示例

```python
from ultralytics import solutions

# 启动 Web 搜索应用
app = solutions.SearchApp(
    data="path/to/img/directory",
    device="cpu"
)

# 运行应用 (默认端口 5000)
app.run(debug=False)

# 访问 http://localhost:5000 进行搜索
```

### 高级用法

```python
from ultralytics import solutions
import numpy as np

# 批量搜索
queries = [
    "a red car on the street",
    "people walking in the park",
    "cats and dogs playing together"
]

searcher = solutions.VisualAISearch(device="cuda")

for query in queries:
    results = searcher(query)
    print(f"\n查询: {query}")
    for i, result in enumerate(results[:5], 1):
        print(f"  {i}. {result['image']} | 相似度: {result['similarity']:.4f}")

# 自定义相似度阈值
def search_with_threshold(searcher, query, threshold=0.3):
    """使用相似度阈值过滤结果"""
    results = searcher(query)
    filtered = [r for r in results if r['similarity'] > threshold]
    return filtered
```

---

## 实例分割与追踪

### 什么是实例分割?

实例分割是在像素级别识别和勾勒图像中各个对象的技术。与语义分割不同,它为每个对象实例提供唯一的标签和掩码。

### InstanceSegmentation 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `model` | str | "yolo11n-seg.pt" | 分割模型路径 |
| `show` | bool | True | 显示结果 |
| `show_conf` | bool | True | 显示置信度 |
| `show_labels` | bool | True | 显示标签 |
| `show_boxes` | bool | True | 显示边界框 |
| `classes` | list | None | 要分割的类别 |

### 完整示例

```python
import cv2
from ultralytics import solutions

# 加载视频
cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "无法读取视频"

# 获取视频属性
w, h, fps = (int(cap.get(x)) for x in (
    cv2.CAP_PROP_FRAME_WIDTH,
    cv2.CAP_PROP_FRAME_HEIGHT,
    cv2.CAP_PROP_FPS
))

# 创建视频写入器
video_writer = cv2.VideoWriter(
    "instance_segmentation.avi",
    cv2.VideoWriter_fourcc(*"mp4v"),
    fps,
    (w, h)
)

# 初始化实例分割
isegment = solutions.InstanceSegmentation(
    show=True,
    model="yolo11n-seg.pt",
    classes=[0, 2],  # 分割人和汽车
    line_width=2
)

# 处理视频
while cap.isOpened():
    success, im0 = cap.read()

    if not success:
        print("视频帧为空或处理完成")
        break

    # 执行分割
    results = isegment(im0)

    # 访问结果
    # print(results)

    # 保存结果
    video_writer.write(results.plot_im)

cap.release()
video_writer.release()
cv2.destroyAllWindows()
```

### 实际应用

#### 1. 废物管理

```python
import cv2
from ultralytics import solutions

# 废物分类和计数
waste_segmenter = solutions.InstanceSegmentation(
    model="yolo11n-seg.pt",
    classes=[
        57,  # plastic
        58,  # paper
        59   # metal
    ],
    show=True
)

# 处理传送带视频
cap = cv2.VideoCapture("conveyor_belt.mp4")
while True:
    ret, frame = cap.read()
    if not ret:
        break

    results = waste_segmenter(frame)

    # 统计各类废物
    waste_counts = {}
    for cls_id in results.clss:
        cls_name = waste_segmenter.names[cls_id]
        waste_counts[cls_name] = waste_counts.get(cls_name, 0) + 1

    print(f"废物统计: {waste_counts}")

cap.release()
```

#### 2. 医学影像分析

```python
import cv2
from ultralytics import solutions

# 细胞分割
cell_segmenter = solutions.InstanceSegmentation(
    model="yolo11n-seg.pt",
    show=False  # 医学应用通常不直接显示
)

# 分析细胞图像
image = cv2.imread("cell_image.jpg")
results = cell_segmenter(image)

# 提取分割掩码
for i, mask in enumerate(results.masks):
    # 计算细胞面积
    area = mask.sum()
    print(f"细胞 {i+1} 面积: {area} 像素")

    # 计算细胞形状特征
    contours, _ = cv2.findContours(
        mask.astype(np.uint8),
        cv2.RETR_EXTERNAL,
        cv2.CHAIN_APPROX_SIMPLE
    )

    if contours:
        # 计算周长
        perimeter = cv2.arcLength(contours[0], True)
        print(f"细胞 {i+1} 周长: {perimeter:.2f}")
```

---

## 锻炼监控

### AIGym 参数说明

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `model` | str | "yolo11n-pose.pt" | 姿态估计模型 |
| `kpts` | list | [6, 8, 10] | 关键点索引 |
| `line_width` | int | 2 | 绘制线宽 |
| `show` | bool | True | 显示结果 |

### 实现代码

```python
import cv2
from ultralytics import solutions

# 加载视频
cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "无法读取视频"

# 获取视频属性
w, h, fps = (int(cap.get(x)) for x in (
    cv2.CAP_PROP_FRAME_WIDTH,
    cv2.CAP_PROP_FRAME_HEIGHT,
    cv2.CAP_PROP_FPS
))

# 创建视频写入器
video_writer = cv2.VideoWriter(
    "workout_output.avi",
    cv2.VideoWriter_fourcc(*"mp4v"),
    fps,
    (w, h)
)

# 初始化锻炼监控
# kpts=[6, 8, 10] 对应俯卧撑的关键点
gym = solutions.AIGym(
    show=True,
    kpts=[6, 8, 10],  # 左肩、左肘、左腕
    model="yolo11n-pose.pt",
    line_width=2
)

# 处理视频
while cap.isOpened():
    success, im0 = cap.read()

    if not success:
        print("视频帧为空或处理完成")
        break

    # 监控锻炼
    results = gym(im0)

    # 保存结果
    video_writer.write(results.plot_im)

cap.release()
video_writer.release()
cv2.destroyAllWindows()
```

### 支持的锻炼类型

```python
from ultralytics import solutions

# 俯卧撑
pushup_gym = solutions.AIGym(
    kpts=[6, 8, 10],  # 肩、肘、腕
    model="yolo11n-pose.pt"
)

# 引体向上
pullup_gym = solutions.AIGym(
    kpts=[6, 8, 10],
    model="yolo11n-pose.pt"
)

# 深蹲
squat_gym = solutions.AIGym(
    kpts=[11, 13, 15],  # 髋、膝、踝
    model="yolo11n-pose.pt"
)

# 腹部锻炼
abworkout_gym = solutions.AIGym(
    kpts=[11, 13, 15],
    model="yolo11n-pose.pt"
)
```

---

## 距离计算

### DistanceCalculation 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `model` | str | "yolo11n.pt" | 检测模型 |
| `show` | bool | True | 显示结果 |

### 使用示例

```python
import cv2
from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "无法读取视频"

# 初始化距离计算器
distance_calculator = solutions.DistanceCalculation(
    model="yolo11n.pt",
    show=True
)

while cap.isOpened():
    success, im0 = cap.read()

    if not success:
        break

    # 计算距离
    results = distance_calculator(im0)
    print(results)

cap.release()
cv2.destroyAllWindows()
```

### 应用场景

```python
# 社交距离监控
social_distance = solutions.DistanceCalculation(
    model="yolo11n.pt",
    classes=[0]  # 只检测人
)

# 车辆间距监控
vehicle_distance = solutions.DistanceCalculation(
    model="yolo11n.pt",
    classes=[2, 3, 5, 7]  # 各种车辆
)
```

---

## VisionEye 视图映射

### VisionEye 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `model` | str | "yolo11n.pt" | 检测模型 |
| `vision_point` | tuple | (50, 50) | 视点坐标 |
| `classes` | list | None | 要显示的类别 |

### 实现代码

```python
import cv2
from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "无法读取视频"

w, h, fps = (int(cap.get(x)) for x in (
    cv2.CAP_PROP_FRAME_WIDTH,
    cv2.CAP_PROP_FRAME_HEIGHT,
    cv2.CAP_PROP_FPS
))

video_writer = cv2.VideoWriter(
    "visioneye_output.avi",
    cv2.VideoWriter_fourcc(*"mp4v"),
    fps,
    (w, h)
)

# 初始化 VisionEye
visioneye = solutions.VisionEye(
    show=True,
    model="yolo11n.pt",
    classes=[0, 2],  # 人和汽车
    vision_point=(50, 50)  # 左上角视点
)

while cap.isOpened():
    success, im0 = cap.read()
    if not success:
        break

    results = visioneye(im0)
    video_writer.write(results.plot_im)

cap.release()
video_writer.release()
cv2.destroyAllWindows()
```

---

## 数据分析与可视化

### Analytics 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `analytics_type` | str | "line" | 图表类型 |
| `model` | str | "yolo11n.pt" | 检测模型 |

### 图表类型

```python
import cv2
from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")

# 1. 折线图
line_analytics = solutions.Analytics(
    analytics_type="line",
    show=True,
    model="yolo11n.pt"
)

# 2. 饼图
pie_analytics = solutions.Analytics(
    analytics_type="pie",
    show=True,
    model="yolo11n.pt"
)

# 3. 柱状图
bar_analytics = solutions.Analytics(
    analytics_type="bar",
    show=True,
    model="yolo11n.pt"
)

# 4. 面积图
area_analytics = solutions.Analytics(
    analytics_type="area",
    show=True,
    model="yolo11n.pt"
)
```

---

## Neural Magic 部署

### DeepSparse 优势

| 特性 | ONNX Runtime | DeepSparse | 提升 |
|------|-------------|------------|------|
| **标准模型** | 42 img/s | 70 img/s | 1.7x |
| **稀疏模型** | 42 img/s | 241 img/s | 5.8x |

### 安装和使用

```bash
# 安装 DeepSparse
pip install "deepsparse[server,yolo,onnxruntime]"

# Python API
from deepsparse import Pipeline

# 创建 Pipeline
model_stub = "zoo:cv/detection/yolov5-s/pytorch/ultralytics/coco/pruned65_quant-none"
yolo_pipeline = Pipeline.create(
    task="yolo",
    model_path=model_stub
)

# 推理
images = ["basilica.jpg"]
pipeline_outputs = yolo_pipeline(
    images=images,
    iou_thres=0.6,
    conf_thres=0.001
)
```

---

## 终端结果查看

### 在 VSCode 终端中显示图像

```bash
# 1. 启用 VSCode 设置
# 在 settings.json 中添加:
{
  "terminal.integrated.enableImages": true,
  "terminal.integrated.gpuAcceleration": "auto"
}

# 2. 安装 python-sixel
pip install sixel

# 3. Python 代码
from ultralytics import YOLO
import cv2
import io

model = YOLO("yolo11n.pt")
results = model.predict(source="ultralytics/assets/bus.jpg")
plot = results[0].plot()

# 转换为 bytes
im_bytes = cv2.imencode(".png", plot)[1].tobytes()
mem_file = io.BytesIO(im_bytes)

# 显示
from sixel import SixelWriter
writer = SixelWriter()
writer.draw(mem_file)
```

---

## 最佳实践总结

### 1. 项目规划

```python
# 项目检查清单
checklist = {
    "问题定义": "是否明确了要解决的具体问题?",
    "任务选择": "是否选择了合适的 CV 任务?",
    "数据准备": "是否有足够的数据?",
    "模型选择": "是否选择了合适的模型?",
    "评估指标": "是否定义了成功标准?",
    "部署计划": "是否考虑了部署环境?"
}

# 逐项检查
for item, question in checklist.items():
    print(f"{item}: {question}")
```

### 2. 数据管理

```python
# 数据集版本控制
dataset_info = {
    "name": "custom_dataset",
    "version": "1.0.0",
    "train_images": 1000,
    "val_images": 200,
    "test_images": 100,
    "classes": 10,
    "annotations": "YOLO format",
    "created_date": "2024-01-01"
}

# 数据质量检查
def check_dataset_quality(dataset_path):
    """检查数据集质量"""
    # 1. 检查文件完整性
    # 2. 检查标注格式
    # 3. 检查类别分布
    # 4. 检查图像质量
    pass
```

### 3. 模型训练

```python
from ultralytics import YOLO

# 推荐的训练流程
model = YOLO("yolo11n.pt")

# 1. 快速验证
model.train(data="dataset.yaml", epochs=10)

# 2. 完整训练
model.train(data="dataset.yaml", epochs=100, batch=16)

# 3. 微调
model.train(data="dataset.yaml", epochs=50, lr0=0.0001)
```

### 4. 性能优化

```python
# 推理优化
model = YOLO("yolo11n.pt")

# 1. 导出优化格式
model.export(format="onnx")
model.export(format="engine")  # TensorRT

# 2. 量化
model.export(format="onnx", half=True)

# 3. 批处理
results = model.predict(
    source="images/",
    batch=32,
    stream=True
)
```

---

## 常见问题解答

### Q1: 如何选择合适的 YOLO11 模型?

**A:**

| 模型 | 参数量 | 速度 | 精度 | 推荐场景 |
|------|--------|------|------|---------|
| yolo11n | 2.6M | 最快 | 中等 | 边缘设备、实时检测 |
| yolo11s | 9.4M | 快 | 良好 | 移动应用、速度优先 |
| yolo11m | 20.1M | 中等 | 很好 | 通用场景、平衡选择 |
| yolo11l | 40.5M | 慢 | 优秀 | 高精度要求 |
| yolo11x | 97.8M | 最慢 | 最佳 | 竞赛、研究项目 |

### Q2: 数据集需要多大?

**A:**
```python
# 最小数据集建议
min_images_per_class = {
    "快速原型": 50,
    "基础训练": 100,
    "良好性能": 500,
    "生产部署": 1000,
    "高精度": 5000
}

# 数据增强可以减少数据需求
# 使用 Mosaic、MixUp 等技术可以提升小数据集性能
```

### Q3: 如何提高检测精度?

**A:**
```python
# 1. 使用更大的模型
model = YOLO("yolo11x.pt")

# 2. 增加训练数据
model.train(data="dataset.yaml", epochs=300)

# 3. 使用数据增强
model.train(
    data="dataset.yaml",
    epochs=100,
    hsv_h=0.015,
    mosaic=1.0,
    mixup=0.15
)

# 4. 调整学习率
model.train(
    data="dataset.yaml",
    lr0=0.0001,
    cos_lr=True
)
```

### Q4: 如何加速推理?

**A:**
```python
# 1. 使用较小的模型
model = YOLO("yolo11n.pt")

# 2. 减小输入尺寸
results = model.predict(source="image.jpg", imgsz=320)

# 3. 使用 GPU
results = model.predict(source="image.jpg", device=0)

# 4. 导出优化格式
model.export(format="onnx")
model.export(format="engine")  # TensorRT
```

---

## 总结

本指南涵盖了 YOLO11 的核心实用功能:

1. **项目规划**: SMART 目标、任务选择、问题定义
2. **数据管理**: 收集、标注、增强策略
3. **核心功能**: 计数、分割、追踪、分析
4. **实际应用**: 零售、交通、医疗、健身
5. **最佳实践**: 模型选择、性能优化、部署

遵循这些指南可以帮助您快速构建高质量的计算机视觉应用。

**参考资源:**
- [Ultralytics 文档](https://docs.ultralytics.com)
- [YOLO11 GitHub](https://github.com/ultralytics/ultralytics)
- [社区论坛](https://community.ultralytics.com)
