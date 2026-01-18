# Yolo - Classification

**Pages:** 2

---

## Reference for ultralytics/data/split.py

**URL:** https://docs.ultralytics.com/zh/reference/data/split/

### 📋 概述

该模块提供了图像分类数据集的分割功能,可以将数据集按指定比例分割为训练集和验证集。这对于机器学习项目中的数据准备阶段非常重要。

### 🔧 核心功能

#### 1. **split_classify_dataset(source_dir, train_ratio)**

将分类数据集分割为训练集和验证集。

**技术说明:**
- 创建新的目录 `{source_dir}_split`
- 保持原有的类别结构
- 默认按 80/20 分割
- 随机打乱数据以确保分布均匀

**参数说明:**
- `source_dir` (str | Path): 分类数据集根目录
  - 目录结构应为: `dataset/class_name/images.jpg`
  - 支持嵌套目录结构
- `train_ratio` (float): 训练集比例,范围 0-1
  - 默认值: 0.8 (80% 训练, 20% 验证)
  - 常用值: 0.7, 0.75, 0.8, 0.9

**返回值:**
- `Path`: 分割后的目录路径

**代码示例:**

```python
from ultralytics.data.split import split_classify_dataset
from pathlib import Path

# 基础用法 - 默认 80/20 分割
split_path = split_classify_dataset("path/to/caltech")
print(f"分割后的数据集保存在: {split_path}")

# 自定义分割比例 - 75/25
split_path = split_classify_dataset(
    "path/to/caltech",
    train_ratio=0.75
)

# 使用 Path 对象
source = Path("datasets/my_dataset")
split_path = split_classify_dataset(source, train_ratio=0.8)

# 验证分割结果
train_path = split_path / "train"
val_path = split_path / "val"

# 统计各类别的样本数
for class_dir in train_path.iterdir():
    if class_dir.is_dir():
        train_count = len(list(class_dir.glob("*.*")))
        val_count = len(list((val_path / class_dir.name).glob("*.*")))
        total = train_count + val_count
        ratio = train_count / total
        print(f"{class_dir.name}: 训练={train_count}, 验证={val_count}, 比例={ratio:.2%}")
```

**目录结构变化:**

```
# 分割前
caltech/
├── class1/
│   ├── img1.jpg
│   ├── img2.jpg
│   └── ...
├── class2/
│   ├── img1.jpg
│   └── ...
└── ...

# 分割后
caltech_split/
├── train/
│   ├── class1/
│   │   ├── img1.jpg  # 原始图像的副本
│   │   └── ...
│   ├── class2/
│   │   ├── img1.jpg
│   │   └── ...
│   └── ...
└── val/
    ├── class1/
    │   ├── img2.jpg
    │   └── ...
    ├── class2/
    │   └── ...
    └── ...
```

**使用场景:**

1. **数据准备阶段:**
   - 在开始训练前划分数据集
   - 确保训练和验证数据不重叠

2. **交叉验证:**
   - 创建多个不同的分割
   - 用于模型评估和选择

3. **实验对比:**
   - 使用固定的验证集
   - 确保实验的可重复性

**高级用法:**

```python
import shutil
from pathlib import Path
from ultralytics.data.split import split_classify_dataset

# 创建多个分割以进行交叉验证
dataset_path = Path("datasets/my_dataset")
for fold in range(5):
    # 每次使用不同的随机种子
    split_path = split_classify_dataset(
        dataset_path,
        train_ratio=0.8
    )

    # 重命名为 fold-specific
    final_path = Path(f"{dataset_path}_fold{fold}")
    shutil.move(split_path, final_path)

    print(f"Fold {fold}: 数据集已创建在 {final_path}")

# 创建自定义分割比例
splits = {
    "train": 0.7,
    "val": 0.15,
    "test": 0.15
}

# 注意: split_classify_dataset 只创建 train/val
# 如需测试集,可以再次分割验证集
split_path = split_classify_dataset("dataset", train_ratio=0.7)

# 从验证集中分割出测试集
# (需要手动实现或使用其他工具)
```

**注意事项:**

1. **数据完整性:**
   - 原始数据集不会被修改
   - 创建新的目录并复制图像
   - 确保有足够的磁盘空间

2. **类别平衡:**
   - 随机分割可能导致类别不平衡
   - 对于小样本数据集要特别注意
   - 可以使用分层采样保持平衡

