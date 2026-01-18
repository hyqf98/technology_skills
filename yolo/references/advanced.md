# Yolo - Advanced

**Pages:** 7

---

## Reference for ultralytics/utils/callbacks/platform.py

**URL:** https://docs.ultralytics.com/zh/reference/utils/callbacks/platform/

### 📋 概述

该模块提供了与 Ultralytics 平台集成的回调函数,用于训练过程中的日志记录、模型上传和性能监控。这些回调函数支持将训练指标、模型检查点和环境信息自动同步到 Ultralytics Platform。

### 🔧 核心功能

#### 1. **slugify(text)**
将文本转换为 URL 安全的 slug 格式。

**技术说明:**
- 使用正则表达式处理文本
- 转换为小写并替换空格为连字符
- 移除特殊字符,只保留字母、数字和连字符
- 限制最大长度为 128 个字符

**使用场景:**
- 创建项目名称标识符
- 生成 URL 友好的资源名称
- 规范化模型和数据集名称

**代码示例:**
```python
from ultralytics.utils.callbacks.platform import slugify

# 转换项目名称
project_name = "My Object Detection Project v1.0"
slug = slugify(project_name)
print(slug)  # 输出: my-object-detection-project-v1-0

# 用于创建唯一的资源标识符
model_name = "YOLOv8-Custom-Model_Final"
identifier = slugify(model_name)
print(identifier)  # 输出: yolov8-custom-model-final
```

#### 2. **resolve_platform_uri(uri, hard=True)**
解析 ul:// URIs 为签名 URL。

**技术说明:**
- 支持两种 URI 格式:
  - `ul://username/datasets/slug` → 返回 NDJSON 文件的签名 URL
  - `ul://username/project/model` → 返回 .pt 文件的签名 URL
- 需要设置 `ULTRALYTICS_API_KEY` 环境变量
- 使用 Bearer Token 进行身份验证

**配置说明:**
```python
# 设置 API Key
import os
os.environ["ULTRALYTICS_API_KEY"] = "your-api-key-here"

# 或使用 Ultralytics settings
from ultralytics import settings
settings.update({"api_key": "your-api-key-here"})
```

**代码示例:**
```python
from ultralytics.utils.callbacks.platform import resolve_platform_uri

# 解析数据集 URI
dataset_uri = "ul://username/datasets/my-dataset"
dataset_url = resolve_platform_uri(dataset_uri)
print(f"数据集 URL: {dataset_url}")

# 解析模型 URI
model_uri = "ul://username/project/my-model"
model_url = resolve_platform_uri(model_uri)
print(f"模型 URL: {model_url}")

# 错误处理
try:
    url = resolve_platform_uri("ul://invalid/resource")
except FileNotFoundError:
    print("资源未找到")
except ValueError as e:
    print(f"无效的 URI: {e}")
```

**注意事项:**
- 确保 API Key 有效且有权限访问资源
- 处理网络异常和超时情况
- 资源可能仍在处理中(409 状态码)

#### 3. **训练回调函数**

##### on_pretrain_routine_start(trainer)
在训练开始时初始化 Platform 日志记录。

**功能说明:**
- 收集环境信息(CPU、GPU、Python 版本等)
- 初始化会话和项目
- 上传初始配置

##### on_fit_epoch_end(trainer)
在每个 epoch 结束时记录训练和系统指标。

**记录内容:**
- 训练损失
- 验证指标(mAP、precision、recall)
- 学习率
- GPU 利用率和内存使用

##### on_train_end(trainer)
训练结束时保存最佳模型并发送验证图表。

**功能说明:**
- 上传最佳模型检查点
- 生成并上传训练曲线图
- 记录最终性能指标

### 🎯 YOLO11 新特性

#### 1. **增强的平台集成**
YOLO11 提供了更强大的平台集成功能:
- 实时训练监控
- 自动模型版本管理
- 分布式训练支持
- 实验对比和分析

#### 2. **性能优化**
- 异步数据上传,减少训练中断
- 智能采样,减少日志数据量
- 增量模型上传,节省带宽

### 📊 实际应用示例

#### 完整的训练流程示例

