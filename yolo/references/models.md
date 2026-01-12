# Yolo - Models

**Pages:** 24

---

## 高级数据可视化：使用 Ultralytics YOLO11 的热图 🚀

**URL:** https://docs.ultralytics.com/zh/guides/heatmaps/

**Contents:**
- 高级 数据可视化：使用 Ultralytics YOLO11 的热图 🚀
- 热图简介
- 为什么选择热图进行数据分析？
- 实际应用
  - Heatmap() 参数
    - 热图 COLORMAPs
- 热图在 Ultralytics YOLO11 中的工作原理
- 常见问题
  - Ultralytics YOLO11 如何生成热图？其优势是什么？
  - 我可以使用 Ultralytics YOLO11 同时执行对象跟踪并生成热图吗？

使用 Ultralytics YOLO11 生成的热图将复杂数据转换为充满活力的颜色编码矩阵。此可视化工具采用一系列颜色来表示不同的数据值，其中暖色调表示较高的强度，而冷色调表示较低的值。热图擅长可视化复杂的数据模式、相关性和异常，从而为跨不同领域的数据解释提供了一种易于访问且引人入胜的方法。

观看： 使用 Ultralytics YOLO11 的热图

使用 Ultralytics YOLO 的热图

这是一个包含以下内容的表格 Heatmap 参数：

您还可以应用不同的 track 中的参数 Heatmap 解决方案。

这些色彩图通常用于以不同的颜色表示形式可视化数据。

Ultralytics YOLO11 中的 热图解决方案扩展了 ObjectCounter 类，以生成和可视化视频流中的移动模式。初始化后，该解决方案会创建一个空白热图层，当对象在帧中移动时，该图层会更新。

结果是一个随时间构建的动态可视化，揭示了视频数据中的交通模式、人群移动或其他空间行为。

Ultralytics YOLO11 通过将复杂数据转换为颜色编码矩阵来生成热图，其中不同的色调代表数据强度。热图可以更轻松地可视化数据中的模式、相关性和异常。较暖的色调表示较高的值，而较冷的色调表示较低的值。主要优势包括直观的数据分布可视化、高效的模式检测以及增强的空间分析以进行决策。有关更多详细信息和配置选项，请参阅热图配置部分。

是的，Ultralytics YOLO11 支持同时进行对象跟踪和热图生成。这可以通过其 Heatmap 与对象跟踪模型集成的解决方案来实现。为此，您需要初始化热图对象并使用 YOLO11 的跟踪功能。这是一个简单的例子：

Ultralytics YOLO11 热图专为与其目标检测和跟踪模型集成而设计，为实时数据分析提供端到端解决方案。与 OpenCV 或 Matplotlib 等通用可视化工具不同，YOLO11 热图针对性能和自动化处理进行了优化，支持持久跟踪、衰减因子调整和实时视频叠加等功能。有关 YOLO11 独特功能的更多信息，请访问Ultralytics YOLO11 简介。

您可以通过在 YOLO 模型的 track() 方法中指定所需的类来可视化特定的对象类别。例如，如果您只想可视化汽车和人（假设它们的类索引为 0 和 2），您可以设置 classes 参数。

Ultralytics YOLO11 提供先进目标检测和实时热力图生成的无缝集成，使其成为寻求更有效数据可视化的企业的理想选择。主要优势包括直观的数据分布可视化、高效的模式检测以及增强的空间分析，以实现更好的决策。此外，YOLO11 的尖端功能，如持久跟踪、可定制的颜色映射和对各种导出格式的支持，使其在综合数据分析方面优于 TensorFlow 和 OpenCV 等其他工具。在 Ultralytics Plans 了解更多商业应用。

**Examples:**

Example 1 (markdown):
```markdown
# Run a heatmap example
yolo solutions heatmap show=True

# Pass a source video
yolo solutions heatmap source="path/to/video.mp4"

# Pass a custom colormap
yolo solutions heatmap colormap=cv2.COLORMAP_INFERNO

# Heatmaps + object counting
yolo solutions heatmap region="[(20, 400), (1080, 400), (1080, 360), (20, 360)]"
```

Example 2 (python):
```python
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "Error reading video file"

# Video writer
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))
video_writer = cv2.VideoWriter("heatmap_output.avi", cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))

# For object counting with heatmap, you can pass region points.
# region_points = [(20, 400), (1080, 400)]                                      # line points
# region_points = [(20, 400), (1080, 400), (1080, 360), (20, 360)]              # rectangle region
# region_points = [(20, 400), (1080, 400), (1080, 360), (20, 360), (20, 400)]   # polygon points

# Initialize heatmap object
heatmap = solutions.Heatmap(
    show=True,  # display the output
    model="yolo11n.pt",  # path to the YOLO11 model file
    colormap=cv2.COLORMAP_PARULA,  # colormap of heatmap
    # region=region_points,  # object counting with heatmaps, you can pass region_points
    # classes=[0, 2],  # generate heatmap for specific classes, e.g., person and car.
)

# Process video
while cap.isOpened():
    success, im0 = cap.read()

    if not success:
        print("Video frame is empty or processing is complete.")
        break

    results = heatmap(im0)

    # print(results)  # access the output

    video_writer.write(results.plot_im)  # write the processed frame.

cap.release()
video_writer.release()
cv2.destroyAllWindows()  # destroy all opened windows
```

Example 3 (python):
```python
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
heatmap = solutions.Heatmap(colormap=cv2.COLORMAP_PARULA, show=True, model="yolo11n.pt")

while cap.isOpened():
    success, im0 = cap.read()
    if not success:
        break
    results = heatmap(im0)
cap.release()
cv2.destroyAllWindows()
```

Example 4 (python):
```python
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
heatmap = solutions.Heatmap(show=True, model="yolo11n.pt", classes=[0, 2])

while cap.isOpened():
    success, im0 = cap.read()
    if not success:
        break
    results = heatmap(im0)
cap.release()
cv2.destroyAllWindows()
```

---

## Axelera AI 导出与部署 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/integrations/axelera/

**Contents:**
- Axelera AI 导出与部署
- 选择合适的硬件
- 硬件产品组合
  - 加速卡
  - 集成系统
- 支持的任务
- 安装
  - Ultralytics 安装
  - Axelera 驱动程序安装
- 将 YOLO 模型导出到 Axelera

这是一个实验性集成，演示了在 Axelera Metis 硬件上的部署。预计到2026 年 2 月将实现完全集成，届时模型导出将无需 Axelera 硬件，并支持标准 pip 安装。

Ultralytics 与 Axelera AI 合作，以在 边缘 AI 设备上实现高性能、高能效的推理。使用 Voyager SDK 将 Ultralytics YOLO 模型直接导出并部署到 Metis® AIPU。

Axelera AI 利用专有的数据流架构和内存计算，为边缘计算机视觉提供专用硬件加速，以低功耗实现高达856 TOPS的性能。

Axelera AI 提供多种外形尺寸，以适应不同的部署限制。下表有助于您为 Ultralytics YOLO 部署选择最佳硬件。

Axelera 硬件产品线经过优化，可运行 Ultralytics YOLO11 和旧版本，具有高每瓦帧率效率。

这些卡片可在现有主机设备中实现 AI 加速，从而促进了棕地部署。

对于交钥匙解决方案，Axelera 与制造商合作提供经过 Metis AIPU 预验证的系统。

目前，对象 detect 模型可以导出为 Axelera 格式。正在集成其他任务：

有关详细说明，请参阅我们的Ultralytics 安装指南。如果您遇到困难，请查阅我们的常见问题指南。

使用标准的 Ultralytics 导出命令导出您训练好的 YOLO 模型。

使用 Ultralytics API 加载导出的模型并运行推理，类似于加载 ONNX 模型。

首次推理运行可能会抛出 ImportError。后续运行将正常工作。此问题将在未来版本中解决。

Metis AIPU最大限度地提高吞吐量，同时最大限度地降低能耗。

基于Axelera AI数据的基准。实际FPS取决于模型大小、批处理和输入分辨率。

Ultralytics YOLO 在 Axelera 硬件上支持先进的边缘计算解决方案：

验证您的 Axelera 设备是否正常运行：

有关详细诊断，请参阅AxDevice 文档。

此集成使用单核配置以确保兼容性。对于需要最大吞吐量的生产环境，Axelera Voyager SDK 提供：

请参阅模型库以获取 FPS 基准测试，或联系 Axelera以获取生产支持。

PyTorch 2.9 兼容性：第一次 yolo export format=axelera 命令可能会因 PyTorch 自动降级到 2.8 而失败。请再次运行该命令以成功执行。

M.2功耗限制：由于电源供应限制，大型或超大型模型在M.2加速器上可能会遇到运行时错误。

首次推理导入错误：第一次推理运行可能会抛出 ImportError。后续运行正常工作。

Voyager SDK 支持导出 YOLOv8 和 YOLO11 模型。

是的。任何使用 Ultralytics 训练模式训练的模型，只要使用支持的层和操作，都可以导出为 Axelera 格式。

Axelera 的 Voyager SDK 会自动为混合精度 AIPU 架构量化模型。对于大多数 对象检测 任务，性能提升（更高的 FPS，更低的功耗）显著超过了对 mAP。量化过程耗时从几秒到几小时不等，具体取决于模型大小。运行 yolo val 导出后验证准确性。

我们建议使用 100 到 400 张图像。超过 400 张不会带来额外的好处，反而会增加量化时间。请尝试使用 100、200 和 400 张图像，以找到最佳平衡。

SDK、驱动程序和编译器工具可通过Axelera开发者门户获取。

**Examples:**

Example 1 (sql):
```sql
graph TD
    A[Start: Select Deployment Target] --> B{Device Type?}
    B -->|Edge Server / Workstation| C{Throughput Needs?}
    B -->|Embedded / Robotics| D{Space Constraints?}
    B -->|Standalone / R&D| E[Dev Kits & Systems]

    C -->|Max Density <br> 30+ Streams| F[**Metis PCIe x4**<br>856 TOPS]
    C -->|Standard PC <br> Low Profile| G[**Metis PCIe x1**<br>214 TOPS]

    D -->|Drones & Handhelds| H[**Metis M.2**<br>2280 M-Key]
    D -->|High Performance Embedded| I[**Metis M.2 MAX**<br>Extended Thermal]

    E -->|ARM-based All-in-One| J[**Metis Compute Board**<br>RK3588 + AIPU]
    E -->|Prototyping| K[**Arduino Portenta x8**<br>Integration Kit]

    click F "https://store.axelera.ai/"
    click G "https://store.axelera.ai/"
    click H "https://store.axelera.ai/"
    click J "https://store.axelera.ai/"
```

Example 2 (unknown):
```unknown
pip install ultralytics
```

Example 3 (unknown):
```unknown
sudo sh -c "curl -fsSL https://software.axelera.ai/artifactory/api/security/keypair/axelera/public | gpg --dearmor -o /etc/apt/keyrings/axelera.gpg"
```

Example 4 (bash):
```bash
sudo sh -c "echo 'deb [signed-by=/etc/apt/keyrings/axelera.gpg] https://software.axelera.ai/artifactory/axelera-apt-source/ ubuntu22 main' > /etc/apt/sources.list.d/axelera.list"
```

---

## YOLOv9 vs. DAMO-YOLO：全面技术比较

**URL:** https://docs.ultralytics.com/zh/compare/yolov9-vs-damo-yolo/

**Contents:**
- YOLOv9 vs. DAMO-YOLO：全面技术比较
- YOLOv9：用于实现卓越精度的可编程梯度信息
  - 架构与核心创新
  - 优势与生态系统
  - 弱点
- DAMO-YOLO：面向速度的神经架构搜索
  - 架构和主要特性
  - 优势
  - 弱点
- 性能分析：速度 vs. 准确性

在快速发展的计算机视觉领域，选择最佳的物体 detect 架构对于项目成功至关重要。本分析提供了两个强大模型的详细技术比较：YOLOv9（以其在梯度信息方面的架构创新而闻名）和 DAMO-YOLO（一个来自阿里巴巴集团的专为高速推理设计的模型）。我们将考察它们独特的架构、性能指标和理想部署场景，以指导开发人员和研究人员做出明智决策。

YOLOv9标志着You Only Look Once (YOLO) 系列的重大演进，专注于解决深度神经网络固有的信息瓶颈问题。通过确保关键输入数据在整个网络层中得以保留，YOLOv9实现了最先进的精度。

作者：王建尧、廖鸿源Chien-Yao Wang 和 Hong-Yuan Mark Liao组织：台湾中央研究院信息科学研究所日期：2024-02-21Arxiv:2402.13616 GitHub:WongKinYiu/yolov9文档：Ultralytics YOLOv9 文档

YOLOv9 的架构建立在两个开创性概念之上，旨在优化深度学习效率：

DAMO-YOLO 证明了自动化架构设计的强大能力。由阿里巴巴开发，它利用神经网络架构搜索 (NAS) 在推理延迟和检测性能之间找到最佳平衡，特别针对工业应用。

作者： 徐贤哲、蒋一奇、陈卫华、黄一伦、张远、孙秀宇机构：阿里巴巴集团日期： 2022-11-23预印本：2211.15444GitHub：tinyvision/DAMO-YOLO

DAMO-YOLO 凭借多项旨在最大化吞吐量的技术进步而脱颖而出：

在比较性能指标时，这两种架构之间的权衡变得清晰。YOLOv9 优先考虑信息保留以实现卓越的精度，在相似模型尺寸下，其 mAP 分数通常超越 DAMO-YOLO。相反，DAMO-YOLO 则侧重于原始吞吐量。

然而，YOLOv9的GELAN架构效率使其在速度方面保持高度竞争力，同时提供更好的detect质量。例如，YOLOv9-C与DAMO-YOLO-L（50.8%）相比，实现了显著更高的mAP（53.0%），同时使用的参数更少（25.3M vs 42.1M）。这突出了YOLOv9在模型复杂性方面实现“事半功倍”的能力。

评估模型时，除了参数数量，还要考虑 FLOPs（浮点运算）。较低的 FLOPs 数量通常表示模型计算量更轻，并且在移动或边缘 AI 硬件上可能更快。

YOLOv9是对精度要求极高的应用的首选。

DAMO-YOLO 在受 严格延迟预算 限制的环境中表现出色。

尽管这两个模型在技术上都令人印象深刻，但在 Ultralytics 生态系统中选择模型——例如 YOLOv9 或尖端的 YOLO11——为开发人员和企业提供了独特的优势。

Ultralytics 优先考虑易用性。模型通过统一的接口访问，该接口抽象了复杂的样板代码。无论您是在自定义数据上训练还是运行推理，过程都是一致且直观的。

Ultralytics 模型由活跃的社区和频繁的更新提供支持。像Ultralytics HUB这样的功能支持基于网络的 数据集管理和训练，而与TensorBoard和MLflow等工具的广泛集成则简化了 MLOps 生命周期。相比之下，像 DAMO-YOLO 这样的研究模型通常缺乏这种持续支持和工具集成水平。

Ultralytics 模型被设计为多功能。虽然 DAMO-YOLO 专注于检测，但像 YOLO11 这样的 Ultralytics 模型将功能扩展到实例分割、姿势估计和旋转框检测 (OBB)。此外，它们针对内存效率进行了优化，与其它架构相比，训练期间通常需要更少的 CUDA 内存，从而节省了硬件成本。

在YOLOv9 与YOLO 的对比中，两个模型都展示了人工智能的飞速发展。YOLO 为纯粹的速度优化提供了令人信服的架构。但是 YOLOv9在大多数实际应用中是更强大的解决方案。它的每个参数都具有极高的准确性，采用先进的架构来防止信息丢失，并且位于蓬勃发展的Ultralytics 生态系统中。对于寻求性能、易用性和长期支持之间最佳平衡的开发人员来说，Ultralytics 模型仍然是值得推荐的选择。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv9 model
model = YOLO("yolov9c.pt")

