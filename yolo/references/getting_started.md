# Yolo - Getting Started

**Pages:** 22

---

## Ultralytics Docker快速入门指南 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/guides/docker-quickstart/

**Contents:**
- Ultralytics Docker 快速入门指南
- 您将学到的内容
- 准备工作
- 使用 NVIDIA 支持设置 Docker
  - 安装 NVIDIA Container Toolkit
  - 使用 Docker 验证 NVIDIA 运行时
- 安装 Ultralytics Docker 镜像
- 在 Docker 容器中运行 Ultralytics
  - 仅使用 CPU
  - 使用 GPU

本指南全面介绍了如何为您的 Ultralytics 项目设置 Docker 环境。Docker 是一个用于在容器中开发、交付和运行应用程序的平台。它特别有利于确保软件始终以相同的方式运行，而无论部署在何处。有关更多详细信息，请访问 Docker Hub 上的 Ultralytics Docker 存储库。

观看： 如何开始使用 Docker | Ultralytics python 包在 Docker 中的使用演示 🎉

首先，通过运行以下命令验证 NVIDIA 驱动程序是否已正确安装：

现在，让我们安装 NVIDIA Container Toolkit 以在 Docker 容器中启用 GPU 支持：

安装最新版本的 nvidia-container-toolkit:

或者，您可以通过设置以下内容来安装特定版本的 nvidia-container-toolkit NVIDIA_CONTAINER_TOOLKIT_VERSION 环境变量：

更新软件包列表并安装 nvidia-container-toolkit 软件包：

或者，您可以通过设置以下内容来安装特定版本的 nvidia-container-toolkit NVIDIA_CONTAINER_TOOLKIT_VERSION 环境变量：

运行 docker info | grep -i runtime 以确保 nvidia 出现在运行时列表中：

Ultralytics 提供了多个针对各种平台和用例优化的 Docker 镜像：

以下是如何执行 Ultralytics Docker 容器：

字段 -it flag 分配一个伪 TTY 并保持 stdin 打开，允许您与容器交互。The --ipc=host flag 允许共享主机的 IPC 命名空间，这对于在进程之间共享内存至关重要。The --gpus flag 允许容器访问主机的 GPU。

要在容器中使用本地计算机上的文件，可以使用 Docker 卷：

替换 /path/on/host 是本地计算机上的目录路径， /path/in/container 是 Docker 容器内的目标路径。

培训成果保存至 /ultralytics/runs/<task>/<name>/ 默认情况下，输出保存在容器内部。若未挂载主机目录，容器移除时输出将丢失。

这将所有训练输出保存到 ./runs 在您的主机上。

以下说明是实验性的。与 Docker 容器共享 X11 socket 存在潜在的安全风险。因此，建议仅在受控环境中测试此解决方案。有关更多信息，请参阅以下关于如何使用的资源 xhost(1)(2).

Docker 主要用于容器化后台应用程序和 CLI 程序，但它也可以运行图形程序。在 Linux 世界中，两个主要的图形服务器处理图形显示：X11（也称为 X Window System）和 Wayland。在开始之前，必须确定您当前正在使用哪个图形服务器。运行以下命令以找出：

X11 或 Wayland 显示服务器的设置和配置不在此指南的范围内。如果上述命令未返回任何内容，那么您需要首先让其中一个为您的系统工作，然后再继续。

如果您使用的是 X11，您可以运行以下命令以允许 Docker 容器访问 X11 socket：

此命令设置 DISPLAY 环境变量为主机的显示，挂载 X11 socket，并将 .Xauthority 文件映射到容器。The xhost +local:docker 命令允许 Docker 容器访问 X11 服务器。

此命令设置 DISPLAY 环境变量为主机的显示，挂载 Wayland socket，并允许 Docker 容器访问 Wayland 服务器。

现在您可以在 Docker 容器中显示图形应用程序。例如，您可以运行以下 CLI 命令 来可视化 YOLO11 模型的预测：

验证 Docker 组是否有权访问 X11 服务器的一个简单方法是运行一个带有 GUI 程序的容器，例如 xclock 或 xeyes或者，您也可以在 Ultralytics Docker 容器中安装这些程序，以测试对 GNU-Linux 显示服务器的 X11 服务器的访问。如果遇到任何问题，请考虑设置环境变量 -e QT_DEBUG_PLUGINS=1设置此环境变量可以启用调试信息的输出，从而帮助进行故障排除。

在这两种情况下，完成后都不要忘记撤销 Docker 组的访问权限。

您现在已设置好使用Docker运行Ultralytics，并准备好利用其功能。有关其他安装方法，请参阅Ultralytics快速入门文档。

要使用 Docker 设置 Ultralytics，首先请确保您的系统上已安装 Docker。如果您有 NVIDIA GPU，请安装 NVIDIA Container Toolkit 以启用 GPU 支持。然后，使用以下命令从 Docker Hub 拉取最新的 Ultralytics Docker 镜像：

有关详细步骤，请参阅我们的 Docker 快速入门指南。

使用 Ultralytics Docker 镜像可确保不同机器上环境的一致性，从而复制相同的软件和依赖项。这对于以下情况尤其有用 跨团队协作，在各种硬件上运行模型，并保持可重复性。对于基于 GPU 的训练，Ultralytics 提供了优化的 Docker 镜像，例如 Dockerfile 用于通用 GPU 使用，以及 Dockerfile-jetson 用于 NVIDIA Jetson 设备。浏览 Ultralytics Docker Hub 了解更多详情。

首先，确保已安装并配置 NVIDIA Container Toolkit。然后，使用以下命令运行支持 GPU 的 Ultralytics YOLO：

此命令设置一个具有 GPU 访问权限的 Docker 容器。有关更多详细信息，请参阅 Docker 快速入门指南。

要在 Docker 容器中使用 GUI 可视化 YOLO 预测结果，您需要允许 Docker 访问您的显示服务器。对于运行 X11 的系统，命令是：

对于运行 Wayland 的系统，请使用：

更多信息可以在在 Docker 容器中运行图形用户界面 (GUI) 应用程序部分找到。

替换 /path/on/host 与本地计算机上的目录，以及 /path/in/container 使用容器内的所需路径。此设置允许您在容器中使用本地文件。有关更多信息，请参阅 关于文件可访问性的说明 部分。

**Examples:**

Example 1 (unknown):
```unknown
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list \
  | sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' \
    | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```

Example 2 (sql):
```sql
sudo apt-get update
```

Example 3 (unknown):
```unknown
sudo apt-get install -y nvidia-container-toolkit \
  nvidia-container-toolkit-base libnvidia-container-tools \
  libnvidia-container1
```

Example 4 (bash):
```bash
export NVIDIA_CONTAINER_TOOLKIT_VERSION=1.17.8-1
sudo apt-get install -y \
  nvidia-container-toolkit=${NVIDIA_CONTAINER_TOOLKIT_VERSION} \
  nvidia-container-toolkit-base=${NVIDIA_CONTAINER_TOOLKIT_VERSION} \
  libnvidia-container-tools=${NVIDIA_CONTAINER_TOOLKIT_VERSION} \
  libnvidia-container1=${NVIDIA_CONTAINER_TOOLKIT_VERSION}
```

---

## 掌握 Google Cloud Platform (GCP) Deep Learning VM 上的 YOLOv5 部署

**URL:** https://docs.ultralytics.com/zh/yolov5/environments/google_cloud_quickstart_tutorial/

**Contents:**
- 掌握 Google Cloud Platform (GCP) Deep Learning VM 上的 YOLOv5 部署
- 步骤 1：创建并配置您的深度学习虚拟机
- 步骤 2：为 YOLOv5 准备虚拟机
- 第三步：训练和部署您的 YOLOv5 模型
- 分配交换空间（可选）
- 训练自定义数据集
- 利用云存储
- 总结
- 评论

当您利用云计算平台的强大功能和灵活性时，踏上人工智能 (AI)和机器学习 (ML)的旅程可能会令人振奋。 Google Cloud Platform (GCP) 提供了专为 ML 爱好者和专业人士量身定制的强大工具。其中一种工具是深度学习 VM，它已针对数据科学和 ML 任务进行了预配置。在本教程中，我们将介绍在 GCP 深度学习 VM 上设置 Ultralytics YOLOv5 的过程。无论您是迈出 ML 的第一步，还是经验丰富的从业者，本指南都提供了一条清晰的途径来实现由 YOLOv5 提供支持的 目标检测 模型。

🆓 此外，如果您是新的 GCP 用户，那么您很幸运能够获得 300 美元的免费信用额度，以启动您的项目。

除了 GCP 之外，还可以探索其他易于访问的 YOLOv5 快速入门选项，例如我们的 Google Colab Notebook 以获得基于浏览器的体验，或 Amazon AWS。此外，容器爱好者可以利用我们在 选择官方 为了获得封装的环境，请参考我们的 Docker 快速入门指南.

让我们首先创建一个针对深度学习优化的虚拟机：

此 VM 预装了必要的工具和框架，包括 Anaconda Python 发行版，它方便地捆绑了 YOLOv5 的许多必要依赖项。

设置好环境后，让我们安装并准备好 YOLOv5：

此设置过程确保您拥有 3.8.0 或更高版本的 python 环境以及 1.8 或更高版本的 PyTorch。我们的脚本会自动从最新的 YOLOv5 release 下载模型和数据集，从而简化模型训练的启动过程。

完成设置后，您就可以在 GCP VM 上使用 YOLOv5 进行训练、验证、预测和导出了：

仅需几条命令，YOLOv5 即可让您训练根据特定需求量身定制的自定义目标检测模型，或利用预训练权重在各种任务中快速获得结果。导出后，探索不同的模型部署选项。

如果您正在处理特别大的数据集，可能会超出 VM 的 RAM，请考虑添加交换空间以防止内存错误：

要在 GCP 中对你的自定义数据集训练 YOLOv5，请按照以下常规步骤操作：

使用您的自定义数据集 yaml 并可能从预训练权重开始，启动训练过程：

有关准备数据和使用自定义数据集进行训练的全面说明，请查阅 Ultralytics YOLOv5 训练文档。

为了实现高效的数据管理，尤其是在处理大型数据集或大量实验时，请将您的 YOLOv5 工作流程与 Google Cloud Storage 集成：

这种方法允许您在云中安全且经济高效地存储大型数据集和训练好的模型，从而最大限度地减少 VM 实例上的存储需求。

恭喜！您现在已准备好利用 Ultralytics YOLOv5 的强大功能以及 Google Cloud Platform 的计算能力。此设置为您的目标检测项目提供可扩展性、效率和多功能性。无论是用于个人探索、学术研究还是构建工业解决方案，您都已朝着云端 AI 和 ML 领域迈出了重要一步。

考虑使用 Ultralytics HUB 来获得简化的、无需代码的训练和管理模型体验。

请记住记录您的进度，与充满活力的 Ultralytics 社区分享见解，并利用 GitHub 讨论 等资源进行协作和支持。现在，开始使用 YOLOv5 和 GCP 进行创新吧！

想要继续提升您的机器学习技能吗？请深入阅读我们的文档，并浏览Ultralytics 博客，获取更多教程和见解。让您的 AI 冒险之旅继续！

**Examples:**

Example 1 (markdown):
```markdown
# Clone the YOLOv5 repository
git clone https://github.com/ultralytics/yolov5
cd yolov5

# Install dependencies
pip install -r requirements.txt
```

Example 2 (julia):
```julia
# Train a YOLOv5 model on your dataset (e.g., yolov5s)
python train.py --data coco128.yaml --weights yolov5s.pt --img 640

# Validate the trained model to check Precision, Recall, and mAP
python val.py --weights yolov5s.pt --data coco128.yaml

# Run inference using the trained model on images or videos
python detect.py --weights yolov5s.pt --source path/to/your/images_or_videos

# Export the trained model to various formats like ONNX, CoreML, TFLite for deployment
python export.py --weights yolov5s.pt --include onnx coreml tflite
```

Example 3 (markdown):
```markdown
# Allocate a 64GB swap file
sudo fallocate -l 64G /swapfile

# Set the correct permissions for the swap file
sudo chmod 600 /swapfile

# Set up the Linux swap area
sudo mkswap /swapfile

# Enable the swap file
sudo swapon /swapfile

# Verify the swap space allocation (should show increased swap memory)
free -h
```

Example 4 (markdown):
```markdown
# Example: Train YOLOv5s on a custom dataset for 100 epochs
python train.py --img 640 --batch 16 --epochs 100 --data custom_dataset.yaml --weights yolov5s.pt
```

---

## Reference for ultralytics/utils/events.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/events/

**Contents:**
- Reference for ultralytics/utils/events.py
- class ultralytics.utils.events.Events
  - method ultralytics.utils.events.Events.__call__
- function ultralytics.utils.events._post

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/events.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Collect and send anonymous usage analytics with rate-limiting.

Event collection and transmission are enabled when sync is enabled in settings, the current process is rank -1 or 0, tests are not running, the environment is online, and the installation source is either pip or the official Ultralytics GitHub repository.

Queue an event and flush the queue asynchronously when the rate limit elapses.

Send a one-shot JSON POST request.

**Examples:**

Example 1 (rust):
```rust
Events(self) -> None
```

Example 2 (python):
```python
class Events:
    """Collect and send anonymous usage analytics with rate-limiting.

    Event collection and transmission are enabled when sync is enabled in settings, the current process is rank -1 or 0,
    tests are not running, the environment is online, and the installation source is either pip or the official
    Ultralytics GitHub repository.

    Attributes:
        url (str): Measurement Protocol endpoint for receiving anonymous events.
        events (list[dict]): In-memory queue of event payloads awaiting transmission.
        rate_limit (float): Minimum time in seconds between POST requests.
        t (float): Timestamp of the last transmission in seconds since the epoch.
        metadata (dict): Static metadata describing runtime, installation source, and environment.
        enabled (bool): Flag indicating whether analytics collection is active.

    Methods:
        __init__: Initialize the event queue, rate limiter, and runtime metadata.
        __call__: Queue an event and trigger a non-blocking send when the rate limit elapses.
    """

    url = "https://www.google-analytics.com/mp/collect?measurement_id=G-X8NCJYTQXM&api_secret=QLQrATrNSwGRFRLE-cbHJw"

    def __init__(self) -> None:
        """Initialize the Events instance with queue, rate limiter, and environment metadata."""
        self.events = []  # pending events
        self.rate_limit = 30.0  # rate limit (seconds)
        self.t = 0.0  # last send timestamp (seconds)
        self.metadata = {
            "cli": Path(ARGV[0]).name == "yolo",
            "install": "git" if GIT.is_repo else "pip" if IS_PIP_PACKAGE else "other",
            "python": PYTHON_VERSION.rsplit(".", 1)[0],  # i.e. 3.13
            "CPU": get_cpu_info(),
            # "GPU": get_gpu_info(index=0) if cuda else None,
            "version": __version__,
            "env": ENVIRONMENT,
            "session_id": round(random.random() * 1e15),
            "engagement_time_msec": 1000,
        }
        self.enabled = (
            SETTINGS["sync"]
            and RANK in {-1, 0}
            and not TESTS_RUNNING
            and ONLINE
            and (IS_PIP_PACKAGE or GIT.origin == "https://github.com/ultralytics/ultralytics.git")
        )
```

Example 3 (python):
```python
def __call__(self, cfg, device = None) -> None
```

Example 4 (python):
```python
def __call__(self, cfg, device=None) -> None:
    """Queue an event and flush the queue asynchronously when the rate limit elapses.

    Args:
        cfg (IterableSimpleNamespace): The configuration object containing mode and task information.
        device (torch.device | str, optional): The device type (e.g., 'cpu', 'cuda').
    """
    if not self.enabled:
        # Events disabled, do nothing
        return

    # Attempt to enqueue a new event
    if len(self.events) < 25:  # Queue limited to 25 events to bound memory and traffic
        params = {
            **self.metadata,
            "task": cfg.task,
            "model": cfg.model if cfg.model in GITHUB_ASSETS_NAMES else "custom",
            "device": str(device),
        }
        if cfg.mode == "export":
            params["format"] = cfg.format
        self.events.append({"name": cfg.mode, "params": params})

    # Check rate limit and return early if under limit
    t = time.time()
    if (t - self.t) < self.rate_limit:
        return

    # Overrate limit: send a snapshot of queued events in a background thread
    payload_events = list(self.events)  # snapshot to avoid race with queue reset
    Thread(
        target=_post,
        args=(self.url, {"client_id": SETTINGS["uuid"], "events": payload_events}),  # SHA-256 anonymized
        daemon=True,
    ).start()

    # Reset queue and rate limit timer
    self.events = []
    self.t = t
```

---

## 快速入门指南：搭载Ultralytics YOLO11 NVIDIA Spark

**URL:** https://docs.ultralytics.com/zh/guides/nvidia-dgx-spark/

**Contents:**
- 快速入门指南：搭载Ultralytics YOLO11 NVIDIA Spark
- NVIDIA Spark？
  - 关键规格
  - DGX 操作系统
  - DGX 仪表板
    - 访问仪表板
- Docker 快速入门
- 本地安装入门
  - 安装 Ultralytics 软件包
  - 安装 PyTorch 和 Torchvision

本综合指南详细介绍了NVIDIA紧凑型桌面AINVIDIA NVIDIA Spark上部署Ultralytics YOLO11 完整流程。此外，指南还展示了性能基准测试，以证明YOLO11 该强大YOLO11 卓越能力。

本指南已在运行基于Ubuntu的DGX OSNVIDIA Spark创始者版上进行过测试。预计可兼容最新版本的DGX OS。

NVIDIA Spark是一款紧凑型桌面人工智能超级计算机，搭载NVIDIA Grace Blackwell超级芯片。它提供高达1 petaflop的FP4精度人工智能计算性能，是需要强大人工智能能力且追求桌面机型开发者、研究人员和数据科学家的理想选择。

