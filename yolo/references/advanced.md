# Yolo - Advanced

**Pages:** 7

---

## Reference for ultralytics/utils/callbacks/platform.py

**URL:** https://docs.ultralytics.com/zh/reference/utils/callbacks/platform/

**Contents:**
- Reference for ultralytics/utils/callbacks/platform.py
- function ultralytics.utils.callbacks.platform.slugify
- function ultralytics.utils.callbacks.platform.resolve_platform_uri
- function ultralytics.utils.callbacks.platform._interp_plot
- function ultralytics.utils.callbacks.platform._send
- function ultralytics.utils.callbacks.platform._send_async
- function ultralytics.utils.callbacks.platform._upload_model
- function ultralytics.utils.callbacks.platform._upload_model_async
- function ultralytics.utils.callbacks.platform._get_environment_info
- function ultralytics.utils.callbacks.platform._get_project_name

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/callbacks/platform.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Convert text to URL-safe slug (e.g., 'My Project 1' -> 'my-project-1').

Resolve ul:// URIs to signed URLs by authenticating with Ultralytics Platform.

Formats: ul://username/datasets/slug -> Returns signed URL to NDJSON file ul://username/project/model -> Returns signed URL to .pt file

Interpolate plot curve data from 1000 to n points to reduce storage size.

Send event to Platform endpoint. Returns response JSON on success.

Send event asynchronously using bounded thread pool.

Upload model checkpoint to Platform via signed URL.

Upload model asynchronously using bounded thread pool.

Collect comprehensive environment info using existing ultralytics utilities.

Get slugified project and name from trainer args.

Initialize Platform logging at training start.

Log training and system metrics at epoch end.

Upload model checkpoint (rate limited to every 15 min).

Log final results, upload best model, and send validation plot data.

**Examples:**

Example 1 (python):
```python
def slugify(text)
```

Example 2 (python):
```python
def slugify(text):
    """Convert text to URL-safe slug (e.g., 'My Project 1' -> 'my-project-1')."""
    if not text:
        return text
    return re.sub(r"-+", "-", re.sub(r"[^a-z0-9\s-]", "", str(text).lower()).replace(" ", "-")).strip("-")[:128]
```

Example 3 (python):
```python
def resolve_platform_uri(uri, hard = True)
```

Example 4 (python):
```python
def resolve_platform_uri(uri, hard=True):
    """Resolve ul:// URIs to signed URLs by authenticating with Ultralytics Platform.

    Formats:
        ul://username/datasets/slug  -> Returns signed URL to NDJSON file
        ul://username/project/model  -> Returns signed URL to .pt file

    Args:
        uri (str): Platform URI starting with "ul://".
        hard (bool): Whether to raise an error if resolution fails (FileNotFoundError only).

    Returns:
        (str | None): Signed URL on success, None if not found and hard=False.

    Raises:
        ValueError: If API key is missing/invalid or URI format is wrong.
        PermissionError: If access is denied.
        RuntimeError: If resource is not ready (e.g., dataset still processing).
        FileNotFoundError: If resource not found and hard=True.
        ConnectionError: If network request fails and hard=True.
    """
    import requests

    path = uri[5:]  # Remove "ul://"
    parts = path.split("/")

    api_key = os.getenv("ULTRALYTICS_API_KEY") or SETTINGS.get("api_key")
    if not api_key:
        raise ValueError(f"ULTRALYTICS_API_KEY required for '{uri}'. Get key at https://alpha.ultralytics.com/settings")

    base = "https://alpha.ultralytics.com/api/webhooks"
    headers = {"Authorization": f"Bearer {api_key}"}

    # ul://username/datasets/slug
    if len(parts) == 3 and parts[1] == "datasets":
        username, _, slug = parts
        url = f"{base}/datasets/{username}/{slug}/export"

    # ul://username/project/model
    elif len(parts) == 3:
        username, project, model = parts
        url = f"{base}/models/{username}/{project}/{model}/download"

    else:
        raise ValueError(f"Invalid platform URI: {uri}. Use ul://user/datasets/name or ul://user/project/model")

    try:
        r = requests.head(url, headers=headers, allow_redirects=False, timeout=30)

        # Handle redirect responses (301, 302, 303, 307, 308)
        if 300 <= r.status_code < 400 and "location" in r.headers:
            return r.headers["location"]  # Return signed URL

        # Handle error responses
        if r.status_code == 401:
            raise ValueError(f"Invalid ULTRALYTICS_API_KEY for '{uri}'")
        if r.status_code == 403:
            raise PermissionError(f"Access denied for '{uri}'. Check dataset/model visibility settings.")
        if r.status_code == 404:
            if hard:
                raise FileNotFoundError(f"Not found on platform: {uri}")
            LOGGER.warning(f"Not found on platform: {uri}")
            return None
        if r.status_code == 409:
            raise RuntimeError(f"Resource not ready: {uri}. Dataset may still be processing.")

        # Unexpected response
        r.raise_for_status()
        raise RuntimeError(f"Unexpected response from platform for '{uri}': {r.status_code}")

    except requests.exceptions.RequestException as e:
        if hard:
            raise ConnectionError(f"Failed to resolve {uri}: {e}") from e
        LOGGER.warning(f"Failed to resolve {uri}: {e}")
        return None
```