# Train the model on your custom dataset
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference on an image
results = model("path/to/image.jpg")
```

---

## YOLOv9 vs. PP-YOLOE+：技术比较 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/compare/yolov9-vs-pp-yoloe/

**Contents:**
- YOLOv9 vs. PP-YOLOE+：技术比较
- YOLOv9：用于增强学习的可编程梯度信息
- PP-YOLOE+：PaddlePaddle 生态系统内的高精度
- 性能分析：速度、准确性和效率
  - 主要内容
- 训练方法与易用性
  - Ultralytics 生态系统优势
  - PaddlePaddle 环境
- 理想用例
  - 何时选择 YOLOv9

对于计算机视觉工程师来说，选择最佳物体检测架构是一项关键决策，需要在高精度需求与计算限制之间取得平衡。本综合指南比较了 YOLOv9和PP-YOLOE+（一种针对PaddlePaddle 框架进行了优化的鲁棒检测器）进行了比较。我们分析了它们的架构创新、基准性能和部署适用性，以帮助您确定最适合您的计算机视觉应用。

YOLOv9 代表了实时对象检测器发展中的一次重大飞跃。它于 2024 年初发布，解决了与深度神经网络中信息丢失相关的根本问题，为准确性和参数效率树立了新的基准。

作者：王建尧、廖鸿源Chien-Yao Wang and Hong-Yuan Mark Liao组织：台湾中央研究院信息科学研究所日期：2024-02-21Arxiv:https://arxiv.org/abs/2402.13616 GitHub:https://github.com/WongKinYiu/yolov9Documentation:ultralytics

该架构引入了两个开创性概念：可编程梯度信息 (PGI) 和广义高效层聚合网络 (GELAN)。随着网络变得更深，计算 损失函数 所必需的数据可能会丢失——这种现象被称为信息瓶颈。PGI 通过辅助可逆分支生成可靠梯度来解决此问题，确保深层特征保留关键信息。同时，GELAN 优化了参数利用率，与基于深度卷积的架构相比，使模型能够以更少的计算资源实现更高的 精度。

集成到 Ultralytics 生态系统中，YOLOv9 受益于以用户为中心的设计，简化了复杂的工作流程。开发人员可以利用统一的 Python API 进行训练、验证和部署，从而大幅缩短从原型到生产的时间。这种集成还确保了与各种数据集和导出格式的兼容性。

PP-YOLOE+是PP-YOLOE的演进版本，由百度开发，作为PaddleDetection套件的一部分。它经过专门设计，可在PaddlePaddle框架上高效运行，为工业应用提供速度和精度之间的强大平衡。

作者： PaddlePaddle 作者机构：百度日期： 2022-04-02预印本：https://arxiv.org/abs/2203.16250GitHub：https://github.com/PaddlePaddle/PaddleDetection/文档：https://github.com/PaddlePaddle/PaddleDetection/blob/release/2.8.1/configs/ppyoloe/README.md

PP-YOLOE+ 采用无anchor机制，无需预定义anchor boxes，从而简化了超参数调优过程。其骨干网络通常使用CSPRepResNet，并具有由任务对齐学习（TAL）驱动的独特头部设计。这种方法对齐了分类和定位任务，以提高 detect 结果的质量。尽管功能强大，PP-YOLOE+ 与 PaddlePaddle 生态系统紧密耦合，这对于已标准化使用 PyTorch 或 TensorFlow 的团队来说可能存在学习曲线。

尽管 PP-YOLOE+ 提供了具有竞争力的性能，但其对 PaddlePaddle 框架的依赖可能会限制其与西方研究社区中常用的更广泛的基于 PyTorch 的工具和库的互操作性。

比较这两种架构时，YOLOv9 在参数效率和峰值精度方面均表现出明显优势。GELAN 的集成使 YOLOv9 能够更有效地处理视觉数据，从而在 COCO 数据集上获得更高的 平均精度 (mAP) 分数，同时通常保持较低的延迟。

这两种模型的开发者体验差异显著，主要受其底层框架和生态系统支持的影响。

通过 Ultralytics 选择 YOLOv9，可获得一套全面的工具，旨在简化机器学习生命周期。

使用 Ultralytics 简化工作流程

您只需几行 Python 代码即可加载、训练和验证 YOLOv9 模型，利用强大的 Ultralytics 引擎进行自动化超参数调优和实验跟踪。

PP-YOLOE+ 需要 PaddleDetection 库。虽然功能强大，但它要求用户熟悉百度生态系统。对于尚未融入 PaddlePaddle 基础设施的用户来说，设置环境、将数据集转换为所需格式以及导出模型进行部署可能会更加复杂。

理解每个模型的优势有助于为特定的实际应用选择合适的工具。

尽管这两个模型在 object detection 领域都是强大的竞争者，但 YOLOv9 成为全球大多数开发人员和企业的卓越选择。它创新性地使用可编程梯度信息 (PGI)，以卓越的效率提供最先进的准确性，在关键指标上优于 PP-YOLOE+，同时使用显著更少的参数。

此外，Ultralytics 生态系统通过提供无与伦比的易用性、详尽的文档和活跃的社区，提升了 YOLOv9。无论您是构建安全警报系统、分析医学图像，还是开发智慧城市基础设施，YOLOv9 都能提供成功所需的性能平衡和多功能性。

如果您正在探索最先进的视觉 AI，请考虑 Ultralytics 的这些其他强大模型：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv9 model
model = YOLO("yolov9c.pt")

# Train the model on a custom dataset
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Validate the model's performance
metrics = model.val()
```

---

## 为错误报告创建最小可重现示例

**URL:** https://docs.ultralytics.com/zh/help/minimum-reproducible-example/

**Contents:**
- 为错误报告创建最小可重现示例
- 1. 隔离问题
- 2. 使用公共模型和数据集
- 3. 包含所有必需的依赖项
- 4. 清晰描述问题
- 5. 正确格式化你的代码
- 6. 测试你的最小可复现示例（MRE）
- MRE 示例
- 常见问题
  - 如何在 Ultralytics YOLO 仓库中为 Bug 报告创建一个有效的最小可复现示例 (MRE)？

为UltralyticsYOLO仓库提交错误报告时，提供一个最小可复现示例 (MRE)至关重要。MRE 是一小段独立的、能够演示您所遇到问题的代码。提供 MRE 有助于维护者和贡献者理解问题并更有效地进行修复。本指南解释了如何在向 Ultralytics YOLO 仓库提交错误报告时创建 MRE。

创建 MRE 的第一步是隔离问题。删除任何与问题没有直接关系的不必要的代码或依赖项。专注于导致问题的代码的特定部分，并消除任何不相关的部分。

创建最小可复现示例（MRE）时，请使用公开可用的模型和数据集来重现问题。例如，使用 yolo11n.pt 模型和 coco8.yaml 数据集。这确保了维护者和贡献者可以轻松运行您的示例并调查问题，而无需访问专有数据或自定义模型。

确保您的 MRE 中包含所有必要的依赖项。如果您的代码依赖于外部库，请指定所需的软件包及其版本。 理想情况下，请使用以下方式在您的错误报告中列出依赖项 yolo checks 如果您有 ultralytics 已安装或 pip list 适用于其他工具。

请提供您遇到的问题的清晰简洁的描述。 解释预期的行为和您遇到的实际行为。 如果适用，请包括任何相关的错误消息或日志。