NVIDIA OS是一款定制化的 Linux 发行版，为在 DGX 系统上运行人工智能、机器学习和分析应用程序提供稳定、经过测试且受支持的操作系统基础。其包含：

DGX OS遵循常规发布计划，通常每年提供两次更新（约在二月和八月），并在主要版本发布之间提供额外的安全补丁。

DGX Spark内置DGX仪表板，提供：

点击Ubuntu桌面左下角的"显示应用程序"按钮，然后选择"DGX Dashboard"在浏览器中打开它。

NVIDIA 后，点击"DGX仪表板"按钮即可在以下地址打开仪表板： http://localhost:11000.

仪表盘内置了一个集成JupyterLab实例，启动时会自动创建虚拟环境并安装推荐的软件包。每个用户账户都分配有专属端口用于访问JupyterLab。

NVIDIA YOLO11 快速启动Ultralytics YOLO11 的最便捷方式是使用预构建的Docker镜像。支持Jetson AGX Thor（JetPack 7.0）的Docker镜像同样适用于搭载DGX OS的DGX Spark。

完成此操作后，请跳至 TensorRT NVIDIA SparkTensorRT 使用TensorRT ”部分。

对于不使用 Docker 的本地安装，请按照以下步骤操作。

在此我们将为DGX Spark安装Ultralytics 及其可选依赖项，以便能够导出 PyTorch 模型导出为其他不同格式。我们将重点关注NVIDIA TensorRT 因为TensorRT 确保我们充分释放DGX Spark的最大性能。

更新软件包列表，安装 pip 并升级到最新版本

安装 ultralytics 带有可选依赖项的 pip 软件包

ultralytics 将安装Torch Torchvision。但通过pip安装的这些软件包可能无法完全优化以适配DGX Spark的ARM64架构CUDA 。因此，我们建议安装兼容CUDA 版本：

NVIDIA Spark上运行PyTorch .9.1时，您可能会遇到以下问题： UserWarning CUDA 例如运行 yolo checks, yolo predict等：

此警告可安全忽略。为永久解决此问题，已在PyTorch #164590中提交修复方案，该方案将包含在PyTorch .10 版本中。

字段 onnxruntime-gpu 托管在PyPI上的包不包含 aarch64 ARM64系统的二进制文件。因此我们需要手动安装此软件包。该软件包是某些导出功能所必需的。

在此我们将下载并安装 onnxruntime-gpu 1.24.0 使用 Python3.12 支持。

Ultralytics支持的所有模型导出格式中，NVIDIA Spark平台上TensorRT 最高推理性能，因此成为我们部署的首选方案。有关配置说明和高级用法，请参阅我们的专用TensorRT 指南。

PyTorch 格式的 YOLO11n 模型被转换为 TensorRT，以使用导出的模型运行推理。

访问导出页面以访问将模型导出为不同模型格式时的其他参数

Ultralytics 在多种模型格式上运行了YOLO11 ，以衡量速度和准确性：PyTorch、TorchScript、ONNX、OpenVINO、TensorRT、TF SavedModel、TF GraphDef、TF 、MNN、NCNN、ExecuTorch。测试NVIDIA Spark平台上以FP32精度运行，默认输入图像尺寸为640。

下表展示了五种不同模型（YOLO11n、YOLO11s、YOLO11m、YOLO11l、YOLO11x）在多种格式下的基准测试结果，呈现了每种组合的状态、尺寸、mAP50(B)指标及推理时间。

Ultralytics .3.249版本进行基准测试

要在所有导出格式上重现上述 Ultralytics 基准测试，请运行以下代码：

请注意，基准测试结果可能因系统的具体硬件和软件配置而异，也可能因运行基准测试时系统的当前工作负载而异。为了获得最可靠的结果，请使用包含大量图像的数据集，例如： data='coco.yaml' （5000 张验证图像）。

NVIDIA Spark时，为YOLO11达到最佳运行性能，需遵循以下最佳实践：

使用NVIDIA 的监控工具track GPU CPU ：

凭借128GB的统一内存，DGX Spark能够处理大规模批次和模型。建议增加批次规模以提升吞吐量：

使用TensorRT FP16 或 INT8 格式

为获得最佳性能，请使用FP16或INT8精度导出模型：

保持您的DGX Spark创始者版系统更新对性能和安全性至关重要。NVIDIA 两种主要方法来更新系统操作系统、驱动程序和固件。

DGX 仪表板是执行系统更新以确保兼容性的推荐方式。它允许您：

在执行更新前，请确保您的系统连接到稳定的电源，并已备份重要数据。

如需进一步学习和支持，请参阅 Ultralytics YOLO11 文档。

NVIDIA YOLO11 Ultralytics YOLO11 非常简单。您可以使用预构建的Docker镜像快速完成设置，或手动安装所需软件包。两种方法的详细步骤请参见"Docker快速入门"和"原生安装入门"章节。

得益于GB10 Grace Blackwell超级芯片YOLO11 在DGX Spark平台上展现出卓越性能。TensorRT 可提供最佳推理性能。具体基准测试结果（涵盖不同模型规模与格式）请参阅详细对比表部分。

强烈TensorRT 在DGX Spark上部署YOLO11 TensorRT 因其TensorRT 提供最佳性能表现。该技术通过充分利用BlackwellGPU 加速推理过程，确保实现最高效率与运行速度。更多详情请参阅" NVIDIA TensorRT "章节。

DGX Spark的计算能力远超Jetson设备，提供高达1 PFLOP的人工智能性能和128GB统一内存，而Jetson AGX Thor仅具备2070 TFLOPS的性能和128GB内存。DGX Spark作为桌面级人工智能超级计算机而设计，而Jetson设备则是针对边缘部署优化的嵌入式系统。

是的！ ultralytics/ultralytics:latest-nvidia-arm64 Docker镜像同时支持NVIDIA Spark（搭载DGX OS）和Jetson AGX Thor（搭载JetPack 7.0），因两者均采用ARM64架构，配备CUDA 及类似的软件堆栈。

**Examples:**

Example 1 (jsx):
```jsx
# Open an SSH tunnel
ssh -L 11000:localhost:11000 <username>@<IP or spark-abcd.local>

# Then open in browser
# http://localhost:11000
```

Example 2 (bash):
```bash
t=ultralytics/ultralytics:latest-nvidia-arm64
sudo docker pull $t && sudo docker run -it --ipc=host --runtime=nvidia --gpus all $t
```

Example 3 (sql):
```sql
sudo apt update
sudo apt install python3-pip -y
pip install -U pip
```

Example 4 (unknown):
```unknown
pip install ultralytics[export]
```

---

## 快速入门指南：Ultralytics YOLO11 与 NVIDIA Jetson

**URL:** https://docs.ultralytics.com/zh/guides/nvidia-jetson/

**Contents:**
- 快速入门指南：Ultralytics YOLO11 与 NVIDIA Jetson
- 什么是 NVIDIA Jetson？
- NVIDIA Jetson 系列对比
- 什么是 NVIDIA JetPack？
- 将 JetPack 刷写到 NVIDIA Jetson
- 基于 Jetson 设备的 JetPack 支持
- Docker 快速入门
- 本地安装入门
  - 在JetPack 7.0上运行
    - 安装 Ultralytics 软件包

本综合指南详细介绍了在 NVIDIA Jetson 设备上部署 Ultralytics YOLO11 的步骤。此外，它还展示了性能基准，以展示 YOLO11 在这些小型且功能强大的设备上的性能。

我们已使用最新的 NVIDIA Jetson AGX Thor Developer Kit 更新了本指南，该套件可提供高达 2070 FP4 TFLOPS 的 AI 计算能力和 128 GB 内存，功耗可在 40 W 至 130 W 之间配置。它比 NVIDIA Jetson AGX Orin 提供高出 7.5 倍以上的 AI 计算能力，能效提高 3.5 倍，可无缝运行最流行的 AI 模型。

观看： 如何在 NVIDIA Jetson 设备上使用 Ultralytics YOLO11

本指南已在以下设备上进行测试：NVIDIA Jetson AGX Thor Developer Kit (Jetson T5000)（运行最新稳定版JetPack JP7.0）、NVIDIA Jetson AGX Orin Developer Kit (64GB)（运行JetPack JP6.2）、NVIDIA Jetson Orin Nano Super Developer Kit（运行JetPack JP6.1）、Seeed Studio reComputer J4012（基于NVIDIA Jetson Orin NX 16GB，运行JetPack JP6.0/JetPack JP5.1.3）以及Seeed Studio reComputer J1020 v2（基于NVIDIA Jetson Nano 4GB，运行JetPack JP4.6.1）。预计它将兼容所有NVIDIA Jetson硬件系列，包括最新和传统设备。

NVIDIA Jetson 是一系列嵌入式计算板，旨在将加速 AI（人工智能）计算引入边缘设备。这些紧凑而强大的设备围绕 NVIDIA 的 GPU 架构构建，可以直接在设备上运行复杂的 AI 算法和深度学习模型，而无需依赖云计算资源。Jetson 板常用于机器人、自动驾驶汽车、工业自动化以及其他需要以低延迟和高效率在本地执行 AI 推理的应用。此外，这些板基于 ARM64 架构，与传统的 GPU 计算设备相比，功耗更低。

NVIDIA Jetson AGX Thor 是基于 NVIDIA Blackwell 架构的 NVIDIA Jetson 系列的最新迭代产品，与前几代产品相比，它极大地提高了 AI 性能。下表比较了生态系统中的一些 Jetson 设备。

有关更详细的比较表，请访问NVIDIA Jetson 官方页面的“比较规格”部分。

为 Jetson 模块提供支持的 NVIDIA JetPack SDK 是最全面的解决方案，它提供了完整的开发环境，用于构建端到端加速 AI 应用程序并缩短上市时间。JetPack 包括带有引导加载程序的 Jetson Linux、Linux 内核、Ubuntu 桌面环境以及一整套用于加速 GPU 计算、多媒体、图形和计算机视觉的库。它还包括主机和开发工具包的示例、文档和开发人员工具，并支持更高级别的 SDK，例如用于流视频分析的 DeepStream、用于机器人的 Isaac 和用于会话 AI 的 Riva。

获得NVIDIA Jetson设备后的第一步是将NVIDIA JetPack刷写到设备上。刷写NVIDIA Jetson设备有几种不同的方法。

对于上述方法 1、4 和 5，在刷写系统并启动设备后，请在设备终端输入 "sudo apt update && sudo apt install nvidia-jetpack -y" 以安装所有剩余所需的 JetPack 组件。

下表重点列出了不同 NVIDIA Jetson 设备支持的 NVIDIA JetPack 版本。

在 NVIDIA Jetson 上开始使用 Ultralytics YOLO11 最快的方法是运行 Jetson 的预构建 Docker 镜像。请参考上表，根据您拥有的 Jetson 设备选择 JetPack 版本。

完成此操作后，跳至 在 NVIDIA Jetson 上使用 TensorRT 部分。

对于没有 Docker 的原生安装，请参考以下步骤。

这里我们将在 Jetson 上安装 Ultralytics 包及其可选依赖项，以便我们可以将 PyTorch 模型导出为其他不同的格式。我们将主要关注 NVIDIA TensorRT 导出，因为 TensorRT 将确保我们能够充分发挥 Jetson 设备的性能。

更新软件包列表，安装 pip 并升级到最新版本

安装 ultralytics 带有可选依赖项的 pip 软件包

上述 Ultralytics 安装将安装 Torch 和 Torchvision。然而，通过 pip 安装的这两个软件包不兼容在搭载 JetPack 7.0 和 CUDA 13 的 Jetson AGX Thor 上运行。因此，我们需要手动安装它们。

安装 torch 和 torchvision 根据JP7.0

字段 onnxruntime-gpu 托管在PyPI上的包不包含 aarch64 Jetson 的二进制文件。因此，我们需要手动安装此软件包。某些导出需要此软件包。

在此我们将下载并安装 onnxruntime-gpu 1.24.0 使用 Python3.12 支持。

这里我们将在 Jetson 上安装 Ultralytics 包及其可选依赖项，以便我们可以将 PyTorch 模型导出为其他不同的格式。我们将主要关注 NVIDIA TensorRT 导出，因为 TensorRT 将确保我们能够充分发挥 Jetson 设备的性能。

更新软件包列表，安装 pip 并升级到最新版本

安装 ultralytics 带有可选依赖项的 pip 软件包

上述 Ultralytics 安装将安装 Torch 和 Torchvision。然而，通过 pip 安装的这两个软件包与基于 ARM64 架构的 Jetson 平台不兼容。因此，我们需要手动安装预构建的 PyTorch pip wheel，并从源代码编译或安装 Torchvision。

安装 torch 2.5.0 和 torchvision 0.20 根据JP6.1

访问 Jetson 的 PyTorch 页面 以访问适用于不同 JetPack 版本的 PyTorch 的所有不同版本。 有关 PyTorch、Torchvision 兼容性的更详细列表，请访问 PyTorch 和 Torchvision 兼容性页面。

安装 cuSPARSELt 修复与以下相关的依赖问题 torch 2.5.0

字段 onnxruntime-gpu 托管在PyPI上的包不包含 aarch64 Jetson 的二进制文件。因此，我们需要手动安装此软件包。某些导出需要此软件包。

您可以找到所有可用的 onnxruntime-gpu 软件包，按 JetPack 版本、python 版本和其他兼容性详细信息进行组织，位于 Jetson Zoo ONNX Runtime 兼容性矩阵.

关于 JetPack 6 使用 Python 3.10 支持，您可以安装 onnxruntime-gpu 1.23.0:

或者，对于 onnxruntime-gpu 1.20.0:

在这里，我们将在 Jetson 上安装 Ultralytics 包以及可选的依赖项，以便我们可以将 PyTorch 模型导出为其他不同的格式。我们将主要关注 NVIDIA TensorRT 导出，因为 TensorRT 将确保我们能够从 Jetson 设备中获得最佳性能。

更新软件包列表，安装 pip 并升级到最新版本

安装 ultralytics 带有可选依赖项的 pip 软件包

上述 Ultralytics 安装将安装 Torch 和 Torchvision。然而，通过 pip 安装的这两个软件包与基于 ARM64 架构的 Jetson 平台不兼容。因此，我们需要手动安装预构建的 PyTorch pip wheel，并从源代码编译或安装 Torchvision。

卸载当前安装的 PyTorch 和 Torchvision

安装 torch 2.2.0 和 torchvision 0.17.2 根据JP5.1.2

访问 Jetson 的 PyTorch 页面 以访问适用于不同 JetPack 版本的 PyTorch 的所有不同版本。 有关 PyTorch、Torchvision 兼容性的更详细列表，请访问 PyTorch 和 Torchvision 兼容性页面。

字段 onnxruntime-gpu 托管在PyPI上的包不包含 aarch64 Jetson 的二进制文件。因此，我们需要手动安装此软件包。某些导出需要此软件包。

您可以找到所有可用的 onnxruntime-gpu 软件包，按 JetPack 版本、python 版本和其他兼容性详细信息进行组织，位于 Jetson Zoo ONNX Runtime 兼容性矩阵。在这里，我们将下载并安装 onnxruntime-gpu 1.17.0 使用 Python3.8 支持。

onnxruntime-gpu 将自动恢复 numpy 版本到最新版本。因此，我们需要重新安装 numpy 到 1.23.5 通过执行以下命令来修复问题：

pip install numpy==1.23.5

在 Ultralytics 支持的所有模型导出格式中，TensorRT 在 NVIDIA Jetson 设备上提供最高的推理性能，使其成为我们对 Jetson 部署的首要推荐。有关设置说明和高级用法，请参阅我们专门的 TensorRT 集成指南。

PyTorch 格式的 YOLO11n 模型被转换为 TensorRT，以使用导出的模型运行推理。

访问导出页面以访问将模型导出为不同模型格式时的其他参数

NVIDIA 深度学习加速器 (DLA) 是一种内置于 NVIDIA Jetson 设备中的专用硬件组件，可优化深度学习推理，从而提高能效和性能。通过将任务从 GPU 卸载（从而释放 GPU 以进行更密集的处理），DLA 使模型能够以更低的功耗运行，同时保持高吞吐量，这非常适合嵌入式系统和实时 AI 应用。

以下 Jetson 设备配备了 DLA 硬件：

当使用 DLA 导出时，某些层可能不支持在 DLA 上运行，并将回退到 GPU 执行。这种回退会引入额外的延迟，并影响整体推理性能。因此，与完全在 GPU 上运行的 TensorRT 相比，DLA 的主要目的不是减少推理延迟，而是提高吞吐量和提高能源效率。

Ultralytics 团队对 YOLO11 进行了基准测试，在 11 种不同的模型格式上测量了速度和精度：PyTorch、TorchScript、ONNX、OpenVINO、TensorRT、TF SavedModel、TF GraphDef、TF Lite、MNN、NCNN、ExecuTorch。基准测试在 NVIDIA Jetson AGX Thor Developer Kit、NVIDIA Jetson AGX Orin Developer Kit (64GB)、NVIDIA Jetson Orin Nano Super Developer Kit 和由 Jetson Orin NX 16GB 设备驱动的 Seeed Studio reComputer J4012 上进行，采用 FP32 精度，默认输入图像大小为 640。

尽管所有模型导出都可在NVIDIA Jetson上运行，但我们只在下面的对比图中包含了PyTorch、TorchScript、TensorRT，因为它们利用了Jetson上的GPU，并保证能产生最佳结果。所有其他导出仅利用CPU，其性能不如上述三种。您可以在此图表后的部分找到所有导出的基准测试结果。

下表显示了针对五种不同模型（YOLO11n、YOLO11s、YOLO11m、YOLO11l、YOLO11x）在 11 种不同格式（PyTorch、TorchScript、ONNX、OpenVINO、TensorRT、TF SavedModel、TF GraphDef、TF Lite、MNN、NCNN、ExecuTorch）下的基准测试结果，提供了每种组合的状态、大小、mAP50-95(B) 指标和推理时间。