---

## Reference for ultralytics/utils/callbacks/wb.py

**URL:** https://docs.ultralytics.com/zh/reference/utils/callbacks/wb/

**Contents:**
- Reference for ultralytics/utils/callbacks/wb.py
- function ultralytics.utils.callbacks.wb._custom_table
- function ultralytics.utils.callbacks.wb._plot_curve
- function ultralytics.utils.callbacks.wb._log_plots
- function ultralytics.utils.callbacks.wb.on_pretrain_routine_start
- function ultralytics.utils.callbacks.wb.on_fit_epoch_end
- function ultralytics.utils.callbacks.wb.on_train_epoch_end
- function ultralytics.utils.callbacks.wb.on_train_end

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/callbacks/wb.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Create and log a custom metric visualization to wandb.plot.pr_curve.

This function crafts a custom metric visualization that mimics the behavior of the default wandb precision-recall curve while allowing for enhanced customization. The visual metric is useful for monitoring model performance across different classes.

Log a metric curve visualization.

This function generates a metric curve based on input data and logs the visualization to wandb. The curve can represent aggregated data (mean) or individual class data, depending on the 'only_mean' flag.

The function leverages the '_custom_table' function to generate the actual visualization.

Log plots to WandB at a specific step if they haven't been logged already.

This function checks each plot in the input dictionary against previously processed plots and logs new or updated plots to WandB at the specified step.

The function uses a shallow copy of the plots dictionary to prevent modification during iteration. Plots are identified by their stem name (filename without extension). Each plot is logged as a WandB Image object.

Initialize and start wandb project if module is present.

Log training metrics and model information at the end of an epoch.

Log metrics and save images at the end of each training epoch.

Save the best model as an artifact and log final plots at the end of training.

**Examples:**

Example 1 (python):
```python
def _custom_table(x, y, classes, title = "Precision Recall Curve", x_title = "Recall", y_title = "Precision")
```

Example 2 (python):
```python
def _custom_table(x, y, classes, title="Precision Recall Curve", x_title="Recall", y_title="Precision"):
    """Create and log a custom metric visualization to wandb.plot.pr_curve.

    This function crafts a custom metric visualization that mimics the behavior of the default wandb precision-recall
    curve while allowing for enhanced customization. The visual metric is useful for monitoring model performance across
    different classes.

    Args:
        x (list): Values for the x-axis; expected to have length N.
        y (list): Corresponding values for the y-axis; also expected to have length N.
        classes (list): Labels identifying the class of each point; length N.
        title (str, optional): Title for the plot.
        x_title (str, optional): Label for the x-axis.
        y_title (str, optional): Label for the y-axis.

    Returns:
        (wandb.Object): A wandb object suitable for logging, showcasing the crafted metric visualization.
    """
    import polars as pl  # scope for faster 'import ultralytics'
    import polars.selectors as cs

    df = pl.DataFrame({"class": classes, "y": y, "x": x}).with_columns(cs.numeric().round(3))
    data = df.select(["class", "y", "x"]).rows()

    fields = {"x": "x", "y": "y", "class": "class"}
    string_fields = {"title": title, "x-axis-title": x_title, "y-axis-title": y_title}
    return wb.plot_table(
        "wandb/area-under-curve/v0",
        wb.Table(data=data, columns=["class", "y", "x"]),
        fields=fields,
        string_fields=string_fields,
    )
```

Example 3 (python):
```python
def _plot_curve(
    x,
    y,
    names=None,
    id="precision-recall",
    title="Precision Recall Curve",
    x_title="Recall",
    y_title="Precision",
    num_x=100,
    only_mean=False,
)
```

