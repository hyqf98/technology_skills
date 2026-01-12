# Yolo - Datasets

**Pages:** 9

---

## 为 Ultralytics 开源项目做贡献

**URL:** https://docs.ultralytics.com/zh/help/contributing/

**Contents:**
- 为 Ultralytics 开源项目做贡献
- 🤝 行为准则
- 🚀 通过 Pull Requests 贡献
  - 📝 CLA 签署
  - ✍️ Google 风格 Docstrings
  - ✅ GitHub Actions CI 测试
- ✨ 代码贡献的最佳实践
- 👀 审查 Pull Requests
- 🐞 报告 Bug
- 📜 许可

欢迎！我们很高兴您考虑为我们的 Ultralytics开源项目贡献力量。您的参与不仅有助于提升我们存储库的质量，也造福于整个 计算机视觉社区。本指南提供了清晰的指导方针和最佳实践，以帮助您入门。

观看： 如何贡献 Ultralytics 仓库 | Ultralytics 模型、数据集和文档 🚀

为了确保为每个人提供一个热情和包容的环境，所有贡献者都必须遵守我们的行为准则。 尊重、友善和专业是我们社区的核心。

我们非常感谢以 pull requests (PRs) 的形式提供的贡献。为了使审查过程尽可能顺利，请按照以下步骤操作：

在我们可以合并您的 pull request 之前，您必须签署我们的 贡献者许可协议 (CLA)。这项法律协议确保您的贡献得到适当的许可，从而允许该项目继续在 AGPL-3.0 许可下分发。

提交 pull request 后，CLA 机器人将指导您完成签名过程。要签署 CLA，只需在您的 PR 中添加一条评论，说明：

添加新函数或类时，请包含 Google 风格的文档字符串 为了清晰、标准化的文档，始终将输入和输出都括起来 types 在括号中（例如， (bool), (np.ndarray)）。

此示例说明了标准的 Google 风格文档字符串格式。请注意它是如何清晰地分隔函数描述、参数、返回值和示例，以实现最大的可读性。

此示例演示了如何记录命名返回值变量。使用命名返回值可以使您的代码更具自文档性且更易于理解，尤其是在处理复杂函数时。

此示例展示了如何记录返回多个值的函数。每个返回值都应单独记录，并提供自己的类型和描述，以确保清晰。

注意：即使 python 将多个值作为元组返回（例如， return masks, scores），为了清晰起见和更好的工具集成，请务必单独记录每个值。当记录返回多个值的函数时：

❌ 错误 - 不要记录为带有嵌套元素的元组：

此示例将 Google 风格的文档字符串与 Python 类型提示相结合。使用类型提示时，您可以省略文档字符串参数部分中的类型信息，因为它已在函数签名中指定。

对于较小或更简单的函数，单行文档字符串可能就足够了。这些应该是简洁但完整的句子，以大写字母开头，句点结尾。

所有拉取请求在合并之前都必须通过 GitHub Actions持续集成 (CI) 测试。这些测试包括代码检查、单元测试和其他检查，以确保您的更改符合项目的质量标准。审查 CI 输出并解决出现的任何问题。

向 Ultralytics 项目贡献代码时，请记住以下最佳实践：

审查拉取请求是另一种有价值的贡献方式。审查 PR 时：

我们高度重视错误报告，因为它们有助于我们提高项目的质量和可靠性。通过 GitHub Issues 报告错误时：

Ultralytics 对其存储库使用 GNU Affero General Public License v3.0 (AGPL-3.0)。该许可证促进软件开发的开放性、透明性和协作改进。它确保所有用户都可以自由使用、修改和共享软件，从而培养强大的协作和创新社区。

我们鼓励所有贡献者熟悉 AGPL-3.0 许可协议的条款，以便有效且合乎道德地为 Ultralytics 开源社区做出贡献。

在您的项目中使用 Ultralytics YOLO 模型或代码？AGPL-3.0 许可要求您的整个衍生作品也必须在 AGPL-3.0 下开源。这确保了建立在开源基础上的修改和更大的项目保持开放。

如果您不想开源您的项目，请考虑获取企业许可证。

遵守意味着根据 AGPL-3.0 许可公开提供您项目的完整对应源代码。

有关实际示例结构，请参阅Ultralytics 模板存储库：

通过遵循这些准则，您可以确保符合 AGPL-3.0，从而支持开源生态系统，该生态系统支持像 Ultralytics YOLO 这样的强大工具。

感谢您对Ultralytics开源 YOLO 项目的贡献兴趣。您的参与对于塑造我们软件的未来以及构建一个充满活力、创新协作的社区至关重要。无论是改进代码、报告错误还是提出新功能建议，您的贡献都弥足珍贵。

我们很高兴看到您的想法变为现实，并感谢您致力于推动目标 detect技术的发展。让我们共同在这个激动人心的开源之旅中不断成长和创新。

为 Ultralytics YOLO 开源存储库做贡献可以改进软件，使其对整个社区来说更加强大和功能丰富。贡献可以包括代码增强、错误修复、文档改进和新功能实现。此外，贡献使您能够与该领域的其他熟练开发人员和专家合作，从而提高您自己的技能和声誉。有关如何开始的详细信息，请参阅通过 Pull Requests 贡献部分。

要签署贡献者许可协议 (CLA)，请在提交 pull request 后按照 CLA 机器人提供的说明进行操作。此过程确保您的贡献已根据 AGPL-3.0 许可正确获得许可，从而维护开源项目的法律完整性。在您的 pull request 中添加一条评论，说明：

Google 风格的文档字符串为函数和类提供了清晰、简洁的文档，提高了代码的可读性和可维护性。这些文档字符串使用特定的格式规则概述了函数的目标、参数和返回值。在为 Ultralytics YOLO 做出贡献时，遵循 Google 风格的文档字符串可确保您的添加内容得到充分记录并易于理解。有关示例和指南，请访问 Google 风格的文档字符串 部分。