```python
from ultralytics import YOLO
import os

# 设置 API Key
os.environ["ULTRALYTICS_API_KEY"] = "your-api-key"

# 加载 YOLO11 模型
model = YOLO("yolo11n.pt")

# 训练模型(自动启用平台集成)
results = model.train(
    data="coco8.yaml",
    epochs=100,
    imgsz=640,
    project="my-project",
    name="experiment-1"
)

# 训练过程中自动记录:
# - 每个 epoch 的指标
# - 验证结果
# - 模型检查点
# - 训练曲线图
```

#### 自定义回调函数

```python
from ultralytics import YOLO
from ultralytics.utils.callbacks import add_integration_callbacks

# 自定义回调函数
def on_train_end(trainer):
    print(f"训练完成! 最佳 mAP: {trainer.best_fitness}")
    # 自定义逻辑
    # ...

# 注册自定义回调
model = YOLO("yolo11n.pt")
model.add_callback("on_train_end", on_train_end)

# 训练
model.train(data="coco8.yaml", epochs=10)
```

### ⚠️ 注意事项

1. **API Key 安全:**
   - 不要将 API Key 提交到版本控制
   - 使用环境变量或配置文件
   - 定期轮换 API Key

2. **网络连接:**
   - 确保稳定的网络连接
   - 处理上传失败的情况
   - 考虑使用重试机制

3. **数据隐私:**
   - 注意上传的数据集和模型
   - 检查敏感信息
   - 遵守数据保护法规

4. **性能影响:**
   - 日志记录可能影响训练速度
   - 可使用 `sync=False` 禁用数据收集
   - 调整上传频率以优化性能

---

## Reference for ultralytics/utils/callbacks/wb.py

**URL:** https://docs.ultralytics.com/zh/reference/utils/callbacks/wb/

### 📋 概述

该模块实现了与 Weights & Biases (W&B) 的集成,用于实验跟踪、可视化和模型管理。W&B 是一个强大的 MLOps 平台,可以监控训练过程、比较实验结果和共享模型。

### 🔧 核心功能

#### 1. **_custom_table(x, y, classes, title, x_title, y_title)**
创建自定义的度量可视化图表。

**技术说明:**
- 使用 Polars DataFrame 处理数据
- 创建面积曲线下的可视化
- 支持多类别对比分析
- 适用于 precision-recall、ROC 曲线等

**参数说明:**
- `x`: x 轴数据点列表
- `y`: y 轴数据点列表
- `classes`: 类别标签列表
- `title`: 图表标题
- `x_title`: x 轴标签
- `y_title`: y 轴标签

**代码示例:**
```python
from ultralytics.utils.callbacks.wb import _custom_table
import numpy as np

# 创建 precision-recall 曲线数据
recall = np.linspace(0, 1, 100)
precision = np.random.rand(100) * 0.3 + 0.7  # 模拟精度数据
classes = ["person"] * 100

# 生成可视化
table = _custom_table(
    x=recall,
    y=precision,
    classes=classes,
    title="Precision-Recall Curve",
    x_title="Recall",
    y_title="Precision"
)
```

#### 2. **_plot_curve(x, y, names, id, title, x_title, y_title, num_x, only_mean)**
记录度量曲线可视化。

**技术说明:**
- 对曲线进行插值以平滑显示
- 支持显示平均曲线和各个类别的曲线
- 自动处理数据点和标签

**配置选项:**
```python
# 只显示平均曲线
plot = _plot_curve(x, y, names, only_mean=True)

# 显示所有类别的曲线
plot = _plot_curve(x, y, names, only_mean=False)

# 自定义插值点数
plot = _plot_curve(x, y, names, num_x=200)
```

#### 3. **_log_plots(plots, step, prefix)**
记录图表到 WandB。

**功能说明:**
- 检查是否已处理过该图表
- 将图像转换为 WandB Image 对象
- 使用文件名作为标识符

### 🎯 YOLO11 与 W&B 集成

#### 安装和配置

```bash
# 安装 W&B
pip install wandb

# 登录
wandb login
```

#### 完整训练示例

```python
import wandb
from ultralytics import YOLO

# 初始化 W&B 项目
wandb.init(
    project="yolo11-experiments",
    name="experiment-1",
    config={
        "learning_rate": 0.01,
        "epochs": 100,
        "batch_size": 16
    }
)

# 训练模型
model = YOLO("yolo11n.pt")
model.train(
    data="coco8.yaml",
    epochs=100,
    imgsz=640
)

# 完成运行
wandb.finish()
```