Example 4 (python):
```python
def _plot_curve(
    x,
    y,
    names=None,
    id="precision-recall",
    title="Precision Recall Curve",
    x_title="Recall",
    y_title="Precision",
    num_x=100,
    only_mean=False,
):
    """Log a metric curve visualization.

    This function generates a metric curve based on input data and logs the visualization to wandb. The curve can
    represent aggregated data (mean) or individual class data, depending on the 'only_mean' flag.

    Args:
        x (np.ndarray): Data points for the x-axis with length N.
        y (np.ndarray): Corresponding data points for the y-axis with shape (C, N), where C is the number of classes.
        names (list, optional): Names of the classes corresponding to the y-axis data; length C.
        id (str, optional): Unique identifier for the logged data in wandb.
        title (str, optional): Title for the visualization plot.
        x_title (str, optional): Label for the x-axis.
        y_title (str, optional): Label for the y-axis.
        num_x (int, optional): Number of interpolated data points for visualization.
        only_mean (bool, optional): Flag to indicate if only the mean curve should be plotted.

    Notes:
        The function leverages the '_custom_table' function to generate the actual visualization.
    """
    import numpy as np

    # Create new x
    if names is None:
        names = []
    x_new = np.linspace(x[0], x[-1], num_x).round(5)

    # Create arrays for logging
    x_log = x_new.tolist()
    y_log = np.interp(x_new, x, np.mean(y, axis=0)).round(3).tolist()

    if only_mean:
        table = wb.Table(data=list(zip(x_log, y_log)), columns=[x_title, y_title])
        wb.run.log({title: wb.plot.line(table, x_title, y_title, title=title)})
    else:
        classes = ["mean"] * len(x_log)
        for i, yi in enumerate(y):
            x_log.extend(x_new)  # add new x
            y_log.extend(np.interp(x_new, x, yi))  # interpolate y to new x
            classes.extend([names[i]] * len(x_new))  # add class names
        wb.log({id: _custom_table(x_log, y_log, classes, title, x_title, y_title)}, commit=False)
```

---

## VOC 探索示例 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/datasets/explorer/explorer/

**Contents:**
- VOC 探索示例
- 设置
- 相似性搜索
- 询问AI：使用自然语言搜索或筛选
- 在您的数据集上运行SQL查询
- 处理嵌入表（高级）
  - 运行原始查询¶
  - 转换为常用数据格式
  - 处理嵌入
  - 散点图

欢迎使用 Ultralytics Explorer API 笔记本。本笔记本介绍了可用于通过语义搜索、向量搜索和 SQL 查询探索数据集的资源。

尝试 yolo explorer (由 Explorer API 提供支持)

安装 ultralytics 并运行 yolo explorer 在您的终端中运行自定义查询和语义搜索，并在您的浏览器中查看。

截至 ultralytics>=8.3.10，Ultralytics Explorer 支持已弃用。类似（且已扩展）的数据集探索功能可在 Ultralytics HUB.

安装 ultralytics 以及所需的 依赖项，然后检查软件和硬件。

利用向量相似度搜索的强大功能，在数据集中找到相似的数据点以及它们在嵌入空间中的距离。只需为给定的数据集-模型对创建一个嵌入表即可。只需创建一次，即可自动重复使用。

嵌入表构建完成后，您可以通过以下任何一种方式运行语义搜索：

您将获得一个Pandas DataFrame，其中包含与输入最相似的有限数量的数据点，以及它们在嵌入空间中的距离。您可以使用此数据集进行进一步筛选。

您还可以使用以下命令直接绘制相似的样本 plot_similar util

您可以向Explorer对象发出提示，说明您想查看的数据点类型，它将尝试返回一个包含这些结果的DataFrame。由于它由LLM驱动，因此并非总能准确无误。在这种情况下，它将返回 None.

要绘制这些结果，您可以使用 plot_query_result 实用工具。示例：

有时您可能希望调查数据集中的特定条目。为此，Explorer 允许您执行 SQL 查询。它接受以下任一格式：

这可以用来调查模型性能和特定的数据点。例如：

您可以结合使用 SQL 查询和语义搜索来筛选到特定类型的结果

就像相似性搜索一样，您还可以使用一个实用程序来直接绘制 sql 查询，使用 exp.plot_sql_query

Explorer 适用于 LanceDB 在内部建立表格。您可以使用以下方式直接访问此表： Explorer.table 对象并运行原始查询，下推预过滤器和后过滤器等。

向量搜索从数据库中查找最近的向量。在推荐系统或搜索引擎中，您可以从搜索的产品中找到类似的产品。在 LLM 和其他 AI 应用程序中，每个数据点都可以由某些模型生成的嵌入来表示，它返回最相关的特征。

在高维向量空间中搜索，是为了找到查询向量的 K 近邻 (KNN)。

指标。在 LanceDB 中，指标是描述一对向量之间距离的方式。目前，它支持以下指标：

您可以从lancedb表中访问原始嵌入并对其进行分析。图像嵌入存储在列中 vector

分析嵌入的初步步骤之一是通过降维将其绘制在二维空间中。让我们尝试一个例子

这是一个由嵌入表驱动的操作的简单示例。Explorer 附带一个 similarity_index 操作-

对于给定的数据集、模型， max_dist & top_k 一旦生成相似度索引，它将被重复使用。如果您的数据集已更改，或者您只需要重新生成相似度索引，您可以传递 force=True。与向量和 SQL 搜索类似，这也带有一个可以直接绘制它的实用程序。让我们看看

让我们创建一个查询，查看相似度计数大于 30 的数据点，并绘制与它们相似的图像。