在您的 pull request 可以被合并之前，它必须通过所有 GitHub Actions 持续集成 (CI) 测试。这些测试包括代码检查、单元测试和其他检查，以确保代码符合项目的质量标准。请查看 CI 输出并修复任何问题。有关 CI 过程和故障排除技巧的详细信息，请参阅 GitHub Actions CI Tests 部分。

要报告错误，请提供清晰简洁的 最小可复现示例 以及您的错误报告。这有助于开发人员快速识别和修复问题。确保您的示例足够小，但足以重现问题。有关报告错误的更多详细步骤，请参阅 报告错误 部分。

如果您在您的项目中使用 Ultralytics YOLO 代码或模型（基于 AGPL-3.0 许可），AGPL-3.0 许可要求您的整个项目（衍生作品）也必须基于 AGPL-3.0 许可，并且其完整的源代码必须公开。这确保了软件的开源性质在其所有衍生版本中都得到保留。如果您无法满足这些要求，则需要获得 企业许可。有关详细信息，请参阅开源您的项目部分。

**Examples:**

Example 1 (unknown):
```unknown
I have read the CLA Document and I sign the CLA
```

Example 2 (python):
```python
def example_function(arg1, arg2=4):
    """Example function demonstrating Google-style docstrings.

    Args:
        arg1 (int): The first argument.
        arg2 (int): The second argument.

    Returns:
        (bool): True if arguments are equal, False otherwise.

    Examples:
        >>> example_function(4, 4)  # True
        >>> example_function(1, 2)  # False
    """
    return arg1 == arg2
```

Example 3 (python):
```python
def example_function(arg1, arg2=4):
    """Example function demonstrating Google-style docstrings.

    Args:
        arg1 (int): The first argument.
        arg2 (int): The second argument.

    Returns:
        equals (bool): True if arguments are equal, False otherwise.

    Examples:
        >>> example_function(4, 4)  # True
    """
    equals = arg1 == arg2
    return equals
```

Example 4 (python):
```python
def example_function(arg1, arg2=4):
    """Example function demonstrating Google-style docstrings.

    Args:
        arg1 (int): The first argument.
        arg2 (int): The second argument.

    Returns:
        equals (bool): True if arguments are equal, False otherwise.
        added (int): Sum of both input arguments.

    Examples:
        >>> equals, added = example_function(2, 2)  # True, 4
    """
    equals = arg1 == arg2
    added = arg1 + arg2
    return equals, added
```

---

## Ultralytics python 包的数据收集 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/help/privacy/

**Contents:**
- Ultralytics python 包的数据收集
- 概述
- 匿名 Google Analytics（分析）
  - 我们收集的内容
  - 我们如何使用这些数据
  - 隐私注意事项
- Sentry 崩溃报告
  - 我们收集的内容
  - 我们如何使用这些数据
  - 隐私注意事项

Ultralytics 致力于持续增强用户体验和我们的 python 包的功能，包括我们开发的先进 YOLO 模型。我们的方法包括收集匿名使用情况统计数据和崩溃报告，帮助我们发现改进机会并确保软件的可靠性。本透明文档概述了我们收集的数据、其目的以及您对此数据收集的选择。

Google Analytics 是 Google 提供的一项 Web 分析服务，用于跟踪和报告网站流量。它允许我们收集有关我们的 python 包如何使用的数据，这对于就设计和功能做出明智的决策至关重要。

有关 Google Analytics（分析）和数据隐私的更多信息，请访问Google Analytics（分析）隐私。

我们采取多项措施来确保您委托给我们的数据的隐私和安全：

Sentry 是一款以开发者为中心的错误跟踪软件，可帮助实时识别、诊断和解决问题，从而确保应用程序的稳健性和可靠性。在我们的软件包中，它通过提供崩溃报告来发挥关键作用，从而极大地促进了我们软件的稳定性和持续改进。

只有在以下情况下，才会激活通过 Sentry 进行的崩溃报告： sentry-sdk 您的系统上预先安装了 python 软件包。此软件包未包含在 ultralytics 先决条件中，并且不会由 Ultralytics 自动安装。

如果 sentry-sdk 您的系统上预先安装了 python 软件包，则崩溃事件可能会发送以下信息：

要了解有关 Sentry 如何处理数据的更多信息，请访问 Sentry 的隐私政策。

通过详细说明用于数据收集的工具，并提供指向其各自隐私页面的 URL 以提供更多背景信息，用户可以全面了解我们的实践，从而强调透明度和对用户隐私的尊重。

我们坚信让用户完全控制自己的数据。默认情况下，我们的软件包配置为收集分析数据和崩溃报告，以帮助改善所有用户的体验。但是，我们尊重某些用户可能希望选择退出此数据收集。

要选择退出发送分析数据和崩溃报告，您可以简单地在您的 YOLO 设置中设置 sync=False 。这可确保不会将任何数据从您的机器传输到我们的分析工具。

要深入了解您设置的当前配置，您可以直接查看它们：

您可以使用 Python 查看您的设置。首先从 settings 模块导入 ultralytics 对象。使用以下命令打印并返回设置：

或者，命令行界面允许您使用一个简单的命令来检查您的设置：

Ultralytics 允许用户轻松修改其设置。可以通过以下方式执行更改：

在 Python 环境中，调用 update 上的 settings 方法来更改您的设置：

如果您喜欢使用命令行界面，以下命令将允许您修改您的设置：

字段 sync=False 设置将阻止任何数据发送到 Google Analytics 或 Sentry。您的设置将在使用 Ultralytics 软件包的所有会话中生效，并保存到磁盘以供将来会话使用。