使用Ultralytics 8.3.226进行基准测试

使用 Ultralytics 8.3.157 进行基准测试

使用 Ultralytics 8.3.157 进行基准测试

使用 Ultralytics 8.3.157 进行基准测试

浏览 Seeed Studio 在不同版本的 NVIDIA Jetson 硬件上运行的更多基准测试。

要在所有导出格式上重现上述 Ultralytics 基准测试，请运行以下代码：

请注意，基准测试结果可能因系统的具体硬件和软件配置而异，也可能因运行基准测试时系统的当前工作负载而异。为了获得最可靠的结果，请使用包含大量图像的数据集，例如： data='coco.yaml' （5000 张验证图像）。

使用 NVIDIA Jetson 时，应遵循一些最佳实践，以在运行 YOLO11 的 NVIDIA Jetson 上实现最佳性能。

在 Jetson 上启用 MAX 功率模式将确保所有 CPU、GPU 核心都已打开。

启用 Jetson 时钟将确保所有 CPU、GPU 核心都以其最大频率进行时钟频率。

我们可以使用 jetson stats 应用程序来监控系统组件的温度，并检查其他系统详细信息，例如查看 CPU、GPU、RAM 使用率，更改电源模式，设置为最大时钟，检查 JetPack 信息。

如需进一步学习和支持，请参阅 Ultralytics YOLO11 文档。

在 NVIDIA Jetson 设备上部署 Ultralytics YOLO11 是一个简单的过程。首先，使用 NVIDIA JetPack SDK 刷写您的 Jetson 设备。然后，可以使用预构建的 Docker 镜像进行快速设置，或者手动安装所需的软件包。有关每种方法的详细步骤，请参见Docker 快速入门和原生安装入门部分。

YOLO11 模型已在各种 NVIDIA Jetson 设备上进行了基准测试，显示出显著的性能改进。例如，TensorRT 格式提供了最佳的推理性能。详细比较表部分中的表格提供了不同模型格式下 mAP50-95 和推理时间等性能指标的全面视图。

强烈建议使用 TensorRT 在 NVIDIA Jetson 上部署 YOLO11 模型，因为它具有最佳性能。它通过利用 Jetson 的 GPU 功能来加速推理，从而确保最高的效率和速度。在在 NVIDIA Jetson 上使用 TensorRT部分中了解更多关于如何转换为 TensorRT 并运行推理的信息。

要在 NVIDIA Jetson 上安装 PyTorch 和 Torchvision，首先卸载任何可能通过 pip 安装的现有版本。然后，手动为 Jetson 的 ARM64 架构安装兼容的 PyTorch 和 Torchvision 版本。有关此过程的详细说明，请参见安装 PyTorch 和 Torchvision部分。

为了在使用 YOLO11 的 NVIDIA Jetson 上实现最佳性能，请遵循以下最佳实践：

有关命令和其他详细信息，请参阅使用 NVIDIA Jetson 时的最佳实践部分。

**Examples:**

Example 1 (bash):
```bash
t=ultralytics/ultralytics:latest-jetson-jetpack4
sudo docker pull $t && sudo docker run -it --ipc=host --runtime=nvidia $t
```

Example 2 (bash):
```bash
t=ultralytics/ultralytics:latest-jetson-jetpack5
sudo docker pull $t && sudo docker run -it --ipc=host --runtime=nvidia $t
```

Example 3 (bash):
```bash
t=ultralytics/ultralytics:latest-jetson-jetpack6
sudo docker pull $t && sudo docker run -it --ipc=host --runtime=nvidia $t
```

Example 4 (bash):
```bash
t=ultralytics/ultralytics:latest-nvidia-arm64
sudo docker pull $t && sudo docker run -it --ipc=host --runtime=nvidia $t
```

---

## 安装 Ultralytics - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/quickstart/

**Contents:**
- 安装 Ultralytics
  - Conda Docker 镜像
- 无头服务器安装
- 高级安装
- 通过 CLI 使用 Ultralytics
- 通过 Python 使用 Ultralytics
- Ultralytics 设置
  - 查看设置
  - 修改设置
  - 设置详解

Ultralytics 提供了多种安装方法，包括 pip、conda 和 Docker。您可以通过以下方式安装 YOLO： ultralytics pip 软件包（用于最新的稳定版本），或通过克隆 Ultralytics GitHub 仓库 对于最新版本。Docker 也是一种在隔离容器中运行软件包的选项，这样可以避免本地安装。

观看： Ultralytics YOLO 快速入门指南

安装或更新 ultralytics 通过运行以下命令，使用 pip 安装软件包 pip install -U ultralytics。有关更多详细信息，请参阅 ultralytics 软件包，请访问 Python 包索引 (PyPI).

您也可以安装 ultralytics 直接从 Ultralytics GitHub 仓库。如果您想要最新的开发版本，这将非常有用。确保您已安装 Git 命令行工具，然后运行：

Conda 可以用作 pip 的替代包管理器。有关更多详细信息，请访问 Anaconda。用于更新 conda 软件包的 Ultralytics feedstock 存储库可在 GitHub 上找到。

如果您在 CUDA 环境中安装，最佳实践是安装 ultralytics, pytorch和 pytorch-cuda 在同一命令中。这允许 conda 包管理器解决任何冲突。或者，安装 pytorch-cuda 最后覆盖 CPU 相关的 pytorch 包（如果必要）。

Ultralytics Conda Docker 镜像也可在以下平台获取 选择官方获取。这些镜像基于 Miniconda3 并提供了一种开始使用的直接方法 ultralytics 的简单方法。

克隆 Ultralytics GitHub 仓库 如果您有兴趣为开发做贡献或希望尝试最新的源代码。克隆后，导航到该目录并将软件包以可编辑模式安装 -e 。

使用 Docker 执行 ultralytics 以隔离容器中的软件包形式存在，确保在各种环境中性能一致。通过选择官方的 ultralytics Docker Hub 选择官方，您可以避免本地安装的复杂性，并获得经过验证的工作环境。Ultralytics 提供五个主要支持的 Docker 镜像，每个镜像都设计为具有高兼容性和效率：

上述命令使用最新的 ultralytics 镜像初始化一个 Docker 容器。 -it flags 分配一个伪 TTY 并保持 stdin 打开，从而允许与容器进行交互。该 --ipc=host 标志将 IPC（进程间通信）命名空间设置为 host，这对于进程之间共享内存至关重要。 --gpus all flag 允许访问容器内所有可用的 GPU，这对于需要 GPU 计算的任务至关重要。

注意：要在容器中使用本地计算机上的文件，请使用 Docker 卷将本地目录挂载到容器中：

替换 /path/on/host 替换为您本地计算机上的目录路径，并将 /path/in/container 是 Docker 容器内的目标路径。

有关高级 Docker 用法，请浏览 Ultralytics Docker 指南。

请参阅 ultralytics pyproject.toml 文件以获取依赖项列表。请注意，以上所有示例都安装了所有必需的依赖项。

PyTorch 的要求因操作系统和 CUDA 要求而异，因此请首先按照 PyTorch 上的说明安装 PyTorch。

对于没有显示器的服务器环境（例如云虚拟机、Docker容器、CI/CD管道），请使用 ultralytics-opencv-headless 包。这与标准的 ultralytics 包但依赖于 opencv-python-headless 代替 opencv-python避免不必要的图形用户界面依赖关系和潜在问题 libGL 错误。

这两个软件包提供相同的功能和API。无头版本仅排除了需要显示OpenCV图形用户界面组件。

虽然标准安装方法涵盖了大多数使用场景，但在开发或自定义配置时，您可能需要更个性化的设置方案。

若需持久化的自定义修改，可Ultralytics 叉Ultralytics仓库，对 pyproject.toml 或其他代码，并从您的 fork 安装。

将仓库克隆到本地，按需修改文件，并以可编辑模式安装。

在您的项目中指定自定义Ultralytics 。 requirements.txt 文件以确保团队内部安装的一致性。

Ultralytics 命令行界面 (CLI) 允许简单的单行命令，而无需 Python 环境。CLI 不需要定制或 Python 代码；使用以下命令从终端运行所有任务 yolo 命令。有关从命令行使用 YOLO 的更多信息，请参见 CLI 指南.

Ultralytics yolo 命令使用以下语法：

查看所有 ARGS 在完整的 配置指南 或使用 yolo cfg CLI 命令。

训练一个检测模型 10 个 epochs，初始学习率为 0.01：

使用预训练的分割模型，以 320 的图像尺寸预测一个 YouTube 视频：

使用 1 的批量大小和 640 的图像大小验证预训练的检测模型：

将 YOLO11n 分类模型导出到 ONNX 格式，图像尺寸为 224x128（无需指定 TASK）：

使用 YOLO11 统计视频或直播流中的物体数量：

使用 YOLO11 姿势估计模型监控锻炼动作:

使用 YOLO11 来计算指定队列或区域中的对象：

使用 Streamlit 在 Web 浏览器中执行对象检测、实例分割或姿势估计：

运行特殊命令以查看版本、查看设置、运行检查等：

参数必须以 arg=value 键值对形式传递，用等号分隔 = 签名并用空格分隔。请勿使用 -- 参数前缀或逗号 , 在参数之间。

Ultralytics YOLO python 接口提供与 python 项目的无缝集成，可以轻松加载、运行和处理模型输出。python 接口设计简单，用户可以快速实现对象检测、分割和分类。这使得 YOLO python 接口成为将这些功能集成到 python 项目中的宝贵工具。

例如，用户只需几行代码即可加载模型、训练模型、评估其性能并将其导出为 ONNX 格式。请浏览Python 指南，详细了解如何在您的 Python 项目中使用 YOLO。

Ultralytics 库包含一个 SettingsManager 用于对实验进行细粒度控制，允许用户轻松访问和修改设置。这些设置存储在环境用户配置目录中的 JSON 文件中，可以在 Python 环境中或通过命令行界面 (CLI) 查看或修改。

使用 python 导入 settings 模块导入 ultralytics 模块。使用以下命令打印并返回设置：

命令行界面允许您使用以下命令检查您的设置：

Ultralytics 可以通过以下方式轻松修改设置：

在 python 中，使用 update 上的 settings 对象：

下表概述了 Ultralytics 内的可调整设置，包括示例值、数据类型和描述。

随着项目的进展或实验的进行，重新审视这些设置，以确保最佳配置。

使用 pip 安装 Ultralytics：

这将安装最新稳定版本的 ultralytics 来自软件包 PyPI。要直接从 GitHub 安装开发版本：

确保您的系统上已安装 Git 命令行工具。

是的，使用 conda 安装 Ultralytics YOLO：

此方法是 pip 的绝佳替代方法，可确保与其他软件包的兼容性。对于 CUDA 环境，请安装 ultralytics, pytorch和 pytorch-cuda 一起解决冲突：

Docker为Ultralytics YOLO提供了一个隔离、一致的环境，确保跨系统的流畅性能，避免本地安装的复杂性。官方的Docker镜像可在Docker Hub上获取，并有针对GPU、CPU、ARM64、NVIDIA Jetson和Conda的变体。要拉取并运行最新镜像：

有关详细的 Docker 说明，请参阅 Docker 快速入门指南。

克隆 Ultralytics 仓库并设置开发环境：

这允许为项目做出贡献或试验最新的源代码。有关详细信息，请访问 Ultralytics GitHub 存储库。

Ultralytics YOLO CLI 简化了运行对象检测任务的过程，无需 python 代码，可以直接从终端使用单行命令进行训练、验证和预测。基本语法是：

在完整的 CLI 指南 中探索更多命令和使用示例。

**Examples:**

Example 1 (go):
```go
# Install or upgrade the ultralytics package from PyPI
pip install -U ultralytics
```

Example 2 (go):
```go
# Install the ultralytics package from GitHub
pip install git+https://github.com/ultralytics/ultralytics.git@main
```

Example 3 (go):
```go
# Install the ultralytics package using conda
conda install -c conda-forge ultralytics
```

Example 4 (julia):
```julia
# Install all packages together using conda
conda install -c pytorch -c nvidia -c conda-forge pytorch torchvision pytorch-cuda=11.8 ultralytics
```

---

## 使用YOLOv5进行多GPU训练 - Ultralytics YOLO文档

**URL:** https://docs.ultralytics.com/zh/yolov5/tutorials/multi_gpu_training/

**Contents:**
- 使用YOLOv5进行多GPU训练
- 开始之前
- 训练
  - 单 GPU
  - 多 GPU DataParallel 模式 (⚠️ 不推荐)
  - 多 GPU DistributedDataParallel 模式 (✅ 推荐)
  - 备注
- 结果
- 常见问题
- 支持的环境

本指南介绍如何正确使用多个 GPU，以便在单台或多台机器上使用 YOLOv5 🚀 训练数据集。

克隆仓库并在 Python>=3.8.0 环境中安装 requirements.txt，包括 PyTorch>=1.8。模型和数据集会自动从最新的 YOLOv5 版本下载。

Docker 镜像 建议用于所有 Multi-GPU 训练。请参阅 Docker 快速入门指南

torch.distributed.run 替换 torch.distributed.launch 在 PyTorch>=1.9。请参阅 PyTorch 分布式文档 详情请见。

选择一个预训练模型作为训练起点。这里我们选择YOLOv5s，它是可用模型中最小、最快的。请参阅我们的README表格，了解所有模型的完整比较。我们将在COCO数据集上使用多GPU训练此模型。

您可以增加 device 要在 DataParallel 模式下使用多个 GPU。

与仅使用 1 个 GPU 相比，此方法速度较慢，几乎无法加速训练。

您将需要传递 python -m torch.distributed.run --nproc_per_node，然后是常用的参数。

上面的代码将使用 GPU 0... (N-1)。您还可以设置 CUDA_VISIBLE_DEVICES=2,3 （或任何其他列表），如果您希望通过环境变量控制设备可见性，请在启动命令之前设置。

如果您得到 RuntimeError: Address already in use，这可能是因为您同时运行了多个训练。要解决此问题，只需通过添加以下内容来使用不同的端口号 --master_port 如下所示：

在配备 8 块 A100 SXM4-40GB 的 AWS EC2 P4d 实例上，针对 YOLOv5l 进行 1 个 COCO epoch 的 DDP 性能分析结果。

如结果所示，使用带有多个 GPU 的 DistributedDataParallel 可以在训练速度上提供近乎线性的扩展。使用 8 个 GPU 时，训练完成速度比使用单个 GPU 快大约 6.5 倍，同时保持每个设备的相同内存使用量。

如果发生错误，请先阅读下面的检查清单！（这可以节省您的时间）

Ultralytics 提供一系列即用型环境，每个环境都预装了必要的依赖项，如 CUDA、CUDNN、Python 和 PyTorch，以快速启动您的项目。

此徽章表示所有 YOLOv5 GitHub Actions 持续集成 (CI) 测试均已成功通过。这些 CI 测试严格检查 YOLOv5 在各个关键方面的功能和性能：训练、验证、推理、导出 和 基准测试。它们确保在 macOS、Windows 和 Ubuntu 上运行的一致性和可靠性，测试每 24 小时进行一次，并在每次提交新内容时进行。

感谢 @MagicFrogSJTU 完成了大量繁重的工作，并感谢 @glenn-jocher 在整个过程中给予的指导。

**Examples:**

Example 1 (unknown):
```unknown
git clone https://github.com/ultralytics/yolov5 # clone
cd yolov5
pip install -r requirements.txt # install
```

Example 2 (unknown):
```unknown
python train.py --batch 64 --data coco.yaml --weights yolov5s.pt --device 0
```

Example 3 (unknown):
```unknown
python train.py --batch 64 --data coco.yaml --weights yolov5s.pt --device 0,1
```

Example 4 (unknown):
```unknown
python -m torch.distributed.run --nproc_per_node 2 train.py --batch 64 --data coco.yaml --weights yolov5s.pt --device 0,1
```

---

## SAM 3: 用概念分割万物 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/models/sam-3/

**Contents:**
- SAM 3: 用概念分割万物
- 概述
  - 可提示概念分割 (PCS) 是什么？
  - 关键性能指标
- 架构
  - 核心组件
  - 主要创新
- SA-Co数据集
  - 训练数据
  - 基准数据