**Examples:**

Example 1 (unknown):
```unknown
!uv pip install ultralytics[explorer] openai
yolo checks
```

Example 2 (unknown):
```unknown
exp = Explorer("VOC.yaml", model="yolo11n.pt")
exp.create_embeddings_table()
```

Example 3 (markdown):
```markdown
# Search dataset by index
similar = exp.get_similar(idx=1, limit=10)
similar.head()
```

Example 4 (unknown):
```unknown
exp.plot_similar(idx=6500, limit=20)
exp.plot_similar(idx=[100, 101], limit=10)  # Can also pass list of idxs or imgs

exp.plot_similar(img="https://ultralytics.com/images/bus.jpg", limit=10, labels=False)  # Can also pass external images
```

---

## Reference for ultralytics/utils/callbacks/dvc.py

**URL:** https://docs.ultralytics.com/zh/reference/utils/callbacks/dvc/

**Contents:**
- Reference for ultralytics/utils/callbacks/dvc.py
- function ultralytics.utils.callbacks.dvc._log_images
- function ultralytics.utils.callbacks.dvc._log_plots
- function ultralytics.utils.callbacks.dvc._log_confusion_matrix
- function ultralytics.utils.callbacks.dvc.on_pretrain_routine_start
- function ultralytics.utils.callbacks.dvc.on_pretrain_routine_end
- function ultralytics.utils.callbacks.dvc.on_train_start
- function ultralytics.utils.callbacks.dvc.on_train_epoch_start
- function ultralytics.utils.callbacks.dvc.on_fit_epoch_end
- function ultralytics.utils.callbacks.dvc.on_train_end

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/callbacks/dvc.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Log images at specified path with an optional prefix using DVCLive.

This function logs images found at the given path to DVCLive, organizing them by batch to enable slider functionality in the UI. It processes image filenames to extract batch information and restructures the path accordingly.

Log plot images for training progress if they have not been previously processed.

Log confusion matrix for a validator using DVCLive.

This function processes the confusion matrix from a validator object and logs it to DVCLive by converting the matrix into lists of target and prediction labels.

Initialize DVCLive logger for training metadata during pre-training routine.

Log plots related to the training process at the end of the pretraining routine.

Log the training parameters if DVCLive logging is active.

Set the global variable _training_epoch value to True at the start of training each epoch.

Log training metrics, model info, and advance to next step at the end of each fit epoch.

This function is called at the end of each fit epoch during training. It logs various metrics including training loss items, validation metrics, and learning rates. On the first epoch, it also logs model information. Additionally, it logs training and validation plots and advances the DVCLive step counter.

This function only performs logging operations when DVCLive logging is active and during a training epoch. The global variable _training_epoch is used to track whether the current epoch is a training epoch.

Log best metrics, plots, and confusion matrix at the end of training.

This function is called at the conclusion of the training process to log final metrics, visualizations, and model artifacts if DVCLive logging is active. It captures the best model performance metrics, training plots, validation plots, and confusion matrix for later analysis.

**Examples:**

Example 1 (typescript):
```typescript
def _log_images(path: Path, prefix: str = "") -> None
```

Example 2 (python):
```python
>>> from pathlib import Path
>>> _log_images(Path("runs/train/exp/val_batch0_pred.jpg"), prefix="validation")
```

Example 3 (python):
```python
def _log_images(path: Path, prefix: str = "") -> None:
    """Log images at specified path with an optional prefix using DVCLive.

    This function logs images found at the given path to DVCLive, organizing them by batch to enable slider
    functionality in the UI. It processes image filenames to extract batch information and restructures the path
    accordingly.

    Args:
        path (Path): Path to the image file to be logged.
        prefix (str, optional): Optional prefix to add to the image name when logging.

    Examples:
        >>> from pathlib import Path
        >>> _log_images(Path("runs/train/exp/val_batch0_pred.jpg"), prefix="validation")
    """
    if live:
        name = path.name

        # Group images by batch to enable sliders in UI
        if m := re.search(r"_batch(\d+)", name):
            ni = m[1]
            new_stem = re.sub(r"_batch(\d+)", "_batch", path.stem)
            name = (Path(new_stem) / ni).with_suffix(path.suffix)

        live.log_image(os.path.join(prefix, name), path)
```

Example 4 (typescript):
```typescript
def _log_plots(plots: dict, prefix: str = "") -> None
```

---

## Ultralytics Explorer API - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/datasets/explorer/api/

**Contents:**
- Ultralytics Explorer API
- 简介
- 安装
- 用法
- 1. 相似性搜索
  - 绘制相似图像
- 2. 询问 AI（自然语言查询）
- 3. SQL 查询
  - 绘制 SQL 查询结果
- 4. 使用嵌入表

截至 ultralytics>=8.3.10，Ultralytics Explorer 支持已弃用。类似（且已扩展）的数据集探索功能可在 Ultralytics HUB.