Ultralytics 非常重视用户隐私。我们在设计数据收集实践时遵循以下原则：

如果您对我们的数据收集行为有任何疑问或疑虑，请通过我们的联系表单或support@ultralytics.com与我们联系。我们致力于确保用户在使用我们的软件包时，对其隐私感到知情和放心。

Ultralytics 通过几项关键措施优先考虑用户隐私。首先，通过 Google Analytics 和 Sentry 收集的所有数据都经过匿名化处理，以确保不会收集任何个人身份信息 (PII)。其次，数据以汇总形式进行分析，使我们能够在不识别个人用户活动的情况下观察模式。最后，我们不收集任何训练或推理图像，从而进一步保护用户数据。这些措施符合我们对透明度和隐私的承诺。有关更多详细信息，请访问我们的隐私注意事项部分。

Ultralytics 使用 Google Analytics 收集三种主要类型的数据：

这些数据有助于我们提升用户体验并优化软件性能。请在匿名 Google Analytics部分了解更多信息。

要选择退出数据收集，您可以简单地设置 sync=False 在您的 YOLO 设置中。此操作会停止传输任何分析或崩溃报告。您可以使用 python 或 CLI 方法禁用数据收集：

有关修改设置的更多详细信息，请参阅修改设置部分。

如果 sentry-sdk 软件包已预先安装，每当发生崩溃事件时，Sentry 都会收集详细的崩溃日志和错误消息。此数据有助于我们及时诊断和解决问题，从而提高 YOLO python 软件包的稳健性和可靠性。收集的崩溃日志会清除任何个人身份信息，以保护用户隐私。有关更多信息，请查看 Sentry 崩溃报告 部分。

是的，您可以轻松查看当前的设置，以了解数据收集首选项的配置。使用以下方法检查这些设置：

**Examples:**

Example 1 (python):
```python
from ultralytics import settings

# View all settings
print(settings)

# Return analytics and crash reporting setting
value = settings["sync"]
```

Example 2 (unknown):
```unknown
yolo settings
```

Example 3 (python):
```python
from ultralytics import settings

# Disable analytics and crash reporting
settings.update({"sync": False})

# Reset settings to default values
settings.reset()
```

Example 4 (markdown):
```markdown
# Disable analytics and crash reporting
yolo settings sync=False

# Reset settings to default values
yolo settings reset
```

---

## Reference for hub_sdk/hub_client.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/hub/sdk/reference/hub_client/

**Contents:**
- Reference for hub_sdk/hub_client.py
- class hub_sdk.hub_client.HUBClient
  - method hub_sdk.hub_client.HUBClient.dataset
  - method hub_sdk.hub_client.HUBClient.dataset_list
  - method hub_sdk.hub_client.HUBClient.login
  - method hub_sdk.hub_client.HUBClient.model
  - method hub_sdk.hub_client.HUBClient.model_list
  - method hub_sdk.hub_client.HUBClient.project
  - method hub_sdk.hub_client.HUBClient.project_list
  - method hub_sdk.hub_client.HUBClient.team

This page is sourced from https://github.com/ultralytics/hub-sdk/blob/main/hub_sdk/hub_client.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A client class for interacting with a HUB service, extending authentication capabilities.

Return an instance of the Datasets class for interacting with datasets.

Return a DatasetList instance for interacting with a list of datasets.

Log in the client using provided authentication credentials.

Return an instance of the Models class for interacting with models.

Return a ModelList instance for interacting with a list of models.

Return an instance of the Projects class for interacting with Projects.

Return a ProjectList instance for interacting with a list of projects.

Returns an instance of the Teams class for interacting with teams.

Fetches a list of team members with optional pagination.

Return an instance of the Users class for interacting with Projects.

Ensure that the wrapped method can only be executed if the client is authenticated.

**Examples:**

Example 1 (rust):
```rust
HUBClient(self, credentials: dict | None = None)
```

Example 2 (python):
```python
class HUBClient(Auth):
    """A client class for interacting with a HUB service, extending authentication capabilities.

    Attributes:
        authenticated (bool): Indicates whether the client is authenticated.
        api_key (str): The API key for authentication.
        id_token (str): The identity token for authentication.
    """

    def __init__(self, credentials: dict | None = None):
        """Initialize the HUBClient instance.

        Args:
            credentials (Dict, optional): A dictionary containing authentication credentials. If None, the client will
                attempt to retrieve the API key from the environment variable "HUB_API_KEY".
        """
        super().__init__()
        self.authenticated = False
        if not credentials:
            self.api_key = os.environ.get("HUB_API_KEY")  # Safely retrieve the API key from an environment variable.
            credentials = {"api_key": self.api_key}

        self.login(**credentials)
```

Example 3 (python):
```python
def dataset(self, dataset_id: str | None = None) -> Datasets
```

Example 4 (python):
```python
@require_authentication
def dataset(self, dataset_id: str | None = None) -> Datasets:
    """Return an instance of the Datasets class for interacting with datasets.

    Args:
        dataset_id (str, optional): The identifier of the dataset. If provided, returns an instance associated with
            the specified dataset_id.

    Returns:
        (Datasets): An instance of the Datasets class.
    """
    return Datasets(dataset_id, self.get_auth_header())
```

---

## Reference for hub_sdk/modules/datasets.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/hub/sdk/reference/modules/datasets/

**Contents:**
- Reference for hub_sdk/modules/datasets.py
- class hub_sdk.modules.datasets.Datasets
  - method hub_sdk.modules.datasets.Datasets.create_dataset
  - method hub_sdk.modules.datasets.Datasets.delete
  - method hub_sdk.modules.datasets.Datasets.get_data
  - method hub_sdk.modules.datasets.Datasets.get_download_link
  - method hub_sdk.modules.datasets.Datasets.update
  - method hub_sdk.modules.datasets.Datasets.upload_dataset