请在 issue 描述中使用代码块正确格式化您的代码。这能让其他人更容易阅读和理解您的代码。在 GitHub 中，您可以使用三个反引号 (```) 包裹您的代码并指定语言来创建一个代码块：

在提交您的 MRE 之前，请对其进行测试，以确保其准确地重现问题。确保其他人可以运行您的示例，而不会出现任何问题或修改。

这是一个假设的错误报告的最小可复现示例 (MRE)：

在0通道图像上运行推理时，我收到一个与输入tensor维度相关的错误。

在此示例中，MRE 通过最少量的代码演示了该问题，并使用了一个公共模型（"yolo11n.pt")，包括所有必要的依赖项，并清楚地描述了问题以及错误消息。

通过遵循这些指南，您将帮助 Ultralytics YOLO 仓库的维护者和 贡献者 更高效地理解和解决您的问题。

要在 Ultralytics YOLO 存储库中为错误报告创建一个有效的最小可复现示例 (MRE)，请按照以下步骤操作：

在您的 MRE 中使用公开可用的模型和数据集，可确保维护人员可以轻松运行您的示例，而无需访问专有数据。 这样可以更快、更有效地解决问题。 例如，使用 yolo11n.pt 模型和 coco8.yaml 数据集有助于标准化和简化调试过程。请在以下位置了解有关公共模型和数据集的更多信息 使用公共模型和数据集 部分。

一个全面的 Ultralytics YOLO 错误报告应包括:

有关完整的检查清单，请参阅编写清晰的问题描述部分。

在 GitHub 上提交错误报告时，要正确格式化您的代码，请执行以下操作：

有关代码格式化的更多技巧，请参见正确格式化您的代码。

有关详细的检查清单，请访问测试您的 MRE部分。

**Examples:**

Example 1 (markdown):
```markdown
```python
# Your Python code goes here
```
```

Example 2 (python):
```python
import torch

from ultralytics import YOLO

# Load the model
model = YOLO("yolo11n.pt")

# Load a 0-channel image
image = torch.rand(1, 0, 640, 640)

# Run the model
results = model(image)
```

Example 3 (yaml):
```yaml
RuntimeError: Expected input[1, 0, 640, 640] to have 3 channels, but got 0 channels instead
```

Example 4 (markdown):
```markdown
```python
# Your Python code goes here
```
```

---

## 如何从 YOLO11 导出到 NCNN 以实现平滑部署

**URL:** https://docs.ultralytics.com/zh/integrations/ncnn/

**Contents:**
- 如何从 YOLO11 导出到 NCNN 以实现平滑部署
- 为什么要导出到 NCNN？
- NCNN 模型的主要特性
- NCNN 的部署选项
- 导出到 NCNN：转换您的 YOLO11 模型
  - 安装
  - 用法
  - 导出参数
- 部署导出的 YOLO11 NCNN 模型
- 总结

在计算能力有限的设备（如移动或嵌入式系统）上部署计算机视觉模型可能很棘手。您需要确保使用针对最佳性能优化的格式。这可以确保即使是处理能力有限的设备也能很好地处理高级计算机视觉任务。

导出为 NCNN 格式的功能允许您优化 Ultralytics YOLO11 模型，以用于轻量级设备应用。在本指南中，我们将引导您完成将模型转换为 NCNN 格式的过程，从而使您的模型更容易在各种移动和嵌入式设备上表现良好。

NCNN框架由腾讯开发，是一个高性能的神经网络推理计算框架，专门为移动平台（包括手机、嵌入式设备和物联网设备）优化。NCNN与包括Linux、Android、iOS和macOS在内的各种平台兼容。

NCNN 以其在移动 CPU 上的快速处理速度而闻名，并能够将 深度学习 模型快速部署到移动平台。这使得构建智能应用程序变得更加容易，将 AI 的强大功能触手可及。

NCNN 模型提供了一系列关键特性，通过帮助开发者在移动设备、嵌入式设备和边缘设备上运行模型，从而实现设备上的 机器学习：

高效和高性能: NCNN 模型旨在实现高效和轻量级，经过优化可在资源有限的移动和嵌入式设备（如 Raspberry Pi）上运行。它们还可以在各种基于计算机视觉的任务中实现高精度。

量化：NCNN 模型通常支持量化，这是一种降低模型权重和激活精度的技术。这可以进一步提高性能并减少内存占用。

兼容性: NCNN模型与流行的深度学习框架（如TensorFlow、Caffe和ONNX）兼容。 这种兼容性使开发人员可以轻松使用现有模型和工作流程。

易于使用：NCNN 模型旨在轻松集成到各种应用程序中，这归功于它们与流行的深度学习框架的兼容性。 此外，NCNN 提供了用户友好的工具，用于在不同格式之间转换模型，从而确保整个开发领域中的平稳互操作性。

在查看将 YOLO11 模型导出为 NCNN 格式的代码之前，让我们先了解 NCNN 模型通常是如何使用的。

NCNN 模型专为效率和性能而设计，与各种部署平台兼容：

移动部署: 专门针对 Android 和 iOS 进行了优化，可以无缝集成到移动应用程序中，以实现高效的设备端推理。

嵌入式系统和物联网设备: 如果您发现使用 Ultralytics 指南 在 Raspberry Pi 上运行推理不够快，切换到 NCNN 导出的模型可能有助于提高速度。NCNN 非常适合 Raspberry Pi 和 NVIDIA Jetson 等设备，尤其是在您需要在设备上快速处理的情况下。

桌面和服务器部署: 能够在 Linux、Windows 和 macOS 的桌面和服务器环境中进行部署，支持具有更高计算能力的开发、训练和评估。

通过将 YOLO11 模型转换为 NCNN 格式，您可以扩展模型的兼容性和部署灵活性。

有关安装过程的详细说明和最佳实践，请查看我们的 Ultralytics 安装指南。如果在为 YOLO11 安装所需软件包时遇到任何困难，请查阅我们的 常见问题指南以获取解决方案和提示。

所有Ultralytics YOLO11 模型都设计为支持开箱即用的导出，从而可以轻松地将其集成到您首选的部署工作流程中。您可以查看支持的导出格式和配置选项的完整列表，以选择最适合您应用程序的设置。

有关导出过程的更多详细信息，请访问Ultralytics 文档页面上的导出。

成功将您的 Ultralytics YOLO11 模型导出为 NCNN 格式后，您现在可以部署它们。运行 NCNN 模型的主要且推荐的第一步是使用 YOLO("yolo11n_ncnn_model/") 方法，如之前的用法代码片段中所示。然而，有关在各种其他环境中部署 NCNN 模型的深入说明，请查阅以下资源：

Android: 这篇博客解释了如何使用 NCNN 模型通过 Android 应用程序执行目标检测等任务。

macOS：了解如何使用 NCNN 模型通过 macOS 执行任务。

Linux：浏览此页面，了解如何在 Raspberry Pi 和其他类似设备等资源有限的设备上部署 NCNN 模型。

使用 VS2017 的 Windows x64: 浏览此博客，了解如何使用 Visual Studio Community 2017 在 Windows x64 上部署 NCNN 模型。

在本指南中，我们介绍了将 Ultralytics YOLO11 模型导出为 NCNN 格式。此转换步骤对于提高 YOLO11 模型的效率和速度至关重要，使其更有效并适用于资源有限的计算环境。

有关使用的详细说明，请参阅NCNN官方文档。

此外，如果您有兴趣探索 Ultralytics YOLO11 的其他集成选项，请务必访问我们的集成指南页面，以获取更多见解和信息。

要将您的 Ultralytics YOLO11 模型导出为 NCNN 格式，请按照以下步骤操作：

Python：使用 export YOLO 类中的 function。

CLI：使用 yolo 命令，使用 export 参数生成。

有关详细的导出选项，请查看文档中的导出页面。

将您的 Ultralytics YOLO11 模型导出到 NCNN 具有以下几个优点：

有关更多详细信息，请参阅文档中的导出到 NCNN部分。

NCNN 由腾讯开发，专门为移动平台优化。使用 NCNN 的主要原因包括：

要了解更多信息，请访问文档中的 NCNN 概述。

如果在 Raspberry Pi 上运行模型速度不够快，转换为 NCNN 格式可能会提高速度，详情请参阅我们的 Raspberry Pi 指南。

要在 Android 上部署 YOLO11 模型：

有关分步说明，请参阅我们的 部署 YOLO11 NCNN 模型 指南。

如需更多高级指南和用例，请访问 Ultralytics 文档页面。

**Examples:**

Example 1 (go):
```go
# Install the required package for YOLO11
pip install ultralytics
```

Example 2 (python):
```python
from ultralytics import YOLO

# Load the YOLO11 model
model = YOLO("yolo11n.pt")

# Export the model to NCNN format
model.export(format="ncnn")  # creates '/yolo11n_ncnn_model'

# Load the exported NCNN model
ncnn_model = YOLO("./yolo11n_ncnn_model")

# Run inference
results = ncnn_model("https://ultralytics.com/images/bus.jpg")
```

Example 3 (markdown):
```markdown
# Export a YOLO11n PyTorch model to NCNN format
yolo export model=yolo11n.pt format=ncnn # creates '/yolo11n_ncnn_model'

# Run inference with the exported model
yolo predict model='./yolo11n_ncnn_model' source='https://ultralytics.com/images/bus.jpg'
```

Example 4 (python):
```python
from ultralytics import YOLO

# Load the YOLO11 model
model = YOLO("yolo11n.pt")

# Export to NCNN format
model.export(format="ncnn")  # creates '/yolo11n_ncnn_model'
```

---

## YOLOv5 vs. EfficientDet：详细技术对比

**URL:** https://docs.ultralytics.com/zh/compare/yolov5-vs-efficientdet/

**Contents:**
- YOLOv5 vs. EfficientDet：详细技术对比
- 性能分析：速度与效率
- 架构差异
  - Ultralytics YOLOv5
  - EfficientDet
- Ultralytics 优势：生态系统与易用性
  - 易用性与集成
  - 训练效率与内存
  - 多功能性与多任务处理
- 理想用例

在不断发展的计算机视觉领域，选择正确的物体检测架构是项目成功的关键。本比较探讨了两种极具影响力的模型： Ultralytics YOLOv5和Google 的 EfficientDet，前者以速度和易用性兼顾而著称，后者则以可扩展性和参数效率著称。通过研究它们的架构、性能指标和部署能力，开发人员可以做出适合其特定应用需求的明智决定。

这两种架构的主要区别在于它们在计算资源与推理延迟方面的设计理念。EfficientDet 针对理论 FLOPs（浮点运算）进行优化，使其在学术基准测试中具有吸引力。相反，YOLOv5 优先考虑在实际硬件（特别是 GPU）上的低延迟，提供生产环境所需的实时推理速度。

下表展示了在COCO val2017数据集上的这种权衡。尽管EfficientDet模型以更少的参数实现了高mAP，YOLOv5在使用TensorRT的NVIDIA T4 GPU上展示了显著更快的推理时间。

如图所示，YOLOv5n在GPU上实现了惊人的1.12毫秒延迟，显著超越了最小的EfficientDet变体。对于毫秒级响应至关重要的应用，例如自动驾驶车辆或高速生产线，这种速度优势至关重要。

理解每个模型的结构设计有助于阐明其性能特征。

YOLOv5 采用 CSPDarknet 主干网络结合 PANet 颈部网络。这种架构旨在最大化梯度流和特征提取效率。

EfficientDet 基于 EfficientNet 主干网络构建，并引入了加权双向特征金字塔网络 (BiFPN)。

了解更多关于 EfficientDet 的信息

原始指标固然重要，但开发者体验往往决定项目的成功。Ultralytics YOLOv5 在提供精致、以用户为中心的环境方面表现出色，这显著缩短了开发时间。

YOLOv5 以其“开箱即用”的可用性而闻名。该模型可以通过简单的 pip 命令安装，并以最少的代码使用。相比之下，EfficientDet 的实现通常需要在 TensorFlow 生态系统或特定的研究代码库中进行更复杂的设置。

借助Ultralytics，您可以在几分钟内从数据集到训练好的模型。与Ultralytics HUB等工具的集成实现了无缝的模型管理、可视化和部署，无需大量样板代码。

Ultralytics 模型针对训练效率进行了优化。与 EfficientDet 的更高扩展层级或基于 Transformer 的模型等复杂架构相比，它们通常收敛更快，并需要更少的 CUDA 内存。这种较低的入门门槛使开发者能够在消费级硬件或像Google Colab这样的标准云实例上训练最先进的模型。

与主要是一个目标检测器的标准 EfficientDet 实现不同，Ultralytics 框架支持广泛的任务。开发人员可以利用相同的 API 进行实例分割和图像分类，为各种计算机视觉挑战提供统一的解决方案。

在 YOLOv5 和 EfficientDet 之间进行选择在很大程度上取决于部署限制和目标。

理解这些模型的背景有助于深入了解其设计目标。

Ultralytics 使推理变得异常简单。下面是一个使用 Python API 检测图像中对象的有效且可运行的示例。

这段简单的代码片段处理了模型下载、图像预处理、前向传播和输出解码——这些任务如果使用原始EfficientDet实现将需要更多的代码。

尽管EfficientDet对模型缩放和参数效率的研究做出了重大贡献，但Ultralytics YOLOv5仍然是实际、真实世界部署的卓越选择。其在速度和精度上的卓越平衡，结合了蓬勃发展且维护良好的生态系统，确保开发者能够有效地构建、训练和部署解决方案。

对于那些希望利用计算机视觉技术绝对最新成果的用户，Ultralytics 在 YOLOv5 之外持续创新。像 YOLOv8 和尖端 YOLO11 这样的模型在架构上提供了进一步的改进，支持更多任务，例如 姿势估计 和 旋转框检测，同时保持了 Ultralytics 体验标志性的易用性。

如果您有兴趣探索更多比较以找到满足您需求的完美模型，请考虑这些资源：

**Examples:**

Example 1 (python):
```python
import torch

# Load the YOLOv5s model from PyTorch Hub
model = torch.hub.load("ultralytics/yolov5", "yolov5s", pretrained=True)

# Define an image URL
img_url = "https://ultralytics.com/images/zidane.jpg"

# Perform inference
results = model(img_url)

# Display results
results.show()

# Print detection data (coordinates, confidence, class)
print(results.pandas().xyxy[0])
```

---

## DAMO-YOLO 对比 YOLOv5：一项全面的技术比较

**URL:** https://docs.ultralytics.com/zh/compare/damo-yolo-vs-yolov5/

**Contents:**
- DAMO-YOLO 对比 YOLOv5：一项全面的技术比较
- DAMO-YOLO：精度驱动的架构
  - 架构创新
  - 优势与劣势
- Ultralytics YOLOv5：实用 AI 的标准
  - 架构与可用性
  - 为什么开发者选择YOLOv5
- 性能对比
  - 实际应用代码
- 结论

选择最佳的物体检测架构是计算机视觉开发的关键一步，需要对精度、推理速度和集成复杂度进行仔细评估。本分析将阿里巴巴集团开发的高精度模型YOLO 与 Ultralytics YOLOv5进行了比较，Ultralytics YOLOv5 是一种行业标准架构，因其在性能、速度和对开发人员友好的生态系统之间的平衡而广受赞誉。我们探讨了它们的架构创新、基准指标和理想应用场景，以帮助您做出明智的决定。

作者： 徐贤哲、蒋一奇、陈卫华、黄一伦、张远、孙秀宇机构：阿里巴巴集团日期： 2022-11-23预印本：https://arxiv.org/abs/2211.15444v2GitHub：https://github.com/tinyvision/DAMO-YOLO文档：DAMO-YOLO README

DAMO-YOLO 代表了阿里巴巴集团在推动检测精度极限同时保持合理延迟方面所做的重大努力。它集成了先进的神经网络架构搜索 (NAS) 技术和新颖的特征融合策略，在静态基准测试中超越了许多同期产品。

DAMO-YOLO 凭借多项技术复杂的组件而独树一帜，这些组件旨在最大限度地发挥网络性能：

DAMO-YOLO 经过大量优化，可在 COCO 等基准测试中实现高 mAP。它对 NAS 和蒸馏技术的使用使其成为学术研究和对精度要求极高的场景的强大工具，即使这会增加训练复杂性。

DAMO-YOLO 的主要优势在于其原始检测精度。通过利用 NAS 和先进的颈部设计，它通常比同代可比模型实现更高的平均精度 (mAP)分数。它在需要细粒度特征辨别的复杂场景中识别物体方面表现出色。

然而，这些优势也伴随着权衡。对NAS骨干网络和蒸馏管道的依赖增加了训练和集成的复杂性。与某些替代方案的即插即用特性不同，为DAMO-YOLO设置自定义训练管道可能需要大量资源。此外，其生态系统相对较小，这意味着与更成熟的框架相比，可用的社区资源、教程和第三方集成较少。

作者：Glenn JocherGlenn Jocher组织：Ultralytics日期：2020-06-26 GitHubyolov5文档yolov5

自发布以来，Ultralytics YOLOv5 已成为真实世界计算机视觉应用的首选解决方案。它在速度、准确性和可用性之间取得了传奇般的平衡，并由一个简化机器学习生命周期各个阶段（从数据集整理到部署）的生态系统提供支持。

YOLOv5 利用CSPDarknet53 主干网络结合PANet 颈部网络，这些架构因其在 GPU 和 CPU 硬件上的鲁棒性和效率而被选中。虽然它使用基于锚框的检测（一种成熟的方法），但其真正的力量在于其工程设计和生态系统：

The Ultralytics生态系统是一个巨大的开发加速器。凭借详尽的文档、活跃的社区论坛和频繁的更新，开发人员可以减少调试时间，投入更多时间进行创新。与Ultralytics HUB等工具的集成进一步简化了模型管理和训练。

YOLOv5 仍然是首选，因为它优先考虑易用性和训练效率。预训练权重易于获取且稳健，支持快速迁移学习。其推理速度卓越，使其成为视频分析、自主导航和工业检测等实时应用的理想选择。

尽管YOLO11等新模型此后引入了无锚框架构并带来了进一步的性能提升，但YOLOv5仍然是无数生产系统中可靠、受良好支持且功能强大的主力。

通过直接比较，两种模型之间的区别变得清晰：DAMO-YOLO 倾向于最大化验证精度 (mAP)，而 YOLOv5 则优化推理速度和部署实用性。下表强调，虽然 DAMO-YOLO 模型在相似的参数数量下通常能获得更高的 mAP 分数，但 YOLOv5 模型（特别是 Nano 和 Small 变体）在 CPU 和 GPU 上提供了卓越的速度，这通常是边缘部署的决定性因素。

Ultralytics模型最有力的论据之一是其集成的简便性。下面是一个经过验证的示例，展示了如何使用PyTorch Hub轻松加载YOLOv5模型并进行推理，这充分体现了该生态系统对开发者友好的特性。

两种架构在计算机视觉领域扮演着不同的角色。DAMO-YOLO 是学术研究和竞赛中的强大选择，在这些场景中，实现最先进的准确性是唯一目标，并且基于 NAS 的训练管道的复杂性可以接受。

然而，对于绝大多数开发人员、研究人员和企业而言，Ultralytics YOLOv5（及其继任者YOLO11）仍然是卓越的推荐选择。维护良好的生态系统的优势不容小觑：简单的API、全面的文档和无缝的导出选项显著缩短了产品上市时间。凭借有效处理实时约束的性能平衡以及在分割和分类等任务中的多功能性，Ultralytics 模型为构建实用的AI解决方案提供了强大且面向未来的基础。

对于那些寻求性能和功能方面绝对最新成果的用户，我们强烈推荐探索 YOLO11，它在 YOLOv5 的传承基础上，实现了更高的精度和效率。

为了进一步评估最适合您需求的模型，请查阅这些详细的比较：

**Examples:**

Example 1 (python):
```python
import torch

# Load YOLOv5s from PyTorch Hub
model = torch.hub.load("ultralytics/yolov5", "yolov5s", pretrained=True)

# Define an image source (URL or local path)
img = "https://ultralytics.com/images/zidane.jpg"

# Run inference
results = model(img)

# Print results to console
results.print()

# Show the results
results.show()
```

---

## 使用 Ultralytics YOLO11 🚀 进行队列管理 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/guides/queue-management/

**Contents:**
- 使用 Ultralytics YOLO11 进行队列管理 🚀
- 什么是队列管理？
- 队列管理的优势
- 实际应用
  - QueueManager 参数
- 实施策略
- 常见问题
  - 如何使用 Ultralytics YOLO11 进行实时队列管理？
  - 使用 Ultralytics YOLO11 进行队列管理的主要优势是什么？
  - 对于队列管理，与 TensorFlow 或 Detectron2 等竞争对手相比，为什么我应该选择 Ultralytics YOLO11？

使用 Ultralytics YOLO11 进行队列管理包括组织和控制人员或车辆的队伍，以减少等待时间并提高效率。它旨在优化队列，以提高零售、银行、机场和医疗机构等各种环境中的客户满意度和系统性能。

观看： 如何使用 Ultralytics YOLO 构建队列管理系统 | 零售、银行和人群用例 🚀

使用 Ultralytics YOLO 进行队列管理

这是一个包含以下内容的表格 QueueManager 参数：

字段 QueueManagement 解决方案也支持一些 track 参数：

在使用YOLO11实施队列管理时，请考虑以下最佳实践：

要使用 Ultralytics YOLO11 进行实时队列管理，您可以按照以下步骤操作：

利用 Ultralytics HUB 可以通过为部署和管理您的队列管理解决方案提供用户友好的平台来简化此过程。

使用 Ultralytics YOLO11 进行队列管理具有以下几个优势：

有关更多详细信息，请浏览我们的队列管理解决方案。

在队列管理方面，Ultralytics YOLO11 比 TensorFlow 和 Detectron2 具有以下几个优势：

了解如何开始使用 Ultralytics YOLO。

是的，Ultralytics YOLO11 可以管理各种类型的队列，包括机场和零售环境中的队列。通过使用特定区域和设置配置 QueueManager，YOLO11 可以适应不同的队列布局和密度。

有关各种应用的更多信息，请查看我们的实际应用部分。

Ultralytics YOLO11 被广泛应用于各种实际场景中的队列管理：

请查看我们的关于实际队列管理的博客，以了解更多关于计算机视觉如何改变各行业队列监控的信息。

**Examples:**

Example 1 (markdown):
```markdown
# Run a queue example
yolo solutions queue show=True

# Pass a source video
yolo solutions queue source="path/to/video.mp4"

# Pass queue coordinates
yolo solutions queue region="[(20, 400), (1080, 400), (1080, 360), (20, 360)]"
```

Example 2 (python):
```python
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "Error reading video file"

# Video writer
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))
video_writer = cv2.VideoWriter("queue_management.avi", cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))

# Define queue points
queue_region = [(20, 400), (1080, 400), (1080, 360), (20, 360)]  # region points
# queue_region = [(20, 400), (1080, 400), (1080, 360), (20, 360), (20, 400)]    # polygon points

# Initialize queue manager object
queuemanager = solutions.QueueManager(
    show=True,  # display the output
    model="yolo11n.pt",  # path to the YOLO11 model file
    region=queue_region,  # pass queue region points
)

# Process video
while cap.isOpened():
    success, im0 = cap.read()
    if not success:
        print("Video frame is empty or processing is complete.")
        break
    results = queuemanager(im0)

    # print(results)  # access the output

    video_writer.write(results.plot_im)  # write the processed frame.

cap.release()
video_writer.release()
cv2.destroyAllWindows()  # destroy all opened windows
```

Example 3 (python):
```python
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
queue_region = [(20, 400), (1080, 400), (1080, 360), (20, 360)]

queuemanager = solutions.QueueManager(
    model="yolo11n.pt",
    region=queue_region,
    line_width=3,
    show=True,
)

while cap.isOpened():
    success, im0 = cap.read()
    if success:
        results = queuemanager(im0)

cap.release()
cv2.destroyAllWindows()
```

Example 4 (unknown):
```unknown
queue_region_airport = [(50, 600), (1200, 600), (1200, 550), (50, 550)]
queue_airport = solutions.QueueManager(
    model="yolo11n.pt",
    region=queue_region_airport,
    line_width=3,
)
```

---

## YOLOv7 对比 YOLOv6-3.0：全面技术比较

**URL:** https://docs.ultralytics.com/zh/compare/yolov7-vs-yolov6/

**Contents:**
- YOLOv7 对比 YOLOv6-3.0：全面技术比较
- YOLOv7: 准确性架构
  - 主要架构特性
  - 优势与劣势
- YOLOv6-3.0：专为工业速度而设计
  - 主要架构特性
  - 优势与劣势
- 性能对比
  - 结果分析
- Ultralytics 优势：超越比较

在快速发展的计算机视觉领域，选择合适的物体 detect 模型对于项目成功至关重要。塑造了该领域的两个重要框架是 YOLOv7 和 YOLOv6-3.0。尽管两者都源自 YOLO (You Only Look Once) 系列，但它们在架构理念和优化目标上存在显著差异。

本指南对这两种模型进行了深入的技术分析，比较了它们的架构、性能指标和理想部署场景。我们还将探讨 Ultralytics YOLO11 等现代替代方案如何将这些前代产品的最佳特性整合到一个统一、用户友好的生态系统中。

于 2022 年 7 月发布的YOLOv7代表了 YOLO 系列的一次重大转变，它优先考虑架构创新，以最大限度地提高准确性，同时不牺牲实时推理能力。它的设计旨在突破COCO dataset基准的界限。

作者：王建尧、Alexey Bochkovskiy、廖鸿源Chien-Yao Wang, Alexey Bochkovskiy, and Hong-Yuan Mark Liao组织：台湾中央研究院信息科学研究所日期：2022-07-06Arxiv:https://arxiv.org/abs/2207.02696 GitHub:https://github.com/WongKinYiu/yolov7Docs:ultralytics

YOLOv7引入了“可训练的免费包”，这是一组优化方法，可在不增加推理成本的情况下提高精度。

YOLOv7 以其高平均精度均值（mAP）而闻名，尤其在小型和遮挡物体的检测上表现出色。它是研究和精度至关重要场景的绝佳选择。然而，其复杂的架构，严重依赖基于拼接的层，与流线型工业模型相比，可能导致训练期间更高的内存消耗。

YOLOv6-3.0 由美团的视觉计算部门开发，非常注重实际的工业应用。它于 2023 年初发布，优先考虑推理速度和硬件效率，使其成为边缘计算的有力候选者。

作者: Chuyi Li, Lulu Li, Yifei Geng, Hongliang Jiang, Meng Cheng, Bo Zhang, Zaidan Ke, Xiaoming Xu, and Xiangxiang Chu机构: 美团日期: 2023-01-13Arxiv:https://arxiv.org/abs/2301.05586GitHub:https://github.com/meituan/YOLOv6文档:https://docs.ultralytics.com/models/yolov6/

YOLOv6-3.0 以其硬件感知设计而著称，专门针对 GPU 和 CPU 吞吐量进行优化。

YOLOv6-3.0在原始吞吐量方面表现出色。对于工业自动化生产线或机器人技术而言，毫秒必争，其优化的推理图是一个显著优势。然而，它主要侧重于detect，缺乏YOLO11等后续迭代版本中原生的多任务通用性。

下表展示了这两种模型之间的权衡。YOLOv6-3.0 通常在相似的准确性级别上提供更快的速度，而 YOLOv7 则将检测精度推向了极限。

尽管基准测试提供了基线，但实际性能在很大程度上取决于部署硬件。YOLOv6 的重参数化在 GPU 上表现出色，而 YOLOv7 基于拼接的架构虽然稳健，但可能对内存带宽要求很高。

虽然YOLOv7和YOLOv6-3.0代表了计算机视觉史上的重大成就，但该领域发展迅速。对于寻求可持续、面向未来的解决方案的开发者来说，Ultralytics YOLO11提供了一个全面的生态系统，超越了单个模型架构的局限性。

使用 Ultralytics 部署最先进的模型简单明了。以下是您如何轻松实现对象 detect 的方法：

YOLOv7 和 YOLOv6-3.0 各自服务于特定领域：YOLOv7 用于高精度研究任务，YOLOv6-3.0 用于工业速度优化。然而，对于大多数开发者和研究人员而言，Ultralytics YOLO11 生态系统提供了最平衡、通用且可维护的解决方案。通过将高性能与卓越的用户体验和广泛的任务支持相结合，Ultralytics 使S用户能够专注于解决实际问题，而不是纠结于模型架构。

如果您对探索计算机视觉领域的更多选项感兴趣，可以参考这些比较：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLO11 model
model = YOLO("yolo11n.pt")

# Run inference on an image
results = model("https://ultralytics.com/images/bus.jpg")

# Process results
for result in results:
    result.save(filename="output.jpg")  # save to disk
```