Explorer API 是一个用于探索数据集的 python API。它支持使用 SQL 查询、向量相似性搜索和语义搜索来过滤和搜索数据集。

观看： Ultralytics Explorer API 概述

Explorer的某些功能依赖于外部库。当您使用Explorer时，这些库会自动安装。要手动安装这些依赖项，请使用以下命令：

给定数据集和模型对的 Embeddings 表仅创建一次并重复使用。这些在底层使用 LanceDB，它可以在磁盘上扩展，因此您可以为像 COCO 这样的大型数据集创建和重复使用 embeddings，而不会耗尽内存。

如果您想强制更新嵌入表，您可以传递 force=True 到 create_embeddings_table 方法。

您可以直接访问 LanceDB 表对象以执行高级分析。有关更多信息，请参阅使用嵌入表部分

相似性搜索是一种查找与给定图像相似的图像的技术。它基于相似的图像将具有相似嵌入的理念。一旦构建了嵌入表，您可以通过以下任何方式运行语义搜索：

您将获得一个Pandas DataFrame，其中包含 limit 与输入最相似的数据点数量，以及它们在嵌入空间中的距离。您可以使用此数据集进行进一步过滤。

您还可以使用以下工具绘制相似的图像 plot_similar method。此方法采用与以下方法相同的参数 get_similar 并在网格中绘制相似的图像。

此功能允许您使用自然语言过滤数据集，无需编写SQL。AI驱动的查询生成器将您的提示转换为查询并返回匹配结果。例如，您可以询问：“给我展示100张恰好包含一个人和2只狗的图像。也可以有其他对象”，它将生成查询并显示这些结果。 注意：此功能使用LLM，因此结果具有概率性，可能不准确。

您可以使用以下工具在数据集上运行 SQL 查询 sql_query 方法。此方法接受 SQL 查询作为输入，并返回一个包含结果的 Pandas DataFrame。

您还可以使用以下工具绘制SQL查询的结果 plot_sql_query method。此方法采用与以下方法相同的参数 sql_query 并在网格中绘制结果。

您还可以直接使用嵌入表。创建嵌入表后，您可以使用 Explorer.table

Explorer 适用于 LanceDB 在内部建立表格。您可以使用以下方式直接访问此表： Explorer.table 对象并运行原始查询，下推预过滤器和后过滤器等。

当使用大型数据集时，您还可以创建一个专用的向量索引，以便更快地查询。这可以通过以下方式完成 create_index LanceDB 表上的 method。

您可以使用嵌入表来执行各种探索性分析。 以下是一些示例：

Explorer 自带 similarity_index 操作：

它返回一个包含以下列的 Pandas DataFrame：

对于给定的数据集、模型， max_dist & top_k 一旦生成相似度索引，它将被重复使用。如果您的数据集已更改，或者您只需要重新生成相似度索引，您可以传递 force=True.

您可以使用相似度索引来构建自定义条件以过滤数据集。 例如，您可以使用以下代码过滤掉与数据集中任何其他图像不相似的图像：

您还可以使用您选择的绘图工具可视化嵌入空间。例如，这是一个使用 matplotlib 的简单示例：

开始使用 Explorer API 创建您自己的计算机视觉数据集探索报告。如需灵感，请查看VOC 探索示例。

试试我们基于 Explorer API 的 GUI 演示

Ultralytics Explorer API 专为全面的数据集探索而设计。它允许用户使用 SQL 查询、向量相似性搜索和语义搜索来过滤和搜索数据集。这个强大的 python API 可以处理大型数据集，非常适合使用 Ultralytics 模型的各种计算机视觉任务。

要安装 Ultralytics Explorer API 及其依赖项，请使用以下命令：

这将自动安装 Explorer API 功能所需的所有外部库。有关其他设置详细信息，请参阅我们文档的安装部分。

您可以使用 Ultralytics Explorer API，通过创建嵌入表并查询相似图像来进行相似性搜索。这是一个基本示例：

LanceDB，由 Ultralytics Explorer 在底层使用，提供可扩展的、基于磁盘的嵌入表。这确保您可以为 COCO 等大型数据集创建和重用嵌入，而不会耗尽内存。这些表只创建一次，可以重复使用，从而提高数据处理效率。

Ask AI 功能允许用户使用自然语言查询来过滤数据集。此功能利用 LLM 在后台将这些查询转换为 SQL 查询。这是一个示例：

**Examples:**

Example 1 (unknown):
```unknown
pip install ultralytics[explorer]
```

Example 2 (python):
```python
from ultralytics import Explorer

# Create an Explorer object
explorer = Explorer(data="coco128.yaml", model="yolo11n.pt")

# Create embeddings for your dataset
explorer.create_embeddings_table()

# Search for similar images to a given image/images
df = explorer.get_similar(img="path/to/image.jpg")

# Or search for similar images to a given index/indices
df = explorer.get_similar(idx=0)
```