- class hub_sdk.modules.datasets.DatasetList

This page is sourced from https://github.com/ultralytics/hub-sdk/blob/main/hub_sdk/modules/datasets.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class representing a client for interacting with Datasets through CRUD operations.

This class extends the CRUDClient class and provides specific methods for working with Datasets.

The 'id' attribute is set during initialization and can be used to uniquely identify a dataset. The 'data' attribute is used to store dataset data fetched from the API.

Create a new dataset with the provided data and set the dataset ID for the current instance.

Delete the dataset resource represented by this instance.

The 'hard' parameter determines whether to perform a soft delete (default) or a hard delete. In a soft delete, the dataset might be marked as deleted but retained in the system. In a hard delete, the dataset is permanently removed from the system.

Retrieve data for the current dataset instance.

If a valid dataset ID has been set, it sends a request to fetch the dataset data and stores it in the instance. If no dataset ID has been set, it logs an error message.

Get dataset download link.

Update the dataset resource represented by this instance.

Upload a dataset file to the hub.

A class for managing a paginated list of datasets from the Ultralytics Hub API.

**Examples:**

Example 1 (rust):
```rust
Datasets(self, dataset_id: str | None = None, headers: dict[str, Any] | None = None)
```

Example 2 (python):
```python
class Datasets(CRUDClient):
    """A class representing a client for interacting with Datasets through CRUD operations.

    This class extends the CRUDClient class and provides specific methods for working with Datasets.

    Attributes:
        hub_client (DatasetUpload): An instance of DatasetUpload used for interacting with dataset uploads.
        id (str | None): The unique identifier of the dataset, if available.
        data (Dict): A dictionary to store dataset data.

    Notes:
        The 'id' attribute is set during initialization and can be used to uniquely identify a dataset.
        The 'data' attribute is used to store dataset data fetched from the API.
    """

    def __init__(self, dataset_id: str | None = None, headers: dict[str, Any] | None = None):
        """Initialize a Datasets client.

        Args:
            dataset_id (str, optional): Unique id of the dataset.
            headers (Dict, optional): Headers to include in HTTP requests.
        """
        super().__init__("datasets", "dataset", headers)
        self.hub_client = DatasetUpload(headers)
        self.id = dataset_id
        self.data = {}
        if dataset_id:
            self.get_data()
```

Example 3 (python):
```python
def create_dataset(self, dataset_data: dict) -> None
```

Example 4 (python):
```python
def create_dataset(self, dataset_data: dict) -> None:
    """Create a new dataset with the provided data and set the dataset ID for the current instance.

    Args:
        dataset_data (Dict): A dictionary containing the data for creating the dataset.
    """
    resp = super().create(dataset_data).json()
    self.id = resp.get("data", {}).get("id")
    self.get_data()
```

---

## Reference for ultralytics/data/split_dota.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/data/split_dota/

**Contents:**
- Reference for ultralytics/data/split_dota.py
- function ultralytics.data.split_dota.bbox_iof
- function ultralytics.data.split_dota.load_yolo_dota
- function ultralytics.data.split_dota.get_windows
- function ultralytics.data.split_dota.get_window_obj
- function ultralytics.data.split_dota.crop_and_save
- function ultralytics.data.split_dota.split_images_and_labels
- function ultralytics.data.split_dota.split_trainval
- function ultralytics.data.split_dota.split_test

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/data/split_dota.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Calculate Intersection over Foreground (IoF) between polygons and bounding boxes.

Polygon format: [x1, y1, x2, y2, x3, y3, x4, y4]. Bounding box format: [x_min, y_min, x_max, y_max].

Load DOTA dataset annotations and image information.

The directory structure assumed for the DOTA dataset: - data_root - images - train - val - labels - train - val

Get the coordinates of sliding windows for image cropping.

Get objects for each window based on IoF threshold.

Crop images and save new labels for each window.

The directory structure assumed for the DOTA dataset: - data_root - images - train - val - labels - train - val

Split both images and labels for a given dataset split.

The directory structure assumed for the DOTA dataset: - data_root - images - split - labels - split and the output directory structure is: - save_dir - images - split - labels - split

Split train and val sets of DOTA dataset with multiple scaling rates.

The directory structure assumed for the DOTA dataset: - data_root - images - train - val - labels - train - val and the output directory structure is: - save_dir - images - train - val - labels - train - val

Split test set of DOTA dataset, labels are not included within this set.

The directory structure assumed for the DOTA dataset: - data_root - images - test and the output directory structure is: - save_dir - images - test

**Examples:**

Example 1 (typescript):
```typescript
def bbox_iof(polygon1: np.ndarray, bbox2: np.ndarray, eps: float = 1e-6) -> np.ndarray
```

