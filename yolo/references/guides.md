# Yolo - Guides

**Pages:** 6

---

## 使用Neural Magic的DeepSparse部署YOLOv5 - Ultralytics YOLO文档

**URL:** https://docs.ultralytics.com/zh/yolov5/tutorials/neural_magic_pruning_quantization/

**Contents:**
- 使用Neural Magic的DeepSparse部署YOLOv5
- DeepSparse 如何实现 GPU 级别的性能？
- 如何创建基于我的数据训练的 YOLOv5 稀疏版本？
- DeepSparse 用法
  - 安装 DeepSparse
  - 收集 ONNX 文件
  - 部署模型
    - Python API
    - HTTP 服务器
    - 标注 CLI

本指南介绍了如何使用 Neural Magic 的 DeepSparse 部署 YOLOv5。

DeepSparse 是一个在 CPU 上具有卓越性能的推理运行时。例如，与 ONNX Runtime 基线相比，DeepSparse 在同一台机器上运行 YOLOv5s 时，速度提高了 5.8 倍！

您的 深度学习 工作负载首次可以在满足生产性能需求的同时，避免硬件加速器的复杂性和成本。简而言之，DeepSparse 为您提供 GPU 的性能和软件的简易性：

DeepSparse 利用模型稀疏性来提高其性能速度。

通过剪枝和量化实现的稀疏化是一种被广泛研究的技术，可以在保持高准确性的同时，大幅减少执行网络所需的大小和计算量。DeepSparse 具有稀疏感知能力，这意味着它可以跳过归零的参数，从而减少前向传递中的计算量。由于稀疏计算现在受内存限制，DeepSparse 以深度方式执行网络，将问题分解为 Tensor Columns，即适合缓存在计算中的垂直条纹。

具有压缩计算的稀疏网络，在缓存中以深度方式执行，使 DeepSparse 能够在 CPU 上提供 GPU 级别的性能！

Neural Magic 的开源模型存储库 SparseZoo 包含每个 YOLOv5 模型的预稀疏化检查点。使用与 Ultralytics 集成的 SparseML，您可以使用单个 CLI 命令将稀疏检查点微调到您的数据上。

查看 Neural Magic 的 YOLOv5 文档以获取更多详细信息。

我们将通过一个示例来演示如何使用 DeepSparse 对 YOLOv5s 的稀疏版本进行基准测试和部署。

运行以下命令来安装 DeepSparse。我们建议您使用带有 Python 的虚拟环境。

DeepSparse 接受 ONNX 格式的模型，可以通过以下方式传递：

以下示例使用标准的密集和剪枝量化 YOLOv5s 检查点，由以下 SparseZoo 存根标识：

DeepSparse 提供了方便的 API，用于将您的模型集成到应用程序中。

要尝试以下部署示例，请提取示例图像并将其另存为 basilica.jpg 包括以下内容：

Pipelines 围绕运行时封装预处理和输出后处理，为向应用程序添加 DeepSparse 提供了一个清晰的接口。DeepSparse-Ultralytics 集成包括一个开箱即用的 Pipeline 接受原始图像并输出边界框。

若在云端运行，可能会出现OpenCV 找到的错误。 libGL.so.1您可以安装缺失的库：

或者使用无头Ultralytics tralytics包，完全避免GUI依赖：

DeepSparse Server 运行在流行的 FastAPI Web 框架和 Uvicorn Web 服务器之上。只需一个 CLI 命令，您就可以使用 DeepSparse 轻松设置模型服务端点。该服务器支持来自 DeepSparse 的任何 Pipeline，包括使用 YOLOv5 的 目标检测，使您可以将原始图像发送到端点并接收边界框。

使用修剪量化的 YOLOv5s 启动服务器：

使用 python 的示例请求 requests 软件包的更多详细信息：

您还可以使用 annotate 命令，让引擎将带注释的照片保存到磁盘。尝试一下： --source 0 来注释您的实时网络摄像头Feed！

运行上述命令将创建一个 annotation-results 文件夹中，并将标注的图像保存在其中。

我们将使用 DeepSparse 的基准测试脚本，比较 DeepSparse 与 ONNX Runtime 在 YOLOv5s 上的吞吐量。

这些基准测试是在 AWS 上运行的。 c6i.8xlarge 实例（16 核）。

在 batch 32 时，ONNX Runtime 使用标准的密集 YOLOv5s 实现了 42 images/秒：