### 📊 高级功能

#### 1. **超参数优化**

```python
import wandb

sweep_config = {
    "method": "bayes",
    "metric": {"name": "mAP50", "goal": "maximize"},
    "parameters": {
        "lr0": {"min": 0.0001, "max": 0.01},
        "momentum": {"min": 0.6, "max": 0.98},
        "weight_decay": {"min": 0.0, "max": 0.001}
    }
}

wandb.sweep(sweep_config, project="yolo11-sweeps")
```

#### 2. **实验对比**

```python
# 在 W&B Dashboard 中对比多个实验
# 访问 https://wandb.ai/your-username/project-name

# 查看训练曲线
# 对比不同超参数的效果
# 分析模型性能
```

#### 3. **模型版本管理**

```python
import wandb

# 保存模型到 W&B
run = wandb.init(project="yolo11-models")
artifact = wandb.Artifact("yolo11n-custom", type="model")
artifact.add_file("runs/train/exp/weights/best.pt")
run.log_artifact(artifact)

# 加载模型
artifact = run.use_artifact("yolo11n-custom:latest")
artifact_dir = artifact.download()
model = YOLO(f"{artifact_dir}/best.pt")
```

### ⚠️ 注意事项

1. **数据隐私:**
   - 检查上传的数据和图像
   - 使用离线模式处理敏感数据
   - 遵守数据保护政策

2. **网络要求:**
   - 需要稳定的网络连接
   - 大文件上传可能需要时间
   - 考虑使用代理或镜像

3. **存储管理:**
   - 注意 W&B 存储配额
   - 定期清理旧的实验
   - 使用标签和备注组织实验

---

## Ultralytics Explorer API

**URL:** https://docs.ultralytics.com/zh/datasets/explorer/api/

### 📋 概述

> ⚠️ **重要提示:** 截至 ultralytics>=8.3.10,Ultralytics Explorer 支持已弃用。类似(且已扩展)的数据集探索功能可在 Ultralytics HUB 获取。

Explorer API 是一个用于探索计算机视觉数据集的 Python API,支持语义搜索、SQL 查询、向量相似性搜索和自然语言提示。

### 🔧 核心功能

#### 1. **安装**

```bash
pip install ultralytics[explorer]
```

依赖库:
- LanceDB: 向量数据库
- OpenAI: 用于自然语言查询(可选)

#### 2. **初始化 Explorer**

```python
from ultralytics import Explorer

# 创建 Explorer 对象
exp = Explorer(
    data="coco8.yaml",      # 数据集配置
    model="yolo11n.pt"       # 使用的模型
)

# 创建嵌入表
exp.create_embeddings_table()
```

**技术说明:**
- 嵌入表只创建一次并可重复使用
- 使用 LanceDB 进行持久化存储
- 支持大型数据集而不耗尽内存
- 强制更新: `exp.create_embeddings_table(force=True)`

### 🎯 主要功能

#### 1. **相似性搜索**

基于向量相似度搜索相似的数据点。

```python
# 按索引搜索
similar = exp.get_similar(idx=1, limit=10)
print(similar.head())

# 按图像搜索
similar = exp.get_similar(
    img="https://ultralytics.com/images/bus.jpg",
    limit=10
)

# 多索引搜索
similar = exp.get_similar(idx=[100, 101], limit=10)

# 绘制相似图像
exp.plot_similar(idx=6500, limit=20)
exp.plot_similar(img="path/to/image.jpg", limit=10, labels=False)
```

**使用场景:**
- 查找相似的数据样本
- 检测数据异常
- 分析数据分布
- 数据集清洗

#### 2. **AI 自然语言查询**

使用自然语言搜索数据集。

```python
# 自然语言查询
df = exp.ask_ai("显示100张恰好包含一个人和2只狗的图像")
print(df.head())

# 绘制查询结果
exp.plot_sql_query("显示10张恰好包含5个人的图像")
```

**技术说明:**
- 使用 LLM 将查询转换为 SQL
- 结果具有概率性,可能不准确
- 需要 OpenAI API Key

```python
# 设置 API Key
from ultralytics import settings
settings.update({"openai_api_key": "your-api-key"})
```