Example 2 (python):
```python
def bbox_iof(polygon1: np.ndarray, bbox2: np.ndarray, eps: float = 1e-6) -> np.ndarray:
    """Calculate Intersection over Foreground (IoF) between polygons and bounding boxes.

    Args:
        polygon1 (np.ndarray): Polygon coordinates with shape (N, 8).
        bbox2 (np.ndarray): Bounding boxes with shape (N, 4).
        eps (float, optional): Small value to prevent division by zero.

    Returns:
        (np.ndarray): IoF scores with shape (N, 1) or (N, M) if bbox2 is (M, 4).

    Notes:
        Polygon format: [x1, y1, x2, y2, x3, y3, x4, y4].
        Bounding box format: [x_min, y_min, x_max, y_max].
    """
    check_requirements("shapely>=2.0.0")
    from shapely.geometry import Polygon

    polygon1 = polygon1.reshape(-1, 4, 2)
    lt_point = np.min(polygon1, axis=-2)  # left-top
    rb_point = np.max(polygon1, axis=-2)  # right-bottom
    bbox1 = np.concatenate([lt_point, rb_point], axis=-1)

    lt = np.maximum(bbox1[:, None, :2], bbox2[..., :2])
    rb = np.minimum(bbox1[:, None, 2:], bbox2[..., 2:])
    wh = np.clip(rb - lt, 0, np.inf)
    h_overlaps = wh[..., 0] * wh[..., 1]

    left, top, right, bottom = (bbox2[..., i] for i in range(4))
    polygon2 = np.stack([left, top, right, top, right, bottom, left, bottom], axis=-1).reshape(-1, 4, 2)

    sg_polys1 = [Polygon(p) for p in polygon1]
    sg_polys2 = [Polygon(p) for p in polygon2]
    overlaps = np.zeros(h_overlaps.shape)
    for p in zip(*np.nonzero(h_overlaps)):
        overlaps[p] = sg_polys1[p[0]].intersection(sg_polys2[p[-1]]).area
    unions = np.array([p.area for p in sg_polys1], dtype=np.float32)
    unions = unions[..., None]

    unions = np.clip(unions, eps, np.inf)
    outputs = overlaps / unions
    if outputs.ndim == 1:
        outputs = outputs[..., None]
    return outputs
```

Example 3 (typescript):
```typescript
def load_yolo_dota(data_root: str, split: str = "train") -> list[dict[str, Any]]
```

Example 4 (typescript):
```typescript
def load_yolo_dota(data_root: str, split: str = "train") -> list[dict[str, Any]]:
    """Load DOTA dataset annotations and image information.

    Args:
        data_root (str): Data root directory.
        split (str, optional): The split data set, could be 'train' or 'val'.

    Returns:
        (list[dict[str, Any]]): List of annotation dictionaries containing image information.

    Notes:
        The directory structure assumed for the DOTA dataset:
            - data_root
                - images
                    - train
                    - val
                - labels
                    - train
                    - val
    """
    assert split in {"train", "val"}, f"Split must be 'train' or 'val', not {split}."
    im_dir = Path(data_root) / "images" / split
    assert im_dir.exists(), f"Can't find {im_dir}, please check your data root."
    im_files = glob(str(Path(data_root) / "images" / split / "*"))
    lb_files = img2label_paths(im_files)
    annos = []
    for im_file, lb_file in zip(im_files, lb_files):
        w, h = exif_size(Image.open(im_file))
        with open(lb_file, encoding="utf-8") as f:
            lb = [x.split() for x in f.read().strip().splitlines() if len(x)]
            lb = np.array(lb, dtype=np.float32)
        annos.append(dict(ori_size=(h, w), label=lb, filepath=im_file))
    return annos
```

---

## Ultralytics Explorer - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/datasets/explorer/

**Contents:**
- Ultralytics Explorer
- 安装可选依赖项
- Explorer API
- GUI 浏览器使用方法
- 常见问题
  - 什么是 Ultralytics Explorer？它如何帮助处理 CV 数据集？
  - 如何安装 Ultralytics Explorer 的依赖项？
  - 如何使用 Ultralytics Explorer 的 GUI 版本？
  - Ultralytics Explorer 中的 Ask AI 功能是什么？
  - 是否可以在 Google Colab 中运行 Ultralytics Explorer？

截至 ultralytics>=8.3.10，Ultralytics Explorer 支持已弃用。类似（且已扩展）的数据集探索功能可在 Ultralytics HUB.

Ultralytics Explorer 是一款用于探索 CV 数据集的工具，它支持语义搜索、SQL 查询、向量相似性搜索和自然语言提示。它还提供 Python API 以访问相同的功能。

观看： Ultralytics Explorer API | 语义搜索、SQL 查询和 Ask AI 功能

Explorer的某些功能依赖于外部库。当您使用Explorer时，这些库会自动安装。要手动安装这些依赖项，请使用以下命令：

Explorer支持嵌入/语义搜索和SQL查询，并由LanceDB无服务器向量数据库提供支持。与传统的内存数据库不同，它将数据持久化到磁盘而不会牺牲性能，因此您可以在本地扩展到像COCO这样的大型数据集而不会耗尽内存。

这是一个用于探索数据集的 python API。它还为 GUI Explorer 提供支持。您可以使用它来创建自己的探索性笔记本或脚本，以深入了解您的数据集。

在 Explorer API 文档中探索完整的功能和使用示例。

GUI 演示在您的浏览器中运行，允许您为数据集创建 embeddings 并搜索相似图像、运行 SQL 查询并执行语义搜索。它可以使用以下命令运行：

Ask AI 功能使用 OpenAI，因此当您首次运行 GUI 时，系统会提示您设置 OpenAI 的 API 密钥。 您可以这样设置 - yolo settings openai_api_key="..."

Ultralytics Explorer 是一款强大的工具，旨在通过语义搜索、SQL 查询、向量相似性搜索，甚至是自然语言来探索计算机视觉 (CV) 数据集。这款多功能工具同时提供 GUI 和 Python API，使用户能够无缝地与其数据集进行交互。通过利用 LanceDB 等技术，Ultralytics Explorer 确保能够高效、可扩展地访问大型数据集，而不会过度消耗内存。无论您是执行详细的数据集分析还是探索数据模式，Ultralytics Explorer 都能简化整个流程。

了解更多关于 Explorer API 的信息。

要手动安装 Ultralytics Explorer 所需的可选依赖项，您可以使用以下命令 pip 命令：

这些依赖项对于语义搜索和 SQL 查询的完整功能至关重要。通过包含由 LanceDB 提供支持的库，安装确保数据库操作保持高效和可扩展，即使对于像 COCO 这样的大型数据集也是如此。