虽然 DeepSparse 在优化后的稀疏模型上表现最佳，但它在标准的密集 YOLOv5s 模型上也能很好地运行。

在 batch 32 时，DeepSparse 使用标准的密集 YOLOv5s 实现了 70 images/秒，性能比 ORT 提升了 1.7 倍！

当稀疏性应用于模型时，DeepSparse相对于ONNX Runtime的性能提升更加显著。

在 batch 32 时，DeepSparse 使用经过剪枝量化的 YOLOv5s 实现了 241 images/秒，性能比 ORT 提升了 5.8 倍！

对于延迟敏感的 batch 1 场景，DeepSparse 也能够获得比 ONNX Runtime 更快的速度。

在 batch 1 时，ONNX Runtime 使用标准的密集 YOLOv5s 实现了 48 images/秒。

在 batch 1 时，DeepSparse 使用经过剪枝量化的 YOLOv5s 实现了 135 items/秒，性能比 ONNX Runtime 提升了 2.8 倍！

由于 c6i.8xlarge 实例具有 VNNI 指令，如果权重以 4 个块为单位进行修剪，则 DeepSparse 的吞吐量可以进一步提高。

在 batch 1 时，DeepSparse 使用 4-block 经过剪枝量化的 YOLOv5s 实现了 180 items/秒，性能比 ONNX Runtime 提升了 3.7 倍！

研究或测试？ DeepSparse Community 免费提供研究和测试。通过他们的文档开始使用。

有关使用 DeepSparse 部署 YOLOv5 的更多信息，请查看 Neural Magic 的 DeepSparse 文档 和 Ultralytics 关于 DeepSparse 集成的博客文章。

**Examples:**

Example 1 (unknown):
```unknown
pip install "deepsparse[server,yolo,onnxruntime]"
```

Example 2 (yaml):
```yaml
zoo:cv/detection/yolov5-s/pytorch/ultralytics/coco/base-none
zoo:cv/detection/yolov5-s/pytorch/ultralytics/coco/pruned65_quant-none
```

Example 3 (unknown):
```unknown
wget -O basilica.jpg https://raw.githubusercontent.com/neuralmagic/deepsparse/main/src/deepsparse/yolo/sample_images/basilica.jpg
```

Example 4 (python):
```python
from deepsparse import Pipeline

# list of images in local filesystem
images = ["basilica.jpg"]

# create Pipeline
model_stub = "zoo:cv/detection/yolov5-s/pytorch/ultralytics/coco/pruned65_quant-none"
yolo_pipeline = Pipeline.create(
    task="yolo",
    model_path=model_stub,
)

# run inference on images, receive bounding boxes + classes
pipeline_outputs = yolo_pipeline(images=images, iou_thres=0.6, conf_thres=0.001)
print(pipeline_outputs)
```

---

## 定义您的计算机视觉项目的实用指南

**URL:** https://docs.ultralytics.com/zh/guides/defining-project-goals/

**Contents:**
- 定义您的计算机视觉项目的实用指南
- 简介
- 定义清晰的问题陈述
  - 商业问题陈述示例
  - 设定可衡量的目标
- 问题陈述与计算机视觉任务之间的联系
- 哪个先来：模型选择、数据集准备还是模型训练方法？
- 社区中的常见讨论点
  - 有哪些不同的计算机视觉任务？
  - 预训练模型能否记住自定义训练之前已知的类别？

任何计算机视觉项目的第一步是定义您想要实现的目标。从一开始就制定清晰的路线图至关重要，其中包括从数据收集到部署模型的所有内容。

观看： 如何定义计算机视觉项目的目标 | 问题陈述和 VisionAI 任务连接 🚀

如果您需要快速回顾计算机视觉项目的基础知识，请花点时间阅读我们关于计算机视觉项目关键步骤的指南。它将为您提供对整个过程的全面概述。一旦您掌握了这些，请回到这里深入了解如何准确地定义和完善您的项目目标。

现在，让我们开始讨论为您的项目定义清晰的问题陈述，并探讨您在此过程中需要做出的关键决策。

为项目设定清晰的目标是找到最有效解决方案的第一步。让我们了解如何清晰地定义项目的问题陈述：

考虑一个计算机视觉项目，您希望估计高速公路上的车辆速度。核心问题是，由于过时的雷达系统和手动流程，当前的速度监控方法效率低下且容易出错。该项目旨在开发一种实时计算机视觉系统，以取代传统的速度估计系统。