3. **可重复性:**
   - 默认随机打乱,每次结果可能不同
   - 可以设置随机种子确保可重复性

4. **性能考虑:**
   - 大型数据集复制需要时间
   - 可以使用硬链接节省空间
   - 考虑使用符号链接(需要手动实现)

#### 2. **autosplit(path, weights=None, annotated_only=False)**

自动分割数据集为 train/val/test 集合。

**技术说明:**
- 生成分割配置文件 `autosplit_*.txt`
- 记录每个图像的归属集合
- 支持按权重分配

**参数说明:**
- `path`: 数据集根目录
- `weights`: 分割权重,如 `(训练, 验证, 测试)`
- `annotated_only`: 是否只包含已标注的图像

**代码示例:**

```python
from ultralytics.data.split import autosplit

# 默认分割 (train/val/test)
autosplit("path/to/dataset")

# 自定义权重
autosplit(
    "path/to/dataset",
    weights=(0.7, 0.2, 0.1)  # 70% 训练, 20% 验证, 10% 测试
)

# 只处理已标注的图像
autosplit(
    "path/to/dataset",
    annotated_only=True
)

# 生成的文件
# - autosplit_train.txt: 训练集图像列表
# - autosplit_val.txt: 验证集图像列表
# - autosplit_test.txt: 测试集图像列表
```

### 🎯 YOLO11 分类训练集成

#### 1. **数据集准备流程**

```python
from ultralytics import YOLO
from ultralytics.data.split import split_classify_dataset

# 1. 准备数据集结构
# datasets/my_classification/
# ├── class1/
# │   ├── img1.jpg
# │   └── ...
# ├── class2/
# │   ├── img1.jpg
# │   └── ...
# └── ...

# 2. 分割数据集
split_path = split_classify_dataset(
    "datasets/my_classification",
    train_ratio=0.8
)

# 3. 创建数据集配置 YAML
# my_dataset.yaml
"""
path: datasets/my_classification_split
train: train
val: val
names:
  0: class1
  1: class2
"""

# 4. 训练 YOLO11 分类模型
model = YOLO("yolo11n-cls.pt")
model.train(
    data="my_dataset.yaml",
    epochs=100,
    imgsz=224
)
```

#### 2. **评估和验证**

```python
# 使用分割后的验证集进行评估
metrics = model.val()

# 在测试集上评估
model = YOLO("runs/train/exp/weights/best.pt")
test_metrics = model.val(
    data="my_dataset.yaml",
    split="test"  # 如果配置中定义了测试集
)

print(f"测试集准确率: {test_metrics.top1}")
print(f"测试集 Top-5 准确率: {test_metrics.top5}")
```

### 📊 实际应用示例

#### 1. **数据集质量检查**

```python
from pathlib import Path
from ultralytics.data.split import split_classify_dataset
import matplotlib.pyplot as plt

# 分割数据集
split_path = split_classify_dataset("datasets/animals", train_ratio=0.8)

# 分析分割结果
train_path = split_path / "train"
val_path = split_path / "val"

class_counts = {}
for class_dir in train_path.iterdir():
    if class_dir.is_dir():
        train_count = len(list(class_dir.glob("*.*")))
        val_count = len(list((val_path / class_dir.name).glob("*.*")))

        class_counts[class_dir.name] = {
            "train": train_count,
            "val": val_count,
            "total": train_count + val_count,
            "ratio": train_count / (train_count + val_count)
        }

# 可视化类别分布
classes = list(class_counts.keys())
train_counts = [class_counts[c]["train"] for c in classes]
val_counts = [class_counts[c]["val"] for c in classes]

plt.figure(figsize=(12, 6))
x = range(len(classes))
width = 0.35

plt.bar([i - width/2 for i in x], train_counts, width, label='Train')
plt.bar([i + width/2 for i in x], val_counts, width, label='Val')

plt.xlabel('Class')
plt.ylabel('Count')
plt.title('Dataset Split Distribution')
plt.xticks(x, classes, rotation=45)
plt.legend()
plt.tight_layout()
plt.savefig("dataset_split_distribution.png")
print("图表已保存到 dataset_split_distribution.png")
```

#### 2. **批量处理多个数据集**