SAM 3 已完全集成到 Ultralytics 包中，截至 版本 8.3.237 (PR #22897)。通过以下方式安装或升级： pip install -U ultralytics 以访问所有 SAM 3 功能，包括基于文本的概念分割、图像示例提示和视频 track。

SAM 3 (Segment Anything Model 3) 是 Meta 发布的用于 可提示概念分割 (PCS) 的基础模型。在 SAM 2 的基础上，SAM 3 引入了一项全新的能力：detect、segment 和 track 通过文本提示、图像示例或两者指定的 所有实例。与之前每个提示分割单个对象的 SAM 版本不同，SAM 3 可以在图像或视频中找到并 segment 概念的每一次出现，这与现代 实例分割 中的开放词汇目标保持一致。

观看： 如何使用Meta Segment Anything 3与Ultralytics 图像与视频的文本提示分割

SAM 3 现已完全集成到 ultralytics 包，提供对概念 segment 的原生支持，支持文本提示、图像示例提示以及视频 track 功能。

SAM 3 在可提示概念分割方面比现有系统实现了 2 倍的性能提升，同时保持并改进了 SAM 2 在交互式 视觉分割方面的能力。该模型擅长开放词汇分割，允许用户使用简单的名词短语（例如，“黄色校车”、“条纹猫”）或提供目标对象的示例图像来指定概念。这些功能补充了依赖于简化 预测 和 跟踪 工作流的生产就绪管道。

PCS 任务将一个concept prompt作为输入，并返回所有匹配对象实例的具有唯一标识的 segmentation masks。concept prompt 可以是：

这与传统的视觉提示（点、框、掩码）不同，传统视觉提示仅分割单个特定对象实例，这由最初的SAM系列推广开来。

有关生产中模型指标和权衡的背景信息，请参阅模型评估洞察和YOLO 性能指标。

SAM 3 由一个共享感知编码器 (PE) 视觉骨干网络的 检测器 和 跟踪器 组成。这种解耦设计避免了任务冲突，同时实现了图像级 detect 和视频级 track，并提供与 Ultralytics python 用法 和 CLI 用法 兼容的接口。

检测器：基于 DETR 的架构，用于图像级概念检测。

跟踪器：继承自 SAM 2 的基于内存的视频分割

存在令牌：一个学习到的全局令牌，用于预测目标概念是否存在于图像/帧中，通过将识别与定位分离来改进检测。

SAM 3 在 Segment Anything with Concepts (SA-Co) 上进行训练，这是 Meta 迄今为止最大、最多样化的分割数据集，超越了诸如 COCO 和 LVIS 等常见基准。

The SA-Co评估基准包含21.4万个独特短语，涵盖12.6万张图像和视频，提供了比现有基准多50倍以上的概念。它包括：

SAM 3 的可扩展人机协作数据引擎通过以下方式实现了 2 倍的标注吞吐量：

SAM 3 在 Ultralytics 8.3.237 版本及更高版本中可用。通过以下方式安装或升级：

与其他 Ultralytics 模型不同，SAM 3 权重 (sam3.pt) 为 不会自动下载您必须先在[平台名称]上申请访问模型权重。 Hugging Face上的SAM 模型页面 然后，一旦获得批准，下载 sam3.pt 文件将下载的文件放置于 sam3.pt 文件在您的工作目录中，或者在加载模型时指定完整路径。

SAM 3 通过不同的预测器接口，同时支持可提示概念分割 (PCS) 和可提示视觉分割 (PVS) 任务：

使用文本描述查找并 segment 概念的所有实例。文本提示需要 SAM3SemanticPredictor 界面。

使用边界框作为视觉提示来查找所有相似实例。这还需要 SAM3SemanticPredictor 用于基于概念的匹配。

提取图像特征一次，并将其重复用于多个segmentation查询，以提高效率。

使用边界框提示在视频帧中检测和跟踪目标实例。

SAM 3 完全保持与 SAM 2 视觉提示的向后兼容性，用于单目标分割：

基本 SAM 界面行为与 SAM 2 完全一致，仅分割由视觉提示（点、框或掩码）指示的特定区域。

使用 SAM("sam3.pt") 带有视觉提示（点/框/掩码）将进行segment 仅特定对象 在该位置，就像 SAM 2 一样。进行分割 某个概念的所有实例，使用 SAM3SemanticPredictor 带有文本或示例提示，如上所示。

SAM 3 在多个基准测试中取得了最先进的结果，包括 LVIS 和 COCO for segmentation 等真实世界数据集：

探索数据集选项以在Ultralytics 数据集中进行快速实验。

SAM 3 在 DAVIS 2017 和 YouTube-VOS 等视频基准测试中，相较于 SAM 2 和此前最先进的技术，展现出显著改进：

SAM 3 擅长以最少示例适应新领域，这与 以数据为中心的 AI 工作流密切相关：

SAM 3 的基于概念的示例提示比视觉提示收敛速度快得多：

SAM 3 通过分割所有实例提供准确的计数，这是 目标计数 中的常见需求：

在此，我们比较SAM 3与SAM 2和YOLO11模型的能力：

SAM 3 引入了专为 PCS 任务设计的新指标，补充了诸如 F1 分数、精确度 和 召回率 等熟悉度量。

CGF1 = 100 × pmF1 × IL_MCC

传统的 AP 指标不考虑校准，使得模型在实践中难以使用。通过仅评估置信度高于 0.5 的预测，SAM 3 的指标强制执行良好的校准，并模拟交互式 predict 和 track 循环中的实际使用模式。

存在头提供了+5.7 CGF1 的提升 (+9.9%)，主要提高了识别能力 (IL_MCC +6.5%)。

难负样本对于开放词汇识别至关重要，将 IL_MCC 提高了 54.5% (0.44 → 0.68)。

高质量人工标注相比单独使用合成数据或外部数据，能带来显著提升。有关数据质量实践的背景信息，请参阅数据收集和标注。

SAM 3 的概念分割能力开辟了新的应用场景：

SAM 3 可以与多模态大型语言模型 (MLLM) 结合，以处理需要推理的复杂查询，其理念类似于 OWLv2 和 T-Rex 等开放词汇系统。

MLLM 向 SAM 3 提出简单的名词短语查询，分析返回的掩码，并迭代直到满意。

尽管 SAM 3 代表了一项重大进步，但它也存在某些局限性：

SAM 3 由 Meta 于 2025 年 11 月 20 日 发布，并自 版本 8.3.237 (PR #22897) 起已完全集成到 Ultralytics 中。现已全面支持 预测模式 和 track 模式。

是的！SAM 已完全集成到Ultralytics Python ，包括概念分割、SAM 视觉提示以及多目标视频跟踪功能。

PCS 是 SAM 3 中引入的一项新任务，它用于 segment 图像或视频中视觉概念的所有实例。与针对特定对象实例的传统 segment 不同，PCS 能够找到某个类别的所有出现。例如：

了解有关目标检测和实例分割的相关背景信息。

SAM 3 保持与 SAM 2 视觉提示的向后兼容性，同时增加了基于概念的功能。

SAM 3 在 Segment Anything with Concepts (SA-Co) 数据集上进行训练：

这种大规模和多样性使得 SAM 3 能够实现跨越开放词汇概念的卓越零样本泛化能力。

SAM 3 和 YOLO11 服务于不同的用例：

SAM 3 专为简单名词短语设计（例如，“红苹果”，“戴帽子的人”）。对于需要推理的复杂查询，将 SAM 3 与 MLLM 结合作为 SAM 3 Agent：

复杂查询（带MLLM的SAM 3 Agent）：

SAM 3 代理通过将 SAM 3 的分割与 MLLM 推理能力相结合，在 ReasonSeg 验证中实现了 76.0 GIoU（而之前的最佳成绩为 65.0，提高了 16.9%）。

在 SA-Co/Gold 基准测试中，采用三重人工标注：

SAM 3 在开放词汇概念分割方面取得了接近人类水平的强大性能，差距主要体现在模糊或主观概念（例如，“小窗户”、“舒适的房间”）上。

**Examples:**

Example 1 (unknown):
```unknown
pip install -U ultralytics
```

Example 2 (sql):
```sql
from ultralytics.models.sam import SAM3SemanticPredictor

# Initialize predictor with configuration
overrides = dict(
    conf=0.25,
    task="segment",
    mode="predict",
    model="sam3.pt",
    half=True,  # Use FP16 for faster inference
    save=True,
)
predictor = SAM3SemanticPredictor(overrides=overrides)

# Set image once for multiple queries
predictor.set_image("path/to/image.jpg")

# Query with multiple text prompts
results = predictor(text=["person", "bus", "glasses"])

# Works with descriptive phrases
results = predictor(text=["person with red cloth", "person with blue cloth"])

# Query with a single concept
results = predictor(text=["a person"])
```

Example 3 (sql):
```sql
from ultralytics.models.sam import SAM3SemanticPredictor

# Initialize predictor
overrides = dict(conf=0.25, task="segment", mode="predict", model="sam3.pt", half=True, save=True)
predictor = SAM3SemanticPredictor(overrides=overrides)

# Set image
predictor.set_image("path/to/image.jpg")

# Provide bounding box examples to segment similar objects
results = predictor(bboxes=[[480.0, 290.0, 590.0, 650.0]])

# Multiple bounding boxes for different concepts
results = predictor(bboxes=[[539, 599, 589, 639], [343, 267, 499, 662]])
```

Example 4 (python):
```python
import cv2

from ultralytics.models.sam import SAM3SemanticPredictor
from ultralytics.utils.plotting import Annotator, colors

# Initialize predictors
overrides = dict(conf=0.50, task="segment", mode="predict", model="sam3.pt", verbose=False)
predictor = SAM3SemanticPredictor(overrides=overrides)
predictor2 = SAM3SemanticPredictor(overrides=overrides)

# Extract features from the first predictor
source = "path/to/image.jpg"
predictor.set_image(source)
src_shape = cv2.imread(source).shape[:2]

# Setup second predictor and reuse features
predictor2.setup_model()

# Perform inference using shared features with text prompt
masks, boxes = predictor2.inference_features(predictor.features, src_shape=src_shape, text=["person"])

# Perform inference using shared features with bounding box prompt
masks, boxes = predictor2.inference_features(predictor.features, src_shape=src_shape, bboxes=[[439, 437, 524, 709]])

# Visualize results
if masks is not None:
    masks, boxes = masks.cpu().numpy(), boxes.cpu().numpy()
    im = cv2.imread(source)
    annotator = Annotator(im, pil=False)
    annotator.masks(masks, [colors(x, True) for x in range(len(masks))])

    cv2.imshow("result", annotator.result())
    cv2.waitKey(0)
```

---

## Ultralytics HUB Inference API - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/hub/inference-api/

**Contents:**
- Ultralytics HUB Inference API
- 专用 Inference API
- 共享Inference API
- Python
- cURL
- 参数
- 响应
  - 分类
  - 检测
  - OBB

在您训练模型后，您可以免费使用共享推理 API。如果您是 Pro 用户，则可以访问专用推理 API。Ultralytics HUB 推理 API 允许您通过我们的 REST API 运行推理，而无需在本地安装和设置 Ultralytics YOLO 环境。

观看： Ultralytics HUB Inference API 演练

为了满足用户的高需求和广泛兴趣，我们非常高兴地推出了Ultralytics HUB专用Inference API），在专用环境中为专业版用户提供单击部署功能！

我们很高兴在公开测试期间免费提供此功能，作为 Pro Plan 的一部分，将来可能会有付费层级。

要使用 Ultralytics HUB 专用 Inference API，请点击 Start Endpoint 按钮。接下来，按照以下指南所述使用唯一的端点 URL。

选择延迟最低的区域以获得最佳性能，如文档中所述。

要使用 Ultralytics HUB 共享 Inference API，请按照以下指南操作。

The Ultralytics HUB共享Inference API具有以下使用限制：

要使用 python 访问 Ultralytics HUB Inference API，请使用以下代码：

替换 MODEL_ID 使用所需的模型 ID， API_KEY 以及您实际的 API 密钥，以及 path/to/image.jpg 以及您想要运行推理的图像路径。

如果您正在使用我们的 专用 Inference API，替换 url 也一样。

要使用 cURL 访问 Ultralytics HUB Inference API，请使用以下代码：

替换 MODEL_ID 使用所需的模型 ID， API_KEY 以及您实际的 API 密钥，以及 path/to/image.jpg 以及您想要运行推理的图像路径。

如果您正在使用我们的 专用 Inference API，替换 url 也一样。

The Ultralytics HUB Inference API返回JSON响应。

**Examples:**

Example 1 (typescript):
```typescript
import requests

# API URL
url = "https://predict.ultralytics.com"

# Headers, use actual API_KEY
headers = {"x-api-key": "API_KEY"}

# Inference arguments (use actual MODEL_ID)
data = {"model": "https://hub.ultralytics.com/models/MODEL_ID", "imgsz": 640, "conf": 0.25, "iou": 0.45}

# Load image and send request
with open("path/to/image.jpg", "rb") as image_file:
    files = {"file": image_file}
    response = requests.post(url, headers=headers, files=files, data=data)

print(response.json())
```

Example 2 (unknown):
```unknown
curl -X POST "https://predict.ultralytics.com" \
  -H "x-api-key: API_KEY" \
  -F "model=https://hub.ultralytics.com/models/MODEL_ID" \
  -F "file=@/path/to/image.jpg" \
  -F "imgsz=640" \
  -F "conf=0.25" \
  -F "iou=0.45"
```

Example 3 (python):
```python
from ultralytics import YOLO

# Load model
model = YOLO("yolov8n-cls.pt")

# Run inference
results = model("image.jpg")

# Print image.jpg results in JSON format
print(results[0].to_json())
```

Example 4 (unknown):
```unknown
curl -X POST "https://predict.ultralytics.com" \
  -H "x-api-key: API_KEY" \
  -F "model=https://hub.ultralytics.com/models/MODEL_ID" \
  -F "file=@/path/to/image.jpg" \
  -F "imgsz=640" \
  -F "conf=0.25" \
  -F "iou=0.45"
```

---

## AWS 深度学习实例上的 Ultralytics YOLOv5 🚀：您的完整指南

**URL:** https://docs.ultralytics.com/zh/yolov5/environments/aws_quickstart_tutorial/

**Contents:**
- AWS 深度学习实例上的 Ultralytics YOLOv5 🚀：您的完整指南
- 步骤 1：AWS 控制台登录
- 步骤 2：启动您的实例
  - 选择合适的 Amazon Machine Image (AMI)
  - 选择实例类型
  - 配置您的实例
- 步骤 3：连接到您的实例
- 步骤 4：运行 Ultralytics YOLOv5
- 可选的额外步骤：增加交换内存
- 评论

建立高性能的深度学习环境似乎令人生畏，尤其是对于新手而言。但是不用担心！🛠️ 本指南提供了在 AWS 深度学习实例上启动并运行 Ultralytics YOLOv5 的分步演练。通过利用 Amazon Web Services (AWS) 的强大功能，即使是 机器学习 (ML) 新手也可以快速且经济高效地开始。AWS 平台的 可扩展性 使其成为实验和生产部署的理想选择。

YOLOv5 的其他快速入门选项包括我们的 Google Colab Notebook, Kaggle 环境, GCP 深度学习 VM，以及我们预构建的 Docker 镜像，可在以下位置获得： 选择官方.

首先创建账户或登录到 AWS 管理控制台。登录后，导航到 EC2 服务控制面板，您可以在其中管理您的虚拟服务器（实例）。

在 EC2 仪表板中，单击 Launch Instance 按钮。 这将启动创建新的虚拟服务器的过程，该服务器可以根据您的需求进行定制。

选择正确的 AMI 至关重要。 这决定了您的实例的操作系统和预安装的软件。 在搜索栏中，键入“深度学习”，然后选择最新的基于 Ubuntu 的深度学习 AMI（除非您对其他操作系统有特定要求）。 亚马逊的深度学习 AMI 预先配置了流行的 深度学习框架（如 YOLOv5 使用的 PyTorch）和必要的 GPU 驱动程序，从而大大简化了设置过程。

对于诸如训练深度学习模型等高要求的任务，强烈建议选择 GPU 加速的实例类型。与 CPU 相比，GPU 可以显著减少模型训练所需的时间。在选择实例大小时，请确保其内存容量（RAM）足以满足您的模型和数据集的需求。

注意： 模型和数据集的大小是关键因素。如果您的 ML 任务需要的内存超过所选实例提供的内存，您需要选择更大的实例类型，以避免性能问题或错误。

浏览 EC2 实例类型页面上可用的 GPU 实例类型，尤其是在 加速计算 类别下。

有关监控和优化 GPU 使用情况的详细信息，请参阅 AWS 关于 GPU 监控和优化 的指南。使用 按需定价 比较成本，并通过 竞价型实例定价 探索潜在的节省。

考虑使用 Amazon EC2 Spot Instances 以获得更具成本效益的方法。Spot Instances 允许您竞标未使用的 EC2 容量，通常比按需价格有很大的折扣。对于需要持久性的任务（即使 Spot Instance 中断也要保存数据），请选择持久性请求。这可确保您的存储卷保持不变。

继续执行实例启动向导的步骤 4-7，以配置存储、添加标签、设置安全组（确保从您的 IP 打开 SSH 端口 22），并在单击 启动 之前查看您的设置。您还需要创建或选择现有的密钥对以进行安全的 SSH 访问。

一旦您的实例状态显示为“running”，请从 EC2 仪表板中选择它。点击 连接 用于查看连接选项的按钮。 在您的本地终端（如 macOS/Linux 上的 Terminal 或 Windows 上的 PuTTY/WSL）中使用提供的 SSH 命令示例来建立安全连接。 您将需要私钥文件（.pem）您在启动期间创建或选择的。

现在您已通过 SSH 连接，您可以设置并运行 YOLOv5。首先，从以下位置克隆官方 YOLOv5 存储库 GitHub 并导航到该目录。然后，使用以下命令安装所需的依赖项 pip。建议使用 Python 3.8 环境或更高版本。必需的模型和数据集将会从最新的 YOLOv5 自动下载。 发布 当你运行诸如训练或检测之类的命令时。

环境准备就绪后，您可以开始使用 YOLOv5 执行各种任务：

有关训练、验证、预测（推理）和导出的详细指南，请参阅 Ultralytics 文档。

如果您正在处理非常大的数据集，或者在训练期间遇到内存限制，增加实例上的交换内存有时会有所帮助。交换空间允许系统使用磁盘空间作为虚拟 RAM。

恭喜！🎉 您已成功设置 AWS 深度学习实例，安装了 Ultralytics YOLOv5，并准备执行目标检测任务。无论您是使用预训练模型进行实验，还是在自己的数据上进行训练，这个强大的设置都为您的计算机视觉项目提供了可扩展的基础。如果您遇到任何问题，请查阅详尽的AWS 文档以及 Ultralytics 社区的有用资源，例如常见问题。祝您检测愉快！

**Examples:**

Example 1 (markdown):
```markdown
# Clone the YOLOv5 repository
git clone https://github.com/ultralytics/yolov5
cd yolov5

# Install required packages
pip install -r requirements.txt
```

Example 2 (julia):
```julia
# Train a YOLOv5 model on a custom dataset (e.g., coco128.yaml)
python train.py --data coco128.yaml --weights yolov5s.pt --img 640

# Validate the performance (Precision, Recall, mAP) of a trained model (e.g., yolov5s.pt)
python val.py --weights yolov5s.pt --data coco128.yaml --img 640

# Run inference (object detection) on images or videos using a trained model
python detect.py --weights yolov5s.pt --source path/to/your/images_or_videos/ --img 640

# Export the trained model to various formats like ONNX, CoreML, TFLite for deployment
# See https://docs.ultralytics.com/modes/export/ for more details
python export.py --weights yolov5s.pt --include onnx coreml tflite --img 640
```

Example 3 (markdown):
```markdown
# Allocate a 64GB swap file (adjust size as needed)
sudo fallocate -l 64G /swapfile

# Set correct permissions
sudo chmod 600 /swapfile

# Set up the file as a Linux swap area
sudo mkswap /swapfile

# Enable the swap file
sudo swapon /swapfile

# Verify the swap memory is active
free -h
```

---

## Ultralytics HUB 快速入门 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/hub/quickstart/

**Contents:**
- Ultralytics HUB 快速入门
- 开始使用
- 主页
  - 最近
  - 上传数据集
  - 创建项目
  - 训练模型
- 反馈
- 需要帮助？
- 评论

Ultralytics HUB 旨在用户友好且直观，允许用户快速上传数据集并训练新的 YOLO 模型。它还提供一系列预训练模型供选择，使用户入门变得极其简单。模型训练完成后，可以在 Ultralytics HUB App 中轻松预览，然后部署用于实时分类、目标检测 和 实例分割 任务。

观看： 如何使用 Ultralytics HUB 在自定义数据集上训练 Ultralytics YOLO11 | HUB 数据集 🚀

Ultralytics HUB提供了多种简单的注册选项。您可以使用您的 Google、Apple 或 GitHub 帐户注册和登录，或者直接使用您的电子邮件地址。

您可以从帐户选项卡上的设置页面更新您的个人资料。

登录后，您将被定向到 Ultralytics HUB的主页，其中提供了全面的概述、快速链接和更新。

侧边栏方便地提供了平台重要模块的链接，例如数据集、项目和模型。

您可以使用主页上的“最近”卡轻松地全局搜索或直接访问您上次更新的数据集、项目或模型。

您可以直接从主页上传数据集。Ultralytics HUB 支持各种数据集格式，并可以轻松地为训练自定义 YOLO 模型准备数据。

阅读更多关于数据集以及如何准备它们以获得最佳训练结果的信息。

您可以直接从主页创建项目。项目有助于您将相关的模型和实验集中组织在一个地方，从而更轻松地跟踪进度和比较结果。

阅读更多关于项目以及它们如何简化您的工作流程的信息。

您可以直接从主页训练模型。Ultralytics HUB 提供多种训练选项，包括云训练、Google Colab 集成或使用您自己的硬件。

阅读更多关于模型以及可用于计算机视觉任务的各种架构的信息。

我们重视您的反馈！随时留下评论，以帮助我们改进平台。

只有我们的团队会看到您的反馈，我们将使用它来改进我们的平台。

如果您遇到任何问题或有任何疑问，我们随时为您提供帮助。

您可以在 GitHub 上报告错误、请求功能或提出问题。

报告错误时，请提供支持页面中的环境详细信息。

您可以加入我们的 Discord 社区进行提问和讨论！

---

## Ultralytics HUB-SDK - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/hub/sdk/

**Contents:**
- Ultralytics HUB-SDK
- 从哪里开始
  - 从 PyPI 安装
- 初始化 HUBClient
  - 选项 1：使用 API 密钥
  - 选项 2：使用电子邮件和密码
  - 整合在一起
- HUB-SDK 功能
- 评论

欢迎阅读 Ultralytics HUB-SDK 文档！如果您希望将强大的机器学习工具和服务集成到您的 python 应用程序中，那么您来对地方了。无论您是人工智能爱好者、经验丰富的机器学习从业者，还是希望利用 Ultralytics 服务功能的软件开发人员，我们的 SDK 都能让这一切变得简单高效。

我们友好且专业的文档将引导您从安装到精通HUB-SDK。让我们深入探索，开始在您的项目中充分利用Ultralytics生态系统的强大功能！

准备好开始使用 HUB-SDK 了吗？我们的快速入门指南提供了一条在您的 Python 环境中启动并运行 SDK 的简单途径。

通过 PyPI 获取 HUB-SDK 的最新稳定版本。只需在您的终端或 shell 中执行以下命令，即可将 SDK 无缝添加到您的 Python 项目中：

运行此命令后，将下载并安装 SDK，从而在您的应用程序中解锁 Ultralytics 服务的各项功能。

与 Ultralytics 服务的集成始于初始化一个 HUBClient 对象。这个关键步骤在您的代码和我们的 API 之间建立了一座桥梁，并且需要适当的凭据进行身份验证。您可以选择标准的 API 密钥方法，或者使用您的电子邮件和密码。让我们一起设置它！🚀

要利用 API 密钥的简易性，请准备一个包含您的密钥的字典，如下所示：

使用 API 密钥是一种常见的身份验证方法，适用于程序化访问。它非常适合将密钥直接集成到框架中以实现快速、安全的服务交互的场景。该 HUBClient 类 继承身份验证功能 来自 Auth 类。

想要使用您的账户凭据？请配置 HUBClient 在凭据字典中使用您的电子邮件和密码：

如果您正在寻找传统的登录体验或旨在利用与您的 Ultralytics 帐户相关的个性化功能，则使用您的电子邮件和密码是一个方便的选择。

现在您的凭据已准备就绪，请启动您的 HUBClient:

这关键的代码行创建了一个新的实例 HUBClient，将您连接到 Ultralytics 平台提供的广阔服务领域。有了安全就位的身份验证详细信息，您就可以开始探索触手可及的功能了！The login 方法 处理身份验证 使用提供的凭据。

Ultralytics HUB-SDK 提供了一系列功能，用于与您的机器学习项目进行交互。以下是您可以执行的一些关键操作：

恭喜您成功设置 Ultralytics HUB-SDK！您现在已准备就绪，可以开始将尖端机器学习服务集成到您的应用程序中。查阅我们的更多文档，以获取有关使用特定 API 的指导，如果您遇到任何障碍，请咨询我们的社区论坛。祝您编程愉快，愿您的项目在 Ultralytics 的强大支持下蓬勃发展！🌟

**Examples:**

Example 1 (unknown):
```unknown
pip install hub-sdk
```

Example 2 (json):
```json
# Replace <YOUR-API-KEY> with the actual key provided to you by Ultralytics.
credentials = {"api_key": "<YOUR-API-KEY>"}
```

Example 3 (json):
```json
# Replace <YOUR-EMAIL> with your email address and <YOUR-PASSWORD> with your password.
credentials = {"email": "<YOUR-EMAIL>", "password": "<YOUR-PASSWORD>"}
```

Example 4 (json):
```json
from hub_sdk import HUBClient

credentials = {"api_key": "<YOUR-API-KEY>"}  # api key
client = HUBClient(credentials)
```

---

## ROS（机器人操作系统）快速入门指南 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/guides/ros-quickstart/

**Contents:**
- ROS (机器人操作系统) 快速入门指南
- 什么是 ROS？
  - ROS 的主要特性
  - ROS 1 与 ROS 2
  - ROS 消息和主题
- 使用 ROS 设置 Ultralytics YOLO
  - 依赖项安装
- 将 Ultralytics 与 ROS 结合使用 sensor_msgs/Image
  - 图像逐步使用指南
  - 使用以下方式发布检测到的类别 std_msgs/String

来自 Open Robotics 在 Vimeo 上的ROS 介绍（带字幕）。

机器人操作系统（ROS）是一个广泛应用于机器人研究和行业的开源框架。ROS提供了一系列库和工具，以帮助开发人员创建机器人应用程序。ROS旨在与各种机器人平台配合使用，使其成为机器人专家的灵活而强大的工具。

模块化架构: ROS 具有模块化架构，允许开发人员通过组合称为 节点 的较小、可重用组件来构建复杂的系统。每个节点通常执行特定的功能，节点之间使用 主题 或 服务 通过消息进行通信。

通信中间件 (Communication Middleware): ROS 提供了一个强大的通信基础设施，支持进程间通信和分布式计算。这是通过数据流（主题）的发布-订阅模型和服务调用的请求-回复模型来实现的。

硬件抽象: ROS 提供了硬件之上的抽象层，使开发人员能够编写与设备无关的代码。这允许相同的代码与不同的硬件设置一起使用，从而促进更容易的集成和实验。

工具和实用程序: ROS 配备了一套丰富的工具和实用程序，用于可视化、调试和仿真。例如，RViz 用于可视化传感器数据和机器人状态信息，而 Gazebo 提供了一个强大的仿真环境，用于测试算法和机器人设计。

广泛的生态系统: ROS 生态系统非常庞大且不断发展，有大量软件包可用于不同的机器人应用，包括导航、操作、感知等。社区积极为这些软件包的开发和维护做出贡献。

自 2007 年开发以来，ROS 经历了多个版本的演变，每个版本都引入了新功能和改进，以满足机器人社区不断增长的需求。ROS 的开发可以分为两个主要系列：ROS 1 和 ROS 2。本指南侧重于 ROS 1 的长期支持 (LTS) 版本，即 ROS Noetic Ninjemys，该代码也应适用于早期版本。

虽然 ROS 1 为机器人开发提供了坚实的基础，但 ROS 2 通过提供以下功能解决了它的缺点：

在 ROS 中，节点之间的通信通过 消息 和 话题 来实现。消息是一种数据结构，用于定义节点之间交换的信息；而话题是一个命名的通道，消息通过该通道发送和接收。节点可以向一个话题发布消息，也可以订阅来自一个话题的消息，从而实现彼此间的通信。这种发布-订阅模型允许异步通信和节点之间的解耦。通常，机器人系统中的每个传感器或执行器都会向一个话题发布数据，然后其他节点可以消费这些数据以进行处理或控制。在本指南中，我们将重点介绍图像、深度和点云消息以及相机话题。

本指南已经使用此 ROS 环境进行了测试，该环境是 ROSbot ROS 存储库的一个分支。此环境包括 Ultralytics YOLO 软件包、一个用于轻松设置的 Docker 容器、全面的 ROS 软件包以及用于快速测试的 Gazebo 世界。它旨在与 Husarion ROSbot 2 PRO 配合使用。提供的代码示例适用于任何 ROS Noetic/Melodic 环境，包括模拟和真实环境。

除了 ROS 环境，您还需要安装以下依赖项：

ROS Numpy 包：这是 ROS 图像消息和 numpy 数组之间快速转换所必需的。

字段 sensor_msgs/Image 消息类型 通常在ROS中用于表示图像数据。它包含编码、高度、宽度和像素数据等字段，使其适用于传输由相机或其他传感器捕获的图像。图像消息广泛用于机器人应用中，例如视觉感知， 对象检测，以及导航。

以下代码片段演示了如何将 Ultralytics YOLO 软件包与 ROS 一起使用。在此示例中，我们订阅一个相机主题，使用 YOLO 处理传入的图像，并将检测到的对象发布到新的主题，用于检测和分割。

首先，导入必要的库并实例化两个模型：一个用于 分割 以及一个用于 检测。初始化一个 ROS 节点（名称为 ultralytics)，以启用与 ROS 主节点的通信。为了确保连接稳定，我们加入了一个短暂的暂停，让节点有足够的时间来建立连接，然后再继续。

初始化两个 ROS 主题：一个用于 检测 以及一个用于 分割。这些主题将用于发布带注释的图像，从而使它们可以用于进一步处理。节点之间的通信是使用以下方式实现的 sensor_msgs/Image 消息。

最后，创建一个订阅者来监听以下消息 /camera/color/image_raw 主题，并为每个新消息调用回调函数。此回调函数接收类型为 sensor_msgs/Image，使用以下方法将它们转换为 numpy 数组 ros_numpy，使用先前实例化的 YOLO 模型处理图像，注释图像，然后将它们发布回相应的topic： /ultralytics/detection/image 用于检测和 /ultralytics/segmentation/image 用于分割。

由于系统的分布式特性，调试 ROS（机器人操作系统）节点可能具有挑战性。以下是一些可以协助此过程的工具：

标准 ROS 消息也包括 std_msgs/String 消息。在许多应用中，没有必要重新发布整个带注释的图像；而是只需要机器人视图中存在的类别。以下示例演示了如何使用 std_msgs/String 消息 以重新发布检测到的类 /ultralytics/detection/classes 主题。这些消息更轻量级，并提供基本信息，使其对各种应用都很有价值。

考虑一个配备了摄像头和物体的仓库机器人 检测模型。机器人可以发布检测到的类别列表，而不是通过网络发送大型带注释的图像，作为 std_msgs/String 消息。例如，当机器人检测到“box”、“pallet”和“forklift”等对象时，它会将这些类别发布到 /ultralytics/detection/classes 主题。这些信息随后可由中央监控系统使用，以实时跟踪库存、优化机器人的路径规划以避开障碍物，或触发特定动作，例如拾取检测到的箱子。这种方法减少了通信所需的带宽，并专注于传输关键数据。

此示例演示了如何将 Ultralytics YOLO 软件包与 ROS 结合使用。在此示例中，我们订阅一个相机主题，使用 YOLO 处理传入的图像，并将检测到的对象发布到新主题 /ultralytics/detection/classes 使用 std_msgs/String 消息。The ros_numpy package 用于将 ROS Image 消息转换为 numpy 数组，以便与 YOLO 一起处理。

除了 RGB 图像，ROS 还支持深度图像，它提供关于物体与相机之间距离的信息。 深度图像对于机器人应用至关重要，例如避障、3D 地图构建和定位。

深度图像是一种图像，其中每个像素表示从相机到物体的距离。与捕获颜色的 RGB 图像不同，深度图像捕获空间信息，使机器人能够感知其环境的 3D 结构。

在 ROS 中，深度图像由以下内容表示 sensor_msgs/Image 消息类型，其中包括编码、高度、宽度和像素数据等字段。深度图像的编码字段通常使用诸如“16UC1”之类的格式，表示每个像素 16 位无符号整数，其中每个值表示到对象的距离。深度图像通常与 RGB 图像结合使用，以提供更全面的环境视图。

使用YOLO，可以从RGB图像和深度图像中提取并结合信息。例如，YOLO可以在RGB图像中检测物体，并且这种检测可以用于精确定位深度图像中对应的区域。这使得能够提取检测到的物体的精确深度信息，从而增强机器人对三维环境的理解能力。

当处理深度图像时，必须确保 RGB 图像和深度图像已正确对齐。RGB-D 相机（例如 Intel RealSense 系列）提供同步的 RGB 图像和深度图像，从而更容易组合来自两个来源的信息。如果使用单独的 RGB 相机和深度相机，则必须对其进行校准以确保准确对齐。

在此示例中，我们使用YOLO对图像进行segment，并应用提取的mask来segment深度图像中的目标。这使我们能够确定感兴趣对象每个像素与相机焦点的距离。通过获取此距离信息，我们可以计算相机与场景中特定对象之间的距离。首先导入必要的库，创建一个ROS节点，并实例化一个分割模型和一个ROS主题。

接下来，定义一个回调函数来处理传入的深度图像消息。该函数等待深度图像和 RGB 图像消息，将它们转换为 numpy 数组，并将分割模型应用于 RGB 图像。然后，它提取每个检测到的对象的分割掩码，并使用深度图像计算对象与摄像头的平均距离。大多数传感器都有一个最大距离，称为裁剪距离，超过该距离的值表示为 inf（np.inf)。在处理之前，重要的是过滤掉这些空值并为它们分配一个值 0。最后，它发布检测到的对象以及它们到 /ultralytics/detection/distance 主题。