主要用户包括交通管理部门和执法部门，而次要利益相关者是高速公路规划人员和受益于更安全道路的公众。关键需求包括评估预算、时间和人员，以及解决高分辨率摄像头和实时数据处理等技术需求。此外，还必须考虑关于隐私和数据安全的法规约束。

设定可衡量的目标是计算机视觉项目成功的关键。这些目标应该清晰、可实现且有时限。

例如，如果您正在开发一个用于估计高速公路上车辆速度的系统，您可以考虑以下可衡量目标：

通过设定具体且可量化的目标，您可以有效地跟踪进展，识别需要改进的领域，并确保项目按计划进行。

您的问题陈述有助于您概念化哪种计算机视觉任务可以解决您的问题。

例如，如果您的目标是监控高速公路上的车辆速度，那么相关的计算机视觉任务是目标跟踪。目标跟踪是合适的，因为它允许系统连续跟踪视频流中的每辆车，这对于准确计算其速度至关重要。

其他任务，如目标检测，则不适用，因为它们不提供连续的位置或运动信息。一旦您确定了合适的计算机视觉任务，它将指导您项目的几个关键方面，如模型选择、数据集准备和模型训练方法。

模型选择、数据集准备和训练方法的顺序取决于您项目的具体情况。以下是一些帮助您做出决定的提示：

清晰的问题理解：如果您的项目问题和目标定义明确，请从模型选择开始。然后，根据模型的要求准备数据集并确定训练方法。

独特或有限的数据：如果您的项目受到独特或有限数据的约束，请从数据集准备开始。例如，如果您有一个罕见的医学图像数据集，请首先标注和准备数据。然后，选择一个在此类数据上表现良好的模型，然后选择合适的训练方法。

需要实验：在实验至关重要的项目中，从训练方法开始。这在研究项目中很常见，您可能最初会测试不同的训练技术。在确定有前途的方法后，改进您的模型选择，并根据您的发现准备数据集。

接下来，让我们看看社区中关于计算机视觉任务和项目规划的一些常见讨论点。

最流行的计算机视觉任务包括图像分类、目标检测和图像分割。

如需各种任务的详细说明，请查看 Ultralytics 文档中关于 YOLO11 任务的页面。

不，预训练模型不会以传统意义上的方式“记住”类别。它们从海量数据集中学习模式，在自定义训练（微调）期间，这些模式会根据您的特定任务进行调整。模型的容量是有限的，专注于新信息可能会覆盖一些先前的学习内容。

如果您想使用模型预训练的类别，一种实用的方法是使用两个模型：一个保留原始性能，另一个针对您的特定任务进行微调。这样，您可以结合两个模型的输出。还有其他选项，例如冻结层、将预训练模型用作特征提取器以及任务特定分支，但这些解决方案更为复杂，需要更多专业知识。

模型部署选项对您的计算机视觉项目的性能至关重要。例如，部署环境必须能够处理模型的计算负载。以下是一些实际示例：

每种部署选项都提供不同的优势和挑战，选择取决于具体的项目需求，如性能、成本和安全性。

与其他计算机视觉爱好者建立联系，通过提供支持、解决方案和新想法，对您的项目非常有帮助。以下是一些学习、解决问题和建立联系的好方法：

定义清晰的问题并设定可衡量的目标是计算机视觉项目成功的关键。我们强调了从一开始就保持清晰和专注的重要性。设定具体目标有助于避免疏漏。此外，通过 GitHub 或 Discord 等平台与社区中的其他人保持联系对于学习和保持最新状态至关重要。简而言之，良好的规划和社区参与是计算机视觉项目成功的重要组成部分。

要为您的 Ultralytics 计算机视觉项目定义清晰的问题陈述，请按照以下步骤操作：

提供明确定义的问题陈述可确保项目保持专注并与您的目标保持一致。有关详细指南，请参阅我们的实用指南。

Ultralytics YOLO11 具有实时对象跟踪功能、高精度以及在检测和监控车辆速度方面的强大性能，因此非常适合速度估计。它通过利用尖端的计算机视觉技术，克服了传统雷达系统的低效率和不准确性。请查看我们关于使用 YOLO11 进行速度估计的博客，以获取更多见解和实际示例。

使用 SMART 标准设置有效且可衡量的目标：

例如，“在六个月内，使用 10,000 张车辆图像数据集，实现 95% 的速度 detect 准确率。”这种方法有助于跟踪进度并找出需要改进的领域。了解更多关于 设定可衡量目标 的信息。