#### 3. **SQL 查询**

直接执行 SQL 查询。

```python
# 完整 SQL 查询
df = exp.sql_query("SELECT * FROM table WHERE labels LIKE '%person%'")

# WHERE 子句
df = exp.sql_query("WHERE labels LIKE '%person%' AND labels LIKE '%dog%'")

# 绘制查询结果
exp.plot_sql_query("WHERE labels LIKE '%car%'")
```

**使用场景:**
- 精确过滤数据
- 复杂条件查询
- 数据统计分析

#### 4. **嵌入表操作**

直接访问和操作嵌入表。

```python
# 访问底层 LanceDB 表
table = exp.table

# 原始查询
results = table.search().limit(10).to_pandas()

# 创建向量索引(加速查询)
table.create_index(num_partitions=10, num_sub_vectors=10)
```

### 📊 高级功能

#### 1. **相似度索引**

计算每个数据点与其他点的相似度。

```python
# 生成相似度索引
exp.similarity_index(max_dist=0.2, top_k=0.01)

# 查询相似度高的数据点
similar_data = exp.sql_query(
    "WHERE similarity_count > 30"
)

# 绘制相似数据
exp.plot_similar(idx=similar_data.index[0], limit=10)
```

**使用场景:**
- 检测重复数据
- 查找代表性样本
- 数据集分析

#### 2. **嵌入可视化**

降维可视化嵌入空间。

```python
import matplotlib.pyplot as plt
import numpy as np

# 获取嵌入向量
embeddings = exp.table.to_pandas()["vector"]

# t-SNE 降维
from sklearn.manifold import TSNE
tsne = TSNE(n_components=2)
embeddings_2d = tsne.fit_transform(np.stack(embeddings))

# 可视化
plt.scatter(embeddings_2d[:, 0], embeddings_2d[:, 1], alpha=0.5)
plt.title("数据集嵌入可视化")
plt.show()
```

### 🎯 实际应用示例

#### 数据集质量分析

```python
from ultralytics import Explorer

exp = Explorer(data="coco8.yaml", model="yolo11n.pt")
exp.create_embeddings_table()

# 1. 查找异常样本
similar = exp.get_similar(idx=0, limit=10)
print(f"最相似的样本距离: {similar['imgs_dist'].max()}")

# 2. 分析类别分布
stats = exp.sql_query("""
    SELECT
        COUNT(*) as count,
        AVG(ims_dist) as avg_similarity
    FROM table
    GROUP BY labels
""")

# 3. 查找潜在错误标注
errors = exp.ask_ai("显示可能有错误标注的图像")
```

#### 数据集清洗

```python
# 1. 查找重复样本
duplicates = exp.sql_query("""
    WHERE similarity_count > 5 AND im_dist < 0.1
""")

# 2. 查找离群样本
outliers = exp.sql_query("""
    WHERE similarity_count < 2
""")

print(f"发现 {len(duplicates)} 个可能的重复样本")
print(f"发现 {len(outliers)} 个离群样本")
```

### ⚠️ 注意事项

1. **性能考虑:**
   - 首次创建嵌入表需要时间
   - 大型数据集建议使用 SSD
   - 定期清理旧的嵌入表

2. **内存使用:**
   - LanceDB 使用磁盘存储,内存占用小
   - 可以处理超过内存大小的数据集
   - 建议使用 `force=True` 更新嵌入表

3. **API Key:**
   - 自然语言查询需要 OpenAI API Key
   - 设置: `yolo settings openai_api_key="your-key"`
   - 注意 API 使用费用

---

## Reference for ultralytics/utils/callbacks/dvc.py

**URL:** https://docs.ultralytics.com/zh/reference/utils/callbacks/dvc/

### 📋 概述

该模块实现了与 DVC (Data Version Control) 的集成,用于实验跟踪和模型管理。DVC 是一个开源的 ML 实验管理工具,可以跟踪指标、参数和模型。

### 🔧 核心功能

#### 1. **_log_images(path, prefix)**
使用 DVCLive 记录图像。

**技术说明:**
- 按 batch 组织图像以启用滑块功能
- 从文件名提取 batch 信息
- 重构路径结构