---

## 使用 ExecuTorch 在移动和边缘设备上部署 YOLO11 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/integrations/executorch/

**Contents:**
- 使用 ExecuTorch 在移动和边缘设备上部署 YOLO11
- 为什么导出到ExecuTorch？
- ExecuTorch 的主要特性
- 使用ExecuTorch的部署选项
- 将 Ultralytics YOLO11 模型导出到 ExecuTorch
  - 安装
  - 用法
  - 导出参数
  - 输出结构
- 使用导出的 ExecuTorch 模型

在智能手机、平板电脑和嵌入式系统等边缘设备上部署计算机视觉模型需要一个优化的运行时，以平衡性能和资源限制。ExecuTorch，PyTorch 的边缘计算解决方案，为 Ultralytics YOLO 模型提供高效的设备上推理。

本指南概述了如何将Ultralytics YOLO模型导出为ExecuTorch格式，使您能够在移动和边缘设备上部署模型，并获得优化的性能。

ExecuTorch 是 PyTorch 的端到端解决方案，用于在移动和边缘设备上实现设备上的推理能力。ExecuTorch 以可移植性和高效性为目标构建，可用于在各种计算平台上运行 PyTorch 程序。

ExecuTorch 提供多项强大功能，用于在边缘设备上部署 Ultralytics YOLO 模型：

可移植模型格式：ExecuTorch 使用 .pte （PyTorch ExecuTorch）格式，该格式针对资源受限设备上的大小和加载速度进行了优化。

XNNPACK后端：默认集成XNNPACK可在移动CPU上提供高度优化的推理，无需专用硬件即可提供卓越性能。

量化支持：内置对量化技术的支持，以在保持精度的同时减小模型大小并提高推理速度。

内存效率：优化的内存管理减少了运行时内存占用，使其适用于 RAM 有限的设备。

模型元数据：导出的模型在一个单独的 yaml 文件中包含元数据（图像大小、类别名称等），便于集成。

ExecuTorch 模型可以部署到各种边缘和移动平台：

移动应用: 在 iOS 和 Android 应用上部署，具有原生性能，从而在移动应用中实现实时目标 detect。

嵌入式系统: 运行在树莓派、NVIDIA Jetson及其他基于ARM的嵌入式Linux设备上，并具有优化性能。

边缘AI设备：部署在专用边缘AI硬件上，借助自定义委托以加速推理。

物联网设备：集成到物联网设备中，用于设备端推理，无需云连接。

将 Ultralytics YOLO11 模型转换为 ExecuTorch 格式，可实现在移动和边缘设备上的高效部署。

ExecuTorch 导出需要 Python 3.10 或更高版本以及特定的依赖项：

有关安装过程的详细说明和最佳实践，请查看我们的YOLO11安装指南。在为YOLO11安装所需软件包时，如果遇到任何困难，请查阅我们的常见问题指南，获取解决方案和技巧。

将 YOLO11 模型导出到 ExecuTorch 非常简单：

ExecuTorch 导出会生成一个目录，其中包含一个 .pte 文件和元数据。在您的移动或嵌入式应用程序中使用ExecuTorch运行时加载 .pte 模型并执行推理。

导出为 ExecuTorch 格式时，您可以指定以下参数：

ExecuTorch 导出会创建一个包含模型和元数据的目录：

导出模型后，您需要使用 ExecuTorch 运行时将其集成到您的目标应用程序中。

对于移动应用程序 (iOS/Android)，您需要：

iOS集成示例 (Objective-C/C++)：

Android集成示例 (Kotlin)：

对于嵌入式 Linux 系统，请使用 ExecuTorch C++ API：

有关将 ExecuTorch 集成到您的应用程序中的更多详细信息，请访问 ExecuTorch 文档。

Ultralytics 团队对 YOLO11 模型进行了基准测试，比较了 PyTorch 和 ExecuTorch 之间的速度和准确性。

问题: Python version error

解决方案：ExecuTorch 需要 Python 3.10 或更高版本。请升级您的 Python 安装：

问题: Export fails during first run

解决方案：ExecuTorch 在首次使用时可能需要下载和编译组件。请确保您已具备：

问题: Import errors for ExecuTorch modules

解决方案：确保 ExecuTorch 已正确安装：

如需更多故障排除帮助，请访问 Ultralytics GitHub 问题 或 ExecuTorch 文档。

将 YOLO11 模型导出到 ExecuTorch 格式，可以在移动和边缘设备上实现高效部署。凭借 PyTorch 原生集成、跨平台支持和优化的性能，ExecuTorch 是边缘 AI 应用的绝佳选择。

使用 Python 或 CLI 将 YOLO11 模型导出到 ExecuTorch：

注意：在首次导出期间，ExecuTorch 将自动下载并编译必要的组件，包括 FlatBuffers 编译器。

ExecuTorch 模型（.pte 文件）旨在利用 ExecuTorch 运行时部署到移动和边缘设备上。它们无法直接通过...加载 YOLO() 用于 python 中的推理。您需要使用 ExecuTorch 运行时库将它们集成到您的目标应用程序中。

ExecuTorch 和 TFLite 都非常适合移动部署：

如果您已经在使用PyTorch并希望获得原生部署路径，请选择ExecuTorch。选择TFLite以获得最大的兼容性和成熟的工具链。

是的！ExecuTorch 通过各种后端支持硬件加速：

请参阅 ExecuTorch 文档 以获取后端特定的设置。

**Examples:**

Example 1 (go):
```go
# Install Ultralytics package
pip install ultralytics
```

Example 2 (python):
```python
from ultralytics import YOLO

# Load the YOLO11 model
model = YOLO("yolo11n.pt")

# Export the model to ExecuTorch format
model.export(format="executorch")  # creates 'yolo11n_executorch_model' directory

executorch_model = YOLO("yolo11n_executorch_model")

results = executorch_model.predict("https://ultralytics.com/images/bus.jpg")
```

Example 3 (markdown):
```markdown
# Export a YOLO11n PyTorch model to ExecuTorch format
yolo export model=yolo11n.pt format=executorch # creates 'yolo11n_executorch_model' directory

# Run inference with the exported model
yolo predict model=yolo11n_executorch_model source=https://ultralytics.com/images/bus.jpg
```

Example 4 (unknown):
```unknown
yolo11n_executorch_model/
├── yolo11n.pte              # ExecuTorch model file
└── metadata.yaml            # Model metadata (classes, image size, etc.)
```

---

## DAMO-YOLO 对比 YOLOv6-3.0：一项技术比较

**URL:** https://docs.ultralytics.com/zh/compare/damo-yolo-vs-yolov6/

**Contents:**
- DAMO-YOLO 对比 YOLOv6-3.0：一项技术比较
- DAMO-YOLO 概述
  - 架构和主要特性
  - 优势与劣势
  - 理想用例
- YOLOv6-3.0 概述
  - 架构和主要特性
  - 优势与劣势
  - 理想用例
- 性能分析

选择理想的对象detect架构对于计算机视觉工程师来说是一个关键决策，通常需要在精度、推理延迟和硬件限制之间进行仔细权衡。本指南提供了一项全面的技术分析，比较了来自阿里巴巴集团的高精度模型DAMO-YOLO，以及来自美团的以效率为中心的框架YOLOv6-3.0。

我们将研究它们的架构创新、在标准数据集上的基准性能以及在实际部署中的适用性。此外，我们还将探讨 Ultralytics YOLO11 如何为寻求统一解决方案的开发者提供一个现代化、多功能的替代方案。

DAMO-YOLO 是阿里巴巴集团开发的一种前沿的 detect 方法。它通过整合神经架构搜索（NAS）和多个旨在消除计算瓶颈的新颖模块，优先考虑速度和准确性之间的权衡。

作者： 徐贤哲、蒋一奇、陈卫华、黄一伦、张远、孙秀宇机构：阿里巴巴集团日期： 2022-11-23预印本：https://arxiv.org/abs/2211.15444v2GitHub：https://github.com/tinyvision/DAMO-YOLO文档：https://github.com/tinyvision/DAMO-YOLO/blob/master/README.md

DAMO-YOLO 引入了由独特架构设计支持的“从小到大”的缩放策略。关键组件包括：

DAMO-YOLO 在对mAP的每一个百分点都至关重要的场景中表现突出。

YOLOv6-3.0 是美团开发的 YOLOv6 框架的第三次迭代。它专为工业应用而设计，强调在 GPU 上的高吞吐量和易于部署。

作者: Chuyi Li, Lulu Li, Yifei Geng, Hongliang Jiang, Meng Cheng, Bo Zhang, Zaidan Ke, Xiaoming Xu, and Xiangxiang Chu机构:美团日期: 2023-01-13Arxiv:https://arxiv.org/abs/2301.05586GitHub:https://github.com/meituan/YOLOv6文档:https://docs.ultralytics.com/models/yolov6/

YOLOv6-3.0专注于最大化GPU利用率的硬件友好型设计：

YOLOv6-3.0是需要标准GPU部署的工业环境的强大工具。

以下比较突出了两种模型在 COCO 数据集上的性能。这些指标侧重于 IoU 0.5-0.95 下的验证 mAP（平均精度均值）、使用 TensorRT 在 T4 GPU 上的推理速度，以及模型复杂度（参数量和 FLOPs）。

YOLOv6.0n是速度冠军，推理速度低于 2 毫秒，非常适合对延迟极为敏感的应用。不过，YOLO-YOLO型号（特别是小型和中型变体）的mAP 分数往往高于YOLOv6 型号，这表明它们的 NAS 主干网具有强大的架构效率。

尽管DAMO-YOLO和YOLOv6-3.0在特定细分领域提供了引人注目的特性，但Ultralytics YOLO11代表了计算机视觉AI的全面演进。YOLO11专为需要不仅仅是检测模型的开发者设计，它融合了最先进的性能与无与伦比的用户体验。

对于严格要求在工业GPU上实现最高吞吐量的项目，YOLOv6-3.0是一个强有力的竞争者。如果您的重点是使用NAS在特定参数预算内最大化精度，DAMO-YOLO是一个出色的研究级选项。

然而，对于绝大多数商业和研究应用而言，Ultralytics YOLO11 在性能、可用性和长期可维护性之间提供了最佳平衡。它处理多任务的能力，结合强大且维护良好的生态系统，使其成为构建可扩展计算机视觉解决方案的推荐选择。

通过探索这些其他详细比较，拓宽您对目标 detect 领域的理解：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLO11 model
model = YOLO("yolo11n.pt")

# Train the model on your custom dataset
model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference on an image
results = model("path/to/image.jpg")

# Export the model to ONNX format for deployment
model.export(format="onnx")
```

---

## Reference for ultralytics/utils/callbacks/tensorboard.py

**URL:** https://docs.ultralytics.com/zh/reference/utils/callbacks/tensorboard/

**Contents:**
- Reference for ultralytics/utils/callbacks/tensorboard.py
- function ultralytics.utils.callbacks.tensorboard._log_scalars
- function ultralytics.utils.callbacks.tensorboard._log_tensorboard_graph
- function ultralytics.utils.callbacks.tensorboard.on_pretrain_routine_start
- function ultralytics.utils.callbacks.tensorboard.on_train_start
- function ultralytics.utils.callbacks.tensorboard.on_train_epoch_end
- function ultralytics.utils.callbacks.tensorboard.on_fit_epoch_end

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/callbacks/tensorboard.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Log scalar values to TensorBoard.

Log model graph to TensorBoard.

This function attempts to visualize the model architecture in TensorBoard by tracing the model with a dummy input tensor. It first tries a simple method suitable for YOLO models, and if that fails, falls back to a more complex approach for models like RTDETR that may require special handling.

This function requires TensorBoard integration to be enabled and the global WRITER to be initialized. It handles potential warnings from the PyTorch JIT tracer and attempts to gracefully handle different model architectures.

Initialize TensorBoard logging with SummaryWriter.

Log TensorBoard graph.

Log scalar statistics at the end of a training epoch.

Log epoch metrics at end of training epoch.

**Examples:**

Example 1 (typescript):
```typescript
def _log_scalars(scalars: dict, step: int = 0) -> None
```

Example 2 (json):
```json
Log training metrics
>>> metrics = {"loss": 0.5, "accuracy": 0.95}
>>> _log_scalars(metrics, step=100)
```

Example 3 (json):
```json
def _log_scalars(scalars: dict, step: int = 0) -> None:
    """Log scalar values to TensorBoard.

    Args:
        scalars (dict): Dictionary of scalar values to log to TensorBoard. Keys are scalar names and values are the
            corresponding scalar values.
        step (int): Global step value to record with the scalar values. Used for x-axis in TensorBoard graphs.

    Examples:
        Log training metrics
        >>> metrics = {"loss": 0.5, "accuracy": 0.95}
        >>> _log_scalars(metrics, step=100)
    """
    if WRITER:
        for k, v in scalars.items():
            WRITER.add_scalar(k, v, step)