部署选项对您的 Ultralytics YOLO 模型的性能至关重要。以下是主要选项：

有关更多信息，请参阅我们的模型部署选项详细指南。

通过彻底的初步研究、与利益相关者的清晰沟通以及对问题陈述和目标的迭代改进来应对这些挑战。在我们的计算机视觉项目指南中了解有关这些挑战的更多信息。

---

## 使用 Ultralytics YOLO 在不同区域进行对象计数 🚀

**URL:** https://docs.ultralytics.com/zh/guides/region-counting/

**Contents:**
- 使用 Ultralytics YOLO 在不同区域进行对象计数 🚀
- 什么是区域内对象计数？
- 区域内目标计数的优势
- 实际应用
- 使用示例
  - RegionCounter 参数
- 常见问题
  - 什么是使用 Ultralytics YOLO11 在指定区域内进行目标计数？
  - 如何使用 Ultralytics YOLO11 运行基于区域的对象计数脚本？
  - 为什么我应该使用 Ultralytics YOLO11 在区域中进行对象计数？

使用 Ultralytics YOLO11 在区域中进行对象计数涉及使用先进的计算机视觉精确确定指定区域内的对象数量。 这种方法对于优化流程、增强安全性以及提高各种应用程序的效率非常有价值。

观看： 使用 Ultralytics YOLO11 在不同区域进行对象计数 | Ultralytics 解决方案 🚀

使用 Ultralytics YOLO 进行区域计数

Ultralytics 区域计数模块可在我们的示例部分中找到。您可以浏览此示例以进行代码自定义，并对其进行修改以适合您的特定用例。

这是一个包含以下内容的表格 RegionCounter 参数：

字段 RegionCounter 解决方案支持使用目标跟踪参数：

使用 Ultralytics YOLO11 在指定区域中进行对象计数涉及使用先进的计算机视觉技术来检测和统计已定义区域内的对象数量。这种精确的方法提高了制造、监控和交通管理等各种应用中的效率和准确性。

按照以下步骤在 Ultralytics YOLO11 中运行对象计数：

克隆 Ultralytics 仓库并导航至该目录：

使用 Ultralytics YOLO11 在区域中进行对象计数具有以下几个优势：

使用 Ultralytics YOLO11 进行对象计数可以应用于许多实际场景：

在实际应用部分和TrackZone解决方案中探索更多示例，以获得额外的基于区域的监控功能。

**Examples:**

Example 1 (python):
```python
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "Error reading video file"

# Pass region as list
# region_points = [(20, 400), (1080, 400), (1080, 360), (20, 360)]

# Pass region as dictionary
region_points = {
    "region-01": [(50, 50), (250, 50), (250, 250), (50, 250)],
    "region-02": [(640, 640), (780, 640), (780, 720), (640, 720)],
}

# Video writer
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))
video_writer = cv2.VideoWriter("region_counting.avi", cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))

# Initialize region counter object
regioncounter = solutions.RegionCounter(
    show=True,  # display the frame
    region=region_points,  # pass region points
    model="yolo11n.pt",  # model for counting in regions, e.g., yolo11s.pt
)

# Process video
while cap.isOpened():
    success, im0 = cap.read()

    if not success:
        print("Video frame is empty or processing is complete.")
        break

    results = regioncounter(im0)

    # print(results)  # access the output

    video_writer.write(results.plot_im)

cap.release()
video_writer.release()
cv2.destroyAllWindows()  # destroy all opened windows
```

Example 2 (unknown):
```unknown
git clone https://github.com/ultralytics/ultralytics
cd ultralytics/examples/YOLOv8-Region-Counter
```

Example 3 (unknown):
```unknown
python yolov8_region_counter.py --source "path/to/video.mp4" --save-img
```

---

## 使用 OpenAI CLIP 和 Meta FAISS 进行语义图像搜索

**URL:** https://docs.ultralytics.com/zh/guides/similarity-search/

**Contents:**
- 使用 OpenAI CLIP 和 Meta FAISS 进行语义图像搜索
- 简介
- 语义图像搜索可视化预览
- 工作原理
- VisualAISearch 类
- VisualAISearch 参数
- 使用 CLIP 和 FAISS 进行语义图像搜索的优势
- 常见问题
  - CLIP 如何理解图像和文本？
  - 为什么 CLIP 被认为在 AI 任务中如此强大？