Example 3 (python):
```python
from ultralytics import Explorer

# create an Explorer object
exp = Explorer(data="coco128.yaml", model="yolo11n.pt")
exp.create_embeddings_table()

similar = exp.get_similar(img="https://ultralytics.com/images/bus.jpg", limit=10)
print(similar.head())

# Search using multiple indices
similar = exp.get_similar(
    img=["https://ultralytics.com/images/bus.jpg", "https://ultralytics.com/images/bus.jpg"],
    limit=10,
)
print(similar.head())
```

Example 4 (python):
```python
from ultralytics import Explorer

# create an Explorer object
exp = Explorer(data="coco128.yaml", model="yolo11n.pt")
exp.create_embeddings_table()

similar = exp.get_similar(idx=1, limit=10)
print(similar.head())

# Search using multiple indices
similar = exp.get_similar(idx=[1, 10], limit=10)
print(similar.head())
```

---

## Reference for ultralytics/utils/callbacks/neptune.py

**URL:** https://docs.ultralytics.com/zh/reference/utils/callbacks/neptune/

**Contents:**
- Reference for ultralytics/utils/callbacks/neptune.py
- function ultralytics.utils.callbacks.neptune._log_scalars
- function ultralytics.utils.callbacks.neptune._log_images
- function ultralytics.utils.callbacks.neptune._log_plot
- function ultralytics.utils.callbacks.neptune.on_pretrain_routine_start
- function ultralytics.utils.callbacks.neptune.on_train_epoch_end
- function ultralytics.utils.callbacks.neptune.on_fit_epoch_end
- function ultralytics.utils.callbacks.neptune.on_val_end
- function ultralytics.utils.callbacks.neptune.on_train_end

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/callbacks/neptune.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Log scalars to the NeptuneAI experiment logger.

Log images to the NeptuneAI experiment logger.

This function logs image data to Neptune.ai when a valid Neptune run is active. Images are organized under the specified group name.

Log plots to the NeptuneAI experiment logger.

Initialize NeptuneAI run and log hyperparameters before training starts.

Log training metrics and learning rate at the end of each training epoch.

Log model info and validation metrics at the end of each fit epoch.

Log validation images at the end of validation.

Log final results, plots, and model weights at the end of training.

**Examples:**

Example 1 (typescript):
```typescript
def _log_scalars(scalars: dict, step: int = 0) -> None
```

Example 2 (json):
```json
>>> metrics = {"mAP": 0.85, "loss": 0.32}
>>> _log_scalars(metrics, step=100)
```

Example 3 (json):
```json
def _log_scalars(scalars: dict, step: int = 0) -> None:
    """Log scalars to the NeptuneAI experiment logger.

    Args:
        scalars (dict): Dictionary of scalar values to log to NeptuneAI.
        step (int, optional): The current step or iteration number for logging.

    Examples:
        >>> metrics = {"mAP": 0.85, "loss": 0.32}
        >>> _log_scalars(metrics, step=100)
    """
    if run:
        for k, v in scalars.items():
            run[k].append(value=v, step=step)
```

Example 4 (typescript):
```typescript
def _log_images(imgs_dict: dict, group: str = "") -> None
```

---

## Ultralytics VS Code 扩展 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/integrations/vscode/

**Contents:**
- Ultralytics VS Code 扩展
- 特性与优势
- 灵感来自 Ultralytics 社区
- 为什么选择 VS Code？
- 安装扩展
  - 在 VS Code 中安装
  - 从 VS Code 扩展市场安装
- 使用 Ultralytics-Snippets 扩展
  - 概述
  - 代码片段字段

观看： 如何使用 Ultralytics Visual Studio Code 扩展 | 即用型代码片段 | Ultralytics YOLO 🎉

✅ 您是一位数据科学家或机器学习工程师，正在使用Ultralytics构建计算机视觉应用程序吗？

✅ 您是否总是忘记 导出、预测、训练、track 或 验证 方法的参数或默认值？

✅ 想开始使用Ultralytics，希望有一种更容易的方式来引用或运行代码示例？

✅ 希望在使用Ultralytics时加快开发周期吗？

如果您使用 Visual Studio Code 并且对上述任何问题回答“是”，那么 Ultralytics-snippets VS Code 扩展程序将为您提供帮助！请继续阅读以了解有关该扩展程序的更多信息、如何安装以及如何使用它。

在 20 秒内使用 Ultralytics YOLO 运行示例代码！🚀

构建此扩展的灵感来自 Ultralytics 社区。社区中关于类似主题和示例的问题推动了该项目的发展。此外，许多 Ultralytics 团队成员使用 VS Code 来加速他们的工作 ⚡。