使用 Ultralytics Explorer 的 GUI 版本非常简单。 安装必要的依赖项后，您可以使用以下命令启动 GUI：

GUI 提供了一个用户友好的界面，用于创建数据集嵌入、搜索相似图像、运行 SQL 查询和执行语义搜索。此外，与 OpenAI 的 Ask AI 功能集成使您可以使用自然语言查询数据集，从而增强了灵活性和易用性。

有关存储和可扩展性信息，请查看我们的安装说明。

Ultralytics Explorer 中的 Ask AI 功能允许用户使用自然语言查询与其数据集进行交互。该功能由 OpenAI 提供支持，使您无需编写 SQL 查询或类似命令即可提出复杂的问题并获得有见地的答案。要使用此功能，您需要在首次运行 GUI 时设置您的 OpenAI API 密钥：

有关此功能以及如何集成它的更多信息，请参见我们的GUI Explorer 使用部分。

是的，Ultralytics Explorer 可以在 Google Colab 中运行，从而为数据集探索提供了一个方便而强大的环境。您可以首先打开提供的 Colab 笔记本，该笔记本已预先配置了所有必要的设置：

此设置允许您充分探索数据集，从而利用Google的云资源。请在我们的Google Colab指南中了解更多信息。

**Examples:**

Example 1 (unknown):
```unknown
pip install ultralytics[explorer]
```

Example 2 (unknown):
```unknown
yolo explorer
```

Example 3 (unknown):
```unknown
pip install ultralytics[explorer]
```

Example 4 (unknown):
```unknown
yolo explorer
```

---

## 使用 Ultralytics HUB-SDK 进行数据集管理 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/hub/sdk/dataset/

**Contents:**
- 使用 Ultralytics HUB-SDK 进行数据集管理
- 通过 ID 获取数据集
- 创建数据集
- 更新数据集
- 删除数据集
- 列出数据集
- 从存储获取 URL
- 上传数据集
- 评论

欢迎阅读 Ultralytics HUB-SDK 数据集管理文档！👋

高效的数据集管理在机器学习中至关重要。无论您是经验丰富的数据科学家还是初学者，了解如何处理数据集操作都可以简化您的工作流程。本页介绍了如何使用Ultralytics HUB-SDK在python中对数据集执行操作的基础知识。提供的示例说明了如何获取、创建、更新、删除和列出数据集，以及如何获取数据集访问的URL和上传数据集。

要使用唯一 ID 快速获取特定数据集，请使用以下代码片段。这使您可以访问重要信息，包括其数据。

有关更多详细信息，请参见 Datasets 类及其方法，请参见 参考 hub_sdk/modules/datasets.py.

要创建新的数据集，请为您的数据集定义一个友好的名称并使用 create_dataset 方法，如下所示：

请参阅 create_dataset API 参考中的 方法，了解更多信息。

随着项目的演变，您可能需要修改数据集的元数据。这就像使用新详细信息运行以下代码一样简单：

字段 update 方法提供了有关更新数据集的更多详细信息。

要删除数据集，无论是为了整理您的工作区还是因为它不再需要，您可以通过调用 delete 方法：

有关删除选项（包括硬删除）的更多信息，请参阅 delete 方法文档。

要浏览您的数据集，请使用分页列出所有数据集。这在处理大量数据集时非常有用。

字段 DatasetList 类提供了有关列出数据集和对数据集进行分页的更多详细信息。

此函数获取用于数据集存储访问的 URL，从而可以轻松下载远程存储的数据集文件或工件。

字段 get_download_link 方法文档提供了更多详细信息。

上传数据集非常简单。设置数据集的 ID 和文件路径，然后使用 upload_dataset 函数：

字段 upload_dataset 方法提供了有关上传数据集的更多详细信息。您还可以了解相关的 DatasetUpload 类。

请记住仔细检查您的数据集 ID 和文件路径，以确保一切顺利运行。

如果您遇到任何问题或有疑问，我们的支持团队随时为您提供帮助。🤝

祝您数据处理顺利，并祝您的模型准确而富有洞察力！🌟

**Examples:**

Example 1 (python):
```python
from hub_sdk import HUBClient

credentials = {"api_key": "<YOUR-API-KEY>"}
client = HUBClient(credentials)

# Fetch a dataset by ID
dataset = client.dataset("<Dataset ID>")  # Replace with your actual Dataset ID
print(dataset.data)  # This prints the dataset information
```

Example 2 (python):
```python
from hub_sdk import HUBClient

credentials = {"api_key": "<YOUR-API-KEY>"}
client = HUBClient(credentials)

# Define your dataset properties
data = {"meta": {"name": "My Dataset"}}  # Replace 'My Dataset' with your desired dataset name

# Create the dataset
dataset = client.dataset()
dataset.create_dataset(data)
print("Dataset created successfully!")
```

Example 3 (sql):
```sql
from hub_sdk import HUBClient

credentials = {"api_key": "<YOUR-API-KEY>"}
client = HUBClient(credentials)

# Obtain the dataset
dataset = client.dataset("<Dataset ID>")  # Insert the correct Dataset ID

# Update the dataset's metadata
dataset.update({"meta": {"name": "Updated Name"}})  # Modify 'Updated Name' as required
print("Dataset updated with new information.")
```

Example 4 (sql):
```sql
from hub_sdk import HUBClient

credentials = {"api_key": "<YOUR-API-KEY>"}
client = HUBClient(credentials)

# Select the dataset by its ID
dataset = client.dataset("<Dataset ID>")  # Ensure the Dataset ID is specified

# Delete the dataset
dataset.delete()
print("Dataset has been deleted.")
```

---

## Reference for ultralytics/data/utils.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/data/utils/