字段 sensor_msgs/PointCloud2 消息类型 是ROS中使用的数据结构，用于表示3D点云数据。此消息类型是机器人应用不可或缺的一部分，支持诸如3D地图绘制、对象识别和定位等任务。

点云是在三维坐标系中定义的数据点集合。这些数据点表示物体或场景的外部表面，通过 3D 扫描技术捕获。云中的每个点都有 X, Y和 Z 坐标，这些坐标对应于它在空间中的位置，并且还可以包括诸如颜色和强度之类的附加信息。

当使用 sensor_msgs/PointCloud2，必须考虑获取点云数据的传感器的参考坐标系。点云最初是在传感器的参考坐标系中捕获的。您可以通过监听以下内容来确定此参考坐标系 /tf_static 主题。但是，根据您的具体应用需求，您可能需要将点云转换为另一个参考系。这种转换可以使用 tf2_ros package，它提供了用于管理坐标系并在它们之间转换数据的工具。

要将 YOLO 与以下工具集成： sensor_msgs/PointCloud2 类型消息，我们可以采用类似于用于深度图的方法。通过利用嵌入在点云中的颜色信息，我们可以提取 2D 图像，使用 YOLO 对此图像执行分割，然后将生成的掩码应用于三维点，以隔离感兴趣的 3D 对象。

对于处理点云，我们建议使用 Open3D (pip install open3d)，一个用户友好的 python 库。Open3D 提供了强大的工具来管理点云数据结构、可视化它们，并无缝地执行复杂的操作。该库可以显著简化该过程，并增强我们结合 YOLO 的分割来操作和分析点云的能力。

导入必要的库并实例化用于分割的 YOLO 模型。

创建函数 pointcloud2_to_array，它转换了一个 sensor_msgs/PointCloud2 消息转换为两个 numpy 数组。The sensor_msgs/PointCloud2 消息包含 n 基于点的 width 和 height 所获取的图像。例如，一个 480 x 640 图像将具有 307,200 点。每个点包括三个空间坐标（xyz) 以及对应的颜色在 RGB 格式。这些可以被认为是两个独立的信息通道。