```

Example 4 (python):
```python
def _log_tensorboard_graph(trainer) -> None
```

---

## Intel OpenVINO 导出 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/integrations/openvino/

**Contents:**
- Intel OpenVINO 导出
- 使用示例
- 导出参数
- OpenVINO 的优势
- OpenVINO 导出结构
- 在部署中使用 OpenVINO 导出
  - 使用Ultralytics进行推理
  - 使用OpenVINO Runtime进行推理
- OpenVINO YOLO11 基准测试
  - Intel Core CPU

在本指南中，我们将介绍如何将 YOLO11 模型导出为 OpenVINO 格式，该格式可提供高达 3 倍的 CPU 速度提升，以及加速 Intel GPU 和 NPU 硬件上的 YOLO 推理。

OpenVINO 是 Open Visual Inference & Neural Network Optimization 工具包的缩写，是一个用于优化和部署 AI 推理模型的综合工具包。即使名称中包含 Visual，OpenVINO 也支持各种其他任务，包括语言、音频、时间序列等。

观看： 如何将 Ultralytics YOLO11 导出为 Intel OpenVINO 格式以实现更快的推理 🚀

将 YOLO11n 模型导出为 OpenVINO 格式，并使用导出的模型运行推理。

有关导出过程的更多详细信息，请访问Ultralytics 文档页面上的导出。

OpenVINO™ 与大多数 Intel® 处理器兼容，但为确保最佳性能：

验证 OpenVINO™ 支持：使用 Intel 的兼容性列表 检查您的 Intel® 芯片是否受 OpenVINO™ 官方支持。

识别您的加速器：通过查阅 Intel 的硬件指南，确定您的处理器是否包含集成 NPU（神经网络处理单元）或 GPU（集成 GPU）。

安装最新驱动程序 如果您的芯片支持 NPU 或 GPU，但 OpenVINO™ 未能 detect 到它，您可能需要安装或更新相关的驱动程序。请遵循驱动程序安装说明以启用完全加速。

通过遵循这三个步骤，您可以确保 OpenVINO™ 在您的 Intel® 硬件上以最佳状态运行。

将模型导出为 OpenVINO 格式后，会生成一个包含以下内容的目录：

您可以使用这些文件通过 OpenVINO 推理引擎运行推理。

一旦您的模型成功导出为 OpenVINO 格式，您有两种主要的运行推理的选项：

使用 ultralytics package，它提供了一个高级 API 并封装了 OpenVINO Runtime。

使用原生 openvino package，用于对推理行为进行更高级或自定义的控制。

通过 ultralytics 包，您可以轻松地使用导出的 OpenVINO 模型，通过 predict 方法运行推理。您还可以指定目标设备（例如， intel:gpu, intel:npu, intel:cpu）使用 device 参数。

当您不需要完全控制推理流程时，此方法非常适合快速原型设计或部署。

OpenVINO Runtime 为所有支持的 Intel 硬件上的推理提供统一的 API。它还提供高级功能，如跨 Intel 硬件的负载平衡和异步执行。有关运行推理的更多信息，请参阅YOLO11 笔记本。

请记住，您需要 XML 和 BIN 文件以及任何特定于应用程序的设置（如输入大小、归一化比例因子等），才能正确设置模型并将其与 Runtime 一起使用。

在您的部署应用程序中，您通常会执行以下步骤：

有关更详细的步骤和代码片段，请参阅 OpenVINO 文档或 API 教程。

Ultralytics 团队对 YOLO11 进行了跨各种模型格式和精度的基准测试，评估了与 OpenVINO 兼容的不同 Intel 设备上的速度和准确性。

下面的基准测试结果仅供参考，可能会因系统的确切硬件和软件配置以及运行基准测试时系统的当前工作负载而异。

所有基准测试均使用 openvino python 包版本 2025.1.0.

Intel® Core® 系列是 Intel 的一系列高性能处理器。该系列包括 Core i3（入门级）、Core i5（中端）、Core i7（高端）和 Core i9（极致性能）。每个系列都满足不同的计算需求和预算，从日常任务到要求严苛的专业工作负载。随着每一代新产品的推出，性能、能源效率和功能都会得到改进。

以下基准测试在 FP32 精度下，于第 12 代 Intel® Core® i9-12900KS CPU 上运行。

Intel® Core™ Ultra™ 系列代表了高性能计算的新标杆，旨在满足现代用户不断变化的需求——从游戏玩家和创作者到利用 AI 的专业人士。这一代产品不仅仅是传统的 CPU 系列；它在单个芯片中结合了强大的 CPU 核心、集成的 GPU 高性能能力和专用的神经处理单元 (NPU)，为各种密集型计算工作负载提供统一的解决方案。

Intel® Core Ultra™ 架构的核心是混合设计，可在传统处理任务、GPU 加速工作负载和 AI 驱动的操作中实现卓越性能。NPU 的加入增强了设备上的 AI 推理能力，从而在各种应用中实现更快、更高效的机器学习和数据处理。

Core Ultra™ 系列包括专为不同性能需求量身定制的各种型号，其选项范围从节能设计到以“H”标识的高功率型号，后者非常适合需要强大计算能力的笔记本电脑和紧凑型外形。在整个产品线中，用户都可以从 CPU、GPU 和 NPU 集成的协同作用中受益，从而提供卓越的效率、响应能力和多任务处理能力。

作为 Intel 不断创新的一部分，Core Ultra™ 系列为面向未来的计算设定了新标准。该系列提供多种型号，并且未来还将推出更多型号，这突显了 Intel 致力于为下一代智能、AI 增强型设备提供尖端解决方案的承诺。

以下基准测试在 FP32 和 INT8 精度下，于 Intel® Core™ Ultra™ 7 258V 和 Intel® Core™ Ultra™ 7 265K 上运行。

Intel® Arc™ 是 Intel 专为高性能游戏、内容创作和 AI 工作负载而设计的独立显卡系列。Arc 系列采用先进的 GPU 架构，支持实时光线追踪、AI 增强图形和高分辨率游戏。Intel® Arc™ 注重性能和效率，旨在与其他领先的 GPU 品牌竞争，同时提供硬件加速 AV1 编码和对最新图形 API 的支持等独特功能。

以下基准测试在 FP32 和 INT8 精度下，于 Intel Arc A770 和 Intel Arc B580 上运行。

要在所有导出格式上重现上述 Ultralytics 基准测试，请运行以下代码：

请注意，基准测试结果可能因系统的具体硬件和软件配置以及运行基准测试时系统的当前工作负载而异。为了获得最可靠的结果，请使用包含大量图像的数据集，例如： data='coco.yaml' （5000 张验证图像）。

基准测试结果清楚地表明了将 YOLO11 模型导出为 OpenVINO 格式的优势。在不同的模型和硬件平台上，OpenVINO 格式在推理速度方面始终优于其他格式，同时保持了相当的准确性。

这些基准测试强调了 OpenVINO 作为部署深度学习模型的有效性。通过将模型转换为 OpenVINO 格式，开发人员可以获得显著的性能提升，从而更轻松地在实际应用中部署这些模型。

有关使用 OpenVINO 的更多详细信息和说明，请参阅官方 OpenVINO 文档。

将 YOLO11 模型导出为 OpenVINO 格式可以显着提高 CPU 速度，并支持 Intel 硬件上的 GPU 和 NPU 加速。要导出，您可以使用 python 或 CLI，如下所示：

将 Intel 的 OpenVINO 工具包与 YOLO11 模型结合使用可带来诸多好处：

有关详细的性能比较，请访问我们的基准测试部分。

将 YOLO11n 模型导出为 OpenVINO 格式后，您可以使用 Python 或 CLI 运行推理：

有关更多详细信息，请参阅我们的预测模式文档。

Ultralytics YOLO11 经过优化，可实现高精度和速度的实时对象检测。具体来说，当与 OpenVINO 结合使用时，YOLO11 提供：

如需深入的性能分析，请查看我们在不同硬件上的详细 YOLO11 基准测试。

是的，您可以对各种格式的 YOLO11 模型进行基准测试，包括 PyTorch、TorchScript、ONNX 和 OpenVINO。 使用以下代码片段在您选择的数据集上运行基准测试：

有关详细的基准测试结果，请参阅我们的基准测试部分和导出格式文档。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a YOLO11n PyTorch model
model = YOLO("yolo11n.pt")

# Export the model
model.export(format="openvino")  # creates 'yolo11n_openvino_model/'

# Load the exported OpenVINO model
ov_model = YOLO("yolo11n_openvino_model/")

# Run inference
results = ov_model("https://ultralytics.com/images/bus.jpg")

# Run inference with specified device, available devices: ["intel:gpu", "intel:npu", "intel:cpu"]
results = ov_model("https://ultralytics.com/images/bus.jpg", device="intel:gpu")
```

Example 2 (markdown):
```markdown
# Export a YOLO11n PyTorch model to OpenVINO format
yolo export model=yolo11n.pt format=openvino # creates 'yolo11n_openvino_model/'

# Run inference with the exported model
yolo predict model=yolo11n_openvino_model source='https://ultralytics.com/images/bus.jpg'

# Run inference with specified device, available devices: ["intel:gpu", "intel:npu", "intel:cpu"]
yolo predict model=yolo11n_openvino_model source='https://ultralytics.com/images/bus.jpg' device="intel:gpu"
```

Example 3 (python):
```python
from ultralytics import YOLO

# Load the exported OpenVINO model
ov_model = YOLO("yolo11n_openvino_model/")  # the path of your exported OpenVINO model
# Run inference with the exported model
ov_model.predict(device="intel:gpu")  # specify the device you want to run inference on
```

Example 4 (python):
```python
from ultralytics import YOLO

# Load a YOLO11n PyTorch model
model = YOLO("yolo11n.pt")

# Benchmark YOLO11n speed and accuracy on the COCO128 dataset for all export formats
results = model.benchmark(data="coco128.yaml")
```

---

## YOLOv6-3.0 与 YOLOv8：全面技术比较

**URL:** https://docs.ultralytics.com/zh/compare/yolov6-vs-yolov8/

**Contents:**
- YOLOv6-3.0 与 YOLOv8：全面技术比较
- YOLOv6-3.0
  - 架构和主要特性
  - 优势与劣势
- Ultralytics YOLOv8
  - 架构和主要特性
  - 为什么选择YOLOv8？
- 性能对比
  - 关键分析
- 应用案例与应用

选择最佳的物体检测架构是计算机视觉开发过程中的关键决策，它影响着从推理延迟到部署灵活性的方方面面。本指南提供了深入的技术分析，比较了美团开发的YOLOv6.0 和 Ultralytics YOLOv8进行了深入的技术分析。 Ultralytics.我们研究了它们的架构特点、性能指标以及在实际应用中的适用性，以帮助您做出明智的选择。

尽管这两个框架都取得了令人印象深刻的成果，但 YOLOv8 凭借无与伦比的多功能性、以开发人员为中心的生态系统以及在各种硬件平台上速度和准确性之间的卓越平衡而脱颖而出。

作者: Chuyi Li, Lulu Li, Yifei Geng, Hongliang Jiang, Meng Cheng, Bo Zhang, Zaidan Ke, Xiaoming Xu, and Xiangxiang Chu组织: Meituan日期: 2023-01-13Arxiv: https://arxiv.org/abs/2301.05586GitHub: https://github.com/meituan/YOLOv6文档: https://docs.ultralytics.com/models/yolov6/

YOLOv6-3.0是一个单阶段物体detect框架，主要专注于工业应用。通过优先考虑硬件友好的网络设计，它旨在最大化专用GPU上的推理吞吐量，使其成为延迟受生产线速度严格限制的环境中的有力竞争者。

YOLOv6-3.0 的架构围绕重参数化概念构建。它采用了 EfficientRep 主干网络和 Rep-PAN 颈部，这使得网络在训练期间可以拥有复杂结构，但在推理期间简化为流线型的卷积层。这种“结构重参数化”有助于降低延迟，同时不牺牲特征提取能力。

此外，YOLOv6-3.0 采用了分离分类和回归任务的解耦头设计，并集成了SimOTA标签分配策略。该框架还强调量化感知训练 (QAT)，以促进在需要低精度算术的边缘设备上部署。

该模型在工业制造场景中表现出色，在有高端 GPU 可用的情况下，提供具有竞争力的推理速度。其对量化的关注也有助于部署到特定的硬件加速器。然而，YOLOv6 主要设计用于目标检测，缺乏更全面框架中对更广泛的计算机视觉任务（如姿势估计或旋转框检测）的原生、无缝支持。此外，其生态系统不够广泛，这可能意味着在与第三方 MLOps 工具集成或寻求社区支持时会遇到更多阻力。

作者: Glenn Jocher, Ayush Chaurasia, and Jing Qiu组织: Ultralytics日期: 2023-01-10Arxiv: 无GitHub: https://github.com/ultralytics/ultralytics文档: https://docs.ultralytics.com/models/yolov8/

Ultralytics YOLOv8 代表了 YOLO 系列的重大飞跃，它不仅被设计为一个模型，而且是一个适用于实际 AI 的统一框架。它通过将架构效率与直观的用户体验相结合，重新定义了最先进的 (SOTA) 性能，使研究人员和开发人员都可以使用高级计算机视觉。

YOLOv8 引入了高效的无锚点检测机制，消除了手动锚框计算的需要，并提高了在多样化数据集上的泛化能力。其架构特点是采用新的骨干网络，利用C2f 模块（带有融合的跨阶段部分连接），在保持轻量级的同时，增强了梯度流和特征丰富性。

YOLOv8中的解耦头独立处理目标性、分类和回归，从而实现更高的收敛精度。至关重要的是，该模型在一个可安装的python包中支持全方位的任务——目标检测、实例segmentation、图像分类、姿势估计和旋转框检测 (OBB)——。

下表详细比较了 COCO val2017 数据集上的性能指标。高亮部分表示每个类别中的最佳性能。

数据揭示了 Ultralytics 架构的独特优势：

YOLOv8 的高效架构使得在训练期间的 GPU 内存需求较低，与许多基于 Transformer 的模型或更重的卷积网络相比。这使得开发者能够在消费级硬件上训练更大的批次或使用更高的分辨率。

这些模型之间的选择通常取决于具体的部署环境和任务要求。

YOLOv8 是绝大多数计算机视觉项目的推荐选择，因为它具有适应性：

YOLOv6-3.0 仍然是利基工业场景的强有力选择：

最显著的差异化因素之一是开发者体验。Ultralytics 优先采用低代码、高功能的方法。

训练 YOLOv8 模型非常简单。该框架自动处理数据增强、超参数演进和绘图。

相比之下，尽管 YOLOv6 提供了训练脚本，但它通常涉及更多手动配置环境变量和依赖项。YOLOv8 与 Ultralytics HUB 的集成通过提供基于网络的模型管理和一键式模型训练进一步简化了这一点。

Ultralytics 社区是 AI 领域最活跃的社区之一。无论您需要 自定义数据集 方面的帮助，还是高级 导出选项 方面的支持，都可以通过全面的文档和社区论坛轻松获取资源。

尽管 YOLOv6-3.0 为特定的工业 GPU-based detect 任务提供了强大的解决方案，但Ultralytics YOLOv8作为现代计算机视觉的卓越、全面的解决方案脱颖而出。其架构效率提供了更高的每参数准确性，并且其在 detect、segment 和分类任务上的多功能性使其面向未来。再加上无与伦比的生态系统和易用性，YOLOv8 使开发者能够自信地构建、部署和扩展 AI 解决方案。

对于那些对更广阔的目标检测领域感兴趣的用户，Ultralytics 支持广泛的模型。您可以将 YOLOv8 与传统 YOLOv5 进行比较，以了解架构的演变，或者探索尖端 YOLO11 以获取性能上的绝对最新成果。此外，对于基于 Transformer 的方法，RT-DETR 模型在实时检测方面提供了独特的优势。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a model
model = YOLO("yolov8n.pt")  # load a pretrained model

# Train the model
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference
results = model("path/to/image.jpg")
```

---

## 适用于 Ultralytics YOLO11 的 Sony IMX500 导出 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/integrations/sony-imx500/

**Contents:**
- 适用于 Ultralytics YOLO11 的 Sony IMX500 导出
- 为什么你应该导出到IMX500？
- 用于 YOLO11 模型的 Sony IMX500 导出
- 支持的任务
- 使用示例
- 导出参数
- 在部署中使用 IMX500 导出
  - 硬件先决条件
  - 软件先决条件
- 基准测试

本指南介绍了如何将 Ultralytics YOLO11 模型导出和部署到配备 Sony IMX500 传感器的 Raspberry Pi AI 摄像头。

在计算能力有限的设备（如 Raspberry Pi AI Camera）上部署计算机视觉模型可能很棘手。使用针对更快性能优化的模型格式会产生巨大的差异。

IMX500 模型格式旨在以最小的功耗提供快速的神经网络性能。它允许您优化 Ultralytics YOLO11 模型，以实现高速和低功耗的推理。在本指南中，我们将引导您完成将模型导出和部署到 IMX500 格式的过程，同时使您的模型更容易在 Raspberry Pi AI 摄像头上表现良好。

索尼的 IMX500 智能视觉传感器 是边缘 AI 处理领域的一项颠覆性硬件。它是世界上首款具有片上 AI 功能的智能视觉传感器。该传感器有助于克服边缘 AI 中的许多挑战，包括数据处理瓶颈、隐私问题和性能限制。 其他传感器仅仅传递图像和帧，而 IMX500 则讲述了一个完整的故事。它直接在传感器上处理数据，使设备能够实时生成见解。

IMX500 旨在改变设备直接在传感器上处理数据的方式，而无需将其发送到云端进行处理。

IMX500 适用于量化模型。量化使模型更小、更快，而不会损失太多准确性。它非常适合边缘计算的有限资源，通过减少延迟并允许在本地快速处理数据（无需云依赖）来使应用程序快速响应。本地处理还可以保护用户数据的隐私和安全，因为它不会发送到远程服务器。

Before You Begin（开始之前）: 为了获得最佳结果，请按照我们的模型训练指南、数据准备指南和超参数调整指南，确保您的 YOLO11 模型已为导出做好充分准备。

目前，您只能将包含以下任务的模型导出为 IMX500 格式。

导出 Ultralytics YOLO11 模型为 IMX500 格式，并使用导出的模型运行推理。

这里我们执行推理只是为了确保模型按预期工作。但是，对于在 Raspberry Pi AI Camera 上的部署和推理，请跳转到在部署中使用 IMX500 导出部分。

Ultralytics 软件包在运行时安装额外的导出依赖项。首次运行导出命令时，您可能需要重新启动控制台，以确保其正常工作。

如果您正在支持 CUDA 的 GPU 上导出，请传递参数 device=0 为了更快的导出。

有关导出过程的更多详细信息，请访问Ultralytics 文档页面上的导出。

导出过程将创建一个用于量化验证的 ONNX 模型，以及一个名为的目录 <model-name>_imx_model。此目录将包含 packerOut.zip 文件，这对于将模型打包以部署在 IMX500 硬件上至关重要。此外， <model-name>_imx_model 文件夹将包含一个文本文件（labels.txt)，其中列出了与模型相关联的所有标签。

将 Ultralytics YOLO11n 模型导出为 IMX500 格式后，可以将其部署到 Raspberry Pi AI Camera 以进行推理。

将 Raspberry Pi AI 摄像头连接到 Raspberry Pi 上的 15 针 MIPI CSI 连接器，然后打开 Raspberry Pi 的电源

本指南已经过在 Raspberry Pi 5 上运行的 Raspberry Pi OS Bookworm 的测试

步骤 1：打开终端窗口并执行以下命令，将 Raspberry Pi 软件更新到最新版本。

步骤 2：安装操作 IMX500 传感器所需的 IMX500 固件。

步骤 3：重新启动 Raspberry Pi 以使更改生效

步骤 4：安装 Aitrios Raspberry Pi 应用程序模块库

步骤 5：通过使用aitrios-rpi-application-module-library 示例中提供的以下脚本，运行 YOLO11 对象 detect、姿势估计、分类和 segment。

请务必替换 model_file 和 labels.txt 在运行这些脚本之前，请根据您的环境设置目录。

YOLOv8n、YOLO11n、YOLOv8n-姿势估计、YOLO11n-姿势估计、YOLOv8n-cls 和 YOLO11n-cls 的以下基准测试由 Ultralytics 团队在 Raspberry Pi AI 摄像头上运行，使用 imx 模型格式，用于衡量速度和准确性。

上述基准测试的验证是使用COCO128数据集用于检测模型，COCO8-姿势数据集用于姿势估计模型，以及ImageNet10用于分类模型。

Sony 的模型压缩工具包 (MCT) 是一款强大的工具，可通过量化和剪枝来优化深度学习模型。它支持各种量化方法，并提供先进的算法来减小模型尺寸和计算复杂度，而不会显着牺牲准确性。MCT 特别适用于在资源受限的设备上部署模型，从而确保高效的推理并减少延迟。

Sony 的 MCT 提供了一系列旨在优化神经网络模型的功能：

MCT 支持多种量化方法，以减少模型大小并提高推理速度：

MCT 引入了结构化的、硬件感知的模型剪枝，专为特定硬件架构设计。此技术通过剪枝 SIMD 组来利用目标平台的单指令多数据 (SIMD) 功能。这减少了模型大小和复杂性，同时优化了通道利用率，与 SIMD 架构对齐，以实现对权重内存占用有针对性的资源利用。可通过 Keras 和 PyTorch API 获得。

IMX500 转换器工具是 IMX500 工具集不可或缺的一部分，允许编译模型以部署在 Sony 的 IMX500 传感器（例如，Raspberry Pi AI 摄像头）上。此工具简化了通过 Ultralytics 软件处理的 Ultralytics YOLO11 模型的转换，确保它们在指定的硬件上兼容并高效运行。模型量化后的导出过程涉及生成二进制文件，这些文件封装了基本数据和设备特定的配置，从而简化了在 Raspberry Pi AI 摄像头上的部署过程。

导出为 IMX500 格式在各行业中具有广泛的适用性。以下是一些示例：

将 Ultralytics YOLO11 模型导出为 Sony 的 IMX500 格式，您可以部署模型以在基于 IMX500 的相机上进行高效推理。通过利用先进的量化技术，您可以减小模型尺寸并提高推理速度，而不会显着降低准确性。

有关更多信息和详细指南，请参阅索尼的 IMX500 网站。

要将 YOLO11 模型导出为 IMX500 格式，请使用 Python API 或 CLI 命令：

导出过程将创建一个包含部署所需文件的目录，包括 packerOut.zip.

IMX500 格式为边缘部署提供了几个重要的优势：

基于 Ultralytics 在 Raspberry Pi AI Camera 上的基准测试：

这表明 IMX500 格式提供了高效的实时推理，同时保持了边缘 AI 应用的良好准确性。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a YOLO11n PyTorch model
model = YOLO("yolo11n.pt")

# Export the model
model.export(format="imx", data="coco8.yaml")  # exports with PTQ quantization by default

# Load the exported model
imx_model = YOLO("yolo11n_imx_model")

# Run inference
results = imx_model("https://ultralytics.com/images/bus.jpg")
```