**Contents:**
- Reference for ultralytics/data/utils.py
- class ultralytics.data.utils.HUBDatasetStats
  - method ultralytics.data.utils.HUBDatasetStats._hub_ops
  - method ultralytics.data.utils.HUBDatasetStats._unzip
  - method ultralytics.data.utils.HUBDatasetStats.get_json
  - method ultralytics.data.utils.HUBDatasetStats.process_images
- function ultralytics.data.utils.img2label_paths
- function ultralytics.data.utils.check_file_speeds
- function ultralytics.data.utils.get_hash
- function ultralytics.data.utils.exif_size

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/data/utils.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class for generating HUB dataset JSON and -hub dataset directory.

Download *.zip files from https://github.com/ultralytics/hub/tree/main/example_datasets i.e. https://github.com/ultralytics/hub/raw/main/example_datasets/coco8.zip for coco8.zip.

Save a compressed image for HUB previews.

Return dataset JSON for Ultralytics HUB.

Compress images for Ultralytics HUB.

Convert image paths to label paths by replacing 'images' with 'labels' and extension with '.txt'.

Check dataset file access speed and provide performance feedback.

This function tests the access speed of dataset files by measuring ping (stat call) time and read speed. It samples up to 5 files from the provided list and warns if access times exceed the threshold.

Return a single hash value of a list of paths (files or dirs).

Return exif-corrected PIL size.

Verify one image-label pair.

Visualize YOLO annotations (bounding boxes and class labels) on an image.

This function reads an image and its corresponding annotation file in YOLO format, then draws bounding boxes around detected objects and labels them with their respective class names. The bounding box colors are assigned based on the class ID, and the text color is dynamically adjusted for readability, depending on the background color's luminance.

Convert a list of polygons to a binary mask of the specified image size.

Convert a list of polygons to a set of binary masks of the specified image size.

Return a (640, 640) overlap mask.

Find and return the YAML file associated with a Detect, Segment or Pose dataset.

This function searches for a YAML file at the root level of the provided directory first, and if not found, it performs a recursive search. It prefers YAML files that have the same stem as the provided path.

Download, verify, and/or unzip a dataset if not found locally.

This function checks the availability of a specified dataset, and if not found, it has the option to download and unzip the dataset. It then reads and parses the accompanying YAML data, ensuring key requirements are met and also resolves paths related to the dataset.

Check a classification dataset such as Imagenet.

This function accepts a dataset name and attempts to retrieve the corresponding dataset information. If the dataset is not found locally, it attempts to download the dataset from the internet and save it locally.

Compress a single image file to reduced size while preserving its aspect ratio and quality using either the

Python Imaging Library (PIL) or OpenCV library. If the input image is smaller than the maximum dimension, it will not be resized.

Load an Ultralytics *.cache dictionary from path.

Save an Ultralytics dataset *.cache dictionary x to path.

**Examples:**

Example 1 (typescript):
```typescript
HUBDatasetStats(self, path: str = "coco8.yaml", task: str = "detect", autodownload: bool = False)
```

Example 2 (sql):
```sql
>>> from ultralytics.data.utils import HUBDatasetStats
>>> stats = HUBDatasetStats("path/to/coco8.zip", task="detect")  # detect dataset
>>> stats = HUBDatasetStats("path/to/coco8-seg.zip", task="segment")  # segment dataset
>>> stats = HUBDatasetStats("path/to/coco8-pose.zip", task="pose")  # pose dataset
>>> stats = HUBDatasetStats("path/to/dota8.zip", task="obb")  # OBB dataset
>>> stats = HUBDatasetStats("path/to/imagenet10.zip", task="classify")  # classification dataset
>>> stats.get_json(save=True)
>>> stats.process_images()
```

Example 3 (python):
```python
class HUBDatasetStats:
    """A class for generating HUB dataset JSON and `-hub` dataset directory.

    Args:
        path (str): Path to data.yaml or data.zip (with data.yaml inside data.zip).
        task (str): Dataset task. Options are 'detect', 'segment', 'pose', 'classify'.
        autodownload (bool): Attempt to download dataset if not found locally.

    Attributes:
        task (str): Dataset task type.
        hub_dir (Path): Directory path for HUB dataset files.
        im_dir (Path): Directory path for compressed images.
        stats (dict): Statistics dictionary containing dataset information.
        data (dict): Dataset configuration data.

    Methods:
        get_json: Return dataset JSON for Ultralytics HUB.
        process_images: Compress images for Ultralytics HUB.

    Examples:
        >>> from ultralytics.data.utils import HUBDatasetStats
        >>> stats = HUBDatasetStats("path/to/coco8.zip", task="detect")  # detect dataset
        >>> stats = HUBDatasetStats("path/to/coco8-seg.zip", task="segment")  # segment dataset
        >>> stats = HUBDatasetStats("path/to/coco8-pose.zip", task="pose")  # pose dataset
        >>> stats = HUBDatasetStats("path/to/dota8.zip", task="obb")  # OBB dataset
        >>> stats = HUBDatasetStats("path/to/imagenet10.zip", task="classify")  # classification dataset
        >>> stats.get_json(save=True)
        >>> stats.process_images()

    Notes:
        Download *.zip files from https://github.com/ultralytics/hub/tree/main/example_datasets
        i.e. https://github.com/ultralytics/hub/raw/main/example_datasets/coco8.zip for coco8.zip.
    """

    def __init__(self, path: str = "coco8.yaml", task: str = "detect", autodownload: bool = False):
        """Initialize class."""
        path = Path(path).resolve()
        LOGGER.info(f"Starting HUB dataset checks for {path}....")

        self.task = task  # detect, segment, pose, classify, obb
        if self.task == "classify":
            unzip_dir = unzip_file(path)
            data = check_cls_dataset(unzip_dir)
            data["path"] = unzip_dir
        else:  # detect, segment, pose, obb
            _, data_dir, yaml_path = self._unzip(Path(path))
            try:
                # Load YAML with checks
                data = YAML.load(yaml_path)
                data["path"] = ""  # strip path since YAML should be in dataset root for all HUB datasets
                YAML.save(yaml_path, data)
                data = check_det_dataset(yaml_path, autodownload)  # dict
                data["path"] = data_dir  # YAML path should be set to '' (relative) or parent (absolute)
            except Exception as e:
                raise Exception("error/HUB/dataset_stats/init") from e

        self.hub_dir = Path(f"{data['path']}-hub")
        self.im_dir = self.hub_dir / "images"
        self.stats = {"nc": len(data["names"]), "names": list(data["names"].values())}  # statistics dictionary
        self.data = data
```