本指南将引导您使用 OpenAI CLIP、Meta FAISS 和 Flask 构建一个 语义图像搜索 引擎。通过将 CLIP 强大的视觉语言嵌入与 FAISS 高效的最近邻搜索相结合，您可以创建一个功能齐全的 Web 界面，您可以使用自然语言查询来检索相关图像。

观看： 相似性搜索如何工作 | 使用 OpenAI CLIP、META FAISS 和 Ultralytics 包进行视觉搜索 🎉

这种架构支持零样本搜索，这意味着您不需要标签或类别，只需要图像数据和一个好的提示。

使用 Ultralytics Python 包进行语义图像搜索

如果您使用的是自己的图像，请确保提供图像目录的绝对路径。否则，由于 Flask 的文件服务限制，图像可能不会显示在网页上。

如果您使用的是自己的图像，请确保提供图像目录的绝对路径。否则，由于 Flask 的文件服务限制，图像可能不会显示在网页上。

下表概述了可用的参数，用于 VisualAISearch:

使用 CLIP 和 FAISS 构建您自己的语义图像搜索系统具有以下几个引人注目的优势：

零样本能力：您无需在您的特定数据集上训练模型。CLIP 的零样本学习使您可以使用自由形式的自然语言对任何图像数据集执行搜索查询，从而节省时间和资源。

类人理解: 与基于关键词的搜索引擎不同，CLIP理解语义上下文。它可以根据抽象的、情感的或关系的查询（如“自然界中快乐的孩子”或“夜晚的未来城市天际线”）来检索图像。

无需标签或元数据：传统的图像搜索系统需要仔细标记的数据。此方法只需要原始图像。CLIP 生成嵌入，而无需任何手动注释。

灵活且可扩展的搜索：FAISS 即使对于大规模数据集也能实现快速的最近邻搜索。它针对速度和内存进行了优化，即使有成千上万（或数百万）的嵌入，也能实现实时响应。

跨领域应用： 无论您是构建个人照片档案、创意灵感工具、产品搜索引擎，甚至是艺术推荐系统，此堆栈都可以通过最少的调整来适应不同的领域。

CLIP（对比语言图像预训练）是由 OpenAI 开发的一种模型，它学习连接视觉和语言信息。它在大量的图像和自然语言标题配对的数据集上进行训练。这种训练使其能够将图像和文本映射到一个共享的嵌入空间中，因此您可以使用向量相似度直接比较它们。

CLIP 的突出之处在于其泛化能力。它并非仅针对特定标签或任务进行训练，而是从自然语言本身中学习。这使其能够处理灵活的查询，如“一个人骑着水上摩托”或“超现实的梦境”，从而使其可用于从分类到创造性语义搜索的各种任务，而无需重新训练。

FAISS（Facebook AI Similarity Search）是一个工具包，可帮助您非常高效地搜索高维向量。一旦 CLIP 将您的图像转换为嵌入，FAISS 就可以快速轻松地找到与文本查询最接近的匹配项，非常适合实时图像检索。

虽然 CLIP 和 FAISS 分别由 OpenAI 和 Meta 开发，但 Ultralytics Python 包 简化了它们集成到完整的语义图像搜索流程中，只需一个可以运行的两行代码工作流程：

是的。当前设置使用 Flask 和一个基本的 HTML 前端，但您可以将其替换为自己的 HTML，或者使用 React、Vue 或其他前端框架构建更动态的用户界面。Flask 可以作为您自定义接口的后端 API。

不能直接实现。一个简单的变通方法是从视频中提取单个帧（例如，每秒一帧），将其视为独立的图像，并将其输入到系统中。这样，搜索引擎就可以对视频中的视觉时刻进行语义索引。

**Examples:**

Example 1 (python):
```python
from ultralytics import solutions

app = solutions.SearchApp(
    # data = "path/to/img/directory" # Optional, build search engine with your own images
    device="cpu"  # configure the device for processing, e.g., "cpu" or "cuda"
)

app.run(debug=False)  # You can also use `debug=True` argument for testing
```

Example 2 (python):
```python
from ultralytics import solutions

searcher = solutions.VisualAISearch(
    # data = "path/to/img/directory" # Optional, build search engine with your own images
    device="cuda"  # configure the device for processing, e.g., "cpu" or "cuda"
)

results = searcher("a dog sitting on a bench")

# Ranked Results:
#     - 000000546829.jpg | Similarity: 0.3269
#     - 000000549220.jpg | Similarity: 0.2899
#     - 000000517069.jpg | Similarity: 0.2761
#     - 000000029393.jpg | Similarity: 0.2742
#     - 000000534270.jpg | Similarity: 0.2680
```

