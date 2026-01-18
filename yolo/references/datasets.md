# YOLO 数据集管理 - 完整指南

本文档提供 Ultralytics YOLO 系列模型（包括 YOLO11）的数据集管理完整指南，涵盖数据集准备、格式转换、增强和管理等方面。

---

## 目录
1. [数据集格式](#数据集格式)
2. [YOLO11 数据集准备](#yolo11-数据集准备)
3. [数据集增强](#数据集增强)
4. [数据集管理工具](#数据集管理工具)
5. [常用数据集](#常用数据集)
6. [最佳实践](#最佳实践)

---

## 数据集格式

### YOLO 格式标准

YOLO 系列模型使用统一的数据集标注格式，支持多种任务类型：

#### 目标检测（Detection）
```
# 标注文件格式（每行一个目标）
<class_id> <x_center> <y_center> <width> <height>

# 坐标值都是相对于图像尺寸的归一化值（0-1）
# 示例：0 0.5 0.5 0.3 0.4
```

#### 实例分割（Segmentation）
```
# 标注文件格式
<class_id> <x1> <y1> <x2> <y2> ... <xn> <yn>

# 多边形的所有顶点坐标（归一化）
# 示例：0 0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8
```

#### 姿态估计（Pose）
```
# 标注文件格式
<class_id> <x1> <y1> <v1> <x2> <y2> <v2> ... <xn> <yn> <vn>

# 关键点坐标和可见性标志（0=不可见, 1=遮挡, 2=可见）
# 示例：0 0.5 0.5 2 0.6 0.6 2 0.4 0.4 2
```

#### 旋转目标检测（OBB）
```
# 标注文件格式
<class_id> <x_center> <y_center> <width> <height> <rotation>

# 旋转角度（弧度）
# 示例：0 0.5 0.5 0.3 0.4 1.57
```

### 数据集目录结构

标准 YOLO 数据集目录结构：
```
dataset/
├── data.yaml              # 数据集配置文件
├── images/
│   ├── train/            # 训练图像
│   ├── val/              # 验证图像
│   └── test/             # 测试图像（可选）
└── labels/
    ├── train/            # 训练标签
    ├── val/              # 验证标签
    └── test/             # 测试标签（可选）
```

### YAML 配置文件

```yaml
# data.yaml 示例
# 数据集路径
path: /path/to/dataset  # 数据集根目录
train: images/train      # 训练图像路径（相对或绝对）
val: images/val          # 验证图像路径
test: images/test        # 测试图像路径（可选）

# 类别信息
names:
  0: person
  1: car
  2: dog
  # ... 更多类别

# 数据集统计
nc: 3  # 类别数量

# 下载URL（可选）
download: https://github.com/ultralytics/assets/releases/download/v0.0.0/dataset.zip
```

---

## YOLO11 数据集准备

### 使用 Python API 准备数据集

```python
from ultralytics import YOLO
from ultralytics.data.utils import check_det_dataset
from pathlib import Path

# 1. 验证数据集结构
dataset_path = "path/to/your/dataset/data.yaml"
data_info = check_det_dataset(dataset_path)
print(f"数据集信息: {data_info}")

# 2. 创建 YOLO11 模型
model = YOLO("yolo11n.pt")

# 3. 查看数据集配置
print(f"类别数量: {len(data_info['names'])}")
print(f"类别名称: {data_info['names']}")
print(f"训练集: {data_info['train']}")
print(f"验证集: {data_info['val']}")

# 4. 数据集统计
from ultralytics.data.utils import HUBDatasetStats

# 生成数据集统计信息（用于上传到 HUB）
stats = HUBDatasetStats(dataset_path, task="detect")
stats.get_json(save=True)  # 保存统计信息
stats.process_images()     # 处理图像
```

### 自定义数据集准备

```python
from pathlib import Path
import shutil
import yaml
from sklearn.model_selection import train_test_split

def prepare_yolo_dataset(
    images_dir: str,
    labels_dir: str,
    output_dir: str,
    class_names: list,
    train_ratio: float = 0.8,
    val_ratio: float = 0.1,
    test_ratio: float = 0.1
):
    """
    准备 YOLO 格式数据集

    Args:
        images_dir: 原始图像目录
        labels_dir: 原始标签目录
        output_dir: 输出目录
        class_names: 类别名称列表
        train_ratio: 训练集比例
        val_ratio: 验证集比例
        test_ratio: 测试集比例
    """
    # 创建输出目录结构
    output_path = Path(output_dir)
    for split in ['train', 'val', 'test']:
        (output_path / 'images' / split).mkdir(parents=True, exist_ok=True)
        (output_path / 'labels' / split).mkdir(parents=True, exist_ok=True)

    # 获取所有图像文件
    image_files = list(Path(images_dir).glob('*.*'))
    image_files = [f for f in image_files if f.suffix.lower() in ['.jpg', '.jpeg', '.png', '.bmp'])

    # 划分数据集
    train_files, temp_files = train_test_split(
        image_files,
        train_size=train_ratio,
        random_state=42
    )
    val_files, test_files = train_test_split(
        temp_files,
        train_size=val_ratio/(val_ratio+test_ratio),
        random_state=42
    )

    # 复制文件到对应目录
    def copy_files(files, split):
        for img_file in files:
            # 复制图像
            shutil.copy(img_file, output_path / 'images' / split / img_file.name)

            # 复制标签
            label_file = Path(labels_dir) / f"{img_file.stem}.txt"
            if label_file.exists():
                shutil.copy(label_file, output_path / 'labels' / split / f"{img_file.stem}.txt")

    copy_files(train_files, 'train')
    copy_files(val_files, 'val')
    copy_files(test_files, 'test')

    # 创建 data.yaml
    data_dict = {
        'path': str(output_path.absolute()),
        'train': 'images/train',
        'val': 'images/val',
        'test': 'images/test',
        'nc': len(class_names),
        'names': {i: name for i, name in enumerate(class_names)}
    }

    with open(output_path / 'data.yaml', 'w', encoding='utf-8') as f:
        yaml.dump(data_dict, f, allow_unicode=True)

    print(f"数据集准备完成！")
    print(f"训练集: {len(train_files)} 张图像")
    print(f"验证集: {len(val_files)} 张图像")
    print(f"测试集: {len(test_files)} 张图像")

# 使用示例
prepare_yolo_dataset(
    images_dir="raw_data/images",
    labels_dir="raw_data/labels",
    output_dir="yolo_dataset",
    class_names=["person", "car", "dog", "cat"],
    train_ratio=0.8,
    val_ratio=0.1,
    test_ratio=0.1
)
```

### 数据格式转换

```python
import json
import cv2
import numpy as np
from pathlib import Path

def convert_coco_to_yolo(
    coco_json_path: str,
    output_dir: str,
    images_dir: str
):
    """
    将 COCO 格式转换为 YOLO 格式

    Args:
        coco_json_path: COCO JSON 文件路径
        output_dir: 输出目录
        images_dir: 图像目录
    """
    # 读取 COCO JSON
    with open(coco_json_path, 'r') as f:
        coco_data = json.load(f)

    # 创建输出目录
    output_path = Path(output_dir)
    (output_path / 'labels').mkdir(parents=True, exist_ok=True)

    # 创建类别映射
    categories = {cat['id']: cat['name'] for cat in coco_data['categories']}
    cat_id_to_idx = {cat_id: idx for idx, cat_id in enumerate(categories.keys())}

    # 创建图像 ID 到文件名的映射
    images = {img['id']: img for img in coco_data['images']}

    # 转换标注
    annotations_by_image = {}
    for ann in coco_data['annotations']:
        image_id = ann['image_id']
        if image_id not in annotations_by_image:
            annotations_by_image[image_id] = []
        annotations_by_image[image_id].append(ann)

    # 为每个图像创建 YOLO 标签文件
    for image_id, annotations in annotations_by_image.items():
        img_info = images[image_id]
        img_width = img_info['width']
        img_height = img_info['height']
        img_filename = img_info['file_name']

        # 读取图像获取实际尺寸（可选）
        # img = cv2.imread(str(Path(images_dir) / img_filename))
        # h, w = img.shape[:2]

        # 创建 YOLO 标签文件
        label_filename = Path(img_filename).stem + '.txt'
        label_path = output_path / 'labels' / label_filename

        with open(label_path, 'w') as f:
            for ann in annotations:
                # 获取类别 ID
                cat_id = ann['category_id']
                class_id = cat_id_to_idx[cat_id]

                # 获取边界框（COCO 格式：[x, y, width, height]）
                bbox = ann['bbox']  # [x, y, w, h]
                x, y, w, h = bbox

                # 转换为 YOLO 格式（归一化的中心点坐标和尺寸）
                x_center = (x + w / 2) / img_width
                y_center = (y + h / 2) / img_height
                width = w / img_width
                height = h / img_height

                # 写入 YOLO 格式
                f.write(f"{class_id} {x_center:.6f} {y_center:.6f} {width:.6f} {height:.6f}\n")

    print(f"转换完成！共处理 {len(annotations_by_image)} 张图像")

# 使用示例
convert_coco_to_yolo(
    coco_json_path="coco/annotations/instances_train2017.json",
    output_dir="yolo_labels",
    images_dir="coco/train2017"
)
```

---

## 数据集增强

### YOLO11 内置增强

```python
from ultralytics import YOLO

# 创建 YOLO11 模型
model = YOLO("yolo11n.pt")

# 训练时使用默认增强
results = model.train(
    data="coco8.yaml",
    epochs=100,
    imgsz=640,
    # 增强参数
    hsv_h=0.015,      # 色调增强（默认 0.015）
    hsv_s=0.7,        # 饱和度增强（默认 0.7）
    hsv_v=0.4,        # 明度增强（默认 0.4）
    degrees=0.0,      # 旋转角度（±deg）
    translate=0.1,    # 平移（±fraction）
    scale=0.5,        # 缩放（gain）
    shear=0.0,        # 剪切角度（±deg）
    perspective=0.0,  # 透视变换（±fraction）
    flipud=0.5,       # 上下翻转概率
    fliplr=0.5,       # 左右翻转概率
    mosaic=1.0,       # Mosaic 增强（概率）
    mixup=0.0,        # MixUp 增强（概率）
    copy_paste=0.0,   # Copy-Paste 增强（概率）
)

print("训练完成！")
```

### 自定义增强管道

```python
import albumentations as A
from albumentations.pytorch import ToTensorV2
import cv2
import numpy as np

class YOLODatasetWithAlbumentations:
    """使用 Albumentations 的自定义 YOLO 数据集"""

    def __init__(self, images_dir, labels_dir, transforms=None):
        self.images_dir = Path(images_dir)
        self.labels_dir = Path(labels_dir)
        self.image_files = list(self.images_dir.glob("*.jpg"))
        self.transforms = transforms

    def __len__(self):
        return len(self.image_files)

    def __getitem__(self, idx):
        # 读取图像
        img_path = self.image_files[idx]
        image = cv2.imread(str(img_path))
        image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)

        # 读取标签
        label_path = self.labels_dir / f"{img_path.stem}.txt"
        boxes = []
        class_labels = []

        if label_path.exists():
            with open(label_path, 'r') as f:
                for line in f:
                    class_id, x_center, y_center, width, height = map(float, line.strip().split())
                    # 转换为像素坐标
                    img_h, img_w = image.shape[:2]
                    x_center *= img_w
                    y_center *= img_h
                    width *= img_w
                    height *= img_h

                    # 转换为 x1, y1, x2, y2 格式
                    x1 = x_center - width / 2
                    y1 = y_center - height / 2
                    x2 = x_center + width / 2
                    y2 = y_center + height / 2

                    boxes.append([x1, y1, x2, y2])
                    class_labels.append(int(class_id))

        # 应用增强
        if self.transforms:
            transformed = self.transforms(
                image=image,
                bboxes=np.array(boxes) if boxes else np.zeros((0, 4)),
                class_labels=class_labels
            )
            image = transformed['image']
            boxes = transformed['bboxes']
            class_labels = transformed['class_labels']

        return image, boxes, class_labels

# 定义增强管道
transforms = A.Compose([
    A.RandomResizedCrop(height=640, width=640, scale=(0.8, 1.0), ratio=(0.9, 1.1), p=1.0),
    A.HorizontalFlip(p=0.5),
    A.VerticalFlip(p=0.2),
    A.Rotate(limit=15, p=0.5),
    A.ColorJitter(brightness=0.3, contrast=0.3, saturation=0.3, hue=0.1, p=0.5),
    A.OneOf([
        A.GaussNoise(p=1.0),
        A.ISONoise(p=1.0),
        A.MultiplicativeNoise(p=1.0),
    ], p=0.3),
    A.OneOf([
        A.MotionBlur(p=1.0),
        A.MedianBlur(p=1.0),
        A.GaussianBlur(p=1.0),
    ], p=0.3),
    A.ShiftScaleRotate(shift_limit=0.1, scale_limit=0.2, rotate_limit=15, p=0.5),
    A.RandomBrightnessContrast(brightness_limit=0.3, contrast_limit=0.3, p=0.5),
    A.CoarseDropout(max_holes=8, max_height=32, max_width=32, p=0.3),
    A.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225]),
    ToTensorV2(),
], bbox_params=A.BboxParams(format='pascal_voc', label_fields=['class_labels']))

# 创建数据集
dataset = YOLODatasetWithAlbumentations(
    images_dir="dataset/images/train",
    labels_dir="dataset/labels/train",
    transforms=transforms
)

# 使用示例
image, boxes, labels = dataset[0]
print(f"图像形状: {image.shape}")
print(f"边界框数量: {len(boxes)}")
print(f"类别标签: {labels}")
```

### Mosaic 和 MixUp 增强

```python
import cv2
import numpy as np
from pathlib import Path

def mosaic_augmentation(
    image_paths: list,
    label_paths: list,
    output_size: int = 640
):
    """
    Mosaic 数据增强

    Args:
        image_paths: 4张图像路径列表
        label_paths: 对应的4个标签路径列表
        output_size: 输出图像尺寸

    Returns:
        mosaic_image: 拼接后的图像
        mosaic_labels: 拼接后的标签
    """
    assert len(image_paths) == 4, "Mosaic 需要 4 张图像"

    # 创建空白画布
    mosaic_img = np.zeros((output_size, output_size, 3), dtype=np.uint8)

    # 计算每个子图像的尺寸
    xc, yc = [int(random.uniform(output_size * 0.5, output_size * 1.5)) for _ in range(2)]
    mosaic_labels = []

    for i, (img_path, label_path) in enumerate(zip(image_paths, label_paths)):
        # 读取图像
        img = cv2.imread(str(img_path))
        h, w = img.shape[:2]

        # 计算放置位置
        if i == 0:  # 左上
            x1, y1, x2, y2 = max(xc - w, 0), max(yc - h, 0), xc, yc
        elif i == 1:  # 右上
            x1, y1, x2, y2 = xc, max(yc - h, 0), min(xc + w, output_size), yc
        elif i == 2:  # 左下
            x1, y1, x2, y2 = max(xc - w, 0), yc, xc, min(yc + h, output_size)
        else:  # 右下
            x1, y1, x2, y2 = xc, yc, min(xc + w, output_size), min(yc + h, output_size)

        # 将图像放置到画布上
        mosaic_img[y1:y2, x1:x2] = img[:y2-y1, :x2-x1]

        # 调整标签坐标
        if label_path.exists():
            with open(label_path, 'r') as f:
                for line in f:
                    class_id, x_center, y_center, bw, bh = map(float, line.strip().split())

                    # 转换为像素坐标
                    x_center *= w
                    y_center *= h
                    bw *= w
                    bh *= h

                    # 调整到新位置
                    new_x_center = x_center + x1
                    new_y_center = y_center + y1

                    # 检查是否在边界内
                    if 0 < new_x_center < output_size and 0 < new_y_center < output_size:
                        # 归一化
                        new_x_center /= output_size
                        new_y_center /= output_size
                        bw /= output_size
                        bh /= output_size

                        mosaic_labels.append(f"{int(class_id)} {new_x_center:.6f} {new_y_center:.6f} {bw:.6f} {bh:.6f}")

    return mosaic_img, mosaic_labels

def mixup_augmentation(img1, labels1, img2, labels2, alpha=0.5):
    """
    MixUp 数据增强

    Args:
        img1: 第一张图像
        labels1: 第一张图像的标签
        img2: 第二张图像
        labels2: 第二张图像的标签
        alpha: MixUp 系数

    Returns:
        mixed_image: 混合后的图像
        mixed_labels: 混合后的标签
    """
    # 生成混合系数
    lam = np.random.beta(alpha, alpha)

    # 混合图像
    mixed_image = (lam * img1 + (1 - lam) * img2).astype(np.uint8)

    # 合并标签
    mixed_labels = labels1 + labels2

    return mixed_image, mixed_labels
```

---

## 数据集管理工具

### 使用 Ultralytics HUB 管理数据集

```python
from hub_sdk import HUBClient

# 初始化 HUB 客户端
credentials = {"api_key": "YOUR_API_KEY"}
client = HUBClient(credentials)

# 1. 创建数据集
dataset = client.dataset()
dataset.create_dataset({
    "meta": {
        "name": "My Custom Dataset",
        "type": "detection",
        "description": "A custom dataset for YOLO11 training"
    }
})
print(f"数据集创建成功！ID: {dataset.id}")

# 2. 上传数据集
dataset.upload_dataset(
    dataset_id=dataset.id,
    dataset_path="path/to/dataset.zip"
)

# 3. 获取数据集信息
dataset_info = client.dataset(dataset.id)
print(f"数据集信息: {dataset_info.data}")

# 4. 更新数据集
dataset.update({
    "meta": {
        "name": "Updated Dataset Name",
        "description": "Updated description"
    }
})

# 5. 列出所有数据集
datasets = client.dataset_list()
for page in datasets:
    for dataset in page:
        print(f"数据集: {dataset.data['meta']['name']}")

# 6. 获取下载链接
download_link = dataset.get_download_link()
print(f"下载链接: {download_link}")

# 7. 删除数据集
# dataset.delete()
```

### 数据集验证和统计

```python
from ultralytics.data.utils import check_det_dataset, verify_image_label
from pathlib import Path
import matplotlib.pyplot as plt

def validate_dataset(dataset_yaml: str):
    """
    验证数据集

    Args:
        dataset_yaml: 数据集 YAML 文件路径
    """
    # 验证数据集
    data_info = check_det_dataset(dataset_yaml)

    print("=" * 50)
    print("数据集验证结果")
    print("=" * 50)
    print(f"数据集路径: {data_info['path']}")
    print(f"类别数量: {data_info['nc']}")
    print(f"类别名称: {data_info['names']}")
    print(f"训练集: {data_info['train']}")
    print(f"验证集: {data_info['val']}")
    print(f"测试集: {data_info.get('test', 'None')}")

    # 统计图像数量
    train_images = list(Path(data_info['train']).glob("*.*"))
    val_images = list(Path(data_info['val']).glob("*.*"))

    print(f"\n图像统计:")
    print(f"训练集图像数量: {len(train_images)}")
    print(f"验证集图像数量: {len(val_images)}")

    # 验证图像-标签对
    print(f"\n验证图像-标签对...")
    train_dir = Path(data_info['train']).parent
    for split in ['train', 'val']:
        img_dir = train_dir / 'images' / split
        label_dir = train_dir / 'labels' / split

        if img_dir.exists() and label_dir.exists():
            img_files = set([f.stem for f in img_dir.glob("*.*")])
            label_files = set([f.stem for f in label_dir.glob("*.txt")])

            missing_labels = img_files - label_files
            missing_images = label_files - img_files

            print(f"\n{split} 集:")
            print(f"  缺失标签: {len(missing_labels)}")
            print(f"  缺失图像: {len(missing_images)}")

            if missing_labels:
                print(f"  缺失标签的图像: {list(missing_labels)[:5]}...")
            if missing_images:
                print(f"  缺失图像的标签: {list(missing_images)[:5]}...")

def visualize_dataset_samples(
    dataset_yaml: str,
    num_samples: int = 9
):
    """
    可视化数据集样本

    Args:
        dataset_yaml: 数据集 YAML 文件路径
        num_samples: 显示样本数量
    """
    from ultralytics.data.utils import check_det_dataset
    import cv2
    import random

    # 加载数据集信息
    data_info = check_det_dataset(dataset_yaml)

    # 读取图像和标签
    img_dir = Path(data_info['train'])
    img_files = list(img_dir.glob("*.*"))
    random.shuffle(img_files)

    fig, axes = plt.subplots(3, 3, figsize=(15, 15))
    axes = axes.flatten()

    for i in range(min(num_samples, len(img_files))):
        img_path = img_files[i]
        label_path = img_path.parent.parent.parent / 'labels' / img_path.parent.name / f"{img_path.stem}.txt"

        # 读取图像
        img = cv2.imread(str(img_path))
        img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
        h, w = img.shape[:2]

        # 读取并绘制标签
        if label_path.exists():
            with open(label_path, 'r') as f:
                for line in f:
                    class_id, x_center, y_center, bw, bh = map(float, line.strip().split())

                    # 转换为像素坐标
                    x_center *= w
                    y_center *= h
                    bw *= w
                    bh *= h

                    # 计算边界框
                    x1 = int(x_center - bw / 2)
                    y1 = int(y_center - bh / 2)
                    x2 = int(x_center + bw / 2)
                    y2 = int(y_center + bh / 2)

                    # 绘制边界框
                    color = [random.randint(0, 255) for _ in range(3)]
                    cv2.rectangle(img, (x1, y1), (x2, y2), color, 2)

                    # 绘制类别标签
                    label = data_info['names'][int(class_id)]
                    cv2.putText(img, label, (x1, y1 - 10),
                               cv2.FONT_HERSHEY_SIMPLEX, 0.5, color, 2)

        axes[i].imshow(img)
        axes[i].axis('off')
        axes[i].set_title(img_path.name)

    plt.tight_layout()
    plt.savefig('dataset_samples.png', dpi=150, bbox_inches='tight')
    print(f"样本图像已保存到 dataset_samples.png")

# 使用示例
validate_dataset("coco8.yaml")
visualize_dataset_samples("coco8.yaml", num_samples=9)
```

### 数据集分析和可视化

```python
import numpy as np
import matplotlib.pyplot as plt
from pathlib import Path
from collections import Counter

def analyze_dataset_statistics(dataset_yaml: str):
    """
    分析数据集统计信息

    Args:
        dataset_yaml: 数据集 YAML 文件路径
    """
    from ultralytics.data.utils import check_det_dataset

    data_info = check_det_dataset(dataset_yaml)

    # 统计类别分布
    class_counts = Counter()
    bbox_sizes = []
    aspect_ratios = []

    # 遍历所有标签文件
    train_dir = Path(data_info['train']).parent
    for split in ['train', 'val']:
        label_dir = train_dir / 'labels' / split
        if not label_dir.exists():
            continue

        for label_file in label_dir.glob("*.txt"):
            with open(label_file, 'r') as f:
                for line in f:
                    parts = line.strip().split()
                    if len(parts) >= 5:
                        class_id = int(parts[0])
                        _, _, bw, bh = map(float, parts[1:5])

                        class_counts[class_id] += 1
                        bbox_sizes.append(bw * bh)
                        aspect_ratios.append(bw / (bh + 1e-6))

    # 创建可视化
    fig, axes = plt.subplots(2, 2, figsize=(15, 12))

    # 1. 类别分布
    ax1 = axes[0, 0]
    classes = [data_info['names'][i] for i in sorted(class_counts.keys())]
    counts = [class_counts[i] for i in sorted(class_counts.keys())]
    ax1.bar(classes, counts)
    ax1.set_xlabel('类别')
    ax1.set_ylabel('数量')
    ax1.set_title('类别分布')
    ax1.tick_params(axis='x', rotation=45)

    # 2. 边界框尺寸分布
    ax2 = axes[0, 1]
    ax2.hist(bbox_sizes, bins=50, edgecolor='black')
    ax2.set_xlabel('边界框面积（归一化）')
    ax2.set_ylabel('频次')
    ax2.set_title('边界框尺寸分布')

    # 3. 长宽比分布
    ax3 = axes[1, 0]
    ax3.hist(aspect_ratios, bins=50, edgecolor='black')
    ax3.set_xlabel('长宽比')
    ax3.set_ylabel('频次')
    ax3.set_title('边界框长宽比分布')
    ax3.set_xlim(0, 5)

    # 4. 统计摘要
    ax4 = axes[1, 1]
    ax4.axis('off')
    stats_text = f"""
    数据集统计摘要
    ===============
    总边界框数量: {sum(class_counts.values())}
    类别数量: {len(class_counts)}

    边界框尺寸统计:
      - 平均: {np.mean(bbox_sizes):.4f}
      - 中位数: {np.median(bbox_sizes):.4f}
      - 标准差: {np.std(bbox_sizes):.4f}

    长宽比统计:
      - 平均: {np.mean(aspect_ratios):.2f}
      - 中位数: {np.median(aspect_ratios):.2f}
      - 标准差: {np.std(aspect_ratios):.2f}
    """
    ax4.text(0.1, 0.5, stats_text, fontsize=12, family='monospace',
             verticalalignment='center')

    plt.tight_layout()
    plt.savefig('dataset_analysis.png', dpi=150, bbox_inches='tight')
    print(f"数据集分析已保存到 dataset_analysis.png")

# 使用示例
analyze_dataset_statistics("coco8.yaml")
```

---

## 常用数据集

### COCO8 数据集

```yaml
# COCO8 数据集配置
# 用于快速测试和调试
path: coco8
train: images/train
val: images/val

# COCO 80 个类别
names:
  0: person
  1: bicycle
  2: car
  # ... 省略中间类别
  79: toothbrush
nc: 80
```

**使用示例：**
```python
from ultralytics import YOLO

# 加载 YOLO11 模型
model = YOLO("yolo11n.pt")

# 在 COCO8 上训练（快速测试）
results = model.train(
    data="coco8.yaml",
    epochs=100,
    imgsz=640,
    batch=16
)
```

### COCO 数据集

```python
# 使用完整 COCO 数据集训练
model = YOLO("yolo11n.pt")

results = model.train(
    data="coco.yaml",  # 完整 COCO 数据集
    epochs=300,
    imgsz=640,
    batch=32,
    workers=8,
    device=0
)
```

### 自定义数据集示例

```yaml
# 自定义数据集配置示例
path: /path/to/custom_dataset
train: images/train
val: images/val
test: images/test

# 自定义类别
names:
  0: person
  1: car
  2: dog
  3: cat
nc: 4
```

---

## 最佳实践

### 数据集准备最佳实践

1. **数据质量**
   - 确保图像清晰且标注准确
   - 平衡各类别的样本数量
   - 避免标注错误和遗漏

2. **数据集划分**
   - 训练集：70-80%
   - 验证集：10-15%
   - 测试集：10-15%

3. **数据增强**
   - 使用 Mosaic 增强提升小目标检测
   - 适当使用 MixUp 提升模型泛化能力
   - 根据任务调整增强参数

4. **数据集大小**
   - 小型项目：1000-5000 张图像
   - 中型项目：5000-50000 张图像
   - 大型项目：50000+ 张图像

### YOLO11 训练建议

```python
from ultralytics import YOLO

# 1. 小型数据集（< 5000 张图像）
model = YOLO("yolo11n.pt")
results = model.train(
    data="custom.yaml",
    epochs=200,
    imgsz=640,
    batch=16,
    patience=50,  # 早停耐心值
    augment=True,  # 启用增强
    mosaic=1.0,    # Mosaic 概率
)

# 2. 中型数据集（5000-50000 张图像）
model = YOLO("yolo11s.pt")
results = model.train(
    data="custom.yaml",
    epochs=300,
    imgsz=640,
    batch=32,
    patience=100,
    augment=True,
    mosaic=1.0,
    mixup=0.1,  # 启用 MixUp
)

# 3. 大型数据集（> 50000 张图像）
model = YOLO("yolo11m.pt")
results = model.train(
    data="custom.yaml",
    epochs=500,
    imgsz=640,
    batch=64,
    patience=150,
    augment=True,
    mosaic=1.0,
    mixup=0.15,
    workers=16,  # 多线程加载数据
    device=[0, 1]  # 多 GPU 训练
)
```

### 数据集优化技巧

```python
from ultralytics import YOLO
from ultralytics.data.utils import check_det_dataset

# 1. 数据集清理
def clean_dataset(dataset_yaml):
    """清理数据集中的无效样本"""
    data_info = check_det_dataset(dataset_yaml)
    train_dir = Path(data_info['train']).parent

    for split in ['train', 'val']:
        img_dir = train_dir / 'images' / split
        label_dir = train_dir / 'labels' / split

        if img_dir.exists() and label_dir.exists():
            for img_file in img_dir.glob("*.*"):
                label_file = label_dir / f"{img_file.stem}.txt"
                # 检查标签是否存在
                if not label_file.exists():
                    print(f"删除无标签图像: {img_file}")
                    img_file.unlink()

                # 检查标签是否为空
                elif label_file.stat().st_size == 0:
                    print(f"删除空标签图像: {img_file}")
                    img_file.unlink()
                    label_file.unlink()

# 2. 数据集平衡
def balance_dataset(dataset_yaml, target_samples_per_class=1000):
    """平衡数据集类别分布"""
    from sklearn.model_selection import train_test_split

    data_info = check_det_dataset(dataset_yaml)
    # 实现数据集平衡逻辑
    # ...

# 3. 数据集导出
def export_dataset_for_hub(dataset_yaml, output_zip):
    """导出数据集用于上传到 HUB"""
    import zipfile

    data_info = check_det_dataset(dataset_yaml)
    dataset_path = Path(data_info['path'])

    with zipfile.ZipFile(output_zip, 'w') as zipf:
        for file in dataset_path.rglob('*'):
            if file.is_file():
                zipf.write(file, file.relative_to(dataset_path))

    print(f"数据集已导出到: {output_zip}")
```

---

## 总结

本文档提供了 YOLO 数据集管理的完整指南，包括：

1. **数据集格式**：YOLO 标准格式和配置文件
2. **数据集准备**：YOLO11 数据集准备和格式转换
3. **数据增强**：内置增强和自定义增强管道
4. **管理工具**：HUB 集成和数据集验证
5. **常用数据集**：COCO8、COCO 等
6. **最佳实践**：数据集准备和训练建议

通过遵循这些指南，您可以高效地准备和管理 YOLO11 训练所需的高质量数据集。

**相关资源：**
- [Ultralytics 文档](https://docs.ultralytics.com)
- [YOLO11 模型文档](./models.md)
- [训练指南](./training.md)
- [Ultralytics HUB](https://hub.ultralytics.com)