Visual Studio Code 在全球开发者中极受欢迎，并在 Stack Overflow 开发者调查中于 2021、2022、2023 和 2024 年被评为最受欢迎的编辑器。 鉴于 VS Code 的高度自定义性、内置功能、广泛的兼容性和可扩展性，如此多的开发者使用它也就不足为奇了。 考虑到它在更广泛的开发者社区以及 Ultralytics Discord、Discourse、Reddit 和 GitHub 社区中的受欢迎程度，构建一个 VS Code 扩展程序来帮助简化您的工作流程并提高您的工作效率是很有意义的。

想让我们知道您使用什么来开发代码吗？请访问我们的 Discourse 社区投票，让我们知道！当您在那里时，也许可以查看我们最喜欢的一些计算机视觉、机器学习、AI 和开发者表情包，甚至发布您最喜欢的！

任何允许安装 VS Code 扩展的代码环境 应该是 与 Ultralytics-snippets 扩展兼容。 发布扩展后，发现 neovim 可以与 VS Code 扩展兼容。要了解更多信息，请参阅 neovim 安装部分 的 Readme 文件中 Ultralytics-Snippets 仓库.

导航到 VS Code 中的扩展菜单 或使用快捷键 Ctrl+Shift ⇑+x，然后搜索 Ultralytics-snippets。

访问 VS Code 扩展市场 并搜索 Ultralytics-snippets，或直接访问 VS Code 市场上的扩展页面。

点击 安装 按钮，并允许您的浏览器启动 VS Code 会话。

Visual Studio Code 扩展市场页面，适用于 Ultralytics-Snippets

🧠 智能代码完成： 借助为 Ultralytics API 量身定制的高级代码完成建议，更快、更准确地编写代码。

⌛ 提高开发速度： 通过消除重复的编码任务并利用预先构建的代码块段代码片段，节省时间。

🔬 改进的代码质量： 通过智能代码完成编写更清晰、更一致且无错误的代码。

💎 精简的工作流程： 通过自动化常见任务，专注于项目的核心逻辑。

仅当以下情况时，该扩展才会运行 语言模式 已针对 python 🐍 进行了配置。这是为了避免在使用任何其他文件类型时插入代码片段。所有代码片段的前缀都以开头 ultra，然后简单地输入 ultra 在安装扩展后，您的编辑器中将显示一个可能使用的代码片段列表。您也可以打开 VS Code 命令面板 使用 Ctrl+平移 ⇑+p 并运行命令 Snippets: Insert Snippet.

许多代码段都有带有默认占位符值或名称的“字段”。例如，来自 预测 方法可以保存到名为的 python 变量中 r, results, detections, preds 或者开发者选择的任何其他方式，这就是为什么代码段包含“字段”。使用 Tab ⇥ 在插入代码片段后，如果您按下键盘上的Tab键，光标会在字段之间快速移动。一旦选中一个字段，输入新的变量名将更改该实例，以及该变量在代码片段中的所有其他实例！

插入代码段后，重命名 model 作为 world_model 更新所有实例。按下 Tab ⇥ 移动到下一个字段，这将打开一个下拉菜单，允许选择模型比例，移动到下一个字段会提供另一个下拉菜单来选择 world 或 worldv2 模型变体。

不需要输入代码片段的完整前缀，甚至不需要从代码片段的开头开始输入。请参见下面的图片示例。

代码片段以尽可能最具描述性的方式命名，但这意味可能需要输入很多内容，如果目的是快速行动，那将适得其反 更快。幸运的是，VS Code 允许用户输入 ultra.example-yolo-predict, example-yolo-predict, yolo-predict，甚至是 ex-yolo-p 并且仍然可以访问到预期的代码片段选项！如果预期的代码片段是 实际上 ultra.example-yolo-predict-kwords，然后只使用键盘箭头 ↑ 或 ↓ 要突出显示所需代码片段并按下 输入 ↵ 或 Tab ⇥ 将插入正确的代码块。

正在输入 ex-yolo-p 将 仍然 得到正确的代码片段。

这些是 Ultralytics-snippets 扩展当前可用的代码片段类别。将来会添加更多，因此请务必检查更新并启用该扩展的自动更新。如果您认为缺少任何内容，您还可以请求添加其他代码片段。

字段 ultra.examples 对于任何希望学习如何开始使用 Ultralytics YOLO 的基础知识的人来说，代码片段都非常有用。示例代码片段旨在插入后运行（有些还具有下拉选项）。此示例显示在动画中 最高 在本页中，插入代码片段后，所有代码都会被选中并使用以下方式进行交互式运行 平移 ⇑+输入 ↵.

就像动画显示的那样 最高 在本页中，您可以使用代码片段 ultra.example-yolo-predict 要插入以下代码示例。插入后，唯一可配置的选项是模型比例，可以是以下任何一种： n, s, m, l或 x.