Example 3 (python):
```python
from ultralytics import solutions

searcher = solutions.VisualAISearch(
    # data = "path/to/img/directory" # Optional, build search engine with your own images
    device="cuda"  # configure the device for processing, e.g., "cpu" or "cuda"
)

results = searcher("a dog sitting on a bench")

# Ranked Results:
#     - 000000546829.jpg | Similarity: 0.3269
#     - 000000549220.jpg | Similarity: 0.2899
#     - 000000517069.jpg | Similarity: 0.2761
#     - 000000029393.jpg | Similarity: 0.2742
#     - 000000534270.jpg | Similarity: 0.2680
```

---

## 计算机视觉的数据收集和标注策略

**URL:** https://docs.ultralytics.com/zh/guides/data-collection-and-annotation/

**Contents:**
- 计算机视觉的数据收集和标注策略
- 简介
- 设置类别和收集数据
  - 为您的项目选择正确的类别
  - 数据来源
  - 避免数据收集中的偏差
- 什么是数据标注？
  - 数据标注的类型
  - 常用标注格式
  - 标注技术

任何 计算机视觉项目 成功的关键在于有效的数据收集和标注策略。数据的质量直接影响模型性能，因此务必理解与数据收集和数据标注相关的最佳实践。

观看： 如何为计算机视觉构建有效的数据收集和标注策略 🚀

关于数据的每一个考虑都应与您的项目目标紧密结合。标注策略的改变可能会转移项目的重点或有效性，反之亦然。考虑到这一点，让我们仔细看看处理数据收集和标注的最佳方法。

为计算机视觉项目收集图像和视频涉及定义类别的数量、数据来源以及考虑伦理影响。在开始收集数据之前，您需要明确：

启动计算机视觉项目时，首先要考虑的问题之一是包含多少个类别。您需要确定类别成员，这涉及到您希望模型识别和区分的不同类别或标签。类别的数量应由项目的具体目标决定。

例如，如果您想监控交通，您的类别可能包括“汽车”、“卡车”、“公共汽车”、“摩托车”和“自行车”。另一方面，对于跟踪商店中的物品，您的类别可以是“水果”、“蔬菜”、“饮料”和“零食”。根据您的项目目标定义类别有助于保持数据集的相关性和重点。

在定义类别时，另一个重要的区别是选择粗略还是精细的类别计数。“计数”指的是您感兴趣的不同类别的数量。这个决定会影响数据的粒度和模型的复杂性。以下是每种方法的注意事项：

从更具体的类别开始会非常有帮助，尤其是在细节至关重要的复杂项目中。更具体的类别允许您收集更详细的数据，获得更深入的洞察，并建立类别之间更清晰的区别。这不仅提高了模型的准确性，而且在需要时也更容易在后期调整模型，从而节省时间和资源。

您可以使用公共数据集或收集您自己的自定义数据。诸如Kaggle和Google Dataset Search Engine上的公共数据集提供了良好注释的标准数据，使其成为训练和验证模型的良好起点。

另一方面，自定义数据收集允许您根据特定需求自定义数据集。您可以使用相机或无人机捕获图像和视频，从网络上抓取图像，或者使用组织中现有的内部数据。自定义数据使您可以更好地控制其质量和相关性。结合公共和自定义数据源有助于创建多样化和全面的数据集。

当某些群体或场景在数据集中代表性不足或过度代表时，就会发生偏差。这会导致模型在某些数据上表现良好，但在其他数据上表现不佳。避免 AI 中的偏差 至关重要，这样您的计算机视觉模型才能在各种场景中表现良好。

遵循这些实践有助于创建一个更强大和公平的模型，该模型可以在实际应用中很好地推广。

数据标注是标记数据的过程，使其可用于训练机器学习模型。在计算机视觉中，这意味着使用模型需要学习的信息来标记图像或视频。如果没有正确标注的数据，模型就无法准确地学习输入和输出之间的关系。

根据计算机视觉任务的特定要求，有不同类型的数据标注。以下是一些示例：

选择一种标注类型后，重要的是选择适当的格式来存储和共享标注。