Example 2 (markdown):
```markdown
# Export a YOLO11n PyTorch model to imx format with Post-Training Quantization (PTQ)
yolo export model=yolo11n.pt format=imx data=coco8.yaml

# Run inference with the exported model
yolo predict model=yolo11n_imx_model source='https://ultralytics.com/images/bus.jpg'
```

Example 3 (python):
```python
from ultralytics import YOLO

# Load a YOLO11n-pose PyTorch model
model = YOLO("yolo11n-pose.pt")

# Export the model
model.export(format="imx", data="coco8-pose.yaml")  # exports with PTQ quantization by default

# Load the exported model
imx_model = YOLO("yolo11n-pose_imx_model")

# Run inference
results = imx_model("https://ultralytics.com/images/bus.jpg")
```

Example 4 (markdown):
```markdown
# Export a YOLO11n-pose PyTorch model to imx format with Post-Training Quantization (PTQ)
yolo export model=yolo11n-pose.pt format=imx data=coco8-pose.yaml

# Run inference with the exported model
yolo predict model=yolo11n-pose_imx_model source='https://ultralytics.com/images/bus.jpg'
```

---

## YOLOv8 vs. PP-YOLOE+：技术对比 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/compare/yolov8-vs-pp-yoloe/

**Contents:**
- YOLOv8 vs. PP-YOLOE+：技术对比
- Ultralytics YOLOv8：通用性和性能
  - 架构和主要特性
  - 优势
- PP-YOLOE+：PaddlePaddle 生态系统中的高精度
  - 架构和主要特性
  - 优势与劣势
- 性能基准分析
  - 用例推荐
- 使用与实现

选择最佳物体检测架构是一项关键决策，会影响计算机视觉应用的准确性、速度和部署灵活性。本指南对以下方面进行了深入的技术分析 Ultralytics YOLOv8和PP-YOLOE+ 的深入技术分析。通过研究它们的架构创新、性能基准和生态系统支持，我们旨在帮助开发人员和研究人员选择适合他们特定计算机视觉需求的工具。

Ultralytics YOLOv8 代表了 YOLO 系列的重大飞跃，旨在成为各种视觉任务的统一框架。它由 Ultralytics 开发，优先考虑无缝用户体验，同时不影响最先进的 (SOTA) 性能。

作者: Glenn Jocher, Ayush Chaurasia, and Jing Qiu机构:Ultralytics日期: 2023-01-10GitHub:https://github.com/ultralytics/ultralytics文档:https://docs.ultralytics.com/models/yolov8/

YOLOv8 引入了尖端的无锚点检测头，消除了手动锚框配置的需要，并提高了收敛性。骨干网络采用 C2f 模块——一种跨阶段部分瓶颈设计——可增强梯度流和特征提取效率。与许多竞争对手不同，YOLOv8 不仅限于目标检测；它原生支持实例分割、图像分类、姿势估计和旋转框检测 (OBB)。

YOLOv8 基于广泛采用的PyTorch框架构建，受益于庞大的工具和库生态系统。其设计侧重于训练效率，与基于 Transformer 的模型或旧的 detect 架构相比，需要显著更少的内存和收敛时间。

PP-YOLOE+ 是 PP-YOLOE 的演进版本，由百度作为PaddleDetection套件的一部分开发。它专注于实现高精度和推理速度，并专门针对PaddlePaddle深度学习框架进行了优化。

作者： PaddlePaddle 作者机构：百度日期： 2022-04-02预印本：https://arxiv.org/abs/2203.16250GitHub：https://github.com/PaddlePaddle/PaddleDetection/文档：https://github.com/PaddlePaddle/PaddleDetection/blob/release/2.8.1/configs/ppyoloe/README.md

PP-YOLOE+ 是一种无anchor、单阶段 detect 器。它集成了CSPRepResNet骨干网络和路径聚合网络（PAN）颈部，用于鲁棒的特征融合。一个显著特点是高效任务对齐头（ET-Head），它使用任务对齐学习（TAL）来更好地同步分类和定位预测。尽管功能强大，该模型却深深植根于百度生态系统，严重依赖 PaddlePaddle 特定的算子和优化工具。

比较 YOLOv8 和 PP-YOLOE+ 时，速度、精度和模型尺寸之间的权衡变得清晰。YOLOv8 展现出卓越的工程效率，以显著更少的参数和 FLOPs 提供具有竞争力或更高的精度。这种效率转化为更快的训练时间、更低的内存消耗和更灵敏的推理速度。

例如，YOLOv8n 是移动和嵌入式应用的理想选择，提供实时性能，同时计算开销极小。相比之下，尽管像 'x' 变体这样的 PP-YOLOE+ 模型突破了准确性的界限，但它们以更重、更慢为代价，这可能不适用于实时视频分析流。

对于生产环境而言，模型大小和速度通常与原始精度同样关键。YOLOv8的高效架构使其能够在更小、更便宜的硬件上部署，而不会显著降低detect质量。

Ultralytics YOLOv8最引人注目的优势之一是其开发者友好的API。虽然PP-YOLOE+需要配置PaddlePaddle生态系统，但YOLOv8只需几行Python代码即可实现。这降低了初学者的入门门槛，并加速了专家的原型开发。

以下是一个示例，展示了加载预训练 YOLOv8 模型并运行推理是多么简单：

训练自定义模型同样简单。Ultralytics 自动处理数据增强、超参数调整和数据集管理，让您能够专注于整理高质量数据。

尽管PP-YOLOE+是一个强大的竞争者，在百度生态系统中推动了检测精度的极限，但Ultralytics YOLOv8对于全球开发者社区而言，是更实用、更通用的选择。它与PyTorch的集成、卓越的每参数效率以及对多种视觉任务的全面支持，使其成为现代AI应用的通用工具。

The Ultralytics生态系统进一步放大了这一优势。借助Ultralytics HUB等工具实现轻松的模型训练和管理，以及详尽的文档指导您完成每一步，YOLOv8确保您的项目从概念到部署过程中的摩擦最小化。无论是构建智慧城市应用还是医疗诊断工具，YOLOv8都能提供成功所需的性能平衡和易用性。

如果您有兴趣拓宽对目标 detect 领域的理解，可以考虑探索这些其他比较：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv8 model
model = YOLO("yolov8n.pt")

# Run inference on an image
results = model.predict("https://ultralytics.com/images/bus.jpg")

# Display results
results[0].show()
```

---

## EfficientDet 对比 YOLOv6-3.0：全面技术比较

**URL:** https://docs.ultralytics.com/zh/compare/efficientdet-vs-yolov6/

**Contents:**
- EfficientDet 对比 YOLOv6-3.0：全面技术比较
- 性能指标比较
- EfficientDet：可扩展效率
  - 架构创新
  - 优势与劣势
- YOLOv6-3.0：工业级速度
  - 架构与设计
  - 理想用例
- 对比分析
  - 精度与速度

在不断发展的计算机视觉领域，选择正确的物体检测架构对于成功部署至关重要。本比较探讨了Google的研究型EfficientDet 和美团的工业级检测器YOLOv6.0 之间的技术区别。EfficientDet 引入了复合缩放等突破性的效率概念，而YOLOv6.0 则是专为低延迟工业应用而设计，突出了从学术基准到实际吞吐量的转变。

以下在COCO数据集上的基准测试说明了架构效率和推理延迟之间的权衡。YOLOv6-3.0利用重参数化技术在GPU硬件上表现出卓越的速度，而EfficientDet则以更高的计算成本保持了有竞争力的准确性。

EfficientDet 通过系统地优化网络深度、宽度和分辨率，代表了模型设计领域的一次范式转变。它基于 EfficientNet 主干网络构建，引入了双向特征金字塔网络 (BiFPN)，实现了简单的多尺度特征融合。

EfficientDet的核心是BiFPN，它允许信息自顶向下和自底向上流动，反复融合不同尺度的特征。这与旧检测器中常用的更简单的特征金字塔网络 (FPN) 形成对比。此外，EfficientDet采用复合缩放，这是一种使用单个复合系数 $\phi$ 统一缩放骨干网络、BiFPN和类别/边界框网络的方法。这种结构化方法确保了模型各维度资源的平衡，避免了手动设计架构中常见的瓶颈。

EfficientDet 在参数效率方面表现出色，与 YOLOv3 等同期模型相比，以相对较少的参数实现了高 mAP。它特别适用于模型大小（存储）是约束但延迟可协商的图像分类和 detect 任务。然而，BiFPN 层中复杂的非规则连接以及深度可分离卷积的广泛使用在标准 GPU 上可能效率低下，导致尽管 FLOP 计数较低，但推理延迟更高。

尽管EfficientDet的FLOPs（浮点运算）较低，但这并不总是意味着在GPU上速度更快。其深度可分离卷积的内存访问成本可能会成为性能瓶颈，相比之下，YOLO模型中使用的标准卷积则没有这个问题。

了解更多关于 EfficientDet 的信息

YOLOv6-3.0 不再仅仅关注学术指标，转而关注实际吞吐量，专门针对工业环境中发现的硬件限制进行优化。

YOLOv6-3.0采用了EfficientRep骨干网络，该网络利用重参数化（RepVGG风格）来解耦训练时和推理时的架构。在训练期间，模型使用复杂的多分支模块以实现更好的梯度流；而在推理时，这些模块会折叠成单个$3 \times 3$卷积，从而最大化GPU的计算密度。3.0版本还集成了量化感知训练（QAT）和自蒸馏等高级策略，使模型在量化到INT8精度后仍能保持准确性，以便在边缘设备上部署。

由于其硬件友好型设计，YOLOv6-3.0非常适合：

这两种模型在设计理念上的差异，根据部署硬件的不同，会产生不同的优势。

如表中所示，YOLOv6-3.0l实现了与EfficientDet-d6（52.6）相当的mAP（52.8），但在T4 GPU上运行速度几乎快10倍（8.95ms vs 89.29ms）。这一巨大差距凸显了深度可分离卷积在高吞吐量硬件上的低效率，相比于YOLOv6的密集卷积。EfficientDet凭借其最大的D7变体在绝对准确性上略占优势，但其延迟成本使其无法进行实时推理。

EfficientDet 严重依赖 TensorFlow 生态系统和 TPU 加速来实现高效训练。相比之下，YOLOv6 适用于 PyTorch 生态系统，这使得它对普通研究人员更易于使用。然而，这两种模型主要都是为 目标检测而设计的。对于需要 实例分割或 姿势估计的项目，用户通常需要寻找外部分支或替代架构。

尽管 YOLOv6-3.0 和 EfficientDet 是有能力的模型，但Ultralytics YOLO11代表了计算机视觉的下一次演进，通过统一的、以用户为中心的框架解决了这两个前代的局限性。

使用Ultralytics，从目标检测切换到实例分割就像更改模型名称一样简单（例如， yolo11n.pt 到 yolo11n-seg.pt）。与为新任务调整不同的架构（如 EfficientDet）相比，这种灵活性大大缩短了开发时间。

体验 Ultralytics API 的简洁性，与复杂的科研代码库相比：

EfficientDet 仍然是模型缩放理论中的一个里程碑，非常适合以 准确性为唯一衡量标准的学术研究或离线处理。YOLOv6-3.0 则推动了工业 边缘 AI 的发展，在支持的硬件上提供了出色的速度。

然而，对于一个兼顾最先进性能和开发者生产力的整体解决方案，Ultralytics YOLO11是推荐的选择。它集成了多样化的视觉任务、更低的内存占用和强大的支持系统，使开发者能够自信地从原型阶段迈向生产。

如果您有兴趣进一步探索，请考虑我们文档中的这些相关比较：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLO11 model
model = YOLO("yolo11n.pt")

# Train the model on your custom dataset
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference on an image
results = model.predict("https://ultralytics.com/images/bus.jpg")
```

---

## 在树莓派上使用 Coral Edge TPU 运行 Ultralytics YOLO11 🚀

**URL:** https://docs.ultralytics.com/zh/guides/coral-edge-tpu-on-raspberry-pi/

**Contents:**
- 在树莓派上使用 Coral Edge TPU 运行 Ultralytics YOLO11 🚀
- 什么是 Coral Edge TPU？
- 使用 Coral Edge TPU 提升 Raspberry Pi 型号的性能
- 带有 TensorFlow Lite 的树莓派上的 Edge TPU（新）⭐
- 准备工作
- 安装步骤详解
  - 安装 Edge TPU 运行时
- 导出到 Edge TPU
- 运行模型
- 基准测试

Coral Edge TPU 是一款紧凑型设备，可为您的系统添加 Edge TPU 协处理器。它支持针对 TensorFlow Lite 模型的低功耗、高性能 ML 推理。请访问 Coral Edge TPU 主页了解更多信息。

观看： 如何使用 Google Coral Edge TPU 在 Raspberry Pi 上运行推理

许多人希望在嵌入式或移动设备（如 Raspberry Pi）上运行他们的模型，因为它们非常节能，并且可以用于许多不同的应用。但是，即使使用 ONNX 或 OpenVINO 等格式，这些设备上的推理性能通常也很差。Coral Edge TPU 是解决此问题的一个很好的方案，因为它可以与 Raspberry Pi 结合使用，并大大提高推理性能。

Coral 提供的关于如何将 Edge TPU 与 Raspberry Pi 配合使用的现有指南已过时，并且当前的 Coral Edge TPU 运行时版本不再适用于当前的 TensorFlow Lite 运行时版本。除此之外，Google 似乎已经完全放弃了 Coral 项目，并且在 2021 年到 2025 年之间没有任何更新。本指南将向您展示如何在 Raspberry Pi 单板计算机 (SBC) 上使用最新版本的 TensorFlow Lite 运行时和更新的 Coral Edge TPU 运行时来使 Edge TPU 正常工作。

本指南假定您已经安装了可用的 Raspberry Pi OS。 ultralytics 以及所有依赖项。要获取 ultralytics 已安装，请访问 快速入门指南 在此继续之前进行设置。

首先，我们需要安装 Edge TPU 运行时。有许多不同的版本可用，因此您需要为您的操作系统选择正确的版本。 高频版本以更高的时钟速度运行 Edge TPU，从而提高了性能。但是，这可能会导致 Edge TPU 热节流，因此建议采取某种冷却机制。