Example 4 (python):
```python
def _hub_ops(self, f: str)
```

---

## Explorer GUI - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/datasets/explorer/dashboard/

**Contents:**
- Explorer GUI
  - 安装
- 向量语义相似度搜索
- 询问 AI
- 在您的 CV 数据集上运行 SQL 查询
- 常见问题
  - 什么是 Ultralytics Explorer GUI？如何安装它？
  - Ultralytics Explorer GUI 中的语义搜索功能是如何工作的？
  - 我可以使用自然语言在Ultralytics Explorer GUI中过滤数据集吗？
  - 如何使用 Ultralytics Explorer GUI 在数据集上运行 SQL 查询？

截至 ultralytics>=8.3.10，Ultralytics Explorer 支持已弃用。类似（且已扩展）的数据集探索功能可在 Ultralytics HUB.

Explorer GUI基于Ultralytics Explorer API构建。它允许您运行语义/向量相似性搜索、SQL查询，以及使用由LLM驱动的“询问AI”功能进行自然语言查询。

观看： Ultralytics Explorer 仪表板概述

Ask AI 功能使用 OpenAI，因此当您首次运行 GUI 时，系统会提示您设置 OpenAI API 密钥。 使用以下方式设置： yolo settings openai_api_key="...".

语义搜索 是一种查找与给定图像相似的图像的技术。它基于相似的图像将具有相似的 嵌入 的思想。在 UI 中，您可以选择一个或多个图像并搜索与其相似的图像。当您想要查找与给定图像或一组未按预期执行的图像相似的图像时，这非常有用。

例如，在此 VOC 探索仪表板中，用户选择了几张飞机图像：

运行相似性搜索后，您应该会看到类似的结果：

此功能允许您使用自然语言过滤数据集，无需编写SQL。AI驱动的查询生成器将您的提示转换为查询并返回匹配结果。例如，您可以询问：“给我展示100张恰好包含一个人和2只狗的图像。也可以有其他对象”，它将生成查询并显示这些结果。以下是当被问及：“显示10张恰好包含5个人的图像”时的示例输出：

注意：此功能使用大型语言模型，因此结果具有概率性，可能不准确。

您可以在数据集上运行SQL查询来筛选它。如果您只提供WHERE子句，它也同样有效。例如，以下WHERE子句返回包含至少一个人和一只狗的图像：

此演示是使用 Explorer API 构建的，您可以使用它来创建自己的探索性笔记本或脚本，以深入了解您的数据集。要开始使用，请查看Explorer API 文档。

Ultralytics Explorer GUI 是一个强大的界面，可以使用 Ultralytics Explorer API 释放高级数据探索功能。它允许您使用由大型语言模型 (LLM) 提供支持的 Ask AI 功能运行语义/向量相似性搜索、SQL 查询和自然语言查询。

要安装 Explorer GUI，您可以使用 pip：

注意：要使用 Ask AI 功能，你需要设置 OpenAI API 密钥： yolo settings openai_api_key="...".

Ultralytics Explorer GUI 中的语义搜索功能允许您根据图像的嵌入查找与给定图像相似的图像。此技术对于识别和探索共享视觉相似性的图像非常有用。要使用此功能，请在 UI 中选择一个或多个图像，然后执行搜索以查找相似的图像。结果将显示与所选图像非常相似的图像，从而有助于高效的数据集探索和异常检测。

访问功能概述部分，了解更多关于语义搜索和其他功能的信息。

是的，借助由大型语言模型 (LLM) 提供支持的 Ask AI 功能，您可以使用自然语言查询来过滤数据集。您无需精通 SQL。例如，您可以提问“显示 100 张恰好有一个人和 2 条狗的图像。也可以有其他对象”，AI 将在后台生成适当的查询以提供所需的结果。

Ultralytics Explorer GUI 允许您直接在数据集上运行 SQL 查询，以高效地过滤和管理数据。要运行查询，请导航到 GUI 中的 SQL 查询部分并编写您的查询。例如，要显示至少有一个人和一条狗的图像，您可以使用：

您也可以只提供 WHERE 子句，从而使查询过程更加灵活。

有关更多详细信息，请参阅SQL 查询部分。

Ultralytics Explorer GUI 通过语义搜索、SQL 查询以及通过 Ask AI 功能进行自然语言交互等功能增强了数据探索。这些功能允许用户：

这些功能使其成为开发人员、研究人员和数据科学家深入了解其数据集的多功能工具。

在Explorer GUI 文档中，了解更多关于这些功能的信息。

**Examples:**

Example 1 (unknown):
```unknown
pip install ultralytics[explorer]
```

Example 2 (sql):
```sql
WHERE labels LIKE '%person%' AND labels LIKE '%dog%'
```

Example 3 (unknown):
```unknown
pip install ultralytics[explorer]
```

Example 4 (sql):
```sql
WHERE labels LIKE '%person%' AND labels LIKE '%dog%'
```

---