```python
import yaml
from pathlib import Path
from ultralytics.data.split import split_classify_dataset

# 定义多个数据集
datasets = [
    {"path": "datasets/animals", "ratio": 0.8},
    {"path": "datasets/vehicles", "ratio": 0.75},
    {"path": "datasets/fruits", "ratio": 0.85}
]

results = []
for dataset in datasets:
    print(f"正在处理 {dataset['path']}...")

    # 分割数据集
    split_path = split_classify_dataset(
        dataset["path"],
        train_ratio=dataset["ratio"]
    )

    # 收集统计信息
    train_path = split_path / "train"
    val_path = split_path / "val"

    total_train = sum(len(list(c.glob("*.*"))) for c in train_path.iterdir() if c.is_dir())
    total_val = sum(len(list(c.glob("*.*"))) for c in val_path.iterdir() if c.is_dir())

    results.append({
        "dataset": Path(dataset["path"]).name,
        "split_path": str(split_path),
        "train_count": total_train,
        "val_count": total_val,
        "ratio": dataset["ratio"]
    })

# 保存结果
with open("dataset_splits.yaml", "w") as f:
    yaml.dump(results, f, default_flow_style=False)

print("所有数据集处理完成!")
print(f"结果已保存到 dataset_splits.yaml")
```

### ⚠️ 注意事项

1. **目录结构要求:**
   - 必须遵循标准的分类数据集格式
   - 每个类别一个子目录
   - 图像直接放在类别目录下

2. **文件扩展名:**
   - 支持常见图像格式: `.jpg`, `.jpeg`, `.png`, `.bmp`
   - 使用 `glob("*.*")` 匹配所有文件
   - 确保不包含其他文件类型

3. **内存和存储:**
   - 大型数据集需要足够的磁盘空间
   - 复制操作可能需要较长时间
   - 考虑使用 SSD 以提高性能

4. **随机性控制:**
   - 使用 `random.shuffle()` 随机打乱
   - 可以在函数中设置随机种子
   - 确保实验的可重复性

5. **错误处理:**
   - 检查目录是否存在
   - 验证图像文件完整性
   - 处理权限问题

---

## Reference for ultralytics/hub/google/__init__.py

**URL:** https://docs.ultralytics.com/zh/reference/hub/google/__init__/

### 📋 概述

该模块提供了 Google Cloud Platform (GCP) 区域管理功能,用于选择最佳的 GCP 区域以获得最佳性能。这对于在 Google Cloud 上部署 YOLO 模型非常有用。

### 🔧 核心功能

#### 1. **GCPRegions 类**

管理和分析 GCP 区域。

**技术说明:**
- 包含所有 GCP 区域的详细信息
- 按层级分类 (Tier 1 和 Tier 2)
- 提供网络延迟分析
- 自动选择最低延迟区域

**属性说明:**

```python
from ultralytics.hub.google import GCPRegions

regions = GCPRegions()

# 访问区域信息
all_regions = regions.regions  # 所有区域的字典

# 区域信息结构:
# {
#     "region_name": (tier, city, country),
#     "asia-east1": (1, "Taiwan", "China"),
#     "us-east1": (1, "South Carolina", "United States"),
#     ...
# }

# 层级说明:
# - Tier 1: 主要区域,通常性能更好
# - Tier 2: 次要区域,成本可能更低
```

**方法说明:**

##### lowest_latency(verbose=True, attempts=3)

确定延迟最低的 GCP 区域。

**参数:**
- `verbose` (bool): 是否显示详细信息
- `attempts` (int): ping 测试次数

**返回:**
- `list`: 排序后的区域列表,按延迟升序

**代码示例:**

```python
from ultralytics.hub.google import GCPRegions

# 初始化区域管理器
regions = GCPRegions()

# 查找最低延迟区域
best_regions = regions.lowest_latency(verbose=True, attempts=3)

# 输出结果
print("延迟最低的区域:")
for region, latency in best_regions[:5]:
    tier, city, country = regions.regions[region]
    print(f"  {region}: {city}, {country} - 延迟: {latency:.2f}ms")

# 选择最佳区域
if best_regions:
    best_region = best_regions[0][0]
    print(f"\n推荐使用区域: {best_region}")
```

##### tier1()

返回所有 Tier 1 区域列表。

**使用场景:**
- 选择性能最优的区域
- 生产环境推荐