该函数返回 xyz 坐标和 RGB 原始相机分辨率格式的值（width x height)。大多数传感器都有一个最大距离，称为剪裁距离，超过这个距离的值表示为 inf (np.inf)。在处理之前，重要的是过滤掉这些空值并为它们分配一个值 0.

接下来，订阅 /camera/depth/points 主题，以接收点云消息并转换 sensor_msgs/PointCloud2 消息转换为包含 XYZ 坐标和 RGB 值的 numpy 数组（使用 pointcloud2_to_array function）。 使用 YOLO 模型处理 RGB 图像以提取分割的对象。 对于每个检测到的对象，提取分割掩码并将其应用于 RGB 图像和 XYZ 坐标，以在 3D 空间中隔离对象。

处理掩码非常简单，因为它由二进制值组成，其中 1 表示物体的存在，以及 0 表示不存在。要应用掩码，只需将原始通道乘以掩码即可。此操作有效地隔离了图像中感兴趣的物体。最后，创建一个Open3D点云对象，并在3D空间中可视化带有相关颜色的分割物体。

机器人操作系统（ROS）是一个常用于机器人领域的开源框架，旨在帮助开发人员创建强大的机器人应用程序。它提供了一系列库和工具，用于构建机器人系统并与之交互，从而更轻松地开发复杂的应用程序。ROS支持使用主题或服务通过消息在节点之间进行通信。

将 Ultralytics YOLO 与 ROS 集成涉及设置 ROS 环境并使用 YOLO 处理传感器数据。首先，安装所需的依赖项，如 ros_numpy 以及 Ultralytics YOLO：

接下来，创建一个 ROS 节点并订阅一个 图像主题，以处理传入的数据。这是一个最简单的例子：

ROS 主题通过使用发布-订阅模型，促进 ROS 网络中节点之间的通信。主题是一个命名通道，节点使用该通道异步发送和接收消息。在 Ultralytics YOLO 的上下文中，您可以使节点订阅图像主题，使用 YOLO 处理图像以执行检测或分割等任务，并将结果发布到新主题。

例如，订阅一个相机主题并处理传入的图像以进行检测：

ROS 中的深度图像，由以下表示 sensor_msgs/Image，提供物体与摄像头的距离，这对于避障、3D 映射和定位等任务至关重要。通过 使用深度信息 结合 RGB 图像，机器人可以更好地理解其 3D 环境。

使用 YOLO，您可以从 RGB 图像中提取 分割掩码，并将这些掩码应用于深度图像，以获得精确的 3D 对象信息，从而提高机器人导航和与其周围环境交互的能力。

要在 ROS 中使用 YOLO 可视化 3D 点云：

以下是使用 Open3D 进行可视化的示例：

这种方法提供了分割对象的三维可视化，对于机器人应用中的导航和操作等任务非常有用。

**Examples:**

Example 1 (unknown):
```unknown
pip install ros_numpy
```

Example 2 (unknown):
```unknown
pip install ultralytics
```

Example 3 (python):
```python
import time

import rospy

from ultralytics import YOLO

detection_model = YOLO("yolo11m.pt")
segmentation_model = YOLO("yolo11m-seg.pt")
rospy.init_node("ultralytics")
time.sleep(1)
```

Example 4 (sql):
```sql
from sensor_msgs.msg import Image

det_image_pub = rospy.Publisher("/ultralytics/detection/image", Image, queue_size=5)
seg_image_pub = rospy.Publisher("/ultralytics/segmentation/image", Image, queue_size=5)
```

---

## 隔离分割对象 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/guides/isolating-segmentation-objects/

**Contents:**
- 分离分割对象
- 操作指南
  - 目标隔离选项
  - 使用黑色像素隔离：子选项
  - 使用透明像素隔离：子选项
- 完整示例代码
- 常见问题
  - 如何使用 Ultralytics YOLO11 隔离对象以进行分割任务？
  - 分割后，有哪些选项可用于保存隔离的对象？
  - 如何使用 Ultralytics YOLO11 将孤立对象裁剪到其边界框？

在执行分割任务后，有时需要从推理结果中提取孤立的对象。本指南提供了一个通用方法，说明如何使用 Ultralytics 预测模式来实现此目的。

观看： 如何使用 Ultralytics YOLO segment 和 Python 中的 OpenCV 移除背景并分离目标 🚀

请参阅Ultralytics 快速入门安装部分，以快速了解如何安装所需的库。

加载模型并运行 predict() source 上的 method。

有关分割模型的更多信息，请访问 分割任务 页面。要了解更多关于 predict() method，请参阅 Predict 模式 文档的章节。

现在，迭代处理结果和轮廓。对于希望将图像保存到文件的workflow，可以使用源图像 base-name 以及检测 class-label 被检索以供后续使用（可选）。

单个图像只会迭代第一个循环一次。仅具有单个检测的单个图像将仅迭代每个循环一次。

首先从源图像生成二元掩码，然后在掩码上绘制填充轮廓。这将允许对象与其他图像部分隔离。来自的示例 bus.jpg 针对其中一个检测到的 person 类对象的显示在右侧。

有关更多信息 c.masks.xy 参见 预测模式中的掩码部分.

此处，这些值被转换为 np.int32 为了兼容 drawContours() function，来自 OpenCV.

OpenCV drawContours() function 期望轮廓的形状为 [N, 1, 2] 下方展开部分的更多详细信息。

- c.masks.xy ：：以格式提供掩码轮廓点的坐标 (x, y)。更多详情，请参考 预测模式中的掩码部分。 - .pop() ：：作为 masks.xy 是一个包含单个元素的列表，该元素使用以下方式提取 pop() 方法。 - .astype(np.int32) ：：使用 masks.xy 将返回数据类型 float32，但这将与 OpenCV 不兼容 drawContours() function，因此这将更改数据类型为 int32 为了兼容性。 - .reshape(-1, 1, 2) ：：将数据重新格式化为所需的形状 [N, 1, 2] 其中 N 是轮廓点的数量，每个点由一个条目表示 1，条目由以下部分组成 2 值。这个 -1 表示此维度上的值的数量是灵活的。

- 封装 contour 方括号内的变量， [contour]，发现在测试过程中能有效地生成所需的轮廓掩码。 - 该值 -1 为以下对象指定 drawContours() 参数指示函数绘制图像中存在的所有轮廓。 - tuple (255, 255, 255) 表示白色，这是在此二元掩码中绘制轮廓所需的颜色。 - 添加了 cv2.FILLED 将以相同的颜色填充轮廓边界内的所有像素，在本例中，所有封闭的像素都将是白色。 - 参见 OpenCV文档关于 drawContours() 更多信息。

接下来，对于从此时开始如何处理图像，以及每个图像的后续选项，有 2 个选项。

首先，二值掩码会从单通道图像转换为三通道图像。此转换对于后续将掩码与原始图像组合的步骤是必需的。两个图像必须具有相同的通道数，才能与混合操作兼容。

原始图像和三通道二值掩码使用 OpenCV 函数进行合并 bitwise_and()。此操作保留 仅 大于零的像素值 (> 0) 来自两张图像。由于掩码像素大于零 (> 0) 仅 在轮廓区域内，原始图像中剩余的像素是与轮廓重叠的像素。

需要执行其他步骤才能裁剪图像以仅包含对象区域。

字段 c.boxes.xyxy.cpu().numpy() 调用会检索边界框作为 NumPy 数组在 xyxy 格式，其中 xmin, ymin, xmax和 ymax 表示边界框矩形的坐标。 见 预测模式中的“框”部分 了解更多详情。

字段 squeeze() 该操作会移除NumPy数组中任何不必要的维度，确保其具有预期的形状。

使用以下方法转换坐标值 .astype(np.int32) 将框坐标数据类型从以下类型更改为 float32 到 int32，使其兼容使用索引切片进行图像裁剪。

最后，使用索引切片从图像中裁剪边界框区域。边界由以下因素定义 [ymin:ymax, xmin:xmax] 检测边界框的坐标。

需要执行其他步骤才能裁剪图像以仅包含对象区域。

当使用时 c.boxes.xyxy.cpu().numpy()，边界框将作为 NumPy 数组返回，使用 xyxy 框坐标格式，对应于点 xmin, ymin, xmax, ymax 有关边界框（矩形），请参见 预测模式中的“框”部分 更多信息。

添加 squeeze() 确保从 NumPy 数组中移除任何多余的维度。

使用以下方法转换坐标值 .astype(np.int32) 将框坐标数据类型从以下类型更改为 float32 到 int32 这将在使用索引切片裁剪图像时兼容。

最后，使用索引切片裁剪边界框的图像区域，其中边界使用以下方式设置 [ymin:ymax, xmin:xmax] 检测边界框的坐标。

这是 Ultralytics 库的一个内置功能。请参阅 save_crop 参数用于 Predict 模式推理参数 详情请见。

接下来做什么完全取决于您作为开发人员。 这里展示了一个可能的后续步骤的基本示例（将图像保存到文件以供将来使用）。

这里，前一节中的所有步骤都组合成一个代码块。为了重复使用，最好定义一个函数来执行包含在其中的部分或全部命令 for循环，但这部分内容留给读者自行完成。

要使用 Ultralytics YOLO11 隔离对象，请按照以下步骤操作：

有关更多信息，请参阅预测模式和分割任务指南。

Ultralytics YOLO11 提供了两种主要的保存隔离对象的方式：

了解更多关于 预测模式 文档中的边界框结果。

Ultralytics YOLO11 提供：

在分割任务文档中，了解使用 YOLO 的优势。

是的，这是 Ultralytics YOLO11 中的一个内置功能。使用 save_crop 中的参数 predict() method。例如：

阅读更多关于 save_crop 中的参数 Predict 模式推理参数 部分。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a model
model = YOLO("yolo11n-seg.pt")

# Run inference
results = model.predict()
```

Example 2 (unknown):
```unknown
'ultralytics/assets/bus.jpg'
'ultralytics/assets/zidane.jpg'
```

Example 3 (typescript):
```typescript
from pathlib import Path

import numpy as np

# (2) Iterate detection results (helpful for multiple images)
for r in results:
    img = np.copy(r.orig_img)
    img_name = Path(r.path).stem  # source image base-name

    # Iterate each object contour (multiple detections)
    for ci, c in enumerate(r):
        # (1) Get detection class name
        label = c.names[c.boxes.cls.tolist().pop()]
```

Example 4 (typescript):
```typescript
import cv2

# Create binary mask
b_mask = np.zeros(img.shape[:2], np.uint8)

# (1) Extract contour result
contour = c.masks.xy.pop()
# (2) Changing the type
contour = contour.astype(np.int32)
# (3) Reshaping
contour = contour.reshape(-1, 1, 2)


# Draw contour onto mask
_ = cv2.drawContours(b_mask, [contour], -1, (255, 255, 255), cv2.FILLED)
```

---

## 在 Docker 中开始使用 YOLOv5 🚀 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/yolov5/environments/docker_image_quickstart_tutorial/

**Contents:**
- 开始在 Docker 中使用 YOLOv5 🚀
- 准备工作
  - 设置 NVIDIA Container Toolkit (GPU 用户)
  - 使用 Docker 验证 NVIDIA 运行时
- 步骤 1：拉取 YOLOv5 Docker 镜像
- 步骤 2：运行 Docker 容器
  - 仅使用 CPU
  - 使用 GPU
  - 挂载本地目录
- 步骤 3：在 Docker 容器中使用 YOLOv5 🚀

欢迎来到 Ultralytics YOLOv5 Docker 快速入门指南！本教程提供了在 Docker 容器中设置和运行 YOLOv5 的分步说明。使用 Docker 使您能够在隔离、一致的环境中运行 YOLOv5，从而简化了跨不同系统的部署和依赖管理。这种方法利用容器化将应用程序及其依赖项打包在一起。

对于其他的设置方法，请参考我们的 Colab Notebook, GCP 深度学习 VM或 Amazon AWS 指南。有关 Ultralytics 模型 Docker 使用的总体概述，请参阅 Ultralytics Docker 快速入门指南.

首先，通过运行以下命令验证您的 NVIDIA 驱动程序是否已正确安装：

此命令应显示有关您的 GPU 以及已安装的驱动程序版本的信息。

接下来，安装 NVIDIA Container Toolkit。以下命令是基于 Debian 的系统（如 Ubuntu）和基于 RHEL 的系统（如 Fedora/CentOS）的典型命令，但请参阅上面链接的官方指南，以获取特定于您的发行版的说明：

更新软件包列表并安装 nvidia-container-toolkit 软件包：

安装最新版本的 nvidia-container-toolkit：

或者，您可以通过设置以下内容来安装特定版本的 nvidia-container-toolkit NVIDIA_CONTAINER_TOOLKIT_VERSION 环境变量：

更新软件包列表并安装 nvidia-container-toolkit 软件包：

或者，您可以通过设置以下内容来安装特定版本的 nvidia-container-toolkit NVIDIA_CONTAINER_TOOLKIT_VERSION 环境变量：

运行 docker info | grep -i runtime 以确保 nvidia 出现在运行时列表中：

您应该会看到 nvidia 被列为可用的运行时之一。

Ultralytics 在以下位置提供官方 YOLOv5 镜像： 选择官方。 latest tag 跟踪最新的存储库提交，确保您始终获得最新版本。使用以下命令拉取镜像：

您可以在 Ultralytics YOLOv5 Docker Hub 存储库中浏览所有可用的图像。

要仅使用 CPU 运行交互式容器实例，请使用 -it 标志。这个 --ipc=host 标志允许共享主机 IPC 命名空间，这对于共享内存访问非常重要。

要在容器内启用 GPU 访问，请使用 --gpus 标志。这需要正确安装 NVIDIA 容器工具包。

有关命令选项的更多详细信息，请参阅Docker run 参考。

要在容器内使用你的本地文件（数据集、模型权重等），请使用 -v 标志将主机目录挂载到容器中：

替换 /path/on/host 替换为您机器上的实际路径，并且 /path/in/container 替换为 Docker 容器内的所需路径（例如， /usr/src/datasets）。

您现在位于正在运行的 YOLOv5 Docker 容器中！从这里，您可以执行标准的 YOLOv5 命令，以进行各种 机器学习和深度学习任务，例如对象检测。

了解更多关于评估指标的信息，例如Precision、Recall和mAP。了解不同的导出格式，例如ONNX、CoreML和TFLite，并探索各种模型部署选项。记住要有效管理您的模型权重。

您已成功YOLOv5 Docker容器YOLOv5 设置并运行YOLOv5 。

**Examples:**

Example 1 (unknown):
```unknown
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list \
  | sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' \
    | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```

Example 2 (sql):
```sql
sudo apt-get update
```

Example 3 (unknown):
```unknown
sudo apt-get install -y nvidia-container-toolkit \
  nvidia-container-toolkit-base libnvidia-container-tools \
  libnvidia-container1
```

Example 4 (bash):
```bash
export NVIDIA_CONTAINER_TOOLKIT_VERSION=1.17.8-1
sudo apt-get install -y \
  nvidia-container-toolkit=${NVIDIA_CONTAINER_TOOLKIT_VERSION} \
  nvidia-container-toolkit-base=${NVIDIA_CONTAINER_TOOLKIT_VERSION} \
  libnvidia-container-tools=${NVIDIA_CONTAINER_TOOLKIT_VERSION} \
  libnvidia-container1=${NVIDIA_CONTAINER_TOOLKIT_VERSION}
```

---

## YOLOv5 快速入门 🚀 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/yolov5/quickstart_tutorial/

**Contents:**
- YOLOv5 快速入门 🚀
- 安装
- 使用 PyTorch Hub 进行推理
- 使用 detect.py 进行推理
- 训练
- 评论

与 Ultralytics YOLOv5 一起踏上实时目标检测的动态领域！本指南旨在作为人工智能爱好者和专业人士掌握 YOLOv5 的综合起点。从初始设置到高级训练技术，我们都能满足您的需求。在本指南结束时，您将掌握使用最先进的深度学习方法自信地将 YOLOv5 实施到您的项目中的知识。让我们点燃引擎，翱翔进入 YOLOv5！

克隆 YOLOv5 仓库 并建立环境，为启动做好准备。这确保安装所有必需的 requirements。检查您是否已准备好 Python>=3.8.0 和 PyTorch>=1.8 以便启动。这些基础工具对于有效运行 YOLOv5 至关重要。

体验 YOLOv5 PyTorch Hub 推理的简易性，模型 可以从最新的 YOLOv5 版本 中无缝下载。此方法利用 PyTorch 的强大功能，简化了模型加载和执行过程，从而轻松获得预测结果。

Harness detect.py 用于多功能 推理 来自各种来源。它会自动获取 模型 来自最新的 YOLOv5 发布 并轻松保存结果。此脚本非常适合命令行使用，并将 YOLOv5 集成到更大的系统中，支持图像、视频、目录、网络摄像头，甚至 实时流.

复现 YOLOv5 COCO 数据集 通过遵循以下步骤进行基准测试 训练说明 如下。必要的 模型 和 数据集 （例如 coco128.yaml 或完整版 coco.yaml）直接从最新的 YOLOv5 中提取 发布。在V100上训练YOLOv5n/s/m/l/x GPU 通常分别需要 1/2/4/6/8 天（请注意 多 GPU 训练 设置工作更快）。通过使用尽可能高的值来最大化性能 --batch-size 或使用 --batch-size -1 用于 YOLOv5 自动批处理 功能，可自动找到最佳 批次大小。以下的批处理大小是V100-16GB GPU的理想选择。请参考我们的 配置指南 有关模型配置文件的详细信息（*.yaml）。