常用格式包括 COCO，它支持多种标注类型，例如 目标检测、关键点检测、背景分割、全景分割 和图像描述，并以 JSON 格式存储。Pascal VOC 使用 XML 文件，常用于目标检测任务。另一方面，YOLO 为每张图像创建一个 .txt 文件，其中包含对象类别、坐标、高度和宽度等标注信息，使其适用于目标检测。

现在，假设您已经选择了一种标注类型和格式，现在是建立清晰和客观的标注规则的时候了。这些规则就像是整个标注过程中保持一致性和准确性的路线图。这些规则的关键方面包括：

定期审查和更新您的标注规则将有助于保持您的标注准确、一致，并与您的项目目标保持一致。

假设您现在准备好进行标注。有几种开源工具可以帮助简化数据标注流程。以下是一些有用的开放标注工具：

这些开源工具经济实惠，并提供一系列功能来满足不同的标注需求。

在您开始标注数据之前，还有一些事项需要注意。您应该了解准确性、精确度、异常值和质量控制，以避免以适得其反的方式标注数据。

重要的是要理解准确性和精确度之间的区别，以及它与标注的关系。准确性是指标注数据与真实值有多接近。它帮助我们衡量标签与真实世界场景的匹配程度。精确度表示标注的一致性。它检查您是否在整个数据集中为同一对象或特征提供相同的标签。高准确性和精确度可以通过减少噪声并提高模型从训练数据中泛化的能力来训练出更好的模型。

异常值是指与数据集中其他观测值偏差很大的数据点。对于标注而言，异常值可能是不正确标记的图像或与数据集的其余部分不符的标注。异常值令人担忧，因为它们会扭曲模型的学习过程，导致不准确的预测和较差的泛化。

与其他技术项目一样，质量控制对于标注数据来说是必不可少的。定期检查标注以确保其准确和一致是一个好习惯。这可以通过以下几种不同的方式完成：

如果您与多人合作，不同标注者之间的一致性非常重要。良好的标注者间一致性意味着指南清晰，并且每个人都以相同的方式遵循指南。它可以使每个人保持一致，并使标注保持一致。

在审查时，如果您发现错误，请纠正它们并更新指南，以避免将来出现错误。向标注者提供反馈并提供定期培训，以帮助减少错误。拥有强大的错误处理流程可以保持数据集的准确性和可靠性。

为了使数据标记过程更顺畅、更有效，请考虑实施以下策略：

这些策略有助于保持高质量的标注，同时减少标注过程所需的时间和资源。

与其他计算机视觉爱好者交流您的想法和疑问可以帮助加速您的项目。以下是一些学习、解决问题和建立联系的好方法：

通过遵循收集和标注数据的最佳实践、避免偏差以及使用正确的工具和技术，您可以显著提高模型的性能。与社区互动并使用可用资源将使您随时了解情况，并帮助您有效地解决问题。请记住，高质量的数据是成功项目的基础，正确的策略将帮助您构建强大而可靠的模型。

避免数据收集中的偏差可确保您的计算机视觉模型在各种场景中表现良好。 为了最大限度地减少偏差，请考虑从不同的来源收集数据，以捕获不同的视角和场景。 确保所有相关群体（如不同年龄、性别和种族）之间的平衡代表性。 定期审查和更新您的数据集，以识别和解决任何新出现的偏差。 过采样代表性不足的类别、数据增强和公平感知算法等技术也有助于减轻偏差。 通过采用这些策略，您可以维护一个强大而公平的数据集，从而增强模型的泛化能力。

确保数据标注的高度一致性和准确性涉及建立清晰且客观的标注指南。 您的说明应详细，并提供示例和图示以阐明期望。 通过为标注各种数据类型设置标准标准，确保所有标注都遵循相同的规则，从而实现一致性。 为了减少个人偏见，请培训标注者保持中立和客观。 定期审查和更新标注规则有助于保持准确性并与项目目标保持一致。 使用自动化工具检查一致性并获得其他标注者的反馈也有助于保持高质量的标注。

为了使用 Ultralytics YOLO 模型进行有效的迁移学习和对象检测，首先每个类别至少需要几百个带注释的对象。如果仅针对一个类别进行训练，请从至少 100 个带注释的图像开始，并训练大约 100 个epochs。更复杂的任务可能需要每个类别数千个图像才能实现高可靠性和性能。高质量的注释至关重要，因此请确保您的数据收集和注释过程严谨且符合项目的特定目标。请在YOLO11 训练指南中探索详细的训练策略。

以下是一些常用的开源工具，可以简化数据标注流程：