除了以下代码片段之外，其他代码片段的目标是 ultra.examples 是为了在使用 Ultralytics 时，使开发更容易和更快。一个常见的代码块，可以在许多项目中使用的，是迭代列表。 Results 从使用模型返回 预测 method。这个 ultra.result-loop 代码片段可以对此有所帮助。

使用 ultra.result-loop 将插入以下默认代码（包括注释）。

然而，由于 Ultralytics 支持众多 任务，当 处理推理结果 还有其他的 Results 您可能希望访问的属性，即 代码片段字段 将会很强大。

跳转到 boxes 字段，会出现一个下拉菜单，允许根据需要选择另一个属性。

所有各种 Ultralytics 都有超过 💯 个关键字参数 任务 和 模式！需要记住的内容太多了，如果参数是，很容易忘记 save_frame 或 save_frames （这绝对是 save_frames 顺便说一句）。这是 ultra.kwargs 代码片段可以提供帮助！

要插入 预测 method，包括所有 推理参数，使用 ultra.kwargs-predict，它将插入以下代码（包括注释）。

此代码段包含所有关键字参数的字段，但也包含 model 和 src 如果您在代码中使用了不同的变量。 在包含关键字参数的每一行上，都包含一个简要说明以供参考。

了解有哪些代码片段可用的最佳方式是下载并安装扩展，然后亲自尝试！如果您想提前查看列表，可以访问 repo 或 VS Code marketplace 上的扩展页面，以查看所有可用代码片段的表格。

VS Code 的 Ultralytics-Snippets 扩展旨在帮助数据科学家和机器学习工程师更高效地使用 Ultralytics YOLO 构建计算机视觉应用程序。通过提供预构建的代码片段和有用的示例，我们帮助您专注于最重要的事情：创建创新解决方案。请访问 VS Code 市场上的扩展页面 并留下评论，分享您的反馈。⭐

可以使用 Ultralytics-Snippets repo 上的 Issues 请求新的代码片段。

VS Code 使用组合键 Ctrl+Space 在预览窗口中显示更多/更少的信息。如果在键入代码片段前缀时没有看到片段预览，使用此组合键应该可以恢复预览。

如果您使用 VS Code，并且开始看到提示您安装 Ultralytics-snippets 扩展的消息，但又不想再看到该消息，则有两种方法可以禁用此消息。

安装 Ultralytics-snippets，此消息将不再显示 😆！

您可以使用 yolo settings vscode_msg False 禁用显示消息，而无需安装扩展。您可以了解更多关于 Ultralytics 设置 在 快速入门 页面，如果您不熟悉。

访问 Ultralytics-snippets 存储库 并打开 Issue 或 Pull Request！

与任何其他 VS Code 扩展一样，您可以通过导航到 VS Code 中的“扩展”菜单来卸载它。在菜单中找到 Ultralytics-snippets 扩展，然后单击齿轮图标 (⚙)，再单击“卸载”以删除该扩展。

**Examples:**

Example 1 (python):
```python
from ultralytics import ASSETS, YOLO

model = YOLO("yolo11n.pt", task="detect")
results = model(source=ASSETS / "bus.jpg")

for result in results:
    print(result.boxes.data)
    # result.show()  # uncomment to view each result image
```

Example 2 (markdown):
```markdown
# reference https://docs.ultralytics.com/modes/predict/#working-with-results

for result in results:
    result.boxes.data  # torch.Tensor array
```

Example 3 (sql):
```sql
model.predict(
    source=src,  # (str, optional) source directory for images or videos
    imgsz=640,  # (int | list) input images size as int or list[w,h] for predict
    conf=0.25,  # (float) minimum confidence threshold
    iou=0.7,  # (float) intersection over union (IoU) threshold for NMS
    vid_stride=1,  # (int) video frame-rate stride
    stream_buffer=False,  # (bool) buffer incoming frames in a queue (True) or only keep the most recent frame (False)
    visualize=False,  # (bool) visualize model features
    augment=False,  # (bool) apply image augmentation to prediction sources
    agnostic_nms=False,  # (bool) class-agnostic NMS
    classes=None,  # (int | list[int], optional) filter results by class, i.e. classes=0, or classes=[0,2,3]
    retina_masks=False,  # (bool) use high-resolution segmentation masks
    embed=None,  # (list[int], optional) return feature vectors/embeddings from given layers
    show=False,  # (bool) show predicted images and videos if environment allows
    save=True,  # (bool) save prediction results
    save_frames=False,  # (bool) save predicted individual video frames
    save_txt=False,  # (bool) save results as .txt file
    save_conf=False,  # (bool) save results with confidence scores
    save_crop=False,  # (bool) save cropped images with results
    stream=False,  # (bool) for processing long videos or numerous images with reduced memory usage by returning a generator
    verbose=True,  # (bool) enable/disable verbose inference logging in the terminal
)
```

---