**代码示例:**
```python
from ultralytics.utils.callbacks.dvc import _log_images
from pathlib import Path

# 记录验证结果
_log_images(
    Path("runs/train/exp/val_batch0_pred.jpg"),
    prefix="validation"
)
```

#### 2. **_log_plots(plots, prefix)**
记录训练图表。

**功能说明:**
- 检查是否已处理过图表
- 记录损失曲线、混淆矩阵等
- 支持多种可视化类型

#### 3. **_log_confusion_matrix(validator)**
记录混淆矩阵。

**技术说明:**
- 从验证器提取混淆矩阵
- 转换为目标和预测标签列表
- 使用 DVCLive 记录

### 🎯 DVC 集成示例

#### 1. **安装 DVC**

```bash
pip install dvc dvclive
dvc init
```

#### 2. **训练时启用 DVC**

```python
from ultralytics import YOLO

# DVC 会自动检测并记录
model = YOLO("yolo11n.pt")
model.train(
    data="coco8.yaml",
    epochs=100,
    imgsz=640
)
```

#### 3. **配置 DVCLive**

创建 `dvc.yaml`:

```yaml
# dvc.yaml
metrics:
  - runs/train/exp/results.csv
plots:
  - runs/train/exp/*.png
  - runs/train/exp/results.csv:
      x: epoch
      y: [mAP50, mAP50-95]
```

#### 4. **比较实验**

```bash
# 训练多个模型
dvc exp run -n exp1
dvc exp run -n exp2

# 比较实验
dvc exp show

# 查看特定指标
dvc exp show --metrics
dvc exp show --params
```

### 📊 实际应用

#### 1. **超参数优化**

```python
from ultralytics import YOLO
import itertools

# 定义超参数网格
learning_rates = [0.001, 0.01, 0.1]
momentums = [0.9, 0.95, 0.99]

# 网格搜索
for lr, momentum in itertools.product(learning_rates, momentums):
    model = YOLO("yolo11n.pt")
    model.train(
        data="coco8.yaml",
        epochs=50,
        lr0=lr,
        momentum=momentum
    )
```

```bash
# 比较所有实验
dvc exp show --sort-by metric:mAP50
```

#### 2. **实验跟踪**

```python
# 自定义指标
from dvclive import Live

with Live("custom_experiment") as live:
    model = YOLO("yolo11n.pt")

    for epoch in range(100):
        # 训练一个 epoch
        results = model.train_one_epoch(...)

        # 记录自定义指标
        live.log_metric("custom_metric", results.custom_value)
        live.next_step()
```

### ⚠️ 注意事项

1. **Git 集成:**
   - DVC 需要与 Git 配合使用
   - 跟踪 `dvc.yaml` 和 `.dvclive` 文件
   - 不要跟踪数据文件和模型

2. **存储管理:**
   - DVC 使用缓存存储大文件
   - 定期清理缓存: `dvc gc`
   - 使用远程存储备份数据

3. **性能:**
   - DVCLive 可能轻微影响训练速度
   - 可以调整日志频率
   - 使用异步记录减少影响

---

## Reference for ultralytics/utils/callbacks/neptune.py

**URL:** https://docs.ultralytics.com/zh/reference/utils/callbacks/neptune/

### 📋 概述

该模块实现了与 Neptune.ai 的集成,用于实验跟踪和模型管理。Neptune 是一个 MLOps 平台,提供全面的实验监控和协作功能。

### 🔧 核心功能

#### 1. **_log_scalars(scalars, step)**
记录标量指标。

**技术说明:**
- 支持多种指标类型
- 自动记录时间序列数据
- 适用于损失、精度等指标

**代码示例:**
```python
from ultralytics.utils.callbacks.neptune import _log_scalars

# 记录训练指标
metrics = {
    "mAP": 0.85,
    "precision": 0.88,
    "recall": 0.82
}
_log_scalars(metrics, step=100)
```

#### 2. **_log_images(imgs_dict, group)**
记录图像。

**功能说明:**
- 按组组织图像
- 支持批量上传
- 自动命名和分类

**代码示例:**
```python
# 记录验证图像
images = {
    "predictions": "runs/val/pred.jpg",
    "ground_truth": "runs/val/true.jpg"
}
_log_images(images, group="validation")
```

#### 3. **_log_plot(plot, name)**
记录图表。