总而言之，YOLOv5 不仅是用于对象检测的先进工具，而且证明了机器学习在通过视觉理解改变我们与世界互动方式方面的强大作用。当您阅读本指南并开始将 YOLOv5 应用于您的项目时，请记住，您正处于一场技术革命的最前沿，有能力在计算机视觉领域取得显著成就。如果您需要更多见解或来自其他有远见者的支持，欢迎访问我们的 GitHub 存储库，这里汇聚了一个蓬勃发展的开发者和研究人员社区。探索更多资源，例如 Ultralytics HUB，无需代码即可进行数据集管理和模型训练，或者查看我们的解决方案页面，获取实际应用和灵感。继续探索，不断创新，享受 YOLOv5 的奇迹。祝您检测愉快！🌠🔍

**Examples:**

Example 1 (unknown):
```unknown
git clone https://github.com/ultralytics/yolov5 # clone repository
cd yolov5
pip install -r requirements.txt # install dependencies
```

Example 2 (python):
```python
import torch

# Model loading
model = torch.hub.load("ultralytics/yolov5", "yolov5s")  # Can be 'yolov5n' - 'yolov5x6', or 'custom'

# Inference on images
img = "https://ultralytics.com/images/zidane.jpg"  # Can be a file, Path, PIL, OpenCV, numpy, or list of images

# Run inference
results = model(img)

# Display results
results.print()  # Other options: .show(), .save(), .crop(), .pandas(), etc. Explore these in the Predict mode documentation.
```

Example 3 (unknown):
```unknown
python detect.py --weights yolov5s.pt --source 0                              # webcam
python detect.py --weights yolov5s.pt --source image.jpg                      # image
python detect.py --weights yolov5s.pt --source video.mp4                      # video
python detect.py --weights yolov5s.pt --source screen                         # screenshot
python detect.py --weights yolov5s.pt --source path/                          # directory
python detect.py --weights yolov5s.pt --source list.txt                       # list of images
python detect.py --weights yolov5s.pt --source list.streams                   # list of streams
python detect.py --weights yolov5s.pt --source 'path/*.jpg'                   # glob pattern
python detect.py --weights yolov5s.pt --source 'https://youtu.be/LNwODJXcvt4' # YouTube video
python detect.py --weights yolov5s.pt --source 'rtsp://example.com/media.mp4' # RTSP, RTMP, HTTP stream
```

Example 4 (markdown):
```markdown
# Train YOLOv5n on COCO128 for 3 epochs
python train.py --data coco128.yaml --epochs 3 --weights yolov5n.pt --batch-size 128

# Train YOLOv5s on COCO for 300 epochs
python train.py --data coco.yaml --epochs 300 --weights '' --cfg yolov5s.yaml --batch-size 64

# Train YOLOv5m on COCO for 300 epochs
python train.py --data coco.yaml --epochs 300 --weights '' --cfg yolov5m.yaml --batch-size 40

# Train YOLOv5l on COCO for 300 epochs
python train.py --data coco.yaml --epochs 300 --weights '' --cfg yolov5l.yaml --batch-size 24

# Train YOLOv5x on COCO for 300 epochs
python train.py --data coco.yaml --epochs 300 --weights '' --cfg yolov5x.yaml --batch-size 16
```

---

## 快速入门指南：Seeed Studio reCamera 与 Ultralytics YOLO11

**URL:** https://docs.ultralytics.com/zh/integrations/seeedstudio-recamera/

**Contents:**
- 快速入门指南：Seeed Studio reCamera 与 Ultralytics YOLO11
- 为什么选择 reCamera？
- reCamera 的快速硬件设置
- 使用预安装的 YOLO11 模型进行推理
- 导出为 cvimodel：转换您的 YOLO11 模型
  - 导出到 ONNX
    - 安装
    - 用法
  - 将 ONNX 导出为 MLIR 和 cvimodel
- 基准测试

reCamera 在 YOLO Vision 2024 (YV24)，Ultralytics 年度混合活动中被引入到 AI 社区。它主要为 边缘 AI 应用 而设计，提供强大的处理能力和轻松的部署。

凭借对多样化硬件配置和开源资源的支持，它成为在边缘进行创新计算机视觉解决方案原型开发和部署的理想平台。

reCamera 系列专为边缘 AI 应用而设计，旨在满足开发者和创新者的需求。以下是它脱颖而出的原因：

RISC-V驱动的性能：其核心是SG200X处理器，它基于RISC-V架构构建，为边缘AI任务提供卓越的性能，同时保持能源效率。它具有每秒执行1万亿次操作（1 TOPS）的能力，可以轻松处理实时对象检测等高要求的任务。

优化视频技术: 支持先进的视频压缩标准，包括 H.264 和 H.265，以减少存储和带宽需求，同时不牺牲质量。HDR 图像、3D 降噪和镜头校正等功能可确保即使在具有挑战性的环境中也能获得专业的视觉效果。

节能双重处理: SG200X 处理复杂的 AI 任务，而较小的 8 位微控制器管理更简单的操作以节省电量，这使得 reCamera 非常适合电池供电或低功耗设置。

模块化和可升级设计: reCamera 采用模块化结构构建，由三个主要组件组成：核心板、传感器板和底板。这种设计使开发人员可以轻松更换或升级组件，从而确保灵活性和面向未来的项目。

请按照reCamera 快速入门指南进行设备的初始设置，例如将设备连接到 WiFi 网络并访问 Node-RED Web UI，以快速预览检测结果。

reCamera 预装了四个 Ultralytics YOLO11 模型，您可以直接在 Node-RED 仪表板中选择所需的模型。

步骤 1：如果已将 reCamera 连接到网络，请在 Web 浏览器中输入 reCamera 的 IP 地址以打开 Node-RED 仪表板。如果已通过 USB 将 reCamera 连接到 PC，则可以输入 192.168.42.1。在这里，您将看到默认加载的 YOLO11n 检测模型。

步骤 2：单击右下角的绿色圆圈以访问 Node-RED 流程编辑器。

步骤 3：点击 model 节点并单击 On Device.

第四步：选择四个预装的YOLO11n模型之一并点击 Done。例如，这里我们将选择 YOLO11n Pose

步骤 5：点击 Deploy 并在完成部署后，点击 Dashboard.

现在您将能够看到 YOLO11n 姿势估计模型在运行了！

如果您想将自定义训练的YOLO11模型与reCamera配合使用，请遵循以下步骤。

在这里，我们首先将一个 PyTorch 模型到 ONNX 然后将其转换为 MLIR 模型格式。最后， MLIR 将被转换为 cvimodel 在设备上运行推理。

将 Ultralytics YOLO11 模型导出为 ONNX 模型格式。

有关安装过程的详细说明和最佳实践，请查看我们的 Ultralytics 安装指南。如果在为 YOLO11 安装所需软件包时遇到任何困难，请查阅我们的 常见问题指南以获取解决方案和提示。

有关导出过程的更多详细信息，请访问Ultralytics 文档页面上的导出。

获得 ONNX 模型后，请参阅转换和量化 AI 模型页面，将 ONNX 模型转换为 MLIR，然后再转换为 cvimodel。

我们正在积极努力将 reCamera 支持直接添加到 Ultralytics 包中，并且很快就会推出。在此期间，请查看我们的博客，了解更多关于将 Ultralytics YOLO 模型与 Seeed Studio 的 reCamera 集成的见解。

reCamera 先进的计算机视觉能力和模块化设计使其适用于广泛的实际场景，帮助开发者和企业轻松应对独特的挑战。

跌倒检测：reCamera专为安全和医疗保健应用设计，可以实时detect跌倒，非常适合老年护理、医院以及需要快速响应的工业环境。

个人防护设备检测：reCamera 可用于通过实时检测 PPE 合规性来确保工作场所安全。它有助于识别工人是否佩戴头盔、手套或其他安全装备，从而降低工业环境中的风险。

火灾检测: reCamera的实时处理能力使其成为工业和住宅区域火灾检测的绝佳选择，提供早期预警以防止潜在的灾难。

废物检测: 它还可以用于废物检测应用，使其成为环境监测和废物管理的绝佳工具。

汽车零件检测: 在制造业和汽车工业中，它有助于检测和分析汽车零件，以进行质量控制、装配线监控和库存管理。

要首次设置您的 reCamera，请按照以下步骤操作：

是的，您可以将自定义训练的 YOLO11 模型与 reCamera 结合使用。该过程包括：

有关详细说明，请参阅 转换和量化 AI 模型 指南。

与需要外部硬件进行处理的传统 IP 摄像头不同，reCamera：

这些特性使得reCamera成为边缘AI应用的独立解决方案，无需额外的外部处理硬件。

**Examples:**

Example 1 (unknown):
```unknown
pip install ultralytics
```

Example 2 (python):
```python
from ultralytics import YOLO

# Load the YOLO11 model
model = YOLO("yolo11n.pt")

# Export the model to ONNX format
model.export(format="onnx", opset=14)  # creates 'yolo11n.onnx'
```

Example 3 (markdown):
```markdown
# Export a YOLO11n PyTorch model to ONNX format
yolo export model=yolo11n.pt format=onnx opset=14 # creates 'yolo11n.onnx'
```

---

## Ultralytics Conda 快速入门指南 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/guides/conda-quickstart/

**Contents:**
- Ultralytics Conda 快速入门指南
- 您将学到的内容
- 准备工作
- 设置 Conda 环境
- 安装 Ultralytics
  - 关于 CUDA 环境的说明
- 使用 Ultralytics
- Ultralytics Conda Docker 镜像
- 使用 Libmamba 加速安装
  - 如何启用 Libmamba

本指南全面介绍了如何为您的 Ultralytics 项目设置 Conda 环境。Conda 是一个开源的软件包和环境管理系统，为安装软件包和依赖项提供了 pip 的绝佳替代方案。其隔离环境使其特别适合数据科学和机器学习工作。有关更多详细信息，请访问 Anaconda 上的 Ultralytics Conda 软件包，并查看 GitHub 上 Ultralytics feedstock 存储库以获取软件包更新。

首先，让我们创建一个新的 Conda 环境。打开您的终端并运行以下命令：

您可以从 conda-forge 频道安装 Ultralytics 包。执行以下命令：

如果您在启用 CUDA 的环境中工作，最好安装 ultralytics, pytorch和 pytorch-cuda 一起以解决任何冲突：

安装 Ultralytics 后，您现在可以开始使用其强大的功能进行 对象检测、实例分割 等。例如，要预测图像，您可以运行：

如果您更喜欢使用 Docker，Ultralytics 提供了包含 Conda 环境的 Docker 镜像。您可以从 DockerHub 拉取这些镜像。

拉取最新的 Ultralytics 镜像：

如果您正在寻找 加速软件包安装 在 Conda 中处理，您可以选择使用 libmamba，一个快速、跨平台且具有依赖感知能力的包管理器，可以作为 Conda 默认求解器的替代方案。

启用 libmamba 作为 Conda 的求解器，您可以执行以下步骤：

首先，安装 conda-libmamba-solver package。如果您的 Conda 版本是 4.11 或更高版本，则可以跳过此步骤，因为 libmamba 默认情况下包含。

接下来，配置 Conda 以使用 libmamba 作为求解器：

就这样！您的 Conda 安装现在将使用 libmamba 作为求解器，这应该会加快软件包的安装过程。

您已成功设置Conda环境，安装了Ultralytics软件包，现在可以探索其功能。有关更高级的教程和示例，请参阅Ultralytics文档。

为 Ultralytics 项目设置 Conda 环境非常简单，可以确保顺畅的包管理。首先，使用以下命令创建一个新的 Conda 环境：

最后，从 conda-forge 频道安装 Ultralytics：

Conda 是一个强大的包和环境管理系统，相比于 pip 具有多个优势。它可以高效地管理依赖项，并确保所有必要的库都兼容。Conda 的隔离环境可以防止包之间的冲突，这在数据科学和机器学习项目中至关重要。此外，Conda 支持二进制包分发，从而加快了安装过程。

是的，您可以通过利用启用 CUDA 的环境来提高性能。确保安装 ultralytics, pytorch和 pytorch-cuda 一起以避免冲突：

此设置启用GPU加速，这对于诸如深度学习模型训练和推理之类的密集型任务至关重要。有关更多信息，请访问Ultralytics安装指南。

使用 Ultralytics Docker 镜像可确保环境的一致性和可重复性，从而消除“在我的机器上可以运行”的问题。这些镜像包含预配置的 Conda 环境，简化了设置过程。您可以使用以下命令拉取并运行最新的 Ultralytics Docker 镜像：

此方法非常适合在生产环境中部署应用程序或运行复杂的workflow，而无需手动配置。 了解更多关于Ultralytics Conda Docker镜像的信息。

您可以使用以下方法加快软件包安装过程 libmamba，一个快速的 Conda 依赖关系求解器。首先，安装 conda-libmamba-solver 软件包的更多详细信息：

然后配置 Conda 以使用 libmamba 作为求解器：

此设置提供更快、更高效的软件包管理。有关优化环境的更多提示，请阅读有关libmamba安装的信息。

**Examples:**

Example 1 (unknown):
```unknown
conda create --name ultralytics-env python=3.11 -y
```

Example 2 (unknown):
```unknown
conda activate ultralytics-env
```

Example 3 (unknown):
```unknown
conda install -c conda-forge ultralytics
```

Example 4 (unknown):
```unknown
conda install -c pytorch -c nvidia -c conda-forge pytorch torchvision pytorch-cuda=11.8 ultralytics
```

---

## 快速入门指南：带有 Ultralytics YOLO11 的 Raspberry Pi

**URL:** https://docs.ultralytics.com/zh/guides/raspberry-pi/

**Contents:**
- 快速入门指南：带有 Ultralytics YOLO11 的 Raspberry Pi
- 什么是 Raspberry Pi？
- Raspberry Pi 系列比较
- 什么是 Raspberry Pi OS？
- 将 Raspberry Pi OS 刷入 Raspberry Pi
- 设置 Ultralytics
  - 从 Docker 开始
  - 不使用 Docker 启动
    - 安装 Ultralytics 软件包
- 在 Raspberry Pi 上使用 NCNN

本综合指南详细介绍了如何在 Raspberry Pi 设备上部署 Ultralytics YOLO11。此外，它还展示了性能基准，以演示 YOLO11 在这些小型且功能强大的设备上的能力。

观看： Raspberry Pi 5 的更新和改进。

本指南已在运行最新 Raspberry Pi OS Bookworm (Debian 12) 的 Raspberry Pi 4 和 Raspberry Pi 5 上进行了测试。只要安装了相同的 Raspberry Pi OS Bookworm，将本指南用于较旧的 Raspberry Pi 设备（如 Raspberry Pi 3）预计也能正常工作。

Raspberry Pi 是一款小巧、经济实惠的单板计算机。它已广泛应用于各种项目和应用，从业余爱好者的家庭自动化到工业用途。Raspberry Pi 板能够运行各种操作系统，并提供 GPIO（通用输入/输出）引脚，可以轻松与传感器、执行器和其他硬件组件集成。它们有不同的型号，规格各不相同，但它们都具有低成本、紧凑和多功能的相同基本设计理念。

Raspberry Pi OS（前称 Raspbian）是一个类 Unix 操作系统，它基于 Debian GNU/Linux 发行版，专为 Raspberry Pi 基金会发布的 Raspberry Pi 系列紧凑型单板计算机而设计。Raspberry Pi OS 针对配备 ARM CPU 的 Raspberry Pi 进行了高度优化，并使用修改后的 LXDE 桌面环境以及 Openbox 堆叠窗口管理器。Raspberry Pi OS 正在积极开发中，重点是提高尽可能多的 Debian 软件包在 Raspberry Pi 上的稳定性和性能。

获得 Raspberry Pi 后，首先要做的是将 Raspberry Pi OS 刷入 micro-SD 卡，插入设备并启动到操作系统。请按照 Raspberry Pi 提供的详细入门文档操作，为首次使用做好设备准备。

在 Raspberry Pi 上设置 Ultralytics 包以构建您的下一个计算机视觉项目有两种方法。您可以选择其中任何一种。

在 Raspberry Pi 上开始使用 Ultralytics YOLO11 的最快方法是运行 Raspberry Pi 的预构建 Docker 镜像。

执行以下命令以拉取 Docker 容器并在 Raspberry Pi 上运行。这基于 arm64v8/debian Docker 镜像，其中包含 Python3 环境中的 Debian 12 (Bookworm)。

完成此操作后，请跳至在 Raspberry Pi 上使用 NCNN 部分。

在这里，我们将在 Raspberry Pi 上安装 Ultralytics 包以及可选依赖项，以便我们可以将 PyTorch 模型导出为其他不同的格式。

更新软件包列表，安装 pip 并升级到最新版本

安装 ultralytics 带有可选依赖项的 pip 软件包

在 Ultralytics 支持的所有模型导出格式中，NCNN 在使用 Raspberry Pi 设备时可提供最佳的推理性能，因为 NCNN 针对移动/嵌入式平台（如 ARM 架构）进行了高度优化。

PyTorch 格式的 YOLO11n 模型被转换为 NCNN，以使用导出的模型运行推理。

有关支持的导出选项的更多详细信息，请访问 Ultralytics 文档页面上的部署选项。

Ultralytics 团队在十种不同的模型格式上运行了 YOLO11 基准测试，测量了速度和 准确性：PyTorch、TorchScript、ONNX、OpenVINO、TF SavedModel、TF GraphDef、TF Lite、PaddlePaddle、MNN、NCNN。基准测试在 Raspberry Pi 5 上以 FP32 精度运行，默认输入图像大小为 640。

我们仅包含了 YOLO11n 和 YOLO11s 模型的基准测试，因为其他模型尺寸过大，无法在 Raspberry Pi 上运行，并且无法提供良好的性能。

下表显示了针对两种不同模型（YOLO11n、YOLO11s）在 10 种不同格式（PyTorch、TorchScript、ONNX、OpenVINO、TF SavedModel、TF GraphDef、TF Lite、PaddlePaddle、MNN、NCNN）下，于 Raspberry Pi 5 上运行的基准测试结果，提供了每种组合的状态、大小、mAP50-95(B) 指标和推理时间。