安装运行时后，将您的 Coral Edge TPU 插入 Raspberry Pi 的 USB 3.0 端口，以便新的 udev 规则可以生效。

如果您已经安装了 Coral Edge TPU 运行时，请使用以下命令卸载它。

要使用 Edge TPU，您需要将模型转换为兼容的格式。建议您在 Google Colab、x86_64 Linux 机器上，使用官方的 Ultralytics Docker 容器，或使用 Ultralytics HUB 运行导出，因为 Edge TPU 编译器在 ARM 上不可用。有关可用参数，请参阅导出模式。

导出的模型将保存在 <model_name>_saved_model/ 名称为 <model_name>_full_integer_quant_edgetpu.tflite。确保文件名以 _edgetpu.tflite 后缀；否则，Ultralytics 将无法 detect 到您正在使用 Edge TPU 模型。

如果您已经安装了 TensorFlow，请使用以下命令卸载它：

然后安装或更新 tflite-runtime:

有关完整预测模式的详细信息，请访问预测页面，获取全面信息。

如果您有多个 Edge TPU，可以使用以下代码选择特定的 TPU。

经 Raspberry Pi OS Bookworm 64位和 USB Coral Edge TPU 测试。

Coral Edge TPU 是一款紧凑型设备，旨在为您的系统添加 Edge TPU 协处理器。该协处理器支持低功耗、高性能的机器学习推理，尤其针对 TensorFlow Lite 模型进行了优化。当使用 Raspberry Pi 时，Edge TPU 可加速 ML 模型推理，从而显著提高性能，尤其对于 Ultralytics YOLO11 模型。您可以在其主页上阅读有关 Coral Edge TPU 的更多信息。

要在 Raspberry Pi 上安装 Coral Edge TPU 运行时，请下载相应的 .deb 适用于您的 Raspberry Pi 操作系统版本的软件包，来自 此链接。下载完成后，使用以下命令安装它：

请务必按照安装步骤部分中概述的步骤卸载之前所有 Coral Edge TPU 运行时版本。

是的，您可以导出您的 Ultralytics YOLO11 模型，使其与 Coral Edge TPU 兼容。建议在 Google Colab、x86_64 Linux 机器上或使用 Ultralytics Docker 容器 执行导出。您还可以使用 Ultralytics HUB 进行导出。以下是如何使用 python 和 CLI 导出您的模型：

如果您的 Raspberry Pi 上安装了 TensorFlow，并且需要切换到 tflite-runtime，您需要先卸载 TensorFlow，使用命令：

然后，安装或更新 tflite-runtime 使用以下命令：

将您的 YOLO11 模型导出为 Edge TPU 兼容格式后，您可以使用以下代码片段运行推理：

关于完整预测模式功能的详细信息，请访问预测页面。

**Examples:**

Example 1 (unknown):
```unknown
sudo dpkg -i path/to/package.deb
```

Example 2 (markdown):
```markdown
# If you installed the standard version
sudo apt remove libedgetpu1-std

# If you installed the high-frequency version
sudo apt remove libedgetpu1-max
```

Example 3 (python):
```python
from ultralytics import YOLO

# Load a model
model = YOLO("path/to/model.pt")  # Load an official model or custom model

# Export the model
model.export(format="edgetpu")
```

Example 4 (unknown):
```unknown
yolo export model=path/to/model.pt format=edgetpu # Export an official model or custom model
```

---

## 使用 Ultralytics YOLO11 🚀 进行停车管理 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/guides/parking-management/

**Contents:**
- 使用 Ultralytics YOLO11 的停车场管理 🚀
- 什么是停车管理系统？
- 停车管理系统的优势
- 实际应用
- 停车场管理系统代码工作流程
  - ParkingManagement 参数
- 常见问题
  - Ultralytics YOLO11 如何增强停车管理系统？
  - 使用 Ultralytics YOLO11 进行智能停车有哪些好处？
  - 如何使用 Ultralytics YOLO11 定义停车位？

通过组织空间和监控可用性，使用 Ultralytics YOLO11 进行停车场管理可确保高效和安全的停车。YOLO11 可以通过实时车辆检测和对停车位占用情况的洞察来改善停车场管理。

观看： 如何使用 Ultralytics YOLO 🚀 实现停车管理

在停车管理系统中，选择停车点是一项至关重要且复杂的任务。Ultralytics通过提供一个工具“停车位注释器”来简化此过程，该工具允许您定义停车场区域，这些区域稍后可用于其他处理。

步骤 1： 从您要管理的停车场的视频或摄像头流中捕获一帧。

步骤 2： 使用提供的代码启动图形界面，您可以在其中选择图像并通过鼠标单击开始勾勒停车区域轮廓以创建多边形。

停车位标注器 Ultralytics YOLO

一般来说， tkinter 已预先打包 python。但是，如果未打包，您可以使用以下突出显示的步骤安装它：

第三步： 使用多边形定义停车区域后，单击 save 将包含数据在内的 JSON 文件存储到您的工作目录中。

步骤 4： 现在，您可以利用提供的代码，通过 Ultralytics YOLO 进行停车管理。

使用 Ultralytics YOLO 进行停车管理

这是一个包含以下内容的表格 ParkingManagement 参数：

字段 ParkingManagement 解决方案允许使用多个 track 参数：

Ultralytics YOLO11 通过提供实时车辆检测和监控，极大地增强了停车管理系统。这可以优化停车位的使用，减少拥堵，并通过持续监控提高安全性。停车管理系统能够实现高效的交通流量，最大限度地减少停车场内的空闲时间和排放，从而为环境可持续性做出贡献。有关更多详细信息，请参阅停车管理代码工作流程。

使用 Ultralytics YOLO11 进行智能停车可带来诸多好处：

使用 Ultralytics YOLO11 定义停车位非常简单：

是的，Ultralytics YOLO11 允许针对特定的停车管理需求进行定制。您可以调整参数，例如 已占用和可用区域颜色，文本显示边距等等。利用 ParkingManagement 类的 参数，您可以定制模型以满足您的特定需求，从而确保最高的效率和效能。

Ultralytics YOLO11 被广泛应用于各种实际场景中的停车场管理，包括：

**Examples:**

Example 1 (python):
```python
from ultralytics import solutions

solutions.ParkingPtsSelection()
```

Example 2 (python):
```python
import cv2

from ultralytics import solutions

# Video capture
cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "Error reading video file"

# Video writer
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))
video_writer = cv2.VideoWriter("parking management.avi", cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))

# Initialize parking management object
parkingmanager = solutions.ParkingManagement(
    model="yolo11n.pt",  # path to model file
    json_file="bounding_boxes.json",  # path to parking annotations file
)

while cap.isOpened():
    ret, im0 = cap.read()
    if not ret:
        break

    results = parkingmanager(im0)

    # print(results)  # access the output

    video_writer.write(results.plot_im)  # write the processed frame.

cap.release()
video_writer.release()
cv2.destroyAllWindows()  # destroy all opened windows
```

---

## 在 NVIDIA Jetson 上使用 DeepStream SDK 和 TensorRT 的 Ultralytics YOLO11

**URL:** https://docs.ultralytics.com/zh/guides/deepstream-nvidia-jetson/

**Contents:**
- 在 NVIDIA Jetson 上使用 DeepStream SDK 和 TensorRT 的 Ultralytics YOLO11
- 什么是 NVIDIA DeepStream？
- 准备工作
- YOLO11 的 DeepStream 配置
  - 运行推理
- INT8 校准
  - 运行推理
- 多流设置
  - 运行推理
- 基准测试结果

观看： 如何在 Jetson Orin NX 上将 Ultralytics YOLO11 模型与 NVIDIA Deepstream 结合使用 🚀

本综合指南详细介绍了如何使用 DeepStream SDK 和 TensorRT 在 NVIDIA Jetson 设备上部署 Ultralytics YOLO11。在此，我们使用 TensorRT 来最大限度地提高 Jetson 平台上的推理性能。

本指南已经过以下测试：运行最新稳定 JetPack 版本的 NVIDIA Jetson Orin Nano Super Developer Kit（JP6.1）、基于 NVIDIA Jetson Orin NX 16GB 且运行 JetPack 版本的 Seeed Studio reComputer J4012（JP5.1.3）以及基于 NVIDIA Jetson Nano 4GB 且运行 JetPack 版本的 Seeed Studio reComputer J1020 v2（JP4.6.4）。预计它可以在包括最新和旧版在内的所有 NVIDIA Jetson 硬件系列上运行。

NVIDIA 的 DeepStream SDK 是一个完整的流分析工具包，基于 GStreamer，用于基于 AI 的多传感器处理、视频、音频和图像理解。它非常适合视觉 AI 开发人员、软件合作伙伴、初创公司和构建 IVA（智能视频分析）应用程序和 OEM。现在，您可以创建包含 神经网络 和其他复杂处理任务（如跟踪、视频编码/解码和视频渲染）的流处理管道。这些管道支持对视频、图像和传感器数据进行实时分析。DeepStream 的多平台支持使您可以更快、更轻松地在本地、边缘和云中开发视觉 AI 应用程序和服务。

在本指南中，我们使用了 Debian 软件包方法将 DeepStream SDK 安装到 Jetson 设备。您还可以访问 Jetson 上的 DeepStream SDK（已存档） 以访问 DeepStream 的旧版本。

这里我们使用 marcoslucianops/DeepStream-Yolo GitHub 仓库，它包括 NVIDIA DeepStream SDK 对 YOLO 模型 的支持。感谢 marcoslucianops 的贡献！

安装 Ultralytics 以及必要的依赖项

克隆 DeepStream-Yolo 仓库

复制 export_yolo11.py 文件来自 DeepStream-Yolo/utils 目录到 ultralytics 文件夹

从 YOLO11 发布版 下载您选择的 Ultralytics YOLO11 检测模型 (.pt)。这里我们使用 yolo11s.pt。

您还可以使用自定义训练的 YOLO11 模型。

对于 DeepStream 5.1，请移除 --dynamic 参数并使用 opset 12 或更低。默认值为 opset 是 17。

简化 ONNX 模型 (DeepStream >= 6.0)

要使用动态批量大小 (DeepStream >= 6.1)

要使用静态批次大小（例如，批次大小 = 4）。

复制生成的 .onnx 模型文件和 labels.txt 文件到 DeepStream-Yolo 文件夹

根据已安装的 JetPack 版本设置 CUDA 版本

编辑 config_infer_primary_yolo11.txt 根据您的模型（对于具有 80 个类别的 YOLO11）的文件

编辑 deepstream_app_config 文件

您还可以更改视频源。 deepstream_app_config 文件。此处加载了一个默认视频文件

在开始推理之前，生成 TensorRT 引擎文件需要很长时间。请耐心等待。

如果您想将模型转换为 FP16 精度，只需设置 model-engine-file=model_b1_gpu0_fp16.engine 和 network-mode=2 在...里面 config_infer_primary_yolo11.txt

如果您想使用INT8精度进行推理，您需要遵循以下步骤：

目前，INT8 不适用于 TensorRT 10.x。本指南的这一部分已使用 TensorRT 8.x 进行了测试，预计可以正常工作。

对于 COCO 数据集，下载 val2017，提取，然后移动到 DeepStream-Yolo 文件夹

运行以下命令，从COCO数据集中选择1000张随机图像以运行校准

NVIDIA 建议至少使用 500 张图像以获得良好的 准确率。在此示例中，选择 1000 张图像是为了获得更好的准确率（图像越多 = 准确率越高）。您可以从 head -1000 进行设置。例如，对于 2000 张图像，使用 head -2000。此过程可能需要很长时间。

创建 calibration.txt 文件，其中包含所有选定的图像

更高的 INT8_CALIB_BATCH_SIZE 值将带来更高的精度和更快的校准速度。请根据您的 GPU 内存进行设置。

更新 config_infer_primary_yolo11.txt 文件

观看： 如何使用 Ultralytics YOLO11 在 Jetson Nano 上通过 DeepStream SDK 运行多个流 🎉

要在单个 DeepStream 应用程序下设置多个流，请对以下内容进行更改： deepstream_app_config.txt 文件：

根据所需的视频流数量，更改行和列以构建网格显示。例如，对于 4 个视频流，我们可以添加 2 行和 2 列。

设置 num-sources=4 并添加 uri 所有四个流的条目。

以下基准测试总结了 YOLO11 模型在 NVIDIA Jetson Orin NX 16GB 上以 640x640 的输入尺寸在不同 TensorRT 精度级别下的性能表现。

本指南最初由我们在 Seeed Studio 的朋友 Lakshantha 和 Elaine 创建。

要在 NVIDIA Jetson 设备上设置 Ultralytics YOLO11，您首先需要安装与您的 JetPack 版本兼容的 DeepStream SDK。请按照我们的快速入门指南中的分步指南配置您的 NVIDIA Jetson 以进行 YOLO11 部署。

将 TensorRT 与 YOLO11 结合使用可优化模型以进行推理，从而显著减少 NVIDIA Jetson 设备上的延迟并提高吞吐量。 TensorRT 通过层融合、精度校准和内核自动调整提供高性能、低延迟的 深度学习 推理。 这可以实现更快、更高效的执行，对于视频分析和自主机器等实时应用尤其有用。

是的，使用 DeepStream SDK 和 TensorRT 部署 Ultralytics YOLO11 的指南与整个 NVIDIA Jetson 系列兼容。这包括诸如带有 JetPack 5.1.3 的 Jetson Orin NX 16GB 和带有 JetPack 4.6.4 的 Jetson Nano 4GB 之类的设备。有关详细步骤，请参阅 YOLO11 的 DeepStream 配置部分。

要将 YOLO11 模型转换为 ONNX 格式以便使用 DeepStream 进行部署，请使用 utils/export_yolo11.py 脚本来自 DeepStream-Yolo 仓库。

有关模型转换的更多详细信息，请查看我们的模型导出部分。

YOLO11 模型在 NVIDIA Jetson Orin NX 16GB 上的性能因 TensorRT 精度级别而异。例如，YOLO11s 模型实现了：

这些基准测试突显了在 NVIDIA Jetson 硬件上使用 TensorRT 优化的 YOLO11 模型的效率和能力。更多详情，请参阅我们的基准测试结果部分。

**Examples:**

Example 1 (unknown):
```unknown
cd ~
pip install -U pip
git clone https://github.com/ultralytics/ultralytics
cd ultralytics
pip install -e ".[export]" onnxslim
```

Example 2 (unknown):
```unknown
cd ~
git clone https://github.com/marcoslucianops/DeepStream-Yolo
```

Example 3 (unknown):
```unknown
cp ~/DeepStream-Yolo/utils/export_yolo11.py ~/ultralytics
cd ultralytics
```

Example 4 (unknown):
```unknown
wget https://github.com/ultralytics/assets/releases/download/v8.3.0/yolo11s.pt
```

---

## 使用 Ray Tune 和 YOLO11 进行高效的超参数调整

**URL:** https://docs.ultralytics.com/zh/integrations/ray-tune/

**Contents:**
- 使用 Ray Tune 和 YOLO11 进行高效的超参数调整
- 使用 Ultralytics YOLO11 和 Ray Tune 加速调优
  - Ray Tune
  - 与 Weights & Biases 集成
- 安装
- 用法
- tune() 方法参数
- 默认搜索空间描述
- 自定义搜索空间示例
- 使用 Ray Tune 恢复中断的超参数调整会话

超参数调整对于通过发现最佳超参数集来实现最佳模型性能至关重要。这包括使用不同的超参数运行试验并评估每个试验的性能。

Ultralytics YOLO11 结合了 Ray Tune 进行超参数调优，从而简化了 YOLO11 模型超参数的优化。借助 Ray Tune，您可以利用高级搜索策略、并行性和提前停止来加速调优过程。

Ray Tune 是一个为效率和灵活性而设计的超参数调整库。它支持各种搜索策略、并行性和提前停止策略，并与包括 Ultralytics YOLO11 在内的流行机器学习框架无缝集成。

YOLO11 还允许与 Weights & Biases 可选集成，以监控调优过程。

字段 tune() YOLO11 中的 method 提供了一个易于使用的界面，用于使用 Ray Tune 进行超参数调整。它接受多个参数，允许您自定义调整过程。以下是每个参数的详细说明：

通过自定义这些参数，您可以微调超参数优化过程，以适应您的特定需求和可用的计算资源。

下表列出了 YOLO11 中使用 Ray Tune 进行超参数调优的默认搜索空间参数。每个参数都有一个由以下内容定义的特定值范围 tune.uniform().

在此示例中，我们将演示如何将自定义搜索空间用于 Ray Tune 和 YOLO11 的超参数调整。通过提供自定义搜索空间，您可以将调整过程集中在感兴趣的特定超参数上。

在上面的代码片段中，我们使用 "yolo11n.pt" 预训练权重创建了一个 YOLO 模型。然后，我们调用 tune() 方法，通过“coco8.yaml”指定数据集配置。我们为初始学习率提供了一个自定义搜索空间 lr0 使用带有键“lr0”和值的字典 tune.uniform(1e-5, 1e-1)。最后，我们将额外的训练参数（例如 epoch 的数量）直接传递给 tune 方法，作为 epochs=50.