**功能说明:**
- 记录 matplotlib 图表
- 保存为图像格式
- 自动添加到实验

### 🎯 Neptune 集成示例

#### 1. **安装和配置**

```bash
pip install neptune-client
```

```python
import neptune

# 初始化 Neptune
run = neptune.init_run(
    project="my-org/yolo11-experiments",
    api_token="your-api-token"
)
```

#### 2. **完整训练流程**

```python
from ultralytics import YOLO
import neptune

# 初始化 Neptune run
run = neptune.init_run(
    project="yolo11-experiments",
    name="experiment-1",
    tags=["yolo11", "object-detection"],
    parameters={
        "learning_rate": 0.01,
        "epochs": 100,
        "batch_size": 16
    }
)

# 训练模型
model = YOLO("yolo11n.pt")
results = model.train(
    data="coco8.yaml",
    epochs=100,
    imgsz=640
)

# 记录最终结果
run["results/mAP"] = results.results_dict.get("metrics/mAP50(B)")
run["results/precision"] = results.results_dict.get("metrics/precision(B)")

# 上传模型
run["model"].upload("runs/train/exp/weights/best.pt")

# 结束 run
run.stop()
```

### 📊 高级功能

#### 1. **模型版本管理**

```python
# 记录模型版本
run["model/version"] = "v1.0"
run["model/architecture"] = "yolo11n"
run["model/framework"] = "ultralytics"

# 上传多个模型
run["models/best"].upload("runs/train/exp/weights/best.pt")
run["models/last"].upload("runs/train/exp/weights/last.pt")
```

#### 2. **自定义指标**

```python
# 记录自定义指标
run["custom/inference_time"] = 0.025
run["custom/model_size"] = 6.3  # MB

# 记录指标序列
for epoch in range(100):
    run["train/loss"].append(loss_value)
    run["val/mAP"].append(map_value)
```

#### 3. **Artifacts 管理**

```python
# 保存数据集配置
from neptune.types import File

run["dataset/config"].upload("coco8.yaml")
run["dataset/sample"].upload("data/sample.jpg")

# 记录数据集信息
run["dataset/size"] = 1000
run["dataset/classes"] = ["person", "car", "dog"]
```

### 🎯 实际应用

#### 1. **实验对比**

```python
# 运行多个实验
experiments = []
for lr in [0.001, 0.01, 0.1]:
    run = neptune.init_run(
        project="yolo11-experiments",
        name=f"lr-{lr}",
        tags=["hyperparameter-sweep"]
    )

    model = YOLO("yolo11n.pt")
    model.train(data="coco8.yaml", epochs=50, lr0=lr)

    experiments.append(run)

# 在 Neptune Dashboard 对比
# https://app.neptune.ai/
```

#### 2. **协作和分享**

```python
# 分享实验链接
run_url = run.get_url()
print(f"查看实验: {run_url}")

# 添加协作者
# 在 Neptune 项目设置中添加团队成员
```

### ⚠️ 注意事项

1. **API Token:**
   - 保存 API Token 到环境变量
   - 不要硬编码在代码中
   - 定期轮换 Token

2. **数据隐私:**
   - 检查上传的数据和图像
   - 使用私有项目保护敏感数据
   - 遵守数据保护法规

3. **存储管理:**
   - 注意 Neptune 存储配额
   - 定期清理旧的实验
   - 使用标签和描述组织实验

---

## Ultralytics VS Code 扩展

**URL:** https://docs.ultralytics.com/zh/integrations/vscode/

### 📋 概述

Ultralytics VS Code 扩展为 Visual Studio Code 提供了 YOLO 代码片段和智能完成功能,帮助开发者更快速地编写代码。

### 🔧 安装

#### 方法 1: 从 VS Code 市场安装

1. 打开 VS Code
2. 按 `Ctrl+Shift+X` 打开扩展面板
3. 搜索 "Ultralytics-snippets"
4. 点击安装

#### 方法 2: 从命令行安装

```bash
code --install-extension ultralytics.ultralytics-snippets
```

### 🎯 主要功能

#### 1. **代码片段**

所有代码片段前缀为 `ultra`,输入后显示可用片段列表。

**常用代码片段:**

