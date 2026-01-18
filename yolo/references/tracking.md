# YOLO11 目标追踪完全指南

**页面数:** 完整版

本文档全面介绍 YOLO11 的目标追踪功能，包括多目标追踪算法、距离计算、区域管理、热力图分析和实际应用场景。

---

## 目录

1. [目标追踪概述](#1-目标追踪概述)
2. [多目标追踪 (MOT)](#2-多目标追踪-mot)
3. [距离计算](#3-距离计算)
4. [区域管理](#4-区域管理)
5. [热力图分析](#5-热力图分析)
6. [队列管理](#6-队列管理)
7. [实战应用案例](#7-实战应用案例)
8. [性能优化](#8-性能优化)

---

## 1. 目标追踪概述

### 1.1 什么是目标追踪？

目标追踪是在视频序列中为每个检测到的对象分配唯一 ID，并在连续帧中跟踪这些 ID 的技术。

**核心概念:**

- **目标检测**: 在每一帧中检测对象
- **目标关联**: 将不同帧中的检测关联到同一对象
- **轨迹生成**: 为每个对象生成随时间变化的轨迹

**追踪算法对比:**

| 算法 | 速度 | 精度 | 适用场景 |
|------|------|------|----------|
| **ByteTrack** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | 通用场景 |
| **BOT-SORT** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | 高精度需求 |
| **OC-SORT** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | 实时应用 |

### 1.2 快速开始

```python
# 示例 1: 基本的目标追踪
from ultralytics import YOLO
import cv2

# 加载 YOLO11 模型
model = YOLO("yolo11n.pt")

# 打开视频文件
cap = cv2.VideoCapture("video.mp4")

# 追踪设置
track_history = {}  # 存储追踪历史

while cap.isOpened():
    success, frame = cap.read()
    if not success:
        break

    # 使用追踪模式
    results = model.track(frame, persist=True, tracker="bytetrack.yaml")

    # 可视化结果
    annotated_frame = results[0].plot()

    # 访问追踪信息
    if results[0].boxes.id is not None:
        boxes = results[0].boxes.xywh.cpu()  # 中心坐标
        track_ids = results[0].boxes.id.int().cpu().tolist()

        for box, track_id in zip(boxes, track_ids):
            x, y, w, h = box
            track = track_history.get(track_id, [])
            track.append((float(x), float(y)))  # 中心点
            track_history[track_id] = track

            # 绘制轨迹
            if len(track) > 2:
                points = track
                cv2.polylines(
                    annotated_frame,
                    [np.int32(points).reshape((-1, 1, 2))],
                    False,
                    (0, 255, 0),
                    2
                )

    cv2.imshow("Tracking", annotated_frame)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
```

---

## 2. 多目标追踪 (MOT)

### 2.1 ByteTrack 追踪器

ByteTrack 是目前最流行的追踪算法之一，基于检测分数进行关联。

```python
# 示例 2: 使用 ByteTrack 追踪
from ultralytics import YOLO

def track_with_bytetrack():
    """使用 ByteTrack 进行多目标追踪"""
    model = YOLO("yolo11n.pt")

    # 追踪配置
    results = model.track(
        source="video.mp4",
        tracker="bytetrack.yaml",  # 使用 ByteTrack
        conf=0.3,                  # 检测置信度阈值
        iou=0.5,                    # NMS IoU 阈值
        imgsz=640,                  # 输入图像尺寸
        show=True,                  # 显示结果
        stream=True,                # 使用生成器
    )

    # 处理结果
    for result in results:
        # 获取追踪 ID
        if result.boxes.id is not None:
            track_ids = result.boxes.id.int().cpu().tolist()
            classes = result.boxes.cls.int().cpu().tolist()

            for track_id, cls in zip(track_ids, classes):
                print(f"追踪 ID: {track_id}, 类别: {cls}")

track_with_bytetrack()
```

**ByteTrack 参数说明:**

```yaml
# bytetrack.yaml 配置文件
tracker_type: botsort  # 追踪器类型
track_high_thresh: 0.5      # 高分阈值
track_low_thresh: 0.1       # 低分阈值
new_track_thresh: 0.6       # 新轨迹阈值
track_buffer: 30            # 追踪缓冲区（帧）
match_thresh: 0.8           # 匹配阈值
fuse_score: True            # 融合检测分数
gmc_method: sparseOptFlow   # 全局运动补偿
proximity_thresh: 0.5       # 接近阈值
```

### 2.2 BOT-SORT 追踪器

BOT-SORT 提供更高的追踪精度，适合复杂场景。

```python
# 示例 3: 使用 BOT-SORT 追踪
def track_with_botsort():
    """使用 BOT-SORT 进行高精度追踪"""
    model = YOLO("yolo11n.pt")

    results = model.track(
        source="video.mp4",
        tracker="botsort.yaml",   # 使用 BOT-SORT
        conf=0.25,
        iou=0.5,
        show=True,
        stream=True,
    )

    # BOT-SORT 优势:
    # - 更高的追踪精度
    # - 更好的遮挡处理
    # - 更稳定的 ID 分配

    for result in results:
        # 处理追踪结果
        pass

track_with_botsort()
```

**BOT-SORT 参数说明:**

```yaml
# botsort.yaml 配置文件
tracker_type: botsort
track_high_thresh: 0.5
track_low_thresh: 0.1
new_track_thresh: 0.6
track_buffer: 30
match_thresh: 0.8
fuse_score: True
gmc_method: sparseOptFlow
proximity_thresh: 0.5
```

### 2.3 自定义追踪器

```python
# 示例 4: 创建自定义追踪配置
def create_custom_tracker():
    """创建自定义追踪配置"""

    custom_tracker_yaml = """
tracker_type: botsort
track_high_thresh: 0.6      # 提高高分阈值
track_low_thresh: 0.15      # 调整低分阈值
new_track_thresh: 0.7       # 提高新轨迹阈值
track_buffer: 50            # 增加缓冲区（更长记忆）
match_thresh: 0.85          # 提高匹配阈值
fuse_score: True
gmc_method: sparseOptFlow
proximity_thresh: 0.6
"""

    # 保存自定义配置
    with open("custom_tracker.yaml", "w") as f:
        f.write(custom_tracker_yaml)

    print("✓ 自定义追踪器配置已创建: custom_tracker.yaml")

    # 使用自定义追踪器
    model = YOLO("yolo11n.pt")
    results = model.track(
        source="video.mp4",
        tracker="custom_tracker.yaml",
        show=True,
    )

    return results

create_custom_tracker()
```

### 2.4 追踪指标评估

```python
# 示例 5: 评估追踪性能
def evaluate_tracking_performance():
    """评估追踪算法性能"""

    # 关键指标:
    metrics = {
        "MOTA": "多目标追踪精度 (Multi-Object Tracking Accuracy)",
        "MOTP": "多目标追踪精度 (Multi-Object Tracking Precision)",
        "IDs": "身份切换次数 (Identity Switches)",
        "Frag": "轨迹碎片化 (Fragmentation)",
        "FP": "假阳性 (False Positives)",
        "FN": "假阴性 (False Negatives)",
    }

    print("追踪性能指标:")
    print("=" * 60)
    for metric, description in metrics.items():
        print(f"{metric}: {description}")

    # 使用标准数据集评估
    # 例如: MOTChallenge, KITTI 等

evaluate_tracking_performance()
```

---

## 3. 距离计算

### 3.1 对象间距离计算

```python
# 示例 6: 计算对象之间的距离
import cv2
from ultralytics import solutions

def calculate_object_distance():
    """计算视频中对象之间的距离"""

    # 打开视频
    cap = cv2.VideoCapture("path/to/video.mp4")
    assert cap.isOpened(), "无法读取视频文件"

    # 获取视频属性
    w, h, fps = (
        int(cap.get(x)) for x in (
            cv2.CAP_PROP_FRAME_WIDTH,
            cv2.CAP_PROP_FRAME_HEIGHT,
            cv2.CAP_PROP_FPS
        )
    )

    # 创建视频写入器
    video_writer = cv2.VideoWriter(
        "distance_output.avi",
        cv2.VideoWriter_fourcc(*"mp4v"),
        fps,
        (w, h)
    )

    # 初始化距离计算对象
    distance_calculator = solutions.DistanceCalculation(
        model="yolo11n.pt",    # YOLO11 模型路径
        show=True,              # 显示输出
        line_dist_thresh=100,   # 线条距离阈值
        centroid_thresh=50,     # 质心阈值
        points=None,            # 自定义点
        thickness=2,            # 线条粗细
        font_size=1.0,          # 字体大小
    )

    # 处理视频
    while cap.isOpened():
        success, frame = cap.read()

        if not success:
            print("视频帧为空或处理完成。")
            break

        # 计算距离
        results = distance_calculator(frame)

        # 打印距离信息
        print(f"计算的距离: {results}")

        # 写入处理后的帧
        video_writer.write(results.plot_im)

    # 清理资源
    cap.release()
    video_writer.release()
    cv2.destroyAllWindows()

calculate_object_distance()
```

### 3.2 实时距离计算

```python
# 示例 7: 实时视频流距离计算
def real_time_distance_calculation():
    """实时视频流中的距离计算"""
    import cv2
    from ultralytics import solutions

    # 打开摄像头
    cap = cv2.VideoCapture(0)  # 0 表示默认摄像头

    # 初始化距离计算
    distance_calc = solutions.DistanceCalculation(
        model="yolo11n.pt",
        show=True,
    )

    while True:
        success, frame = cap.read()
        if not success:
            break

        # 计算距离
        results = distance_calc(frame)

        # 按 'q' 退出
        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    cap.release()
    cv2.destroyAllWindows()

real_time_distance_calculation()
```

**DistanceCalculation 参数说明:**

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `model` | str | "yolo11n.pt" | YOLO 模型路径 |
| `show` | bool | False | 是否显示输出 |
| `line_dist_thresh` | int | 100 | 线条距离阈值（像素） |
| `centroid_thresh` | int | 50 | 质心阈值（像素） |
| `points` | list | None | 自定义点坐标 |
| `thickness` | int | 2 | 线条粗细 |
| `font_size` | float | 1.0 | 字体大小 |

---

## 4. 区域管理

### 4.1 区域计数

```python
# 示例 8: 区域内对象计数
import cv2
from ultralytics import solutions

def count_objects_in_region():
    """计算特定区域内的对象数量"""

    # 定义区域（多边形顶点）
    region_points = [
        (100, 100),   # 左上
        (500, 100),   # 右上
        (500, 400),   # 右下
        (100, 400)    # 左下
    ]

    # 初始化区域计数器
    counter = solutions.ObjectCounter(
        model="yolo11n.pt",
        show=True,
        region=region_points,    # 定义计数区域
        line_width=2,            # 线宽
        font_size=1.0,           # 字体大小
        display_names=True,      # 显示类别名称
        display_counts=True,     # 显示计数
        display_labels=True,     # 显示标签
        count_colors=True,       # 彩色计数
    )

    # 打开视频
    cap = cv2.VideoCapture("video.mp4")

    while cap.isOpened():
        success, frame = cap.read()
        if not success:
            break

        # 处理帧并计数
        results = counter(frame)

        # 获取计数信息
        print(f"区域内对象数量: {results.counts_dict}")

        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    cap.release()
    cv2.destroyAllWindows()

count_objects_in_region()
```

### 4.2 线条穿越检测

```python
# 示例 9: 检测穿越线条的对象
import cv2
from ultralytics import solutions

def line_crossing_detection():
    """检测穿越特定线条的对象"""

    # 定义线条（起点和终点）
    line_points = [(200, 300), (600, 300)]

    # 初始化线条计数器
    line_counter = solutions.LineCounter(
        model="yolo11n.pt",
        show=True,
        line=line_points,          # 计数线
        line_width=2,              # 线宽
        font_size=1.0,             # 字体大小
        display_names=True,        # 显示类别名称
        display_counts=True,       # 显示计数
        display_labels=True,       # 显示标签
        count_colors=True,         # 彩色计数
        direction=True,            # 检测方向
    )

    # 打开视频
    cap = cv2.VideoCapture("video.mp4")

    while cap.isOpened():
        success, frame = cap.read()
        if not success:
            break

        # 处理帧
        results = line_counter(frame)

        # 获取穿越信息
        in_count = results.in_count      # 进入计数
        out_count = results.out_count    # 离开计数

        print(f"进入: {in_count}, 离开: {out_count}")

        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    cap.release()
    cv2.destroyAllWindows()

line_crossing_detection()
```

### 4.3 多区域管理

```python
# 示例 10: 管理多个计数区域
def multi_region_counting():
    """管理多个计数区域"""

    # 定义多个区域
    regions = {
        "region_1": [(100, 100), (300, 100), (300, 300), (100, 300)],
        "region_2": [(400, 100), (600, 100), (600, 300), (400, 300)],
        "region_3": [(100, 400), (300, 400), (300, 600), (100, 600)],
    }

    # 为每个区域创建计数器
    counters = {}
    for name, points in regions.items():
        counters[name] = solutions.ObjectCounter(
            model="yolo11n.pt",
            region=points,
            show=False,  # 不显示，手动绘制
        )

    # 处理视频
    cap = cv2.VideoCapture("video.mp4")

    while cap.isOpened():
        success, frame = cap.read()
        if not success:
            break

        # 为每个区域计数
        for name, counter in counters.items():
            results = counter(frame)
            print(f"{name}: {results.counts_dict}")

        # 绘制所有区域
        for name, points in regions.items():
            cv2.polylines(
                frame,
                [np.array(points, np.int32).reshape((-1, 1, 2))],
                True,
                (0, 255, 0),
                2
            )

        cv2.imshow("Multi-Region Counting", frame)

        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    cap.release()
    cv2.destroyAllWindows()

multi_region_counting()
```

---

## 5. 热力图分析

### 5.1 生成热力图

```python
# 示例 11: 生成对象运动热力图
import cv2
from ultralytics import solutions

def generate_heatmap():
    """生成视频中对象的热力图"""

    # 初始化热力图对象
    heatmap = solutions.Heatmap(
        model="yolo11n.pt",
        show=True,
        colormap=cv2.COLORMAP_JET,  # 颜色映射
        imw_step=5,                  # 宽度步长
        imh_step=5,                  # 高度步长
        view_img=True,               # 查看图像
        view_in_counts=True,         # 查看输入计数
        view_out_counts=True,        # 查看输出计数
    )

    # 打开视频
    cap = cv2.VideoCapture("video.mp4")

    while cap.isOpened():
        success, frame = cap.read()
        if not success:
            break

        # 生成热力图
        results = heatmap(frame)

        # 保存热力图帧
        cv2.imwrite(f"heatmap_frame_{cap.get(cv2.CAP_PROP_POS_FRAMES)}.jpg", results.plot_im)

        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    cap.release()
    cv2.destroyAllWindows()

generate_heatmap()
```

### 5.2 自定义热力图

```python
# 示例 12: 自定义热力图样式
def custom_heatmap():
    """创建自定义样式的热力图"""

    # 自定义热力图配置
    heatmap = solutions.Heatmap(
        model="yolo11n.pt",
        colormap=cv2.COLORMAP_HOT,      # 使用热力图颜色
        imw_step=10,                     # 更大的步长（更快）
        imh_step=10,
        alpha=0.5,                       # 透明度
        radius=20,                       # 热力点半径
    )

    # 处理视频
    cap = cv2.VideoCapture("video.mp4")

    while cap.isOpened():
        success, frame = cap.read()
        if not success:
            break

        results = heatmap(frame)
        cv2.imshow("Custom Heatmap", results.plot_im)

        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    cap.release()
    cv2.destroyAllWindows()

custom_heatmap()
```

### 5.3 热力图分析应用

```python
# 示例 13: 分析热力图数据
def analyze_heatmap_data():
    """分析热力图数据获取洞察"""

    heatmap = solutions.Heatmap(
        model="yolo11n.pt",
        show=False,
    )

    # 收集热力图数据
    cap = cv2.VideoCapture("video.mp4")
    heat_data = []

    while cap.isOpened():
        success, frame = cap.read()
        if not success:
            break

        results = heatmap(frame)
        heat_data.append(results.heatmap)

    # 分析热力图
    avg_heat = np.mean(heat_data, axis=0)

    # 找出热点区域
    hotspots = np.where(avg_heat > np.percentile(avg_heat, 90))

    print("热点区域坐标:")
    for y, x in zip(*hotspots):
        print(f"  ({x}, {y})")

    cap.release()

analyze_heatmap_data()
```

---

## 6. 队列管理

### 6.1 队列计数

```python
# 示例 14: 队列管理
import cv2
from ultralytics import solutions

def queue_management():
    """管理队列中的对象"""

    # 定义队列区域
    queue_region = [
        (100, 200),
        (300, 200),
        (300, 500),
        (100, 500)
    ]

    # 初始化队列管理器
    queue_manager = solutions.QueueManager(
        model="yolo11n.pt",
        show=True,
        region=queue_region,        # 队列区域
        line_width=2,               # 线宽
        font_size=1.0,              # 字体大小
        display_names=True,         # 显示名称
        display_counts=True,        # 显示计数
        display_labels=True,        # 显示标签
        count_colors=True,          # 彩色计数
    )

    # 处理视频
    cap = cv2.VideoCapture("queue_video.mp4")

    while cap.isOpened():
        success, frame = cap.read()
        if not success:
            break

        # 处理队列
        results = queue_manager(frame)

        # 获取队列统计
        print(f"队列中的人数: {results.count}")

        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    cap.release()
    cv2.destroyAllWindows()

queue_management()
```

### 6.2 多队列管理

```python
# 示例 15: 管理多个队列
def multi_queue_management():
    """管理多个队列"""

    # 定义多个队列区域
    queues = {
        "queue_A": [(50, 100), (200, 100), (200, 400), (50, 400)],
        "queue_B": [(250, 100), (400, 100), (400, 400), (250, 400)],
        "queue_C": [(450, 100), (600, 100), (600, 400), (450, 400)],
    }

    # 为每个队列创建管理器
    managers = {}
    for name, region in queues.items():
        managers[name] = solutions.QueueManager(
            model="yolo11n.pt",
            region=region,
            show=False,
        )

    # 处理视频
    cap = cv2.VideoCapture("multi_queue_video.mp4")

    while cap.isOpened():
        success, frame = cap.read()
        if not success:
            break

        # 处理每个队列
        queue_counts = {}
        for name, manager in managers.items():
            results = manager(frame)
            queue_counts[name] = results.count

        # 显示统计
        print(f"队列统计: {queue_counts}")

        # 绘制队列区域
        for name, region in queues.items():
            count = queue_counts[name]
            color = (0, 255, 0) if count < 5 else (0, 0, 255)  # 绿色/红色

            cv2.polylines(
                frame,
                [np.array(region, np.int32).reshape((-1, 1, 2))],
                True,
                color,
                2
            )

            # 显示计数
            center = np.mean(region, axis=0).astype(int)
            cv2.putText(
                frame,
                f"{name}: {count}",
                tuple(center),
                cv2.FONT_HERSHEY_SIMPLEX,
                0.7,
                color,
                2
            )

        cv2.imshow("Multi-Queue Management", frame)

        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    cap.release()
    cv2.destroyAllWindows()

multi_queue_management()
```

---

## 7. 实战应用案例

### 7.1 交通监控

```python
# 示例 16: 交通车辆追踪与统计
def traffic_monitoring():
    """交通监控：车辆追踪和统计"""

    # 加载模型
    model = YOLO("yolo11n.pt")

    # 定义计数线
    line_points = [(400, 300), (800, 300)]

    # 初始化计数器
    counter = solutions.LineCounter(
        model="yolo11n.pt",
        line=line_points,
        classes=[2, 3, 5, 7],  # car, motorcycle, bus, truck
        show=True,
    )

    # 处理视频
    cap = cv2.VideoCapture("traffic_video.mp4")

    while cap.isOpened():
        success, frame = cap.read()
        if not success:
            break

        # 追踪并计数
        results = counter(frame)

        # 统计信息
        print(f"车辆进入: {results.in_count}, 离开: {results.out_count}")

        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    cap.release()
    cv2.destroyAllWindows()

traffic_monitoring()
```

### 7.2 人员流量分析

```python
# 示例 17: 人员流量分析
def people_flow_analysis():
    """分析人员流量和热力图"""

    # 初始化热力图
    heatmap = solutions.Heatmap(
        model="yolo11n.pt",
        classes=[0],  # 只统计人
        show=True,
    )

    # 初始化计数器
    counter = solutions.LineCounter(
        model="yolo11n.pt",
        line=[(100, 300), (700, 300)],
        classes=[0],
        show=False,
    )

    # 处理视频
    cap = cv2.VideoCapture("people_flow.mp4")

    while cap.isOpened():
        success, frame = cap.read()
        if not success:
            break

        # 生成热力图
        heat_results = heatmap(frame)

        # 计数
        count_results = counter(frame)

        # 综合显示
        combined = heat_results.plot_im
        cv2.putText(
            combined,
            f"In: {count_results.in_count} Out: {count_results.out_count}",
            (10, 30),
            cv2.FONT_HERSHEY_SIMPLEX,
            1,
            (0, 255, 0),
            2
        )

        cv2.imshow("People Flow Analysis", combined)

        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    cap.release()
    cv2.destroyAllWindows()

people_flow_analysis()
```

### 7.3 零售店分析

```python
# 示例 18: 零售店顾客行为分析
def retail_store_analysis():
    """零售店顾客行为分析"""

    # 定义兴趣区域
    regions = {
        "entrance": [(50, 100), (200, 100), (200, 300), (50, 300)],
        "shelf_A": [(250, 100), (400, 100), (400, 300), (250, 300)],
        "checkout": [(450, 100), (600, 100), (600, 300), (450, 300)],
    }

    # 初始化热力图
    heatmap = solutions.Heatmap(
        model="yolo11n.pt",
        classes=[0],
        show=True,
    )

    # 处理视频
    cap = cv2.VideoCapture("store_video.mp4")

    dwell_times = {region: 0 for region in regions}

    while cap.isOpened():
        success, frame = cap.read()
        if not success:
            break

        # 生成热力图
        results = heatmap(frame)

        # 分析停留时间
        # (简化版本，实际需要更复杂的逻辑)

        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    cap.release()

    # 生成报告
    print("顾客行为分析报告:")
    print("=" * 50)
    for region, time in dwell_times.items():
        print(f"{region}: {time:.2f} 秒")

retail_store_analysis()
```

### 7.4 体育运动分析

```python
# 示例 19: 体育运动员追踪
def sports_player_tracking():
    """追踪和分析运动员运动"""

    # 加载模型
    model = YOLO("yolo11n.pt")

    # 追踪设置
    track_history = {}

    cap = cv2.VideoCapture("sports_video.mp4")

    while cap.isOpened():
        success, frame = cap.read()
        if not success:
            break

        # 追踪运动员
        results = model.track(
            frame,
            persist=True,
            classes=[0],  # 人
            conf=0.3,
        )

        # 分析运动轨迹
        if results[0].boxes.id is not None:
            boxes = results[0].boxes.xywh.cpu()
            track_ids = results[0].boxes.id.int().cpu().tolist()

            for box, track_id in zip(boxes, track_ids):
                x, y, w, h = box
                track = track_history.get(track_id, [])
                track.append((float(x), float(y)))
                track_history[track_id] = track

                # 绘制轨迹（最近 50 帧）
                if len(track) > 2:
                    recent_track = track[-50:]
                    points = np.array(recent_track, dtype=np.int32)
                    cv2.polylines(
                        frame,
                        [points.reshape((-1, 1, 2))],
                        False,
                        (0, 255, 0),
                        2
                    )

        cv2.imshow("Sports Tracking", frame)

        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    cap.release()
    cv2.destroyAllWindows()

    # 分析运动数据
    analyze_movement_data(track_history)

def analyze_movement_data(track_history):
    """分析运动员运动数据"""
    print("\n运动数据分析:")
    print("=" * 50)

    for track_id, track in track_history.items():
        if len(track) < 2:
            continue

        # 计算总距离
        total_distance = 0
        for i in range(1, len(track)):
            dx = track[i][0] - track[i-1][0]
            dy = track[i][1] - track[i-1][1]
            distance = np.sqrt(dx**2 + dy**2)
            total_distance += distance

        print(f"运动员 {track_id}:")
        print(f"  总移动距离: {total_distance:.2f} 像素")
        print(f"  追踪帧数: {len(track)}")

sports_player_tracking()
```

---

## 8. 性能优化

### 8.1 追踪性能优化

```python
# 示例 20: 优化追踪性能
def optimize_tracking_performance():
    """优化追踪性能"""

    model = YOLO("yolo11n.pt")

    # 性能优化技巧:
    results = model.track(
        source="video.mp4",

        # 1. 降低输入分辨率
        imgsz=320,  # 从 640 降到 320

        # 2. 使用更快的模型
        # model = YOLO("yolo11n.pt")  # nano 最快

        # 3. 调整检测参数
        conf=0.3,   # 提高置信度阈值
        iou=0.5,    # NMS IoU 阈值
        max_det=100,  # 限制最大检测数

        # 4. 使用半精度
        half=True,

        # 5. 批处理
        batch=8,

        # 6. 使用更快的追踪器
        tracker="bytetrack.yaml",  # ByteTrack 最快

        show=True,
    )

    return results

optimize_tracking_performance()
```

### 8.2 内存优化

```python
# 示例 21: 优化内存使用
def optimize_memory_usage():
    """优化长时间追踪的内存使用"""

    model = YOLO("yolo11n.pt")

    # 使用流式处理避免内存堆积
    results = model.track(
        source="long_video.mp4",
        stream=True,  # 使用生成器模式

        # 限制追踪历史长度
        tracker="bytetrack.yaml",
        track_buffer=30,  # 只保留最近 30 帧的轨迹
    )

    # 逐帧处理，不存储所有结果
    frame_count = 0
    for result in results:
        frame_count += 1

        # 处理当前帧
        if result.boxes.id is not None:
            track_ids = result.boxes.id.int().cpu().tolist()
            print(f"帧 {frame_count}: 追踪 {len(track_ids)} 个对象")

        # 不存储结果，及时释放内存
        del result

        if frame_count > 1000:  # 限制处理帧数
            break

optimize_memory_usage()
```

### 8.3 多线程处理

```python
# 示例 22: 多线程视频处理
import threading
import queue

def multi_threaded_tracking():
    """使用多线程加速视频处理"""

    model = YOLO("yolo11n.pt")

    # 创建输入输出队列
    input_queue = queue.Queue(maxsize=10)
    output_queue = queue.Queue(maxsize=10)

    def capture_thread():
        """捕获帧的线程"""
        cap = cv2.VideoCapture("video.mp4")
        while cap.isOpened():
            success, frame = cap.read()
            if not success:
                break
            input_queue.put(frame)
        cap.release()

    def process_thread():
        """处理帧的线程"""
        while True:
            frame = input_queue.get()
            if frame is None:
                break

            # 追踪处理
            results = model.track(frame, persist=True)
            output_queue.put((frame, results))

    def display_thread():
        """显示结果的线程"""
        while True:
            frame, results = output_queue.get()
            if results is None:
                break

            annotated = results[0].plot()
            cv2.imshow("Tracking", annotated)

            if cv2.waitKey(1) & 0xFF == ord('q'):
                break

    # 启动线程
    threads = []
    threads.append(threading.Thread(target=capture_thread))
    threads.append(threading.Thread(target=process_thread))
    threads.append(threading.Thread(target=display_thread))

    for t in threads:
        t.start()

    for t in threads:
        t.join()

    cv2.destroyAllWindows()

# 注意: 多线程处理需要仔细的同步和错误处理
# multi_threaded_tracking()
```

### 8.4 GPU 加速

```python
# 示例 23: 使用 GPU 加速
def gpu_accelerated_tracking():
    """使用 GPU 加速追踪"""

    model = YOLO("yolo11n.pt")

    # 检查 CUDA 可用性
    import torch
    if torch.cuda.is_available():
        print(f"使用 GPU: {torch.cuda.get_device_name(0)}")
        device = 0  # 使用第一个 GPU
    else:
        print("使用 CPU")
        device = "cpu"

    # 使用 GPU 追踪
    results = model.track(
        source="video.mp4",
        device=device,  # 指定设备
        half=True,      # 使用半精度（GPU）
        stream=True,
    )

    for result in results:
        # 处理结果
        pass

gpu_accelerated_tracking()
```

---

## 9. 总结

本文档全面介绍了 YOLO11 的目标追踪功能，包括:

### 核心功能

1. **多目标追踪**: ByteTrack、BOT-SORT 等算法
2. **距离计算**: 对象间距离测量
3. **区域管理**: 区域计数、线条穿越检测
4. **热力图分析**: 运动热力图生成
5. **队列管理**: 队列统计和管理

### 应用场景

- **交通监控**: 车辆追踪、流量统计
- **人员管理**: 人流分析、热力图
- **零售分析**: 顾客行为分析
- **体育分析**: 运动员追踪

### 最佳实践

1. **选择合适的追踪器**: 根据场景选择 ByteTrack 或 BOT-SORT
2. **优化性能**: 降低分辨率、使用半精度
3. **内存管理**: 使用流式处理、限制历史长度
4. **GPU 加速**: 充分利用 GPU 资源

### 相关资源

- **YOLO 文档**: https://docs.ultralytics.com
- **MOTChallenge**: https://motchallenge.net
- **ByteTrack 论文**: https://arxiv.org/abs/2110.06864

---

**文档版本:** YOLO11-Tracking v1.0
**最后更新:** 2026-01-18
**作者:** YOLO 技术团队