这些工具可以帮助提高标注工作流程的效率和准确性。有关详细的功能列表和指南，请参阅我们的数据标注工具文档。

不同类型的数据标注适用于各种计算机视觉任务：

选择合适的标注类型取决于您的项目需求。请参阅我们的数据标注指南，详细了解如何实施这些标注及其格式。

---

## 在终端中查看推理结果 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/guides/view-results-in-terminal/

**Contents:**
- 在终端中查看推理结果
- 动机
- 处理
- 推理结果示例
- 完整代码示例
- 常见问题
  - 如何在 macOS 或 Linux 上的 VSCode 终端中查看 YOLO 推理结果？
  - 为什么 sixel 协议只在 Linux 和 macOS 上有效？
  - 如果在 VSCode 终端中显示图像时遇到问题，该怎么办？
  - YOLO可以使用sixel在终端中显示视频推理结果吗？

当连接到远程机器时，通常无法可视化图像结果，或者需要将数据移动到带有 GUI 的本地设备。VSCode 集成终端允许直接渲染图像。这是一个关于如何结合使用它的简短演示 ultralytics 使用 预测结果.

仅与 Linux 和 MacOS 兼容。查看 VSCode 仓库，检查 问题状态或 文档 用于获取有关 Windows 支持的更新，以便在终端中使用以下工具查看图像 sixel.

用于使用集成终端查看图像的 VSCode 兼容协议是 sixel 和 iTerm。本指南将演示如何使用 sixel 协议。

首先，您必须启用设置 terminal.integrated.enableImages 和 terminal.integrated.gpuAcceleration 在 VSCode 中。

使用 pip 安装 python-sixel 您虚拟环境中的库。这是一个 fork（派生） 的 PySixel 库，该库不再维护。

加载模型并执行推理，然后绘制结果并存储在变量中。有关推理参数和使用结果的更多信息，请参见预测模式页面。

现在，使用 OpenCV 要转换 np.ndarray 到 bytes 数据。然后使用 io.BytesIO 以创建一个“类文件”对象。

创建一个 SixelWriter 实例，然后使用 .draw() 在终端中绘制图像的 method。

尚未测试将此示例用于视频或动画 GIF 帧。尝试风险自负。

您可能需要使用 clear “擦除”终端中图像的视图。

要在 macOS 或 Linux 的 VSCode 终端中查看 YOLO 推理结果，请按照以下步骤操作：

sixel 协议目前仅在 Linux 和 macOS 上受支持，因为这些平台具有与 sixel 图形兼容的本机终端功能。使用 sixel 的 Windows 终端图形支持仍在开发中。有关 Windows 兼容性的更新，请查看 VSCode Issue 状态和文档。

如果在 VSCode 终端中使用 sixel 显示图像时遇到问题：

检查您的图像数据转换和绘图代码是否存在错误。例如：

如果问题仍然存在，请查阅 VSCode 仓库，并访问 绘图方法参数 部分以获取更多指导。

目前尚未测试在终端中使用 sixel 显示视频推理结果或动画 GIF 帧，并且可能不支持。我们建议从静态图像开始并验证兼容性。尝试视频结果时，请自行承担风险，并注意性能限制。有关绘制推理结果的更多信息，请访问 predict mode 页面。

要解决以下问题： python-sixel 库：

验证您是否具备必要的 Python 和系统依赖项。

有关更多文档和社区支持，请参阅python-sixel GitHub 存储库。

仔细检查您的代码是否存在潜在错误，特别是以下用法 SixelWriter 以及图像数据转换步骤。

有关使用YOLO模型和sixel集成的更多帮助，请参阅导出和预测模式文档页面。

**Examples:**

Example 1 (json):
```json
"terminal.integrated.gpuAcceleration": "auto" # "auto" is default, can also use "on"
"terminal.integrated.enableImages": true
```

Example 2 (unknown):
```unknown
pip install sixel
```

Example 3 (python):
```python
from ultralytics import YOLO

# Load a model
model = YOLO("yolo11n.pt")

# Run inference on an image
results = model.predict(source="ultralytics/assets/bus.jpg")

# Plot inference results
plot = results[0].plot()  # (1)!
```

Example 4 (markdown):
```markdown
import io

import cv2

# Results image as bytes
im_bytes = cv2.imencode(
    ".png",  # (1)!
    plot,
)[1].tobytes()  # (2)!

# Image bytes as a file-like object
mem_file = io.BytesIO(im_bytes)
```

---