使用 Ultralytics 8.3.152 进行基准测试

要复现上述 Ultralytics 在所有导出格式上的基准测试，请运行以下代码：

请注意，基准测试结果可能因系统的具体硬件和软件配置而异，也可能因运行基准测试时系统的当前工作负载而异。为了获得最可靠的结果，请使用包含大量图像的数据集，例如： data='coco.yaml' （5000 张验证图像）。

当使用 Raspberry Pi 进行计算机视觉项目时，获取实时视频流以执行推理至关重要。Raspberry Pi 上的板载 MIPI CSI 连接器允许您连接官方 Raspberry PI 摄像头模块。在本指南中，我们使用了Raspberry Pi Camera Module 3 来获取视频流并使用 YOLO11 模型执行推理。

了解更多关于 Raspberry Pi 提供的不同摄像头模块，以及 如何开始使用 Raspberry Pi 摄像头模块。

Raspberry Pi 5 使用比 Raspberry Pi 4 更小的 CSI 连接器（15 针 vs 22 针），因此您需要一根 15 针转 22 针的适配器电缆才能连接到 Raspberry Pi Camera。

将摄像头连接到 Raspberry Pi 后，执行以下命令。您应该会看到来自摄像头的实时视频流，持续约 5 秒。

了解更多关于 rpicam-hello 在官方 Raspberry Pi 文档上的用法

使用树莓派摄像头在 YOLO11 模型上运行推理有两种方法。

我们可以使用 picamera2 预装在树莓派操作系统中，用于访问摄像头并在YOLO11模型上运行推理。

我们需要用 rpicam-vid 从连接的摄像头启动一个 TCP 流，这样我们就可以在稍后进行推理时使用这个流 URL 作为输入。执行以下命令以启动 TCP 流。

了解更多关于 rpicam-vid 在官方 Raspberry Pi 文档上的用法

如果您想更改图像/视频输入类型，请查看我们的推理源文档

为了在运行 YOLO11 的 Raspberry Pi 上实现最佳性能，需要遵循一些最佳实践。

当 Raspberry Pi 用于 24x7 全天候持续使用时，建议使用 SSD 作为系统盘，因为 SD 卡无法承受连续写入，可能会损坏。借助 Raspberry Pi 5 上的板载 PCIe 连接器，现在您可以使用适配器（例如 NVMe Base for Raspberry Pi 5）连接 SSD。

刷写 Raspberry Pi OS 时，您可以选择不安装桌面环境 (Raspberry Pi OS Lite)，这样可以节省设备上的一些 RAM，从而为计算机视觉处理留下更多空间。

如果您想在使用 Ultralytics YOLO11 模型在 Raspberry Pi 5 上运行时获得一点性能提升，您可以将 CPU 从其基本频率 2.4GHz 超频至 2.9GHz，并将 GPU 从 800MHz 超频至 1GHz。如果系统变得不稳定或崩溃，请以 100MHz 的增量降低超频值。确保安装适当的散热装置，因为超频会增加热量产生，并可能导致热节流。

d. 按 CTRL + X 保存并退出，然后按 Y，然后按 ENTER

您已成功在您的Raspberry Pi上设置YOLO。如需进一步学习和支持，请访问Ultralytics YOLO11 文档和Kashmir World Foundation。

本指南最初由 Daan Eeltink 为 Kashmir World Foundation 创建，该组织致力于使用 YOLO 来保护濒危物种。我们感谢他们在目标检测技术领域的开创性工作和教育重点。

有关 Kashmir World Foundation 活动的更多信息，您可以访问他们的网站。

要在没有 Docker 的 Raspberry Pi 上设置 Ultralytics YOLO11，请按照以下步骤操作：

有关详细说明，请参阅在没有 Docker 的情况下启动部分。

Ultralytics YOLO11 的 NCNN 格式针对移动和嵌入式平台进行了高度优化，非常适合在 Raspberry Pi 设备上运行 AI 任务。NCNN 通过利用 ARM 架构最大限度地提高推理性能，与其他格式相比，提供更快、更高效的处理。有关支持的导出选项的更多详细信息，请访问 Ultralytics 文档页面上的部署选项。

您可以使用 python 或 CLI 命令将 PyTorch YOLO11 模型转换为 NCNN 格式：

有关更多详细信息，请参见在 Raspberry Pi 上使用 NCNN部分。

与 Raspberry Pi 4 相比，这些增强功能有助于提高 Raspberry Pi 5 上 YOLO11 模型的性能基准。有关更多详细信息，请参阅Raspberry Pi 系列比较表。

有两种方法可以为 YOLO11 推理设置 Raspberry Pi 相机：

有关详细的设置说明，请访问摄像头推理部分。

**Examples:**

Example 1 (bash):
```bash
t=ultralytics/ultralytics:latest-arm64
sudo docker pull $t && sudo docker run -it --ipc=host $t
```

Example 2 (sql):
```sql
sudo apt update
sudo apt install python3-pip -y
pip install -U pip
```

Example 3 (unknown):
```unknown
pip install ultralytics[export]
```

Example 4 (unknown):
```unknown
sudo reboot
```

---

## AzureML 快速入门上的 Ultralytics YOLOv5 🚀 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/yolov5/environments/azureml_quickstart_tutorial/

**Contents:**
- AzureML 快速入门上的 Ultralytics YOLOv5 🚀
- 什么是 Azure？
- 什么是 Azure 机器学习 (AzureML)？
- 准备工作
- 创建计算实例
- 打开终端
- 设置并运行 YOLOv5
  - 1. 创建一个虚拟环境
  - 2. 克隆YOLOv5仓库
  - 3. 安装依赖包

欢迎来到 Ultralytics YOLOv5 Microsoft Azure 机器学习 (AzureML) 快速入门指南！本指南将引导您在 AzureML 计算实例上设置 YOLOv5，涵盖从创建虚拟环境到使用模型进行训练和运行推理的所有内容。

Azure 是 Microsoft 的全面 云计算 平台。它提供包括计算能力、数据库、分析工具、机器学习 功能和网络解决方案在内的大量服务。Azure 使组织能够通过 Microsoft 管理的数据中心来构建、部署和管理应用程序和服务，从而促进从本地基础设施到云的工作负载迁移。

Azure Machine Learning (AzureML) 是 Microsoft 专为开发、训练和部署机器学习模型而设计的专业云服务。它提供一个协作环境，其中的工具适用于所有技能水平的数据科学家和开发人员。主要功能包括 自动化机器学习 (AutoML)，用于模型创建的拖放式界面，以及一个强大的 Python SDK，用于更粒细地控制 ML 生命周期。AzureML 简化了将 预测建模 嵌入应用程序的过程。

要遵循本指南，您需要一个有效的 Azure 订阅 并访问 AzureML 工作区。如果您没有设置工作区，请参阅官方 Azure 文档创建一个。

AzureML 中的计算实例为数据科学家提供了一个基于云的托管工作站。

一旦您的计算实例运行，您可以直接从 AzureML studio 访问其终端。

现在，让我们设置环境并运行 Ultralytics YOLOv5。

最佳实践是使用虚拟环境来管理依赖项。我们将使用 Conda，它已预先安装在 AzureML 计算实例上。有关详细的 Conda 设置指南，请参阅 Ultralytics Conda 快速入门指南。

创建一个 Conda 环境 (例如， yolov5env）使用特定的 python 版本并激活它：

使用 Git 从 GitHub 克隆官方 Ultralytics YOLOv5 仓库：

安装必要的python软件包，这些软件包在 requirements.txt 文件。我们还安装了 ONNX 用于模型导出功能。

完成设置后，您现在可以训练、验证、执行推理并导出您的 YOLOv5 模型。

在诸如COCO128这样的数据集上训练模型。有关更多详细信息，请查阅训练模式文档。

验证训练模型的性能，使用精度（Precision）、召回率（Recall）和mAP等指标。有关选项，请参阅验证模式指南。

在新图像或视频上运行推理。请查阅预测模式文档，了解各种推理来源。

导出模型为不同的格式，如 ONNX、TensorRT或CoreML以进行部署。请参阅导出模式指南和ONNX 集成页面。

如果您喜欢交互式体验，则可以在 AzureML Notebook 中运行这些命令。您需要创建一个链接到您的 Conda 环境的自定义 IPython kernel。

创建内核后，刷新浏览器。当您打开或创建一个 .ipynb notebook 文件，从右上角的内核下拉菜单中选择您的新内核（“Python (yolov5env)”）。

Python单元格： Python单元格中的代码将使用选定的内容自动执行 yolov5env 内核。

Bash Cells: 要运行 shell 命令，请使用 %%bash 在单元格的开头使用 magic 命令。请记住在每个 bash 单元格中激活您的 Conda 环境，因为它们不会自动继承 notebook 的内核环境上下文。

恭喜！您已成功在 AzureML 上设置并运行 Ultralytics YOLOv5。如需进一步探索，请考虑查看其他 Ultralytics 集成或详细的 YOLOv5 文档。您可能还会发现 AzureML 文档对于分布式训练或将模型部署为端点等高级场景很有用。

**Examples:**

Example 1 (unknown):
```unknown
conda create --name yolov5env -y python=3.10 # Create a new Conda environment
conda activate yolov5env                     # Activate the environment
conda install pip -y                         # Ensure pip is installed
```

Example 2 (sql):
```sql
git clone https://github.com/ultralytics/yolov5 # Clone the repository
cd yolov5                                       # Navigate into the directory
# Initialize submodules (if any, though YOLOv5 typically doesn't require this step)
# git submodule update --init --recursive
```

Example 3 (unknown):
```unknown
pip install -r requirements.txt # Install core dependencies
pip install "onnx>=1.12.0"      # Install ONNX for exporting
```

Example 4 (julia):
```julia
# Start training using yolov5s pretrained weights on the COCO128 dataset
python train.py --data coco128.yaml --weights yolov5s.pt --img 640 --epochs 10 --batch 16
```

---

## 快速入门：安装 Ultralytics HUB-SDK - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/hub/sdk/quickstart/

**Contents:**
- 快速入门：安装 Ultralytics HUB-SDK
- 准备工作
- 安装
  - 从 PyPI 安装
- 初始化 HUBClient
  - 选项 1：使用 API 密钥
  - 选项 2：使用电子邮件和密码
  - 创建 HUBClient 对象
- 评论

欢迎！🎉 本指南为经验丰富的开发者和初学者提供了安装和初始化 Ultralytics HUB-SDK 的分步说明。

您可以使用以下方法之一安装 HUB-SDK：

为了稳定且易于安装，请从以下位置安装 HUB-SDK 的最新版本 PyPI 使用 pip:

此命令会将 HUB-SDK 的稳定版本下载并安装到您的 Python 环境中。这是最快的入门方式。

安装后，初始化 HUBClient 用于与 Ultralytics HUB 生态系统交互。有两种可用的身份验证方法：

替换 <YOUR-API-KEY> 使用来自 Ultralytics 的实际 API 密钥。此方法是安全 API 访问的首选方法。您可以在您的 Ultralytics HUB 设置页面.

替换 <YOUR-EMAIL> 和 <YOUR-PASSWORD> 使用您的 Ultralytics 登录凭据。

创建一个 HUBClient 使用您选择的身份验证方法的对象：

使用 HUBClient 实例已初始化，您现在可以使用 Ultralytics 服务执行各种操作。The HUBClient 类扩展了身份验证功能，并充当与 Ultralytics HUB 服务交互的网关。有关更多详细信息，请参见 hub_sdk.hub_client.HUBClient 参考文档.

一切就绪！🚀 HUB-SDK已安装并 HUBClient 初始化后，您现在可以探索 Ultralytics 生态系统的各项功能。有关更多指导，请参阅 Ultralytics HUB-SDK 文档 如果您遇到任何问题，我们的支持团队随时准备为您提供帮助。 祝您编码愉快！

**Examples:**

Example 1 (unknown):
```unknown
pip install hub-sdk
```

Example 2 (json):
```json
credentials = {"api_key": "<YOUR-API-KEY>"}
```

Example 3 (json):
```json
credentials = {"email": "<YOUR-EMAIL>", "password": "<YOUR-PASSWORD>"}
```

Example 4 (json):
```json
from hub_sdk import HUBClient

credentials = {"api_key": "<YOUR-API-KEY>"}  # API key
client = HUBClient(credentials)
```

---

## YOLO11 🚀 on AzureML - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/guides/azureml-quickstart/

**Contents:**
- AzureML 上的 YOLO11 🚀
- 什么是 Azure？
- 什么是 Azure 机器学习 (AzureML)？
- AzureML 如何使 YOLO 用户受益？
- 准备工作
- 创建计算实例
- 从终端快速开始
  - 创建 virtualenv
  - 执行 YOLO11 任务
- 从 Notebook 快速开始

Azure 是 Microsoft 的 云计算 平台，旨在帮助组织将其工作负载从本地数据中心迁移到云端。凭借包括计算、数据库、分析、机器学习和网络在内的全方位云服务，用户可以选择这些服务来开发和扩展新应用程序，或在公共云中运行现有应用程序。

Azure 机器学习（通常称为 AzureML）是一种完全托管的云服务，使数据科学家和开发人员能够有效地将预测分析嵌入到他们的应用程序中，帮助组织使用海量数据集并将云的所有优势带到机器学习中。AzureML 提供了各种服务和功能，旨在使机器学习易于访问、易于使用和可扩展。它提供诸如自动化机器学习、拖放模型训练以及强大的 Python SDK 等功能，以便开发人员可以充分利用他们的机器学习模型。

对于 YOLO（You Only Look Once）的用户，AzureML 提供了一个强大、可扩展且高效的平台，用于训练和部署机器学习模型。无论您是希望运行快速原型还是扩展以处理更广泛的数据，AzureML 灵活且用户友好的环境都提供了各种工具和服务来满足您的需求。您可以利用 AzureML 来：

在后续章节中，您将找到一个快速入门指南，详细介绍如何使用 AzureML 从计算终端或笔记本电脑运行 YOLO11 对象检测模型。

在开始之前，请确保您有权访问 AzureML 工作区。如果没有，您可以按照 Azure 的官方文档创建一个新的 AzureML 工作区。此工作区充当管理所有 AzureML 资源的中心位置。

从您的 AzureML 工作区中，选择“计算”>“计算实例”>“新建”，然后选择具有所需资源的实例。

创建一个使用您偏好 Python 版本的 conda 虚拟环境，并在其中安装 pip。Python 3.13.1 目前在 AzureML 中存在依赖问题，因此请改用 Python 3.12。

使用 0.01 的初始 learning_rate 训练一个检测模型 10 个 epochs：

您可以在此处找到更多关于使用 Ultralytics CLI 的说明。

在您的计算终端中，使用 Python 3.12 创建一个新的 ipykernel，您的笔记本将使用它来管理依赖项：

关闭您的终端并创建一个新的笔记本。在您的笔记本中，选择新创建的内核。

然后打开一个 notebook 单元并安装所需的依赖项：

请注意，您需要运行 source activate yolo11env 在每个 %%bash 单元格，以确保该单元格使用预期的环境。

使用 Ultralytics CLI 运行一些预测：

或者使用 Ultralytics python 接口，例如训练模型：

您可以按照上面终端部分中的描述，使用 Ultralytics CLI 或 Python 接口来运行 YOLO11 任务。

通过遵循这些步骤，您应该能够在 AzureML 上快速运行 YOLO11 以进行快速试验。对于更高级的用法，您可以参考本指南开头链接的完整 AzureML 文档。

本指南旨在向您介绍如何在 AzureML 上启动并运行 YOLO11。然而，这仅仅是 AzureML 功能的冰山一角。要更深入地了解并充分发挥 AzureML 在机器学习项目中的潜力，请考虑探索以下资源：

在 AzureML 上运行 YOLO11 进行模型训练涉及以下几个步骤：

创建计算实例: 从您的 AzureML 工作区，导航到计算 > 计算实例 > 新建，然后选择所需的实例。

设置环境：启动您的计算实例，打开终端，并创建 Conda 环境。设置您的 Python 版本（Python 3.13.1 暂不支持）：

运行 YOLO11 任务：使用 Ultralytics CLI 训练您的模型：

更多详情，您可以参考使用 Ultralytics CLI 的说明。

AzureML 提供了一个强大而高效的生态系统，用于训练 YOLO11 模型：

这些优势使 AzureML 成为从快速原型设计到大规模部署的理想平台。有关更多提示，请查看 AzureML Jobs。

解决 YOLO11 在 AzureML 上的常见问题可能涉及以下步骤：

如需其他指导，请查看我们的 YOLO 常见问题 文档。

是的，AzureML 允许您无缝使用 Ultralytics CLI 和 Python 接口：

CLI: 适合快速完成任务以及直接从终端运行标准脚本。

Python 接口: 适用于需要自定义编码以及在 notebooks 中集成的更复杂任务。

有关分步说明，请参阅 CLI 快速入门指南 和 Python 快速入门指南。

与同类对象检测模型相比，Ultralytics YOLO11 具有以下几个独特的优势：

要了解更多关于 YOLO11 的功能，请访问 Ultralytics YOLO 页面以获取详细信息。

**Examples:**

Example 1 (unknown):
```unknown
conda create --name yolo11env -y python=3.12
conda activate yolo11env
conda install pip -y
```

Example 2 (unknown):
```unknown
cd ultralytics
pip install -r requirements.txt
pip install ultralytics
pip install onnx
```

Example 3 (unknown):
```unknown
yolo predict model=yolo11n.pt source='https://ultralytics.com/images/bus.jpg'
```

Example 4 (unknown):
```unknown
yolo train data=coco8.yaml model=yolo11n.pt epochs=10 lr0=0.01
```

---