```python
# 快速开始示例
ultra.example-yolo-predict
# 生成:
from ultralytics import ASSETS, YOLO

model = YOLO("yolo11n.pt", task="detect")
results = model(source=ASSETS / "bus.jpg")

for result in results:
    print(result.boxes.data)
```

```python
# 结果循环
ultra.result-loop
# 生成:
for result in results:
    result.boxes.data
```

```python
# 所有预测参数
ultra.kwargs-predict
# 生成完整的参数列表和注释
```

#### 2. **智能完成**

- 自动补全函数名
- 参数提示
- 文档集成

#### 3. **可配置字段**

使用 `Tab` 键在字段间快速移动:

```python
# 输入: ultra.example-yolo-predict
# 结果:
model = YOLO("yolo11n.pt")  # 可配置模型
results = model(source=...)   # Tab 到下一个字段
```

### 📊 代码片段类别

#### 1. **基础示例 (ultra.examples)**

```python
# 预测示例
ultra.example-yolo-predict

# 训练示例
ultra.example-yolo-train

# 验证示例
ultra.example-yolo-val

# 导出示例
ultra.example-yolo-export
```

#### 2. **结果处理**

```python
# 结果循环
ultra.result-loop

# 提取框
ultra.result-boxes

# 提取掩码
ultra.result-masks

# 提取关键点
ultra.result-keypoints
```

#### 3. **完整参数列表**

```python
# 预测参数
ultra.kwargs-predict

# 训练参数
ultra.kwargs-train

# 验证参数
ultra.kwargs-val

# 导出参数
ultra.kwargs-export
```

### 🎯 实际应用示例

#### 1. **快速原型开发**

```python
# 使用代码片段快速开始
# 输入: ultra.example-yolo-predict

from ultralytics import ASSETS, YOLO

model = YOLO("yolo11n.pt", task="detect")
results = model(source=ASSETS / "bus.jpg")

for result in results:
    result.boxes.data  # torch.Tensor array
```

#### 2. **批量处理**

```python
# 输入: ultra.result-loop

# 添加自定义逻辑
for result in results:
    boxes = result.boxes.data
    for box in boxes:
        x1, y1, x2, y2, conf, cls = box
        print(f"检测到 {model.names[int(cls)]}, 置信度: {conf:.2f}")
```

#### 3. **自定义训练**

```python
# 输入: ultra.kwargs-train

# 修改关键参数
model.train(
    data="custom.yaml",
    epochs=200,          # 增加训练轮数
    imgsz=1280,         # 提高图像分辨率
    batch=32,           # 调整批量大小
    lr0=0.001,          # 自定义学习率
    lrf=0.01,           # 最终学习率因子
    momentum=0.937,     # 动量
    weight_decay=0.0005 # 权重衰减
)
```

### 💡 使用技巧

#### 1. **快速访问**

- 不需要输入完整前缀
- 可以输入部分关键词
- 使用 `Tab` 键快速插入

```python
# 以下都可以触发代码片段:
ultra.example-yolo-predict
example-yolo-predict
yolo-predict
ex-yolo-p
```

#### 2. **字段导航**

- 按 `Tab` 跳转到下一个字段
- 按 `Shift+Tab` 返回上一个字段
- 使用下拉菜单选择选项

#### 3. **自定义配置**

```json
// settings.json
{
    "ultralytics.snippets.enable": true,
    "ultralytics.snippets.completions": true,
    "ultralytics.vscode_msg": true
}
```

### ⚠️ 注意事项

1. **Python 语言模式:**
   - 代码片段只在 Python 文件中可用
   - 确保文件扩展名为 `.py`
   - VS Code 语言模式设置为 Python

2. **版本兼容:**
   - 支持 YOLOv8、YOLOv10、YOLO11
   - 定期更新扩展以获取最新功能
   - 检查扩展版本兼容性

3. **禁用消息:**

```python
# 方法 1: 安装扩展
# 消息自动禁用

# 方法 2: 禁用消息
from ultralytics import settings
settings.update({"vscode_msg": False})
```

### 🔗 相关资源

- **扩展页面:** [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=ultralytics.ultralytics-snippets)
- **GitHub:** [Ultralytics-Snippets](https://github.com/ultralytics/ultralytics-snippets)
- **文档:** [Ultralytics 文档](https://docs.ultralytics.com)