**代码示例:**

```python
# 获取 Tier 1 区域
tier1_regions = regions.tier1()

print("Tier 1 区域:")
for region in tier1_regions:
    tier, city, country = regions.regions[region]
    print(f"  {region}: {city}, {country}")
```

##### tier2()

返回所有 Tier 2 区域列表。

**使用场景:**
- 选择成本较低的区域
- 开发和测试环境

### 🎯 实际应用

#### 1. **部署优化**

```python
from ultralytics.hub.google import GCPRegions
from ultralytics import YOLO

# 查找最佳区域
regions = GCPRegions()
best_region = regions.lowest_latency(verbose=True)[0][0]

print(f"使用最佳区域: {best_region}")

# 在 GCP 上部署模型
model = YOLO("yolo11n.pt")
# 配置 GCP 部署时使用最佳区域
# ...
```

#### 2. **成本优化**

```python
from ultralytics.hub.google import GCPRegions

regions = GCPRegions()

# 对比 Tier 1 和 Tier 2 区域
tier1 = regions.tier1()
tier2 = regions.tier2()

print("Tier 1 区域 (性能优先):")
for region in tier1[:5]:
    tier, city, country = regions.regions[region]
    print(f"  {region}: {city}, {country}")

print("\nTier 2 区域 (成本优先):")
for region in tier2[:5]:
    tier, city, country = regions.regions[region]
    print(f"  {region}: {city}, {country}")

# 根据需求选择
# - 生产环境: Tier 1
# - 开发/测试: Tier 2
```

#### 3. **区域分析**

```python
import pandas as pd
from ultralytics.hub.google import GCPRegions

regions = GCPRegions()

# 创建区域信息 DataFrame
data = []
for region, (tier, city, country) in regions.regions.items():
    data.append({
        "region": region,
        "tier": f"Tier {tier}",
        "city": city,
        "country": country
    })

df = pd.DataFrame(data)

# 分析区域分布
print("按国家统计:")
print(df.groupby("country").size().sort_values(ascending=False))

print("\n按层级统计:")
print(df.groupby("tier").size())
```

### ⚠️ 注意事项

1. **网络依赖:**
   - 需要 Internet 连接进行 ping 测试
   - 防火墙可能阻止连接
   - 结果可能因网络状况而异

2. **延迟测量:**
   - 多次测量取平均值
   - 考虑网络波动
   - 不同时间可能结果不同

3. **区域选择:**
   - 不仅考虑延迟
   - 还要考虑成本、合规性等
   - 咨询云服务提供商的建议

4. **GCP 限制:**
   - 某些区域可能有配额限制
   - 检查资源可用性
   - 提前申请必要的配额

### 📊 完整示例

```python
from ultralytics.hub.google import GCPRegions
import time

class GCPRegionSelector:
    """GCP 区域选择器"""

    def __init__(self):
        self.regions = GCPRegions()

    def find_best_region(self, attempts=5, verbose=True):
        """查找最佳区域"""
        if verbose:
            print(f"正在测试 {len(self.regions.regions)} 个区域...")

        start_time = time.time()
        best_regions = self.regions.lowest_latency(
            verbose=verbose,
            attempts=attempts
        )
        elapsed = time.time() - start_time

        if verbose:
            print(f"\n测试完成,耗时: {elapsed:.2f}秒")

        return best_regions

    def recommend_region(self, use_tier1=True):
        """推荐区域"""
        if use_tier1:
            candidates = self.regions.tier1()
            criteria = "性能"
        else:
            candidates = self.regions.tier2()
            criteria = "成本"

        best = self.find_best_region()

        for region, _ in best:
            if region in candidates:
                tier, city, country = self.regions.regions[region]
                print(f"\n推荐区域 ({criteria}优先):")
                print(f"  区域: {region}")
                print(f"  位置: {city}, {country}")
                print(f"  层级: Tier {tier}")
                return region

        # 如果没有匹配的,返回最佳区域
        region, _ = best[0]
        print(f"\n推荐区域 (最低延迟):")
        print(f"  区域: {region}")
        return region

# 使用示例
selector = GCPRegionSelector()

# 生产环境推荐
prod_region = selector.recommend_region(use_tier1=True)

# 开发环境推荐
dev_region = selector.recommend_region(use_tier1=False)
```