您可以通过传递以下参数来恢复中断的 Ray Tune 会话 resume=True。您可以选择性地传递目录 name 由 Ray Tune 在以下情况下使用 runs/{task} 恢复训练。否则，它将恢复上次中断的会话。你不需要提供 iterations 和 space 再次，但是您需要再次提供其余的训练参数，包括 data 和 epochs.

使用 resume=True 使用 model.tune()

在使用 Ray Tune 运行超参数调优实验后，您可能需要对获得的结果执行各种分析。本指南将引导您完成处理和分析这些结果的常见工作流程。

在使用以下工具运行调优实验后 tuner.fit()，您可以从目录加载结果。如果您在初始训练脚本退出后执行分析，这将特别有用。

获取试验执行情况的概述。您可以快速检查试验期间是否存在任何错误。

您可以绘制每次试验报告的指标历史记录，以查看指标随时间的变化情况。

在本指南中，我们介绍了使用Ultralytics和Ray Tune运行实验结果的常见分析工作流程。关键步骤包括从目录加载实验结果、执行基本的实验级和试用级分析，以及绘制指标。

查看 Ray Tune 的分析结果文档页面，以充分利用您的超参数调整实验，从而进行进一步探索。

要使用 Ray Tune 调整 Ultralytics YOLO11 模型的超参数，请按照以下步骤操作：

它利用 Ray Tune 的高级搜索策略和并行性，从而高效地优化模型的超参数。更多信息，请查阅 Ray Tune 文档。

Ultralytics YOLO11 使用以下默认超参数，以便使用 Ray Tune 进行调整：

可以自定义这些超参数以满足您的特定需求。有关完整列表和更多详细信息，请参阅超参数调优指南。

要将 Weights & Biases (W&B) 与您的 Ultralytics YOLO11 调优过程集成：

此设置将使您能够监控调优过程、跟踪超参数配置，并在W&B中可视化结果。

Ray Tune 为超参数优化提供了许多优势：

Ray Tune 与 Ultralytics YOLO11 无缝集成，提供了一个易于使用的界面，可以有效地调整超参数。要开始使用，请查看超参数调整指南。

要使用 Ray Tune 为您的 YOLO11 超参数调整定义自定义搜索空间：

这可以自定义超参数的范围，例如在调整过程中要探索的初始学习率和动量。有关高级配置，请参阅自定义搜索空间示例部分。

**Examples:**

Example 1 (sql):
```sql
# Install and update Ultralytics and Ray Tune packages
pip install -U ultralytics "ray[tune]"

# Optionally install W&B for logging
pip install wandb
```

Example 2 (python):
```python
from ultralytics import YOLO

# Load a YOLO11n model
model = YOLO("yolo11n.pt")

# Start tuning hyperparameters for YOLO11n training on the COCO8 dataset
result_grid = model.tune(data="coco8.yaml", use_ray=True)
```

Example 3 (python):
```python
from ray import tune

from ultralytics import YOLO

# Define a YOLO model
model = YOLO("yolo11n.pt")

# Run Ray Tune on the model
result_grid = model.tune(
    data="coco8.yaml",
    space={"lr0": tune.uniform(1e-5, 1e-1)},
    epochs=50,
    use_ray=True,
)
```

Example 4 (python):
```python
from ultralytics import YOLO

# Define a YOLO model
model = YOLO("yolo11n.pt")

# Resume previous run
results = model.tune(use_ray=True, data="coco8.yaml", epochs=50, resume=True)

# Resume Ray Tune run with name 'tune_exp_2'
results = model.tune(use_ray=True, data="coco8.yaml", epochs=50, name="tune_exp_2", resume=True)
```

---

## 如何从 YOLO11 导出到 TF GraphDef 以进行部署

**URL:** https://docs.ultralytics.com/zh/integrations/tf-graphdef/

**Contents:**
- 如何从 YOLO11 导出到 TF GraphDef 以进行部署
- 为什么要导出到 TF GraphDef？
- TF GraphDef 模型的主要特性
- TF GraphDef 的部署选项
- 将 YOLO11 模型导出到 TF GraphDef
  - 安装
  - 用法
  - 导出参数
- 部署导出的 YOLO11 TF GraphDef 模型
- 总结

当您在不同的环境中部署像 YOLO11 这样的前沿 计算机视觉 模型时，您可能会遇到兼容性问题。Google 的 TensorFlow GraphDef（或 TF GraphDef）通过提供模型的序列化、平台无关的表示来提供解决方案。使用 TF GraphDef 模型格式，您可以在可能无法使用完整的 TensorFlow 生态系统的环境（例如移动设备或专用硬件）中部署 YOLO11 模型。

在本指南中，我们将逐步引导您完成将 Ultralytics YOLO11 模型导出为 TF GraphDef 模型格式的过程。通过转换您的模型，您可以简化部署并在更广泛的应用程序和平台上使用 YOLO11 的计算机视觉功能。

TF GraphDef 是由 Google 开发的 TensorFlow 生态系统的一个强大组件。它可用于优化和部署 YOLO11 等模型。导出到 TF GraphDef 使您能够将模型从研究阶段转向实际应用。它允许模型在没有完整 TensorFlow 框架的环境中运行。

GraphDef 格式将模型表示为序列化的计算图。这支持各种优化技术，例如常量折叠、量化和图转换。这些优化确保了高效的执行、减少的内存使用和更快的推理速度。

GraphDef 模型可以使用硬件加速器，例如 GPU、TPU 和 AI 芯片，从而为 YOLO11 推理管线释放显著的性能提升。TF GraphDef 格式创建了一个包含模型及其依赖项的独立软件包，从而简化了部署和集成到各种系统中。

TF GraphDef 提供了用于简化模型部署和优化的独特功能。

模型序列化: TF GraphDef 提供了一种以平台无关的格式序列化和存储 TensorFlow 模型的方法。 这种序列化表示允许您加载和执行模型，而无需原始 python 代码库，从而简化了部署。

图优化: TF GraphDef 支持计算图的优化。这些优化可以通过简化执行流程、减少冗余以及调整操作以适应特定硬件来提高性能。

部署灵活性: 导出为 GraphDef 格式的模型可用于各种环境，包括资源受限的设备、Web 浏览器和具有专用硬件的系统。这为更广泛地部署 TensorFlow 模型开辟了可能性。

专注于生产: GraphDef 专为生产部署而设计。它支持高效执行、序列化功能以及与实际用例相符的优化。

在深入研究将 YOLO11 模型导出到 TF GraphDef 的过程之前，让我们看看使用此格式的一些典型部署情况。

以下是如何在各种平台上高效部署 TF GraphDef：

TensorFlow Serving： 此框架旨在生产环境中部署 TensorFlow 模型。TensorFlow Serving 提供模型管理、版本控制以及用于大规模高效模型服务的基础设施。这是一种将基于 GraphDef 的模型无缝集成到生产 Web 服务或 API 中的方法。

移动和嵌入式设备： 借助 TensorFlow Lite 等工具，您可以将 TF GraphDef 模型转换为针对智能手机、平板电脑和各种嵌入式设备优化的格式。然后，您的模型可以用于设备端推理，其中执行在本地完成，通常提供性能提升和离线功能。

Web浏览器：TensorFlow.js支持将TF GraphDef模型直接部署到Web浏览器中。它为利用JavaScript通过YOLO11的功能在客户端运行实时目标检测应用铺平了道路。

专用硬件： TF GraphDef 的平台无关性使其能够面向定制硬件，例如加速器和 TPU（张量处理单元）。这些设备可以为计算密集型模型提供性能优势。

您可以将您的 YOLO11 对象检测模型转换为 TF GraphDef 格式，该格式与各种系统兼容，以提高其在平台上的性能。

有关安装过程的详细说明和最佳实践，请查看我们的 Ultralytics 安装指南。如果在为 YOLO11 安装所需软件包时遇到任何困难，请查阅我们的 常见问题指南以获取解决方案和提示。

所有Ultralytics YOLO11 模型都设计为支持开箱即用的导出，从而可以轻松地将其集成到您首选的部署工作流程中。您可以查看支持的导出格式和配置选项的完整列表，以选择最适合您应用程序的设置。

有关导出过程的更多详细信息，请访问Ultralytics 文档页面上的导出。

将您的 YOLO11 模型导出为 TF GraphDef 格式后，下一步是部署。运行 TF GraphDef 模型的主要和推荐的第一步是使用 YOLO("model.pb") 方法，如先前在用法代码片段中所示。

但是，有关部署 TF GraphDef 模型的更多信息，请查看以下资源：

TensorFlow Serving: 一份关于 TensorFlow Serving 的指南，该指南讲授如何在生产环境中高效地部署和服务机器学习模型。

TensorFlow Lite: 此页面介绍如何将机器学习模型转换为针对使用 TensorFlow Lite 进行设备上推理优化的格式。

TensorFlow.js: 一份关于模型转换的指南，该指南讲授如何将 TensorFlow 或 Keras 模型转换为 TensorFlow.js 格式，以便在 Web 应用程序中使用。

在本指南中，我们探讨了如何将 Ultralytics YOLO11 模型导出为 TF GraphDef 格式。通过这样做，您可以灵活地在不同的环境中部署您优化的 YOLO11 模型。

有关使用详情，请访问 TF GraphDef 官方文档。

有关将 Ultralytics YOLO11 与其他平台和框架集成的更多信息，请参阅我们的 集成指南页面。

Ultralytics YOLO11 模型可以无缝导出为 TensorFlow GraphDef (TF GraphDef) 格式。此格式提供了一种序列化的、平台无关的模型表示，非常适合在移动和 Web 等各种环境中部署。要将 YOLO11 模型导出为 TF GraphDef，请按照以下步骤操作：

有关不同导出选项的更多信息，请访问Ultralytics 模型导出文档。

将 YOLO11 模型导出到 TF GraphDef 格式具有以下多个优点，包括：

在我们的文档的 TF GraphDef 部分中阅读更多关于优势的信息。

Ultralytics YOLO11 与 YOLOv5 和 YOLOv7 等其他模型相比，具有诸多优势。主要优势包括：

在我们的 YOLO11 介绍 中探索更多细节。

一旦 YOLO11 模型导出为 TF GraphDef 格式，您就可以将其部署到各种专用硬件平台上。典型的部署场景包括：

为了解决导出 YOLO11 模型时遇到的常见问题，Ultralytics 提供了全面的指南和资源。如果在安装或模型导出过程中遇到问题，请参考：

这些资源应该可以帮助您解决与 YOLO11 模型导出和部署相关的大多数问题。

**Examples:**

Example 1 (go):
```go
# Install the required package for YOLO11
pip install ultralytics
```

Example 2 (python):
```python
from ultralytics import YOLO

# Load the YOLO11 model
model = YOLO("yolo11n.pt")

# Export the model to TF GraphDef format
model.export(format="pb")  # creates 'yolo11n.pb'

# Load the exported TF GraphDef model
tf_graphdef_model = YOLO("yolo11n.pb")

# Run inference
results = tf_graphdef_model("https://ultralytics.com/images/bus.jpg")
```

Example 3 (markdown):
```markdown
# Export a YOLO11n PyTorch model to TF GraphDef format
yolo export model=yolo11n.pt format=pb # creates 'yolo11n.pb'

# Run inference with the exported model
yolo predict model='yolo11n.pb' source='https://ultralytics.com/images/bus.jpg'
```

Example 4 (python):
```python
from ultralytics import YOLO

# Load the YOLO11 model
model = YOLO("yolo11n.pt")

# Export the model to TF GraphDef format
model.export(format="pb")  # creates 'yolo11n.pb'

# Load the exported TF GraphDef model
tf_graphdef_model = YOLO("yolo11n.pb")

# Run inference
results = tf_graphdef_model("https://ultralytics.com/images/bus.jpg")
```

---

## YOLOX 对比 YOLOv5：探索无锚框创新与成熟效率

**URL:** https://docs.ultralytics.com/zh/compare/yolox-vs-yolov5/

**Contents:**
- YOLOX 对比 YOLOv5：探索无锚框创新与成熟效率
- 性能分析：速度、准确性和效率
- YOLOX：无锚框竞争者
  - 架构与创新
  - 优势与劣势
  - 理想用例
- YOLOv5：生产标准
  - 架构与生态系统
  - 优势与劣势
  - 理想用例

在快速发展的物体 detect 领域，选择合适的架构对于项目成功至关重要。本比较探讨了两个有影响力的模型：YOLOX（一个以其无锚点设计而闻名的学术强项）和 YOLOv5（速度和部署便捷性的行业标准）。这两个模型都塑造了计算机视觉领域，但它们服务于不同的需求，具体取决于您的优先级是研究级精度还是生产就绪效率。

评估 YOLOX 和 YOLOv5 时，区别通常归结为原始精度和操作效率之间的权衡。YOLOX 引入了显著的架构变化，例如解耦头和无锚机制，这使其在发布时能够实现最先进的mAP (mean Average Precision)分数。它在对精度要求极高的场景中表现出色，尤其是在像 COCO 这样的困难基准测试中。

相反，Ultralytics YOLOv5 的设计重点是“实际”性能。它优先考虑推理速度和低延迟，使其非常适合移动应用程序、嵌入式系统和边缘 AI 设备。虽然 YOLOX 在特定大型模型的 mAP 上可能略占优势，但 YOLOv5 在吞吐量（每秒帧数）和部署灵活性方面始终优于 YOLOX，这得益于全面的Ultralytics 生态系统。

下表提供了不同尺寸模型之间的详细并排比较。请注意，YOLOv5在保持竞争性准确性的同时，提供了显著更快的推理时间，尤其是在使用TensorRT进行优化时。

YOLOX 由旷视科技（Megvii）的研究人员开发，旨在弥合 YOLO 系列与无锚框检测学术进展之间的差距。通过消除预定义锚框的限制，YOLOX 简化了训练过程，并减少了启发式调优的需求。

YOLOX集成了解耦头，将分类和回归任务分离到不同的分支中。这种设计与早期YOLO版本的耦合头形成对比，据报道能提高收敛速度和准确性。此外，它还利用SimOTA，这是一种先进的标签分配策略，能够动态分配正样本，从而增强模型在密集场景中的鲁棒性。

YOLOX 的主要优势在于其高准确性上限，特别是其最大变体（YOLOX-x），以及其简洁的无锚框设计，这吸引了研究人员。然而，这些优势也伴随着权衡。解耦头增加了计算复杂性，与 YOLOv5 相比，通常会导致更慢的推理速度。此外，作为一个以研究为重点的模型，它缺乏 Ultralytics 生态系统中那种内聚、用户友好的工具，这可能会使集成到商业流水线中变得复杂。

自 2020 年发布以来，Ultralytics YOLOv5 已成为全球开发者的首选模型。它在性能和实用性之间取得了卓越的平衡，并由一个旨在简化整个机器学习操作 (MLOps) 生命周期的平台提供支持。

YOLOv5 利用 CSPNet 主干网络和路径聚合网络 (PANet) 颈部网络，针对高效特征提取进行了优化。虽然它最初在 PyTorch 中推广了基于锚框的方法，但其最大的优势在于其周边生态系统。用户受益于自动导出到 ONNX、CoreML 和 TFLite 等格式，以及与Ultralytics HUB的无缝集成，用于模型训练和管理。

YOLOv5 不限于边界框。它支持多种任务，包括实例分割和图像分类，使其成为复杂视觉流水线的通用工具。

易用性是 YOLOv5 的标志性特点。通过简单的 Python API，开发者只需几行代码即可加载预训练权重并运行推理。该模型针对速度进行了高度优化，与 YOLOX 相比，在 CPU 和 GPU 上始终提供更低的延迟。它在训练期间还具有更低的内存需求，使其可在标准硬件上访问。尽管其基于锚框的设计需要针对自定义数据集进行锚框演进（YOLOv5 自动处理），但其可靠性和维护良好的生态系统使其在生产环境中表现更优。

Ultralytics python 包使得利用 YOLOv5 模型变得非常简单。下面是使用预训练模型运行推理的示例。

两种模型都代表了计算机视觉领域的重大成就，但它们面向不同的受众。YOLOX 对于那些推动无锚点 detect 边界并乐于使用更分散工具集的研究人员来说，是一个强大的选择。

然而，对于绝大多数开发人员、工程师和企业而言，Ultralytics YOLOv5 仍然是卓越的选择。它结合了无与伦比的速度、多功能性以及强大活跃的生态系统，确保您能够以最小的阻力从概念走向部署。此外，采用 Ultralytics 框架提供了通往下一代模型（如YOLO11）的清晰升级路径，YOLO11 将无锚点设计的优点与 Ultralytics 标志性的效率相结合。

探索这些模型与其他架构的对比，以找到最适合您特定需求的模型：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv5 model (Nano version for speed)
model = YOLO("yolov5nu.pt")

# Run inference on an image
results = model("https://ultralytics.com/images/bus.jpg")

# Display the results
results[0].show()
```

---
