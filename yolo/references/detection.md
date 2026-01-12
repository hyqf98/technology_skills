# Yolo - Detection

**Pages:** 92

---

## RTDETRv2 与 EfficientDet：综合技术比较

**URL:** https://docs.ultralytics.com/zh/compare/rtdetr-vs-efficientdet/

**Contents:**
- RTDETRv2 与 EfficientDet：综合技术比较
- 模型概述
  - RTDETRv2
  - EfficientDet
- 架构分析
  - RTDETRv2：Transformer 力量
  - EfficientDet：可扩展效率
- 性能对比
  - 优势与劣势
- 理想用例

在不断发展的计算机视觉领域，选择正确的物体检测架构对项目的成功至关重要。本比较深入探讨了RTDETRv2 和EfficientDet，前者是专为实时性能设计的transformer尖端模型，后者是为提高效率而优化的可扩展卷积神经网络 (CNN) 系列。我们分析了它们的架构创新、性能指标和理想部署方案，以帮助开发人员做出明智的决定。

这两种模型之间的选择通常取决于目标硬件的具体限制和应用程序的准确性要求。

RTDETRv2 (实时检测 Transformer v2) 代表了将 Transformer 架构应用于实时目标检测的重大进步。它由百度研究人员开发，在原始 RT-DETR 的成功基础上，优化了混合编码器和查询选择机制，以在 GPU 硬件上实现最先进的精度和具有竞争力的推理速度。

EfficientDet由Google Brain开发，通过引入一种系统地扩展模型维度的方法，在其发布时彻底改变了该领域。它将EfficientNet骨干网络与加权双向特征金字塔网络（BiFPN）相结合，提供了一系列模型（D0-D7），这些模型在计算成本和准确性之间进行权衡，使其在各种资源限制下具有高度通用性。

了解更多关于 EfficientDet 的信息

根本区别在于它们的核心构建块：一个利用 Transformer 的全局上下文，而另一个则优化了卷积的效率。

RTDETRv2 采用混合编码器，可高效处理多尺度特征。与传统 CNN 不同，它使用 IoU 感知的查询选择机制，将注意力集中在图像最相关的部分。这使得模型能够有效处理具有遮挡和不同对象尺度的复杂场景。该架构解耦了尺度内交互和跨尺度融合，从而减少了通常与视觉 Transformer (ViTs)相关联的计算开销。

RTDETRv2 中的注意力机制允许全局感受野，使模型能够比典型 CNN 更好地理解场景中远距离物体之间的关系。

EfficientDet 基于EfficientNet 骨干网络并引入了BiFPN。BiFPN 通过学习不同输入特征的重要性，实现了轻松快速的多尺度特征融合。此外，EfficientDet 采用了一种复合缩放方法，统一缩放网络的解析度、深度和宽度。这确保了模型可以进行定制——从用于移动应用的轻量级 D0 到用于高精度服务器任务的重型 D7。

性能基准测试突显了设计理念上的明显区别。RTDETRv2 旨在强大硬件上实现峰值 accuracy，而 EfficientDet 提供了精细的效率梯度。

如表中所示，RTDETRv2-x实现了54.3的卓越mAP，甚至超越了最大的EfficientDet-d7（53.7 mAP），同时在TensorRT上显著更快（15.03ms vs 128.07ms）。然而，对于极其受限的环境，EfficientDet-d0仍然是一个极其轻量级的选择，具有最少的参数（3.9M）和FLOPs。

选择合适的模型很大程度上取决于具体的应用环境。

尽管 RTDETRv2 和 EfficientDet 各有优点，但 Ultralytics YOLO11 提供了它们最佳功能的引人注目的综合，并封装在一个开发人员友好的生态系统中。

Ultralytics 模型的设计不仅是为了基准测试，更是为了实际可用性。

Ultralytics 提供预训练权重，可促进迁移学习，显著减少训练时间。以下是开始训练 YOLO11 模型是多么简单：

Ultralytics 模型可以通过单个命令导出为 ONNX、TensorRT、CoreML 和 OpenVINO 等多种格式，从而简化了从研究到生产的路径。了解更多导出模式。

在RTDETRv2 与 EfficientDet 的比较中，胜负取决于您的限制条件。RTDETRv2在高精度、GPU环境中表现出色，证明了变压器也可以很快。对于高约束、低功耗的边缘方案，EfficientDet仍然是一个可靠的选择。

然而，对于大多数寻求多功能、易用且高性能解决方案的开发人员而言，Ultralytics YOLO11 脱颖而出。它能够在单一、内聚的生态系统中处理多种视觉任务，结合卓越的内存效率和训练速度，使其成为现代计算机视觉应用的最佳选择。

要拓宽您对现有目标 detect 模型的理解，请考虑查阅这些相关比较：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLO11 model
model = YOLO("yolo11n.pt")

# Train the model on the COCO8 dataset for 100 epochs
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)
```

---

## YOLO11 与 RTDETRv2：实时 detect 器的技术对比

**URL:** https://docs.ultralytics.com/zh/compare/yolo11-vs-rtdetr/

**Contents:**
- YOLO11 与 RTDETRv2：实时 detect 器的技术对比
- Ultralytics YOLO11：实时计算机视觉的标准
  - 架构与优势
- RTDETRv2：Transformer 驱动的精度
  - 架构与特点
- 性能分析：速度、准确性和效率
  - 定量比较
  - 结果分析
- 工作流程与可用性
  - 易用性与生态系统

选择最佳物体检测架构需要在推理速度、检测精度和计算资源效率之间进行复杂的权衡。本分析对以下几种结构进行了全面的技术比较 Ultralytics YOLO11和高性能实时检测Transformer RTDETRv2 之间进行了全面的技术比较。

尽管 RTDETRv2 展示了 Transformer 架构在高精度任务中的潜力，但 YOLO11 通常为实际部署提供了更优的平衡，它提供了更快的推理速度、显著更低的内存占用和更强大的开发者生态系统。

Ultralytics YOLO11 代表了多年来对高效卷积神经网络 (CNN) 研究的顶峰。它旨在成为实际计算机视觉应用的权威工具，在不影响最先进的准确性的前提下，优先考虑效率。

作者: Glenn Jocher, Jing Qiu机构:Ultralytics日期: 2024-09-27GitHub:https://github.com/ultralytics/ultralytics文档:https://docs.ultralytics.com/models/yolo11/

YOLO11 采用精炼的单阶段、无锚点架构。它集成了先进的特征提取模块，包括优化的 C3k2 块和 SPPF（空间金字塔池化 - 快速）模块，以捕获不同尺度的特征。

RTDETRv2 是一种实时检测 Transformer (RT-DETR)，它利用视觉 Transformer (ViT) 的强大功能，在基准数据集上实现了高精度。它旨在解决传统上与 DETR 类模型相关的延迟问题。

作者： 吕文宇、赵一安、常钦尧、黄奎、王冠中、刘毅机构： 百度日期： 2023-04-17预印本：https://arxiv.org/abs/2304.08069GitHub：https://github.com/lyuwenyu/RT-DETR/tree/main/rtdetrv2_pytorch文档：https://github.com/lyuwenyu/RT-DETR/tree/main/rtdetrv2_pytorch#readme

RTDETRv2 采用混合架构，结合了 CNN 骨干网络和高效的 Transformer 编码器-解码器。自注意力机制使模型能够捕获全局上下文，这对于具有复杂对象关系的场景非常有利。

比较 YOLO11 和 RTDETRv2 时，区别在于纯精度指标和操作效率之间的架构权衡。

基于 Transformer 的模型，如 RTDETRv2，通常需要强大的 GPU 才能进行有效的训练和推理。相比之下，基于 CNN 的模型，如 YOLO11，针对更广泛的硬件进行了高度优化，包括 CPU 和树莓派等边缘 AI 设备。

下表展示了COCO数据集上的性能指标。尽管RTDETRv2展现出强大的mAP分数，YOLO11提供了具有竞争力的准确性，并显著提升了推理速度，尤其是在CPU上。

对于开发者而言，模型的“成本”包括集成时间、训练稳定性和部署便捷性。

The Ultralytics Python API将复杂的训练循环抽象为几行代码。

相比之下，尽管 RTDETRv2 是一个强大的研究工具，但它通常需要更多的手动配置和对底层代码库更深入的了解，才能适应自定义数据集或导出为 ONNX 或 TensorRT 等特定格式。

训练 Transformer 模型通常需要显著更高的 GPU 内存 (VRAM)。这可能迫使开发人员使用更小的 批次大小 或租用更昂贵的云硬件。YOLO11 的 CNN 架构内存高效，允许在消费级 GPU 上使用更大的批次大小并实现更快的收敛。

尽管 RTDETRv2 展示了 Transformer 架构在视觉领域的学术进展，但 Ultralytics YOLO11 仍然是绝大多数实际应用的实用选择。其卓越的速度-精度比、更低的内存需求以及处理多种视觉任务的能力，使其成为一个多功能且强大的工具。再结合成熟且维护良好的生态系统，YOLO11 使开发者能够以最小的阻力从概念走向生产。

比较模型有助于根据您的具体限制选择合适的工具。在 Ultralytics 文档中探索更多比较：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pretrained YOLO11 model
model = YOLO("yolo11n.pt")

# Train on a custom dataset with a single command
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference on an image
results = model("path/to/image.jpg")
```

---

## YOLOv6-3.0 对比 EfficientDet：目标检测中的速度与精度平衡

**URL:** https://docs.ultralytics.com/zh/compare/yolov6-vs-efficientdet/

**Contents:**
- YOLOv6-3.0 对比 EfficientDet：目标检测中的速度与精度平衡
- 性能指标与技术分析
- YOLOv6-3.0：为工业应用而生
  - 架构与优势
- EfficientDet：可扩展精度
  - 架构与优势
- 实际应用案例
  - YOLOv6-3.0 的理想应用场景
  - EfficientDet 的理想应用场景
- Ultralytics 的优势：为什么选择 YOLO11？

在快速发展的计算机视觉领域，选择合适的物体 detect 架构对于您的项目成功至关重要。本比较深入探讨了 YOLOv6-3.0 和 EfficientDet，这两个从不同角度应对视觉识别挑战的著名模型。尽管 EfficientDet 侧重于参数效率和可扩展性，YOLOv6-3.0 则是专为工业应用而设计，在这些应用中，推理延迟和实时速度是不可妥协的。

这两种架构的根本区别在于它们的设计理念。EfficientDet 依赖于一种复杂的特征融合机制，即 BiFPN，它提高了准确性，但通常以牺牲 GPU 上的计算速度为代价。相反，YOLOv6-3.0 采用硬件感知设计，利用重参数化来简化推理过程中的操作，从而显著提高FPS（每秒帧数）。

下表说明了这种权衡。尽管EfficientDet-d7实现了高mAP，但其延迟很高。相比之下，YOLOv6-3.0l提供了可比的准确性，同时显著减少了推理时间，使其更适用于实时推理场景。

对于工业部署，将 YOLOv6-3.0 与 TensorRT 结合可以带来巨大的速度提升。YOLOv6 架构的简洁性使其能够比旧模型中复杂的特征金字塔网络更高效地映射到 GPU 硬件指令。

YOLOv6-3.0是一个单阶段物体detect器，旨在弥合学术研究与工业需求之间的鸿沟。它优先考虑速度，同时不牺牲质量检测等任务所需的精度。

作者: Chuyi Li, Lulu Li, Yifei Geng, Hongliang Jiang, Meng Cheng, Bo Zhang, Zaidan Ke, Xiaoming Xu, and Xiangxiang Chu机构:美团日期: 2023-01-13Arxiv:YOLOv6 v3.0：全面重载GitHub:meituan/YOLOv6文档:YOLOv6 文档

YOLOv6-3.0的核心是其高效的骨干网络和“RepOpt”设计。通过利用重参数化，模型将训练时的多分支结构与推理时的单分支结构解耦。这使得模型易于训练且梯度丰富，同时执行速度极快。

EfficientDet 通过将复合缩放的概念引入目标检测领域，彻底改变了该领域。它同时优化网络深度、宽度和分辨率，以实现卓越的每参数性能。

作者: Mingxing Tan, Ruoming Pang, and Quoc V. Le机构:Google日期: 2019-11-20Arxiv:EfficientDet：可扩展高效的目标检测GitHub:google/automl/efficientdet

EfficientDet 依赖于 EfficientNet 主干网络，并引入了双向特征金字塔网络 (BiFPN)。这种复杂的颈部结构实现了简单快速的多尺度特征融合。

尽管 BiFPN 在提高精度方面非常有效，但其不规则的内存访问模式可能导致在 GPU 上的速度比 YOLO 架构中使用的密集、规则卷积块慢。这就是为什么 EfficientDet 尽管参数较少，但通常在基准测试中表现出更高的 推理延迟。

了解更多关于 EfficientDet 的信息

这些模型之间的选择通常取决于部署环境的具体限制。

尽管 YOLOv6-3.0 和 EfficientDet 是有能力的模型，但Ultralytics YOLO11代表了计算机视觉技术的尖端。YOLO11 提炼了之前 YOLO 世代的最佳特性，并将其集成到一个无缝、用户友好的生态系统中。

如果您正在评估计算机视觉管道的选项，可以考虑探索 Ultralytics 目录中的其他模型。YOLOv8 为各种任务提供了强大的性能，而基于 Transformer 的 RT-DETR 则为需要全局上下文感知的场景提供了另一种选择。对于移动端特定应用，YOLOv10 也值得研究。将这些模型与 EfficientDet 进行比较，有助于根据您的特定硬件和精度要求进行精细选择。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load the YOLO11 model (recommended over older versions)
model = YOLO("yolo11n.pt")

# Perform inference on an image
results = model("https://ultralytics.com/images/bus.jpg")

# Display results
results[0].show()
```

---

## Reference for ultralytics/solutions/object_counter.py

**URL:** https://docs.ultralytics.com/zh/reference/solutions/object_counter/

**Contents:**
- Reference for ultralytics/solutions/object_counter.py
- class ultralytics.solutions.object_counter.ObjectCounter
  - method ultralytics.solutions.object_counter.ObjectCounter.count_objects
  - method ultralytics.solutions.object_counter.ObjectCounter.display_counts
  - method ultralytics.solutions.object_counter.ObjectCounter.process

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/solutions/object_counter.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class to manage the counting of objects in a real-time video stream based on their tracks.

This class extends the BaseSolution class and provides functionality for counting objects moving in and out of a specified region in a video stream. It supports both polygonal and linear regions for counting.

Count objects within a polygonal or linear region based on their tracks.

Display object counts on the input image or frame.

Process input data (frames or object tracks) and update object counts.

This method initializes the counting region, extracts tracks, draws bounding boxes and regions, updates object counts, and displays the results on the input image.

**Examples:**

Example 1 (rust):
```rust
ObjectCounter(self, **kwargs: Any) -> None
```

Example 2 (json):
```json
>>> counter = ObjectCounter()
>>> frame = cv2.imread("frame.jpg")
>>> results = counter.process(frame)
>>> print(f"Inward count: {counter.in_count}, Outward count: {counter.out_count}")
```

Example 3 (python):
```python
class ObjectCounter(BaseSolution):
    """A class to manage the counting of objects in a real-time video stream based on their tracks.

    This class extends the BaseSolution class and provides functionality for counting objects moving in and out of a
    specified region in a video stream. It supports both polygonal and linear regions for counting.

    Attributes:
        in_count (int): Counter for objects moving inward.
        out_count (int): Counter for objects moving outward.
        counted_ids (list[int]): List of IDs of objects that have been counted.
        classwise_count (dict[str, dict[str, int]]): Dictionary for counts, categorized by object class.
        region_initialized (bool): Flag indicating whether the counting region has been initialized.
        show_in (bool): Flag to control display of inward count.
        show_out (bool): Flag to control display of outward count.
        margin (int): Margin for background rectangle size to display counts properly.

    Methods:
        count_objects: Count objects within a polygonal or linear region based on their tracks.
        display_counts: Display object counts on the frame.
        process: Process input data and update counts.

    Examples:
        >>> counter = ObjectCounter()
        >>> frame = cv2.imread("frame.jpg")
        >>> results = counter.process(frame)
        >>> print(f"Inward count: {counter.in_count}, Outward count: {counter.out_count}")
    """

    def __init__(self, **kwargs: Any) -> None:
        """Initialize the ObjectCounter class for real-time object counting in video streams."""
        super().__init__(**kwargs)

        self.in_count = 0  # Counter for objects moving inward
        self.out_count = 0  # Counter for objects moving outward
        self.counted_ids = []  # List of IDs of objects that have been counted
        self.classwise_count = defaultdict(lambda: {"IN": 0, "OUT": 0})  # Dictionary for counts, categorized by class
        self.region_initialized = False  # Flag indicating whether the region has been initialized

        self.show_in = self.CFG["show_in"]
        self.show_out = self.CFG["show_out"]
        self.margin = self.line_width * 2  # Scales the background rectangle size to display counts properly
```

Example 4 (python):
```python
def count_objects(
    self,
    current_centroid: tuple[float, float],
    track_id: int,
    prev_position: tuple[float, float] | None,
    cls: int,
) -> None
```

---

## Reference for ultralytics/nn/modules/utils.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/nn/modules/utils/

**Contents:**
- Reference for ultralytics/nn/modules/utils.py
- function ultralytics.nn.modules.utils._get_clones
- function ultralytics.nn.modules.utils.bias_init_with_prob
- function ultralytics.nn.modules.utils.linear_init
- function ultralytics.nn.modules.utils.inverse_sigmoid
- function ultralytics.nn.modules.utils.multi_scale_deformable_attn_pytorch

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/nn/modules/utils.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Create a list of cloned modules from the given module.

Initialize conv/fc bias value according to a given probability value.

This function calculates the bias initialization value based on a prior probability using the inverse error function. It's commonly used in object detection models to initialize classification layers with a specific positive prediction probability.

Initialize the weights and biases of a linear module.

This function initializes the weights of a linear module using a uniform distribution within bounds calculated from the input dimension. If the module has a bias, it is also initialized.

Calculate the inverse sigmoid function for a tensor.

This function applies the inverse of the sigmoid function to a tensor, which is useful in various neural network operations, particularly in attention mechanisms and coordinate transformations.

Implement multi-scale deformable attention in PyTorch.

This function performs deformable attention across multiple feature map scales, allowing the model to attend to different spatial locations with learned offsets.

**Examples:**

Example 1 (python):
```python
def _get_clones(module, n)
```

Example 2 (typescript):
```typescript
>>> import torch.nn as nn
>>> layer = nn.Linear(10, 10)
>>> clones = _get_clones(layer, 3)
>>> len(clones)
3
```

Example 3 (python):
```python
def _get_clones(module, n):
    """Create a list of cloned modules from the given module.

    Args:
        module (nn.Module): The module to be cloned.
        n (int): Number of clones to create.

    Returns:
        (nn.ModuleList): A ModuleList containing n clones of the input module.

    Examples:
        >>> import torch.nn as nn
        >>> layer = nn.Linear(10, 10)
        >>> clones = _get_clones(layer, 3)
        >>> len(clones)
        3
    """
    return nn.ModuleList([copy.deepcopy(module) for _ in range(n)])
```

Example 4 (python):
```python
def bias_init_with_prob(prior_prob = 0.01)
```

---

## RTDETRv2 与 YOLOv7：详细技术比较

**URL:** https://docs.ultralytics.com/zh/compare/rtdetr-vs-yolov7/

**Contents:**
- RTDETRv2 与 YOLOv7：详细技术比较
- 性能指标：准确性对比速度
- RTDETRv2：Transformer 方法
  - 主要架构特性
- YOLOv7: CNN巅峰
  - 主要架构特性
- 比较分析
  - 架构与多功能性
  - 训练与易用性
  - 部署与生态系统

实时目标检测领域见证了卷积神经网络 (CNN) 与新兴视觉Transformer (ViT) 之间的激烈竞争。这一演进中的两个重要里程碑是RTDETRv2（实时检测Transformer v2）和YOLOv7（You Only Look Once 第7版）。YOLOv7代表了高效CNN架构优化的巅峰，而RTDETRv2则引入了Transformer的强大功能，消除了对非极大值抑制 (NMS) 等后处理步骤的需求。

本比较探讨了两种模型的技术规格、架构差异和性能指标，旨在帮助开发人员为其计算机视觉应用选择合适的工具。

下表直接比较了关键性能指标。RTDETRv2-x 凭借其基于 Transformer 的全局上下文理解能力，展现出更高的 mAP 和卓越的准确性。然而，YOLOv7 仍具竞争力，尤其是在需要更轻量级和在不同硬件上平衡推理速度的场景中。

RTDETRv2 建立在原始 RT-DETR 的成功之上，原始 RT-DETR 是第一个在实时速度上真正能与 YOLO 模型匹敌的基于 Transformer 的检测器。由百度研究人员开发，它解决了标准 DETR 架构中与多尺度交互相关的计算瓶颈。

RTDETRv2 利用混合编码器，通过解耦尺度内交互和跨尺度融合来高效处理多尺度特征。这种设计与标准 Transformer 相比显著降低了计算成本。一个突出特点是其IoU 感知查询选择，它改进了对象查询的初始化，从而实现更快的收敛和更高的准确性。与基于 CNN 的模型不同，RTDETRv2 是免 NMS 的，这意味着它不需要非极大值抑制后处理，从而简化了部署流程并减少了延迟抖动。

RTDETRv2 架构的主要优势在于其捕获全局上下文的能力。虽然 CNN 关注局部感受野，但 Transformer 中的自注意力机制允许模型在检测物体时考虑整个图像上下文，这对于解决复杂遮挡场景中的歧义非常有利。

YOLOv7 突破了卷积神经网络的潜力极限。它专注于优化训练过程和模型架构，以实现“免费增强”——即在不增加推理成本的情况下提高准确性的方法。

YOLOv7 引入了E-ELAN（扩展高效层聚合网络），通过控制梯度路径长度来增强网络的学习能力。它还采用了模型重参数化技术，该技术在训练期间使用复杂的模型结构以实现更好的学习，但在推理期间简化以提高速度。这使得 YOLOv7 能够在GPU 设备上保持高性能，同时与 Transformer 模型相比，参数量相对较低。

根本区别在于骨干网络和头部设计。YOLOv7 依赖于深度 CNN 结构，这些结构针对CUDA加速进行了高度优化，但在处理图像中的长距离依赖关系时可能会遇到困难。RTDETRv2 利用注意力机制来理解远距离像素之间的关系，使其在杂乱环境中表现稳健。然而，这会以训练期间更高的内存消耗为代价。

Ultralytics 模型，例如 YOLO11，通过提供集成现代类注意力模块的基于 CNN 的架构来弥补这一差距，在提供 CNN 速度的同时，实现了通常只有 Transformer 模型才能达到的精度。此外，虽然 RTDETRv2 主要是一个目标检测器，但较新的 Ultralytics 模型原生支持实例分割、姿势估计和分类。

与 YOLOv7 等 CNN 相比，训练 RTDETRv2 等 Transformer 模型通常需要大量的 GPU 内存和更长的训练周期才能收敛。

对于寻求 训练效率 和 易用性，Ultralytics 生态系统具有明显的优势。借助 ultralytics Python 包，用户只需几行代码即可训练、验证和部署模型，并可访问一系列用于不同任务的预训练权重。

YOLOv7因其发布时间较早而得到广泛支持，但集成到现代MLOps流水线中可能需要手动操作。RTDETRv2较新，支持度正在增长。相比之下，Ultralytics模型受益于维护良好的生态系统，包括无缝导出到ONNX、TensorRT和CoreML，以及与Ultralytics HUB等工具集成，用于云训练和数据集管理。

虽然YOLOv7和RTDETRv2功能强大，但YOLO11代表了视觉AI的最新演进。它比Transformer需要更少的CUDA内存，训练速度更快，并在从边缘设备到云服务器的更广泛硬件范围内提供最先进的准确性。

RTDETRv2 和 YOLOv7 都塑造了计算机视觉的发展方向。RTDETRv2 成功挑战了 Transformer 对于实时应用来说速度过慢的观念，而 YOLOv7 则展示了 CNN 经久不衰的效率。然而，对于当今大多数实际应用而言，Ultralytics YOLO11 模型提供了卓越的开发者体验，它结合了这些前代模型的最佳特性以及一个现代化的、支持性的生态系统。

**Examples:**

Example 1 (python):
```python
from ultralytics import RTDETR, YOLO

# Load an Ultralytics YOLOv7-style model (if available) or YOLO11
model_yolo = YOLO("yolo11n.pt")  # Recommended for best performance
model_yolo.train(data="coco8.yaml", epochs=10)

# Load RT-DETR for comparison
model_rtdetr = RTDETR("rtdetr-l.pt")
model_rtdetr.predict("asset.jpg")
```

---

## DAMO-YOLO 与 PP-YOLOE+ 的技术对比

**URL:** https://docs.ultralytics.com/zh/compare/damo-yolo-vs-pp-yoloe/

**Contents:**
- DAMO-YOLO 与 PP-YOLOE+ 的技术对比
- DAMO-YOLO：来自阿里巴巴的速度导向创新
  - 架构和主要特性
- PP-YOLOE+：PaddlePaddle 内部的精密工程
  - 架构和主要特性
- 性能分析：指标与效率
  - 优势与劣势
- 应用案例与应用
- Ultralytics 的优势：为什么选择 YOLO11？
  - 无与伦比的多功能性和生态系统

选择最优的对象detect架构是一个关键决策，它影响着计算机视觉项目的效率、准确性和可扩展性。这项全面的比较分析了两个著名的模型：DAMO-YOLO（来自阿里巴巴的专注于速度的detect器）和PP-YOLOE+（来自百度PaddlePaddle生态系统的高精度模型）。我们深入探讨了它们独特的架构、性能指标和理想的部署场景，以帮助开发人员做出明智的选择。

DAMO-YOLO 由阿里巴巴集团开发，代表了高效目标检测领域的一大飞跃。它优先考虑卓越的速度-精度权衡，利用神经架构搜索 (NAS) 等先进技术以优化资源受限设备上的性能。

DAMO-YOLO 凭借其模块化设计理念以及融合的多项前沿技术而独树一帜：

DAMO-YOLO 为其较小变体（Tiny, Small）大量利用 知识蒸馏。通过将知识从较大的“教师”模型转移到较小的“学生”模型，它实现了比通常此类轻量级架构可能达到的更高准确性。

PP-YOLOE+ 是 PP-YOLO 系列的演进版本，由百度研究人员开发。它是一种无锚框、单阶段检测器，旨在突破 COCO 数据集等标准基准测试的精度极限，并专门针对 PaddlePaddle 深度学习框架进行了优化。

PP-YOLOE+ 专注于优化和高精度组件：

在比较 DAMO-YOLO 和 PP-YOLOE+ 时，权衡通常在于纯粹的推理速度和绝对精度之间。DAMO-YOLO 旨在 GPU 硬件上实现更快的速度，而 PP-YOLOE+ 则追求顶级的精度，但这通常以增加模型大小和 FLOPs 为代价。

尽管 DAMO-YOLO 和 PP-YOLOE+ 都提供了特定的优势，但 Ultralytics YOLO11 提供了一个全面的解决方案，平衡了性能、可用性和生态系统支持。对于大多数开发人员而言，YOLO11 代表了将 computer vision 投入生产的最实用和强大的选择。

与专用检测器不同，YOLO11 是一个多模态强大工具。它支持广泛的任务，包括目标检测、实例分割、姿势估计、分类和旋转框检测 (OBB)——所有这些都在一个单一、统一的框架内。

Ultralytics 生态系统旨在实现从研究到生产的无缝过渡。凭借积极的维护、频繁的更新以及与 TensorRT 和 OpenVINO 等工具的集成，开发人员可以自信地部署模型。

YOLO11 入门非常简单。以下代码片段演示了如何加载预训练模型并在图像上运行推理：

这种简洁性与强大的性能相结合，使Ultralytics YOLO11成为寻求构建可扩展和可维护AI解决方案的开发人员的首选。

DAMO-YOLO 和 PP-YOLOE+ 都为计算机视觉领域做出了重大贡献。DAMO-YOLO 展示了神经架构搜索在效率方面的强大作用，而 PP-YOLOE+ 则突出了 PaddlePaddle 生态系统中无锚点设计所能实现的精度。

然而，对于一个兼具速度、准确性和易用性的多功能、生产就绪型解决方案，Ultralytics YOLO11仍然是卓越的推荐。它对多种视觉任务的全面支持、低内存占用和详尽的文档使开发者能够更快、更有效地进行创新。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLO11 model
model = YOLO("yolo11n.pt")

# Run inference on a local image source
results = model("path/to/image.jpg")

# Display the inference results
results[0].show()
```

---

## Reference for ultralytics/utils/metrics.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/metrics/

**Contents:**
- Reference for ultralytics/utils/metrics.py
- class ultralytics.utils.metrics.ConfusionMatrix
  - method ultralytics.utils.metrics.ConfusionMatrix._append_matches
  - method ultralytics.utils.metrics.ConfusionMatrix.matrix
  - method ultralytics.utils.metrics.ConfusionMatrix.plot
  - method ultralytics.utils.metrics.ConfusionMatrix.plot_matches
  - method ultralytics.utils.metrics.ConfusionMatrix.print
  - method ultralytics.utils.metrics.ConfusionMatrix.process_batch
  - method ultralytics.utils.metrics.ConfusionMatrix.process_cls_preds
  - method ultralytics.utils.metrics.ConfusionMatrix.summary

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/metrics.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Bases: DataExportMixin

A class for calculating and updating a confusion matrix for object detection and classification tasks.

Append the matches to TP, FP, FN or GT list for the last batch.

This method updates the matches dictionary by appending specific batch data to the appropriate match type (True Positive, False Positive, or False Negative).

For masks, handles both overlap and non-overlap cases. When masks.max() > 1.0, it indicates overlap_mask=True with shape (1, H, W), otherwise uses direct indexing.

Return the confusion matrix.

Plot the confusion matrix using matplotlib and save it to a file.

Plot grid of GT, TP, FP, FN for each image.

Print the confusion matrix to the console.

Update confusion matrix for object detection task.

Update confusion matrix for classification task.

Generate a summarized representation of the confusion matrix as a list of dictionaries, with optional

normalization. This is useful for exporting the matrix to various formats such as CSV, XML, HTML, JSON, or SQL.

Return true positives and false positives.

Class for computing evaluation metrics for Ultralytics YOLO models.

Return the Average Precision (AP) at an IoU threshold of 0.5 for all classes.

Return the Average Precision (AP) at an IoU threshold of 0.5-0.95 for all classes.

Return the Mean Precision of all classes.

Return the Mean Recall of all classes.

Return the mean Average Precision (mAP) at an IoU threshold of 0.5.

Return the mean Average Precision (mAP) at an IoU threshold of 0.75.

Return the mean Average Precision (mAP) over IoU thresholds of 0.5 - 0.95 in steps of 0.05.

Return mAP of each class.

Return a list of curves for accessing specific metrics curves.

Return a list of curves for accessing specific metrics curves.

Return class-aware result, p[i], r[i], ap50[i], ap[i].

Return model fitness as a weighted combination of metrics.

Return mean of results, mp, mr, map50, map.

Update the evaluation metrics with a new set of results.

Bases: SimpleClass, DataExportMixin

Utility class for computing detection metrics such as precision, recall, and mean average precision (mAP).

Return a list of keys for accessing specific metrics.

Return mean Average Precision (mAP) scores per class.

Return the fitness of box object.

Return the average precision index per class.

Return dictionary of computed performance metrics and statistics.

Return a list of curves for accessing specific metrics curves.

Return a list of computed performance metrics and statistics.

Return the result of evaluating the performance of an object detection model on a specific class.

Clear the stored statistics.

Calculate mean of detected objects & return precision, recall, mAP50, and mAP50-95.

Process predicted results for object detection and update metrics.

Generate a summarized representation of per-class detection metrics as a list of dictionaries. Includes

shared scalar metrics (mAP, mAP50, mAP75) alongside precision, recall, and F1-score for each class.

Update statistics by appending new values to existing stat collections.

Calculate and aggregate detection and segmentation metrics over a given set of classes.

Return a list of keys for accessing metrics.

Return mAP scores for object detection and semantic segmentation models.

Return the fitness score for both segmentation and bounding box models.

Return a list of curves for accessing specific metrics curves.

Return a list of computed performance metrics and statistics.

Return classification results for a specified class index.

Return the mean metrics for bounding box and segmentation results.

Process the detection and segmentation metrics over the given set of predictions.

Generate a summarized representation of per-class segmentation metrics as a list of dictionaries. Includes

both box and mask scalar metrics (mAP, mAP50, mAP75) alongside precision, recall, and F1-score for each class.

Calculate and aggregate detection and pose metrics over a given set of classes.

Return a list of evaluation metric keys.

Return the mean average precision (mAP) per class for both box and pose detections.

Return combined fitness score for pose and box detection.

Return a list of curves for accessing specific metrics curves.

Return a list of computed performance metrics and statistics.

Return the class-wise detection results for a specific class i.

Return the mean results of box and pose.

Process the detection and pose metrics over the given set of predictions.

Generate a summarized representation of per-class pose metrics as a list of dictionaries. Includes both box

and pose scalar metrics (mAP, mAP50, mAP75) alongside precision, recall, and F1-score for each class.

Bases: SimpleClass, DataExportMixin

Class for computing classification metrics including top-1 and top-5 accuracy.

Return mean of top-1 and top-5 accuracies as fitness score.

Return a dictionary with model's performance metrics and fitness score.

Return a list of keys for the results_dict property.

Return a list of curves for accessing specific metrics curves.

Return a list of curves for accessing specific metrics curves.

Process target classes and predicted classes to compute metrics.

Generate a single-row summary of classification metrics (Top-1 and Top-5 accuracy).

Metrics for evaluating oriented bounding box (OBB) detection.

Calculate the intersection over box2 area given box1 and box2.

Calculate intersection-over-union (IoU) of boxes.

Calculate the Intersection over Union (IoU) between bounding boxes.

This function supports various shapes for box1 and box2 as long as the last dimension is 4. For instance, you may pass tensors shaped like (4,), (N, 4), (B, N, 4), or (B, N, 1, 4). Internally, the code will split the last dimension into (x, y, w, h) if xywh=True, or (x1, y1, x2, y2) if xywh=False.

Calculate Object Keypoint Similarity (OKS).

Generate covariance matrix from oriented bounding boxes.

Calculate probabilistic IoU between oriented bounding boxes.

OBB format: [center_x, center_y, width, height, rotation_angle].

References: https://arxiv.org/pdf/2106.06072v1.pdf

Calculate the probabilistic IoU between oriented bounding boxes.

Compute smoothed positive and negative Binary Cross-Entropy targets.

Box filter of fraction f.

Plot precision-recall curve.

Plot metric-confidence curve.

Compute the average precision (AP) given the recall and precision curves.

Compute the average precision per class for object detection evaluation.

**Examples:**

Example 1 (typescript):
```typescript
ConfusionMatrix(self, names: dict[int, str] = {}, task: str = "detect", save_matches: bool = False)
```

Example 2 (python):
```python
class ConfusionMatrix(DataExportMixin):
    """A class for calculating and updating a confusion matrix for object detection and classification tasks.

    Attributes:
        task (str): The type of task, either 'detect' or 'classify'.
        matrix (np.ndarray): The confusion matrix, with dimensions depending on the task.
        nc (int): The number of classes.
        names (list[str]): The names of the classes, used as labels on the plot.
        matches (dict): Contains the indices of ground truths and predictions categorized into TP, FP and FN.
    """

    def __init__(self, names: dict[int, str] = {}, task: str = "detect", save_matches: bool = False):
        """Initialize a ConfusionMatrix instance.

        Args:
            names (dict[int, str], optional): Names of classes, used as labels on the plot.
            task (str, optional): Type of task, either 'detect' or 'classify'.
            save_matches (bool, optional): Save the indices of GTs, TPs, FPs, FNs for visualization.
        """
        self.task = task
        self.nc = len(names)  # number of classes
        self.matrix = np.zeros((self.nc, self.nc)) if self.task == "classify" else np.zeros((self.nc + 1, self.nc + 1))
        self.names = names  # name of classes
        self.matches = {} if save_matches else None
```

Example 3 (python):
```python
def _append_matches(self, mtype: str, batch: dict[str, Any], idx: int) -> None
```

Example 4 (python):
```python
def _append_matches(self, mtype: str, batch: dict[str, Any], idx: int) -> None:
    """Append the matches to TP, FP, FN or GT list for the last batch.

    This method updates the matches dictionary by appending specific batch data to the appropriate match type (True
    Positive, False Positive, or False Negative).

    Args:
        mtype (str): Match type identifier ('TP', 'FP', 'FN' or 'GT').
        batch (dict[str, Any]): Batch data containing detection results with keys like 'bboxes', 'cls', 'conf',
            'keypoints', 'masks'.
        idx (int): Index of the specific detection to append from the batch.

    Notes:
        For masks, handles both overlap and non-overlap cases. When masks.max() > 1.0, it indicates
        overlap_mask=True with shape (1, H, W), otherwise uses direct indexing.
    """
    if self.matches is None:
        return
    for k, v in batch.items():
        if k in {"bboxes", "cls", "conf", "keypoints"}:
            self.matches[mtype][k] += v[[idx]]
        elif k == "masks":
            # NOTE: masks.max() > 1.0 means overlap_mask=True with (1, H, W) shape
            self.matches[mtype][k] += [v[0] == idx + 1] if v.max() > 1.0 else [v[idx]]
```

---

## YOLO11 与 YOLOv9：全面技术比较

**URL:** https://docs.ultralytics.com/zh/compare/yolo11-vs-yolov9/

**Contents:**
- YOLO11 与 YOLOv9：全面技术比较
- Ultralytics YOLO11：生产级 AI 的标准
  - 架构与特性
  - 生产中的优势
- YOLOv9：解决信息瓶颈
  - 架构创新
  - 性能与权衡
- 性能指标：速度对比准确性
  - 分析
- 可用性与生态系统

在飞速发展的计算机视觉领域，选择正确的物体检测模型对项目的成功至关重要。本比较探讨了 Ultralytics YOLO11和 YOLOv9 之间的技术细节。 YOLOv9之间的技术比较。我们分析了它们的架构差异、性能指标以及对不同部署场景的适用性。

由Glenn Jocher和Jing Qiu在Ultralytics于2024年9月27日发布，YOLO11代表了在高效神经网络设计方面广泛研发的结晶。与通常将理论指标置于实际可用性之上的学术模型不同，YOLO11旨在为开发人员和企业提供速度、准确性和资源效率的最佳平衡。

YOLO11 引入了精炼的架构，增强了特征提取能力，同时保持了紧凑的尺寸。它采用了改进的骨干网络和颈部结构，专门设计用于以更少的参数捕获复杂模式，与 YOLOv8 等前几代模型相比。这种设计理念确保 YOLO11 模型在资源受限的硬件（例如边缘设备）上表现出色，而不会牺牲 detect 能力。

YOLO11 的一个突出特点是其原生的多功能性。虽然许多模型严格来说是目标检测器，但 YOLO11 在单一框架内支持广泛的计算机视觉任务：

对于开发者而言，YOLO11 的主要优势在于其与 Ultralytics 生态系统的集成。这通过简洁的 Python API 和全面的 CLI 确保了流畅的用户体验。

YOLO11 大幅缩短了 AI 解决方案的“上市时间”。其在训练和推理过程中较低的内存需求使其适用于更广泛的硬件，避免了与基于 Transformer 的替代方案相关的高昂 VRAM 成本。

由王建尧和廖弘源于 2024 年初推出，YOLOv9 专注于解决深度学习理论挑战，特别是信息瓶颈问题。它证明了学术严谨性，突破了特征保存的极限。

YOLOv9围绕两个核心概念构建：可编程梯度信息 (PGI)和广义高效层聚合网络 (GELAN)。PGI旨在保留输入信息在通过深层时的完整性，为损失函数计算可靠的梯度。GELAN优化了参数利用率，使模型能够在COCO dataset上，相对于其规模实现高精度。

YOLOv9 在原始精度基准测试中表现出色，其最大变体 YOLOv9-E 取得了令人印象深刻的 mAP 分数。然而，这种学术上的侧重可能会导致部署的复杂性更高。尽管功能强大，但原始实现缺乏 Ultralytics 框架中固有的多任务通用性，主要侧重于 detect。此外，与 YOLO11 的高度优化管道相比，训练这些架构可能需要更多的资源。

在选择模型时，了解推理速度和检测精度之间的权衡至关重要。下表对比了这两个模型系列在 COCO 数据集上的性能。

数据突出显示了 YOLO11 中设计的性能平衡。

在“软技能”方面——易用性、文档和支持——Ultralytics模型真正脱颖而出。

YOLO11 的设计易于使用。在标准的 Python 环境中，您只需几行代码即可训练、验证和部署模型。Ultralytics 提供了预训练权重，支持迁移学习，显著减少了训练时间和 AI 开发的碳足迹。

相比之下，尽管 YOLOv9 在 Ultralytics 包中可用，但其原始研究代码库需要对深度学习配置有更深入的理解。YOLO11 用户受益于统一的接口，无论您是执行 segmentation 还是 classification，该接口都以相同的方式工作。

使用 Ultralytics python API 训练 YOLO11 模型非常简单。

选择 YOLO11 意味着进入一个受支持的环境。Ultralytics 生态系统包括：

YOLO11 因其多功能性和速度，是 95% 商业和业余项目的推荐选择。

YOLOv9最适合特定的学术或高精度场景。

尽管 YOLOv9 为学术界引入了 PGI 和 GELAN 等引人入胜的概念，但 Ultralytics YOLO11 作为构建 AI 产品的卓越实用选择脱颖而出。它在速度、准确性、多功能性和易用性方面的无与伦比的结合，使其成为现代 computer vision 的首选模型。凭借强大的生态系统和为效率而设计，YOLO11 使开发人员能够自信地从概念走向部署。

如果您对进一步比较感兴趣，可以探索 Ultralytics 库中的其他这些高性能模型：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLO11 model
model = YOLO("yolo11n.pt")

# Train on a custom dataset
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference
results = model("path/to/image.jpg")
```

---

## YOLOv10 对比 YOLOv9：全面技术比较

**URL:** https://docs.ultralytics.com/zh/compare/yolov10-vs-yolov9/

**Contents:**
- YOLOv10 对比 YOLOv9：全面技术比较
- YOLOv10：端到端实时检测器
  - 架构与关键创新
- YOLOv9：优化信息保留
  - 架构与关键创新
- 性能指标：速度对比准确性
- 理想用例
  - 何时选择 YOLOv10
  - 何时选择 YOLOv9
- 与Ultralytics一起使用

目标检测领域发展迅速，YOLO（You Only Look Once）架构的迭代版本不断突破速度和准确性的极限。YOLOv10和YOLOv9是该领域最重要的近期贡献。尽管这两种模型都在COCO数据集上实现了最先进的性能，但它们在设计理念和架构目标上存在显著差异。

YOLOv10 通过消除对非极大值抑制 (NMS) 的需求，优先实现低延迟和端到端效率，而 YOLOv9 则侧重于通过可编程梯度信息 (PGI) 最大化信息保留和精度。本指南提供了详细的技术比较，以帮助开发人员和研究人员为其计算机视觉应用选择最佳模型。

由清华大学研究人员于2024年5月发布，YOLOv10代表了YOLO系列中的范式转变。其主要创新是移除了非极大值抑制（NMS）后处理步骤，这一步骤传统上一直是推理延迟的瓶颈。

YOLOv10通过结合一致性双重分配和整体效率-精度驱动的模型设计来实现其效率。

YOLOv10中移除NMS对于边缘部署尤其有利。在CPU资源稀缺的设备上，避免对数千个候选框进行排序和过滤的计算成本可以带来显著的加速。

由王建尧和廖弘源于 2024 年 2 月推出，YOLOv9 旨在解决深度神经网络中固有的“信息瓶颈”问题。当数据通过连续层（特征提取）时，关键信息可能会丢失，导致精度下降，特别是对于小型或难以 detect 的目标。

YOLOv9 引入了新颖的概念，以确保网络尽可能多地保留和利用输入信息。

YOLOv9 对信息保留的关注使其在复杂场景中 detect 物体方面表现出色，在这些场景中，特征细节在骨干网络的下采样操作中可能会丢失。

这两种模型之间的选择通常归结为原始推理速度和检测精度之间的权衡。下表强调了不同模型规模下的性能差异。

理解每个模型的优势有助于为您的特定项目目标选择合适的工具。

使用这些模型的一个显著优势是它们与Ultralytics生态系统的集成。YOLOv10和YOLOv9都可以通过相同的统一Python API和命令行界面（CLI）进行使用，从而简化了从训练到部署的工作流程。

以下代码演示了如何使用以下方法加载并运行这两个模型的推理 ultralytics 软件包。

为您的计算机视觉项目选择 Ultralytics 不仅在模型架构方面，还提供了多项优势：

YOLOv10和YOLOv9都代表了目标检测领域的重要里程碑。YOLOv10凭借其创新的无NMS架构，成为优先考虑速度和效率的应用的明显赢家。相反，YOLOv9仍然是要求尽可能高的准确性和信息保留的场景的稳健选择。

对于寻求最新、最多功能解决方案的开发者而言，我们也推荐探索 YOLO11。YOLO11 建立在这些前代模型的优势之上，为 detect、segment 和姿势估计任务提供了速度、准确性和功能的精细平衡。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a YOLOv10 model (NMS-free, high speed)
model_v10 = YOLO("yolov10n.pt")

# Load a YOLOv9 model (High accuracy)
model_v9 = YOLO("yolov9c.pt")

# Run inference on an image
# The API remains consistent regardless of the underlying architecture
results_v10 = model_v10("https://ultralytics.com/images/bus.jpg")
results_v9 = model_v9("https://ultralytics.com/images/bus.jpg")

# Print results
for r in results_v10:
    print(f"YOLOv10 Detections: {r.boxes.shape[0]}")

for r in results_v9:
    print(f"YOLOv9 Detections: {r.boxes.shape[0]}")
```

---

## Reference for ultralytics/trackers/utils/matching.py

**URL:** https://docs.ultralytics.com/zh/reference/trackers/utils/matching/

**Contents:**
- Reference for ultralytics/trackers/utils/matching.py
- function ultralytics.trackers.utils.matching.linear_assignment
- function ultralytics.trackers.utils.matching.iou_distance
- function ultralytics.trackers.utils.matching.embedding_distance
- function ultralytics.trackers.utils.matching.fuse_score

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/trackers/utils/matching.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Perform linear assignment using either the scipy or lap.lapjv method.

Compute cost based on Intersection over Union (IoU) between tracks.

Compute distance between tracks and detections based on embeddings.

Fuse cost matrix with detection scores to produce a single similarity matrix.

**Examples:**

Example 1 (typescript):
```typescript
def linear_assignment(cost_matrix: np.ndarray, thresh: float, use_lap: bool = True)
```

Example 2 (unknown):
```unknown
>>> cost_matrix = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
>>> thresh = 5.0
>>> matched_indices, unmatched_a, unmatched_b = linear_assignment(cost_matrix, thresh, use_lap=True)
```

Example 3 (sql):
```sql
def linear_assignment(cost_matrix: np.ndarray, thresh: float, use_lap: bool = True):
    """Perform linear assignment using either the scipy or lap.lapjv method.

    Args:
        cost_matrix (np.ndarray): The matrix containing cost values for assignments, with shape (N, M).
        thresh (float): Threshold for considering an assignment valid.
        use_lap (bool): Use lap.lapjv for the assignment. If False, scipy.optimize.linear_sum_assignment is used.

    Returns:
        matched_indices (list[list[int]] | np.ndarray): Matched indices of shape (K, 2), where K is the number of
            matches.
        unmatched_a (np.ndarray): Unmatched indices from the first set, with shape (L,).
        unmatched_b (np.ndarray): Unmatched indices from the second set, with shape (M,).

    Examples:
        >>> cost_matrix = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
        >>> thresh = 5.0
        >>> matched_indices, unmatched_a, unmatched_b = linear_assignment(cost_matrix, thresh, use_lap=True)
    """
    if cost_matrix.size == 0:
        return np.empty((0, 2), dtype=int), tuple(range(cost_matrix.shape[0])), tuple(range(cost_matrix.shape[1]))

    if use_lap:
        # Use lap.lapjv
        # https://github.com/gatagat/lap
        _, x, y = lap.lapjv(cost_matrix, extend_cost=True, cost_limit=thresh)
        matches = [[ix, mx] for ix, mx in enumerate(x) if mx >= 0]
        unmatched_a = np.where(x < 0)[0]
        unmatched_b = np.where(y < 0)[0]
    else:
        # Use scipy.optimize.linear_sum_assignment
        # https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.linear_sum_assignment.html
        x, y = scipy.optimize.linear_sum_assignment(cost_matrix)  # row x, col y
        matches = np.asarray([[x[i], y[i]] for i in range(len(x)) if cost_matrix[x[i], y[i]] <= thresh])
        if len(matches) == 0:
            unmatched_a = list(np.arange(cost_matrix.shape[0]))
            unmatched_b = list(np.arange(cost_matrix.shape[1]))
        else:
            unmatched_a = list(frozenset(np.arange(cost_matrix.shape[0])) - frozenset(matches[:, 0]))
            unmatched_b = list(frozenset(np.arange(cost_matrix.shape[1])) - frozenset(matches[:, 1]))

    return matches, unmatched_a, unmatched_b
```

Example 4 (python):
```python
def iou_distance(atracks: list, btracks: list) -> np.ndarray
```

---

## RTDETRv2 与 PP-YOLOE+：Transformer 和 CNN 的技术比较

**URL:** https://docs.ultralytics.com/zh/compare/rtdetr-vs-pp-yoloe/

**Contents:**
- RTDETRv2 与 PP-YOLOE+：Transformer 和 CNN 的技术比较
- RTDETRv2：Transformer 演进
  - 架构和主要特性
  - 优势与劣势
- PP-YOLOE+：无锚框CNN的强大模型
  - 架构和主要特性
  - 优势与劣势
- 性能分析：准确性与效率
- Ultralytics 优势：开发者为何选择 YOLO11
  - Ultralytics 模型的主要优势

目标检测领域已显著发展，分支为独特的架构理念。一方面，我们有卷积神经网络 (CNN) 已有的效率；另一方面，则是视觉Transformer (ViT) 新兴的力量。本文将探讨百度开发的两个著名模型：RTDETRv2（实时检测Transformer v2）和PP-YOLOE+。

尽管 PP-YOLOE+ 代表了 PaddlePaddle 生态系统中基于 CNN 的精炼无锚点 detect 的巅峰，但 RTDETRv2 则通过将 Transformer 架构应用于实时应用来突破界限。理解这两者之间的细微差别——从它们的 神经网络 设计到部署要求——对于工程师选择适合其 计算机视觉 项目的工具至关重要。

RTDETRv2 建立在原始 RT-DETR 的成功之上，旨在解决通常与基于 DETR 的模型相关的高计算成本问题，同时保留其卓越的全局上下文理解能力。它旨在弥合 Transformer 的高准确性与实时推理所需速度之间的差距。

RTDETRv2 采用混合编码器，可高效处理多尺度特征。与严重依赖局部卷积的传统 CNN 不同，Transformer 架构利用自注意力机制来捕获图像中的长距离依赖关系。一项关键创新是 IoU 感知查询选择，它改进了对象查询的初始化，从而实现更快的收敛和更高的准确性。此外，它消除了对非极大值抑制 (NMS) 后处理的需求，使整个流程真正实现端到端。

PP-YOLOE+ 是专门为 PaddlePaddle 框架开发的 YOLO 系列的演进版本。它专注于实际部署，利用纯 CNN 架构优化推理速度和 detect 精度之间的权衡。

PP-YOLOE+ 采用CSPRepResNet 骨干网络和路径聚合网络（PAN）颈部。至关重要的是，它使用无anchor头部，通过消除对预定义anchor boxes的需求来简化设计。该模型采用任务对齐学习（TAL），这是一种动态标签分配策略，可确保分类和定位任务良好同步，从而提高最终预测的质量。

RTDETRv2 和 PP-YOLOE+ 之间的选择通常取决于部署环境的具体限制。如果硬件允许更高的计算开销，RTDETRv2 提供卓越的检测能力。相反，对于严格受限的实时推理场景，PP-YOLOE+ 仍然是一个强有力的竞争者。

尽管探索RTDETRv2和PP-YOLOE+等模型能深入了解不同的架构方法，但大多数开发者需要一个在性能、可用性和生态系统支持之间取得平衡的解决方案。这正是Ultralytics YOLO11的优势所在。

Ultralytics YOLO11 不仅仅是一个模型；它是一个综合性视觉AI框架的一部分，旨在简化整个机器学习操作 (MLOps)生命周期。

训练 Transformer 模型通常需要配备 24GB 以上显存的高端 GPU。相比之下，Ultralytics YOLO11 模型经过高度优化，通常可以在仅有 8GB 显存的标准 GPU 上进行微调，大幅降低了开发人员和初创公司的入门门槛。

以下代码演示了使用Ultralytics Python API训练和部署模型是多么轻松，突出了与更复杂的学术代码库相比，其用户友好的设计。

在 RTDETRv2、PP-YOLOE+ 和 Ultralytics YOLO11 之间做出选择时，应根据您的具体应用需求来指导决策。

为了进一步了解这些架构如何与其他领先解决方案竞争，请查阅这些详细的比较：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLO11 model
model = YOLO("yolo11n.pt")

# Train the model on your custom dataset
# This handles data loading, augmentation, and logging automatically
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference on an image
# Returns a list of Result objects with boxes, masks, keypoints, etc.
results = model("path/to/image.jpg")

# Export the model to ONNX for deployment
model.export(format="onnx")
```

---

## Reference for ultralytics/data/annotator.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/data/annotator/

**Contents:**
- Reference for ultralytics/data/annotator.py
- function ultralytics.data.annotator.auto_annotate

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/data/annotator.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Automatically annotate images using a YOLO object detection model and a SAM segmentation model.

This function processes images in a specified directory, detects objects using a YOLO model, and then generates segmentation masks using a SAM model. The resulting annotations are saved as text files in YOLO format.

**Examples:**

Example 1 (typescript):
```typescript
def auto_annotate(
    data: str | Path,
    det_model: str = "yolo11x.pt",
    sam_model: str = "sam_b.pt",
    device: str = "",
    conf: float = 0.25,
    iou: float = 0.45,
    imgsz: int = 640,
    max_det: int = 300,
    classes: list[int] | None = None,
    output_dir: str | Path | None = None,
) -> None
```

Example 2 (sql):
```sql
>>> from ultralytics.data.annotator import auto_annotate
>>> auto_annotate(data="ultralytics/assets", det_model="yolo11n.pt", sam_model="mobile_sam.pt")
```

Example 3 (python):
```python
def auto_annotate(
    data: str | Path,
    det_model: str = "yolo11x.pt",
    sam_model: str = "sam_b.pt",
    device: str = "",
    conf: float = 0.25,
    iou: float = 0.45,
    imgsz: int = 640,
    max_det: int = 300,
    classes: list[int] | None = None,
    output_dir: str | Path | None = None,
) -> None:
    """Automatically annotate images using a YOLO object detection model and a SAM segmentation model.

    This function processes images in a specified directory, detects objects using a YOLO model, and then generates
    segmentation masks using a SAM model. The resulting annotations are saved as text files in YOLO format.

    Args:
        data (str | Path): Path to a folder containing images to be annotated.
        det_model (str): Path or name of the pre-trained YOLO detection model.
        sam_model (str): Path or name of the pre-trained SAM segmentation model.
        device (str): Device to run the models on (e.g., 'cpu', 'cuda', '0'). Empty string for auto-selection.
        conf (float): Confidence threshold for detection model.
        iou (float): IoU threshold for filtering overlapping boxes in detection results.
        imgsz (int): Input image resize dimension.
        max_det (int): Maximum number of detections per image.
        classes (list[int], optional): Filter predictions to specified class IDs, returning only relevant detections.
        output_dir (str | Path, optional): Directory to save the annotated results. If None, creates a default directory
            based on the input data path.

    Examples:
        >>> from ultralytics.data.annotator import auto_annotate
        >>> auto_annotate(data="ultralytics/assets", det_model="yolo11n.pt", sam_model="mobile_sam.pt")
    """
    det_model = YOLO(det_model)
    sam_model = SAM(sam_model)

    data = Path(data)
    if not output_dir:
        output_dir = data.parent / f"{data.stem}_auto_annotate_labels"
    Path(output_dir).mkdir(exist_ok=True, parents=True)

    det_results = det_model(
        data, stream=True, device=device, conf=conf, iou=iou, imgsz=imgsz, max_det=max_det, classes=classes
    )

    for result in det_results:
        if class_ids := result.boxes.cls.int().tolist():  # Extract class IDs from detection results
            boxes = result.boxes.xyxy  # Boxes object for bbox outputs
            sam_results = sam_model(result.orig_img, bboxes=boxes, verbose=False, save=False, device=device)
            segments = sam_results[0].masks.xyn

            with open(f"{Path(output_dir) / Path(result.path).stem}.txt", "w", encoding="utf-8") as f:
                for i, s in enumerate(segments):
                    if s.any():
                        segment = map(str, s.reshape(-1).tolist())
                        f.write(f"{class_ids[i]} " + " ".join(segment) + "\n")
```

---

## DAMO-YOLO 对比 YOLOv10：深入探讨目标检测的演进

**URL:** https://docs.ultralytics.com/zh/compare/damo-yolo-vs-yolov10/

**Contents:**
- DAMO-YOLO 对比 YOLOv10：深入探讨目标检测的演进
- 性能指标
- DAMO-YOLO：研究驱动的创新
  - 架构和主要特性
  - 优势与劣势
- YOLOv10：端到端实时检测时代
  - 架构与创新
  - Ultralytics的易用性
- 对比分析
  - 速度与延迟

选择合适的目标检测模型是一个关键决策，它影响从部署成本到用户体验方方面面。本次技术比较探讨了阿里巴巴集团推出的研究驱动型模型DAMO-YOLO与由清华大学研究人员开发并集成到Ultralytics生态系统中的最新实时端到端检测器YOLOv10之间的差异。

尽管这两个模型都旨在优化速度和准确性之间的权衡，但它们采用了截然不同的架构策略。本分析深入探讨了它们的技术规范、性能指标和理想用例，以帮助您驾驭复杂的 computer vision 领域。

下表直接比较了COCO数据集上的效率和准确性。主要亮点包括参数效率和推理速度，其中YOLOv10由于其无NMS设计而展现出显著优势。

DAMO-YOLO于2022年末发布，代表了阿里巴巴集团的一项重大努力，旨在通过先进的神经网络架构搜索和新颖的特征融合技术，突破YOLO风格检测器的界限。

技术细节：作者：Xianzhe Xu、Yiqi Jiang、Weihua Chen 等组织：阿里巴巴集团日期：2022-11-23Arxiv：https://arxiv.org/abs/2211.15444v2GitHub：https://github.com/tinyvision/DAMO-YOLO

DAMO-YOLO 集成了多项前沿概念以实现其性能：

尽管DAMO-YOLO引入了令人印象深刻的创新，但其对NAS和专用组件的依赖可能会使训练流程更加复杂，对于需要快速定制或在不同硬件上部署而无需大量调优的开发者来说，不易上手。

YOLOv10 于 2024 年 5 月由清华大学研究人员发布，代表了 YOLO 系列的一个范式转变。通过消除对非极大值抑制 (NMS) 的需求，它实现了真正的端到端性能，显著降低了推理延迟。

技术细节：作者：王傲、陈辉、刘力豪 等组织：清华大学日期：2024-05-23Arxiv：https://arxiv.org/abs/2405.14458GitHub：https://github.com/THU-MIG/yolov10文档：https://docs.ultralytics.com/models/yolov10/

YOLOv10 专注于整体效率，同时针对架构和后处理流程：

YOLOv10 最显著的优势之一是它与Ultralytics 生态系统的无缝集成。开发者可以使用与 YOLOv8 和 YOLO11 相同的简单 API 来训练、验证和部署 YOLOv10。

在比较 DAMO-YOLO 和 YOLOv10 时，区别在于它们在效率方法和操作生态系统上的不同。

YOLOv10 在实际应用延迟方面具有显著优势。标准的 YOLO 模型（以及 DAMO-YOLO）需要非极大值抑制 (NMS)来过滤重叠的边界框。NMS 的执行时间随检测到的对象数量而变化，导致不可预测的延迟。YOLOv10 的端到端设计提供了确定性延迟，使其在时间敏感型应用中表现更优，例如自动驾驶或高速工业机器人。

如性能表所示，YOLOv10s实现了比DAMO-YOLO-S（46.0%）更高的mAP（46.7%），同时使用的参数量不到其一半（7.2M vs 16.3M）。这种减少的内存占用对于边缘部署至关重要。Ultralytics模型以其在训练和推理期间较低的内存需求而闻名，使得在消费级GPU上进行训练成为可能，而其他架构可能会遇到内存不足（OOM）错误。

尽管DAMO-YOLO是一个扎实的学术贡献，但YOLOv10受益于维护良好的Ultralytics生态系统。这包括：

如果您的项目需要超越边界框的多功能性——例如实例分割、姿势估计或旋转框检测 (obb)——可以考虑探索YOLO11或YOLOv8。虽然YOLOv10在纯检测方面表现出色，但更广泛的Ultralytics系列为这些复杂的任务需求提供了最先进的解决方案。

两种模型都代表了计算机视觉领域的里程碑。DAMO-YOLO 在2022年展示了 NAS 和高级特征融合的强大能力。然而，对于2024年及以后的现代应用，YOLOv10 提供了更具吸引力的方案。其 NMS-free 端到端架构解决了目标 detect 领域长期存在的瓶颈，同时其与 Ultralytics 生态系统的集成确保了其易于访问、维护和部署。

对于寻求速度、准确性和易用性最佳平衡的开发者而言，YOLOv10——以及多功能的 YOLO11——是构建强大AI解决方案的卓越选择。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv10n model
model = YOLO("yolov10n.pt")

# Train the model on your custom dataset
model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference on an image
results = model("path/to/image.jpg")
```

---

## Reference for ultralytics/solutions/distance_calculation.py

**URL:** https://docs.ultralytics.com/zh/reference/solutions/distance_calculation/

**Contents:**
- Reference for ultralytics/solutions/distance_calculation.py
- class ultralytics.solutions.distance_calculation.DistanceCalculation
  - method ultralytics.solutions.distance_calculation.DistanceCalculation.mouse_event_for_distance
  - method ultralytics.solutions.distance_calculation.DistanceCalculation.process

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/solutions/distance_calculation.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class to calculate distance between two objects in a real-time video stream based on their tracks.

This class extends BaseSolution to provide functionality for selecting objects and calculating the distance between them in a video stream using YOLO object detection and tracking.

Handle mouse events to select regions in a real-time video stream for distance calculation.

Process a video frame and calculate the distance between two selected bounding boxes.

This method extracts tracks from the input frame, annotates bounding boxes, and calculates the distance between two user-selected objects if they have been chosen.

**Examples:**

Example 1 (rust):
```rust
DistanceCalculation(self, **kwargs: Any) -> None
```

Example 2 (unknown):
```unknown
>>> distance_calc = DistanceCalculation()
>>> frame = cv2.imread("frame.jpg")
>>> results = distance_calc.process(frame)
>>> cv2.imshow("Distance Calculation", results.plot_im)
>>> cv2.waitKey(0)
```

Example 3 (python):
```python
class DistanceCalculation(BaseSolution):
    """A class to calculate distance between two objects in a real-time video stream based on their tracks.

    This class extends BaseSolution to provide functionality for selecting objects and calculating the distance between
    them in a video stream using YOLO object detection and tracking.

    Attributes:
        left_mouse_count (int): Counter for left mouse button clicks.
        selected_boxes (dict[int, Any]): Dictionary to store selected bounding boxes keyed by track ID.
        centroids (list[list[int]]): List to store centroids of selected bounding boxes.

    Methods:
        mouse_event_for_distance: Handle mouse events for selecting objects in the video stream.
        process: Process video frames and calculate the distance between selected objects.

    Examples:
        >>> distance_calc = DistanceCalculation()
        >>> frame = cv2.imread("frame.jpg")
        >>> results = distance_calc.process(frame)
        >>> cv2.imshow("Distance Calculation", results.plot_im)
        >>> cv2.waitKey(0)
    """

    def __init__(self, **kwargs: Any) -> None:
        """Initialize the DistanceCalculation class for measuring object distances in video streams."""
        super().__init__(**kwargs)

        # Mouse event information
        self.left_mouse_count = 0
        self.selected_boxes: dict[int, list[float]] = {}
        self.centroids: list[list[int]] = []  # Store centroids of selected objects
```

Example 4 (python):
```python
def mouse_event_for_distance(self, event: int, x: int, y: int, flags: int, param: Any) -> None
```

---

## YOLOv7 vs YOLOv5：详细技术比较

**URL:** https://docs.ultralytics.com/zh/compare/yolov7-vs-yolov5/

**Contents:**
- YOLOv7 vs YOLOv5：详细技术比较
- 性能指标比较
- YOLOv7: 突破准确性极限
  - 架构与关键创新
  - 适用于 YOLOv7 的理想用例
- Ultralytics YOLOv5：行业标准
  - 架构与生态系统效益
  - 适用于 YOLOv5 的理想用例
- 详细比较分析
  - 1. 速度与准确性之间的权衡

选择合适的目标检测架构是一个关键决策，它会影响您的计算机视觉项目的速度、准确性和部署可行性。本页面对 YOLOv7 和 Ultralytics YOLOv5 这两个 YOLO 系列中有影响力的模型进行了全面的技术比较。我们深入探讨了它们的架构创新、性能基准和理想用例，以帮助您为应用程序选择最合适的模型。

虽然YOLOv7在2022年引入了重大的学术进展，但Ultralytics YOLOv5因其无与伦比的易用性、鲁棒性和部署灵活性，在行业中仍占据主导地位。对于寻求最新性能的用户，我们还将探讨这些模型如何为尖端的Ultralytics YOLO11铺平道路。

下表突出了两种架构之间的性能权衡。尽管 YOLOv7 追求更高的平均精度均值 (mAP)，但 YOLOv5 在推理速度和特定模型尺寸的更少参数数量方面具有明显优势。

YOLOv7于2022年7月发布，旨在为实时目标检测器树立新的技术标杆。它着重于架构优化，以提高准确性，同时不显著增加推理成本。

作者：王建尧、Alexey Bochkovskiy、廖鸿源Chien-Yao Wang, Alexey Bochkovskiy, and Hong-Yuan Mark Liao组织：台湾中央研究院信息科学研究所日期：2022-07-06Arxiv:https://arxiv.org/abs/2207.02696 GitHub:https://github.com/WongKinYiu/yolov7Docs:ultralytics

YOLOv7 引入了几项复杂的架构改进，旨在改进特征学习：

“免费赠品袋”指的是一系列训练方法和数据增强技术，可以在不增加推理成本的情况下提高目标检测模型的准确性。在YOLOv7中，这包括诸如由粗到精引导标签分配等复杂策略。

由于其对高精度的侧重，YOLOv7特别适用于：

Ultralytics YOLOv5 被广泛认为是现有的最实用、最用户友好的目标检测模型之一。自 2020 年发布以来，凭借其在速度、准确性和卓越工程方面的平衡，它已成为无数商业应用的核心。

作者: Glenn Jocher机构:Ultralytics日期: 2020-06-26GitHub:https://github.com/ultralytics/yolov5文档:https://docs.ultralytics.com/models/yolov5/

YOLOv5 利用 CSP-Darknet53 主干网络、PANet 颈部网络和 YOLOv3 检测头，针对多样化的部署目标进行了优化。然而，其真正的优势在于Ultralytics 生态系统：

YOLOv5 在需要可靠性和速度的实际场景中表现出色：

在 YOLOv7 和 YOLOv5 之间做出选择时，除了 mAP 分数之外，还有几个技术因素需要考虑。

YOLOv7在COCO数据集上实现了更高的峰值精度。例如，YOLOv7x达到了53.1%的mAP，而YOLOv5x为50.7%。然而，这以复杂性为代价。YOLOv5提供了更平滑的模型梯度；YOLOv5n（Nano）模型速度极快（73.6毫秒CPU速度）且轻量（2.6M参数），为YOLOv7未明确以相同粒度定位的超低资源环境创造了一个利基市场。

YOLOv7采用基于拼接的E-ELAN架构，这增加了训练期间所需的内存带宽。这可能导致其训练速度比YOLOv5慢，并且更耗内存。相比之下，Ultralytics YOLOv5使用流线型架构，针对训练效率进行了高度优化，实现了更快的收敛和更低的内存使用，这对于计算预算有限的工程师来说是一个显著优势。

这正是 Ultralytics YOLOv5 真正出彩的地方。Ultralytics 框架提供了一个统一的体验，并配备了强大的工具，用于数据增强、超参数演进和实验跟踪。

虽然YOLOv7拥有一个仓库，但它缺乏Ultralytics生态系统所支持的完善的、生产就绪的CI/CD管道、广泛的集成指南和社区支持。

尽管这两个模型主要是 object detection 架构，但围绕 YOLOv5 的 Ultralytics 生态系统已经发展到无缝支持 instance segmentation 和 image classification。YOLOv7 也支持这些任务，但通常需要不同的代码分支或分叉，而 Ultralytics 则提供了一种更统一的方法。

Ultralytics 模型开箱即用，支持多种导出格式。您可以使用简单的 CLI 命令或 Python 脚本，轻松将训练好的模型转换为适用于 Android 的 TFLite、适用于 iOS 的 CoreML 或适用于优化 GPU 推理的 TensorRT。

YOLOv7 和 YOLOv5 之间的选择取决于您的项目优先级：

尽管 YOLOv5 和 YOLOv7 是优秀的模型，但计算机视觉领域发展迅速。对于寻求两全其美的开发者——超越 YOLOv7 的准确性和 YOLOv5 的速度/可用性——我们强烈建议探索Ultralytics YOLO11。

YOLO11 代表了最新演进，采用了无锚点架构，简化了训练流程并提升了所有任务的性能，包括 detect、segment、姿势估计和 obb。

如果您有兴趣比较 YOLO 系列中的其他模型，请查看这些相关页面：

**Examples:**

Example 1 (python):
```python
import torch

# Example: Loading YOLOv5s from PyTorch Hub for inference
model = torch.hub.load("ultralytics/yolov5", "yolov5s", pretrained=True)

# Inference on an image
results = model("https://ultralytics.com/images/zidane.jpg")

# Print results
results.print()
```

---

## PP-YOLOE+ vs. EfficientDet：目标检测技术对比

**URL:** https://docs.ultralytics.com/zh/compare/pp-yoloe-vs-efficientdet/

**Contents:**
- PP-YOLOE+ vs. EfficientDet：目标检测技术对比
- PP-YOLOE+：速度与精度优化
  - 主要架构特性
- EfficientDet：可扩展效率
  - 主要架构特性
- 性能分析：速度 vs. 准确性
- 优势与劣势
  - PP-YOLOE+
  - EfficientDet
- 实际应用案例

选择合适的目标检测模型是一个关键决策，它会影响计算机视觉应用的性能、可扩展性和效率。在本次技术比较中，我们分析了两种杰出的架构：来自百度PaddlePaddle生态系统的高性能无锚点检测器PP-YOLOE+，以及Google的可扩展架构EfficientDet，后者以其复合缩放方法而闻名。

PP-YOLOE+ 代表了 YOLO 系列的重大演进，旨在实现精度和推理速度之间的最佳平衡。它基于无锚框范式构建，简化了检测流程，同时利用了任务对齐学习 (TAL) 等先进技术。

PP-YOLOE+ 集成了CSPRepResNet骨干网络，它结合了CSPNet的效率和ResNet的重参数化能力。这使得模型能够在不产生过多计算成本的情况下捕获丰富的特征表示。颈部利用路径聚合网络（PAN）进行有效的多尺度特征融合，确保以更高的可靠性 detect 小目标。

一个显著特点是高效任务对齐头部 (ET-Head)。与传统的耦合头部不同，ET-Head 将分类和定位任务解耦，利用 TAL 动态地将最佳锚点与真实对象对齐。这种方法显著提高了收敛速度和最终准确性。

EfficientDet 引入了一种新颖的模型缩放方法，专注于同时优化准确性和效率。它基于 EfficientNet 骨干网络，并引入了加权双向特征金字塔网络 (BiFPN)。

EfficientDet的核心创新是BiFPN，它实现了简单快速的多尺度特征融合。与之前平等求和特征的FPN不同，BiFPN为每个输入特征分配权重，使网络能够学习不同输入特征的重要性。此外，EfficientDet采用了一种复合缩放方法，统一缩放所有骨干网络、特征网络和边界框/类别预测网络的分辨率、深度和宽度，从而提供了一系列针对不同资源限制的模型（D0到D7）。

了解更多关于 EfficientDet 的信息

评估这些模型时，推理速度和平均精度 (mAP) 之间的权衡变得清晰。EfficientDet 在发布时设定了高标准，但像 PP-YOLOE+ 这样的新架构利用了硬件感知设计，在现代 GPU 上实现了卓越的性能。

数据突出显示，PP-YOLOE+ 在 GPU 推理延迟方面显著优于 EfficientDet。例如，PP-YOLOE+l 实现了比 EfficientDet-d6 (52.6) 更高的 mAP (52.9)，同时在 T4 GPU 上速度快了 10 倍以上 (8.36 毫秒 vs 89.29 毫秒)。EfficientDet 在 FLOPs 是主要约束的场景（例如极低功耗的移动 CPU）中仍具有相关性，但在高吞吐量服务器环境中难以竞争。

PP-YOLOE+ 的架构选择专门设计为对 TensorRT 等 GPU 硬件加速器友好。操作结构旨在最大化并行性，而 EfficientDet 的 BiFPN 中复杂的连接有时会在 GPU 上造成内存访问瓶颈。

理解每个模型的优缺点有助于为特定的计算机视觉任务选择合适的工具。

这些模型基于其架构优势在不同环境中表现出色。

制造与工业自动化: PP-YOLOE+ 是制造业质量控制的绝佳选择。其高推理速度使其能够在毫秒必争的快速移动装配线上进行实时缺陷检测。

智能零售与库存：对于零售分析，例如自动化结账或货架监控，PP-YOLOE+ 的准确性确保即使在杂乱的场景中也能正确识别产品。

遥感与航空影像：EfficientDet 能够扩展到更高分辨率（例如 D7），使其适用于分析高分辨率卫星或无人机图像，在这种场景下，处理速度不如detect大图像中的小特征那么关键。

低功耗边缘设备：对于总FLOPs是硬性限制且无法进行GPU加速的传统边缘AI硬件，有时会首选较小的EfficientDet变体（D0-D1）。

尽管PP-YOLOE+和EfficientDet提供了稳健的解决方案，但Ultralytics YOLO11模型为大多数开发者和研究人员提供了卓越的体验。它将现代架构创新的精华与以用户为中心的生态系统相结合。

只需几行代码即可加载预训练的 YOLO11 模型并运行推理，这展示了 Ultralytics 工作流程的简洁性。

PP-YOLOE+ 和 EfficientDet 都对 计算机视觉 领域做出了重大贡献。PP-YOLOE+ 对于深度集成到百度生态系统并需要高 GPU 吞吐量的用户来说，是一个强有力的竞争者。EfficientDet 仍然是参数效率和可扩展设计的经典范例。

然而，对于那些寻求多功能、高性能且开发人员友好的解决方案的人而言，Ultralytics YOLO11 是推荐选择。它结合了尖端精度、实时速度和支持性生态系统，使其成为构建下一代AI应用的理想平台。

如需进一步比较，请考虑探索 YOLO11 与 EfficientDet 或 PP-YOLOE+ 与 YOLOv10，以了解这些模型如何与其它最先进的架构进行比较。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLO11n model
model = YOLO("yolo11n.pt")

# Run inference on an image
results = model("path/to/image.jpg")

# Display the results
results[0].show()
```

---

## 性能指标深入分析 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/guides/yolo-performance-metrics/

**Contents:**
- 性能指标深度解析
- 简介
- 对象检测指标
- 如何计算 YOLO11 模型的指标
  - 解释输出
    - 按类别划分的指标
    - 速度指标
    - COCO 指标评估
    - 可视化输出
    - 结果存储

性能指标是评估准确性和目标检测模型效率的关键工具。它们揭示了模型在图像中识别和定位目标的有效程度。此外，它们还有助于理解模型对假正例和假负例的处理。这些见解对于评估和提高模型的性能至关重要。在本指南中，我们将探讨与 YOLO11 相关的各种性能指标、它们的意义以及如何解读它们。

观看： Ultralytics YOLO11 性能指标 | MAP、F1 分数、 精确度，IoU & 准确率

首先，让我们讨论一些不仅对 YOLO11 重要，而且广泛适用于不同目标检测模型的指标。

交并比 (IoU): IoU 是一种量化预测边界框与真实边界框之间重叠程度的度量。它在评估对象定位的准确性方面起着 фундаментальную роль.

平均精度 (AP)：AP 计算精度-召回曲线下的面积，提供一个单一值，概括了模型的精度和召回性能。

平均精度均值 (mAP): mAP 通过计算多个对象类别的平均 AP 值来扩展 AP 的概念。这在多类对象 detect 场景中非常有用，可以提供对模型性能的全面评估。

精度与召回率：精度量化了所有正向预测中真阳性的比例，评估模型避免假阳性的能力。另一方面，召回率计算了所有实际阳性中真阳性的比例，衡量模型检测某一类别所有实例的能力。

F1 分数： F1 分数是精度和召回率的调和平均值，在考虑假正例和假负例的同时，对模型的性能进行均衡评估。

现在，我们可以探索YOLO11 的验证模式，该模式可用于计算上述讨论的评估指标。

使用验证模式很简单。一旦你训练好了一个模型，你就可以调用 model.val() 函数。然后，此函数将处理验证数据集并返回各种性能指标。但是，这些指标是什么意思？你应该如何解读它们？

让我们分解 model.val() 函数的输出，并理解输出的每个部分。

输出的部分内容是性能指标的按类别细分。当您试图了解模型在每个特定类别上的表现时，此细粒度信息非常有用，尤其是在具有多种目标类别的数据集中。对于数据集中的每个类别，将提供以下信息：

类别：这表示目标类别的名称，例如“人”、“汽车”或“狗”。

图像：此指标告诉你验证集中包含目标类别的图像数量。

实例：这提供了该类别在验证集中所有图像中出现的次数。

Box(P, R, mAP50, mAP50-95)：此指标提供了模型在 detect 对象方面的性能洞察：

P (精确率): 检测到的物体的准确度，表示有多少检测结果是正确的。

R (召回率): 模型识别图像中所有物体实例的能力。

mAP50: 在交并比 (IoU) 阈值为 0.50 时计算的平均精度均值。它衡量了模型在仅考虑“简单”检测时的准确性。

mAP50-95: 在 0.50 到 0.95 范围内的不同 IoU 阈值下计算的平均精度均值。它全面反映了模型在不同检测难度下的性能。

推理速度与准确性同样重要，尤其是在实时对象检测场景中。本节将详细介绍验证过程中各个阶段所花费的时间，从预处理到后处理。

对于在 COCO 数据集上进行验证的用户，会使用 COCO 评估脚本计算额外的指标。这些指标提供了在不同 IoU 阈值和不同大小对象下的精度和召回率洞察。

除了生成数值指标外，model.val() 函数还会生成可视化输出，从而可以更直观地了解模型的性能。以下是您可以预期的可视化输出的详细信息：

F1 分数曲线 (F1_curve.png): 此曲线表示 F1 分数 在各种阈值上的表现。解释此曲线可以深入了解模型在不同阈值下假阳性和假阴性之间的平衡。

精确率-召回率曲线 (PR_curve.png): 作为任何分类问题不可或缺的可视化工具，此曲线展示了精确率和 召回率 在不同阈值下的权衡。在处理不平衡类别时，这一点尤其重要。

精确率曲线 (P_curve.png): 精确率值在不同阈值下的图形表示。此曲线有助于了解精确率如何随阈值变化而变化。

召回率曲线 (R_curve.png): 相应地，此图说明了召回率值如何在不同阈值上变化。

混淆矩阵 (confusion_matrix.png): 混淆矩阵提供了结果的详细视图，展示了每个类别的真阳性、真阴性、假阳性和假阴性的计数。

归一化混淆矩阵 (confusion_matrix_normalized.png): 此可视化是混淆矩阵的归一化版本。它以比例而不是原始计数来表示数据。此格式使得比较各个类别的性能变得更加简单。

验证批次标签 (val_batchX_labels.jpg): 这些图像描绘了来自验证数据集的不同批次的真实标签。它们清晰地展示了根据数据集，对象是什么及其各自的位置。

验证批次预测 (val_batchX_pred.jpg)与标签图像形成对比的是，这些可视化图像展示了 YOLO11 模型对相应批次所做的预测。通过将这些图像与标签图像进行比较，您可以轻松地评估模型在视觉上检测和分类物体的效果。

供将来参考，结果将保存到一个目录中，通常命名为 runs/detect/val。

IoU：在精确对象定位至关重要时不可或缺。

Precision: 当最大限度地减少错误检测是一项优先任务时，此指标非常重要。

召回率：在需要detect每个目标实例时至关重要。

F1 Score: 当需要在精确率和召回率之间取得平衡时，此指标非常有用。

对于实时应用，FPS（每秒帧数）和延迟等速度指标对于确保及时获得结果至关重要。

理解这些指标非常重要。以下是一些常见的较低分数可能表明的问题：

低mAP: 表明模型可能需要进行整体优化。

低IoU: 模型可能难以准确识别对象。不同的边界框方法可能会有所帮助。

Low Precision: 模型可能检测到过多的不存在的对象。调整置信度阈值可能会减少这种情况。

Low Recall: 模型可能遗漏了真实对象。改进 特征提取 或使用更多数据可能会有所帮助。

Imbalanced F1 Score: 精确率和召回率之间存在差异。

类别特定的 AP：此处的低分数可以突出模型难以处理的类别。

实际示例可以帮助阐明这些指标在实践中如何工作。

情况: mAP和F1分数不理想，但召回率良好，精确率不佳。

Interpretation & Action: 可能存在过多的不正确检测。收紧置信度阈值可以减少这些检测，尽管这也可能会略微降低召回率。

情况: mAP和召回率尚可，但IoU不足。

Interpretation & Action: 该模型可以很好地检测对象，但可能无法精确定位它们。改进边界框预测可能会有所帮助。

情况: 即使整体mAP表现尚可，某些类别的AP也远低于其他类别。

Interpretation & Action: 这些类别对于模型来说可能更具挑战性。为这些类别使用更多数据或在训练期间调整类别权重可能是有益的。

融入由爱好者和专家组成的社区可以扩大您使用 YOLO11 的旅程。以下是一些可以促进学习、故障排除和交流的途径。

GitHub Issues： GitHub 上的 YOLO11 仓库有一个 Issues 选项卡，您可以在其中提问、报告错误和建议新功能。社区和维护者会积极参与，这是一个获得特定问题帮助的好地方。

Ultralytics Discord 服务器： Ultralytics 有一个 Discord 服务器，您可以在其中与其他用户和开发人员互动。

利用这些资源不仅能指导您应对各种挑战，还能让您及时了解 YOLO11 社区的最新趋势和最佳实践。

在本指南中，我们仔细研究了 YOLO11 的关键性能指标。这些指标是了解模型性能的关键，对于任何想要微调其模型的人来说都至关重要。它们为改进提供了必要的见解，并确保模型在实际情况下有效工作。

请记住，YOLO11 和 Ultralytics 社区是一笔宝贵的财富。与开发人员和专家互动可以开启通往标准文档中找不到的见解和解决方案的大门。在您进行目标检测的过程中，保持学习的热情，尝试新的策略，并分享您的发现。通过这样做，您将为社区的集体智慧做出贡献，并确保其发展。

平均精度 (mAP) 对于评估 YOLO11 模型至关重要，因为它提供了一个单一指标，概括了多个类别的精度和召回率。mAP@0.50 衡量在 IoU 阈值为 0.50 时的精度，侧重于模型正确 detect 对象的能力。mAP@0.50:0.95 平均了 IoU 阈值范围内的精度，提供了对 detect 性能的全面评估。高 mAP 分数表明模型有效地平衡了精度和召回率，这对于 自动驾驶 和监控系统等应用至关重要，在这些应用中，准确 detect 和最小化误报都至关重要。

交并比 (IoU) 衡量预测边界框与真实边界框之间的重叠程度。IoU 值范围从 0 到 1，值越高表示定位精度越好。IoU 为 1.0 意味着完美对齐。通常，在 mAP 等指标中，IoU 阈值 0.50 用于定义真阳性。较低的 IoU 值表明模型在精确目标定位方面存在困难，这可以通过改进边界框回归或提高训练数据集中的标注精度来改善。

F1 Score 对于评估 YOLO11 模型非常重要，因为它提供了精确率和召回率的调和平均值，从而平衡了假正例和假负例。在处理不平衡的数据集或仅靠精确率或召回率不足的应用时，它尤其有价值。高 F1 Score 表明该模型有效地检测对象，同时最大限度地减少了漏检和误报，使其适用于安全系统和医学成像等关键应用。

Ultralytics YOLO11 为实时目标检测提供了多项优势：

这使得 YOLO11 非常适合从自动驾驶汽车到智慧城市解决方案的各种应用。

来自YOLO11的验证指标，如精确率、召回率、mAP和IoU，通过深入了解检测的不同方面，有助于诊断和改进模型性能：

通过分析这些指标，可以针对特定的弱点，例如调整置信度阈值以提高精度，或收集更多样化的数据以提高召回率。有关这些指标的详细解释以及如何解释它们，请查看目标检测指标，并考虑实施超参数调整以优化您的模型。

---

## PP-YOLOE+ 与 DAMO-YOLO：技术比较 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/compare/pp-yoloe-vs-damo-yolo/

**Contents:**
- PP-YOLOE+ 与 DAMO-YOLO：技术比较
- 性能指标比较
- PP-YOLOE+：Paddle 生态系统中的精炼精度
  - 架构与核心技术
  - 优势与劣势
- DAMO-YOLO：来自阿里巴巴的速度导向创新
  - 架构和主要特性
  - 优势与劣势
- Ultralytics 优势：为何 YOLO11 是卓越之选
  - 无与伦比的易用性和生态系统

选择最优的对象detect模型是开发高效计算机视觉应用的关键一步。它涉及在精度、推理延迟和硬件限制之间进行复杂的权衡。这项技术比较探讨了来自亚洲科技巨头的两个著名模型：PP-YOLOE+（由百度PaddlePaddle团队开发）和DAMO-YOLO（由阿里巴巴集团设计）。这两个模型都代表了实时detect器演进中的重大进步，提供了独特的架构创新和性能特征。

在分析这些模型时，考虑 vision AI 的更广阔前景是有益的。像 Ultralytics YOLO11 这样的解决方案提供了一个引人注目的替代方案，它专注于可用性和强大的、与框架无关的生态系统，同时提供最先进的性能。

下表直接比较了关键性能指标，包括平均精度均值 (mAP)、使用TensorRT在 T4 GPU 上的推理速度、参数量以及计算复杂度 (FLOPs)。

PP-YOLOE+ 是 PP-YOLOE 的演进版本，代表了百度旗舰级的单阶段无anchor detect 器。它于2022年作为PaddleDetection套件的一部分发布，强调高精度 detect，并为 PaddlePaddle 深度学习框架进行了深度优化。

PP-YOLOE+ 集成了多项先进组件，以简化 detect 流程并提升精度。

PP-YOLOE+ 原生于 PaddlePaddle。虽然在该环境中效率很高，但熟悉 PyTorch 的用户可能会发现过渡和工具（例如 paddle2onnx （用于导出）与原生 PyTorch 模型相比，需要额外的学习。

优势： PP-YOLOE+ 在优先考虑原始准确性的场景中表现出色。“中型”、“大型”和“超大型”变体在COCO 数据集上展示出强大的 mAP 分数，使其适用于工业质量控制等详细检测任务。

弱点： 主要限制在于其框架耦合性。其工具、部署路径和社区资源主要围绕 PaddlePaddle，这对于已在 PyTorch 或 TensorFlow 生态系统中建立的团队来说可能是一个痛点。此外，其较小模型的参数数量（例如 s）非常高效，但其较大的模型在计算上可能很繁重。

DAMO-YOLO 由阿里巴巴集团于 2022 年末推出，旨在实现低延迟与高性能之间的最佳平衡点。它广泛利用 神经架构搜索 (NAS) 以自动发现高效结构。

DAMO-YOLO 的特点是对推理速度进行了激进优化。

优势： DAMO-YOLO 速度极快。其“微型”和“小型”模型在速度方面提供了令人印象深刻的 mAP，在实时推理场景中超越了许多竞争对手。这使其成为毫秒级延迟至关重要的边缘 AI 应用的理想选择，例如自主无人机或交通监控。

缺点： 作为一项以研究为中心的发布，DAMO-YOLO 可能缺乏在更成熟项目中找到的完善的部署工具和详尽的文档。它对特定 NAS 结构的依赖也可能使希望修改架构的用户在定制和微调方面面临更多复杂性。

尽管PP-YOLOE+和DAMO-YOLO在各自的细分领域提供了有竞争力的功能，但Ultralytics YOLO11作为现代计算机视觉最平衡、最通用且对开发者友好的解决方案脱颖而出。

Ultralytics 通过优先考虑用户体验，实现了 AI 的普及化。与可能需要复杂设置的研究型代码库不同，YOLO11 可通过简单的 pip 安装和直观的 Python API 访问。Ultralytics 生态系统得到积极维护，确保与最新硬件（如 NVIDIA Jetson、Apple M 系列芯片）和软件库的兼容性。

YOLO11 旨在提供最先进的精度，同时不牺牲速度。它通常与 PP-YOLOE+ 等模型的精度相当或更高，同时保持实时应用所需的推理效率。这种平衡对于精度和吞吐量都不可妥协的实际部署至关重要。

Ultralytics模型的一个关键优势是其多功能性。虽然DAMO-YOLO和PP-YOLOE+主要专注于目标检测，但单一的YOLO11模型架构支持：

此外，与许多基于 Transformer 的替代方案或旧版 YOLO 相比，YOLO11 在训练和推理期间都针对更低的内存需求进行了优化。这种效率使开发者能够在标准 GPU 上训练更大的批次，并部署到更受限制的边缘设备上。

凭借现成的预训练权重和优化的训练流程，用户可以在自定义数据集上以最短的训练时间实现高性能。

使用 Ultralytics 部署高级视觉功能简单明了。

PP-YOLOE+ 和 DAMO-YOLO 都对计算机视觉领域做出了卓越贡献。PP-YOLOE+ 对于深度融入 PaddlePaddle 生态系统并需要高精度的用户来说，是一个强有力的选择。DAMO-YOLO 提供了创新的架构选择，以最大限度地提高 边缘设备 上的速度。

然而，对于绝大多数开发人员和企业而言，Ultralytics YOLO11 仍然是推荐选择。它结合了PyTorch原生支持、多任务通用性、卓越的文档和活跃的社区支持，显著缩短了AI解决方案的上市时间。无论您是构建安全警报系统还是制造质量控制管线，YOLO11 都提供了成功所需的可靠性和性能。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLO11 model
model = YOLO("yolo11n.pt")

# Perform object detection on an image
results = model("https://ultralytics.com/images/bus.jpg")

# Display results
results[0].show()
```

---

## YOLOX 对比 YOLOv7：详细技术比较

**URL:** https://docs.ultralytics.com/zh/compare/yolox-vs-yolov7/

**Contents:**
- YOLOX 对比 YOLOv7：详细技术比较
- 性能正面交锋
- YOLOX：通过无锚框设计实现简洁
  - 架构与关键创新
  - 用例与局限性
- YOLOv7: “免费赠品包”的强大力量
  - 架构与关键创新
  - 用例与局限性
- Ultralytics 优势：为何现代化？
  - 简化的开发者体验

在目标检测模型领域中探索，需要深入理解架构的细微差别和性能权衡。本指南对YOLOX和YOLOv7，这两个对计算机视觉领域产生深远影响的架构进行了全面的技术比较。我们将探讨它们的结构创新、基准指标和实际应用，以帮助您为项目确定最佳选择。尽管这两个模型在各自发布时都代表了最先进的进展，但现代开发者通常会转向Ultralytics生态系统，以获得统一的工作流程和尖端性能。

在选择模型时，平均精度 (mAP) 和推理延迟之间的平衡通常是决定性因素。YOLOX 提供了一个从 Nano 到 X 的高度可扩展模型系列，通过其无锚设计强调了简洁性。相反，YOLOv7 专注于通过使用先进的架构优化来最大化实时应用的速度-精度权衡。

数据说明了不同的优势。YOLOXnano 极其轻量，非常适合资源极度受限的环境。然而，对于高性能场景，YOLOv7x 展现出卓越的精度 (53.1% mAP) 和效率，与 YOLOXx 相比，它以显著更少的浮点运算 (FLOPs) 和更快的 T4 GPU 推理时间提供了更高的精度。

YOLOX通过放弃基于锚框的机制，转而采用无锚框方法，标志着YOLO系列的一次范式转变。这种设计选择简化了训练过程，并消除了手动锚框调优的需要，而手动调优通常需要领域特定的启发式优化。

YOLOX集成了解耦头结构，将分类和回归任务分离。这种分离使模型能够学习识别对象“是什么”和“在哪里”的不同特征，从而实现更快的收敛和更高的准确性。此外，YOLOX采用SimOTA，这是一种先进的标签分配策略，能够动态地将正样本与真实对象匹配，从而提高模型在拥挤场景中的鲁棒性。

传统 YOLO 模型（YOLOX 之前）使用预定义的“锚框”来预测对象尺寸。YOLOX 的无锚点方法直接从像素位置预测边界框，减少了超参数的数量，并使模型对各种数据集更具泛化能力。

YOLOX在模型部署需要跨各种硬件平台且无需大量超参数调优的场景中表现出色。其轻量级变体（Nano/Tiny）在移动应用中很受欢迎。然而，其在大规模任务上的峰值性能已被YOLOv7和YOLO11等更新的架构超越，这些架构采用了更复杂的特征聚合网络。

在 YOLOX 发布一年后，YOLOv7 引入了一系列架构改革，旨在优化训练过程，纯粹通过“可训练的免费赠品包”来提升推理结果。

YOLOv7的核心是扩展高效层聚合网络 (E-ELAN)。这种架构通过控制最短和最长的梯度路径，使网络能够学习更多样化的特征，确保超深网络的有效收敛。此外，YOLOv7利用专门为基于拼接的模型设计的模型缩放技术，确保增加模型深度和宽度能够线性地转化为性能提升，而不会出现收益递减。

YOLOv7在训练期间还有效地采用了辅助头，以提供从粗到细的监督，这项技术在部署时不会增加计算成本，同时提高了主检测头的精度。

凭借其卓越的速度与精度比，YOLOv7是实时视频分析和边缘计算任务的有力竞争者，在这些任务中，每一毫秒都至关重要。它突破了标准GPU硬件（如V100和T4）的性能极限。然而，其架构的复杂性可能使其难以针对标准目标检测之外的自定义任务进行修改或微调。

尽管YOLOX和YOLOv7仍然是强大的工具，但计算机视觉领域发展迅速。现代开发者和研究人员越来越倾向于选择Ultralytics生态系统中的模型，例如YOLO11和YOLOv8，因为它们提供全面的支持、统一的设计和易用性。

旧模型面临的最大障碍之一是代码库的碎片化。Ultralytics通过提供一个统一的Python API和CLI来解决这个问题，该API和CLI在所有模型版本中都能保持一致的工作。您可以通过一行代码在detect、segment或classify之间切换。

YOLOX 和 YOLOv7 都在计算机视觉史上占有一席之地。YOLOX 普及了无锚方法，提供了一个易于理解并在小型设备上部署的简化流水线。YOLOv7 突破了性能极限，证明了高效的架构设计可以带来速度和准确性上的巨大提升。

然而，对于当今构建生产级AI系统的人而言，强烈推荐Ultralytics YOLO系列。借助YOLO11，您可以获得一个多功能、强大且用户友好的平台，它能处理MLOps的复杂性，让您专注于解决实际问题。

为了进一步为您的模型选择提供信息，可以探索这些相关的比较：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a model (YOLO11 or YOLOv8)
model = YOLO("yolo11n.pt")  # or "yolov8n.pt"

# Run inference on an image
results = model("path/to/image.jpg")

# Export to ONNX for deployment
model.export(format="onnx")
```

---

## Reference for ultralytics/solutions/object_blurrer.py

**URL:** https://docs.ultralytics.com/zh/reference/solutions/object_blurrer/

**Contents:**
- Reference for ultralytics/solutions/object_blurrer.py
- class ultralytics.solutions.object_blurrer.ObjectBlurrer
  - method ultralytics.solutions.object_blurrer.ObjectBlurrer.process

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/solutions/object_blurrer.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class to manage the blurring of detected objects in a real-time video stream.

This class extends the BaseSolution class and provides functionality for blurring objects based on detected bounding boxes. The blurred areas are updated directly in the input image, allowing for privacy preservation or other effects.

Apply a blurring effect to detected objects in the input image.

This method extracts tracking information, applies blur to regions corresponding to detected objects, and annotates the image with bounding boxes.

**Examples:**

Example 1 (rust):
```rust
ObjectBlurrer(self, **kwargs: Any) -> None
```

Example 2 (json):
```json
>>> blurrer = ObjectBlurrer()
>>> frame = cv2.imread("frame.jpg")
>>> processed_results = blurrer.process(frame)
>>> print(f"Total blurred objects: {processed_results.total_tracks}")
```

Example 3 (python):
```python
class ObjectBlurrer(BaseSolution):
    """A class to manage the blurring of detected objects in a real-time video stream.

    This class extends the BaseSolution class and provides functionality for blurring objects based on detected bounding
    boxes. The blurred areas are updated directly in the input image, allowing for privacy preservation or other effects.

    Attributes:
        blur_ratio (int): The intensity of the blur effect applied to detected objects (higher values create more blur).
        iou (float): Intersection over Union threshold for object detection.
        conf (float): Confidence threshold for object detection.

    Methods:
        process: Apply a blurring effect to detected objects in the input image.
        extract_tracks: Extract tracking information from detected objects.
        display_output: Display the processed output image.

    Examples:
        >>> blurrer = ObjectBlurrer()
        >>> frame = cv2.imread("frame.jpg")
        >>> processed_results = blurrer.process(frame)
        >>> print(f"Total blurred objects: {processed_results.total_tracks}")
    """

    def __init__(self, **kwargs: Any) -> None:
        """Initialize the ObjectBlurrer class for applying a blur effect to objects detected in video streams or images.

        Args:
            **kwargs (Any): Keyword arguments passed to the parent class and for configuration including:
                - blur_ratio (float): Intensity of the blur effect (0.1-1.0, default=0.5).
        """
        super().__init__(**kwargs)
        blur_ratio = self.CFG["blur_ratio"]
        if blur_ratio < 0.1:
            LOGGER.warning("blur ratio cannot be less than 0.1, updating it to default value 0.5")
            blur_ratio = 0.5
        self.blur_ratio = int(blur_ratio * 100)
```

Example 4 (python):
```python
def process(self, im0) -> SolutionResults
```

---

## YOLOv9 与 YOLOv5：技术比较 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/compare/yolov9-vs-yolov5/

**Contents:**
- YOLOv9 与 YOLOv5：技术比较
- YOLOv9：实现最高精度的架构创新
  - 核心架构
  - 优势与劣势
- Ultralytics YOLOv5：多功能行业标准
  - 核心架构
  - Ultralytics 优势
- 性能指标：速度对比准确性
- 训练与可用性
  - 代码示例

在快速发展的计算机视觉领域，选择正确的物体检测模型是项目成功的关键。本分析报告对 YOLOv9之间进行了详细的技术比较。 Ultralytics YOLOv5之间进行了详细的技术比较。我们将探讨它们的架构差异、性能基准和理想用例，帮助您做出明智的决定。

YOLOv9于2024年初发布，旨在通过解决深度学习信息流中的根本问题，挑战目标检测的理论极限。它专为精度至关重要的场景而设计。

作者: Chien-Yao Wang, Hong-Yuan Mark Liao机构:台湾中央研究院资讯科学研究所日期: 2024-02-21Arxiv:arXiv:2402.13616GitHub:WongKinYiu/yolov9文档:YOLOv9 文档

YOLOv9 引入了两项开创性概念：可编程梯度信息 (PGI) 和 广义高效层聚合网络 (GELAN)。PGI 通过确保为 损失函数保留完整的输入信息，解决了深度神经网络固有的信息瓶颈问题，从而提高了梯度可靠性。GELAN 优化了参数效率，与以前使用深度可分离卷积的架构相比，使模型能够以更少的计算资源实现更高的精度。

YOLOv9 的主要优势在于其在COCO 数据集等基准测试中表现出的最先进的准确性。它在检测其他模型可能失败的小型或被遮挡物体方面表现出色。然而，这种对检测准确性的关注伴随着权衡。训练过程可能更资源密集型，尽管它已集成到 Ultralytics 生态系统中，但与更早建立的模型相比，更广泛的社区支持和第三方工具仍在成熟中。此外，其主要关注点仍是检测，而其他模型则提供更广泛的原生多任务支持。

自 2020 年发布以来，Ultralytics YOLOv5 定义了实用、真实世界 AI 部署的标准。它在性能和可用性之间取得了精确的平衡，使其成为历史上使用最广泛的模型之一。

作者：Glenn JocherGlenn Jocher组织：Ultralytics日期：2020-06-26 GitHub:yolov5文档：YOLOv5 文档

YOLOv5 采用精细的基于锚框的架构，具有CSPDarknet53 主干网络和PANet 颈部网络，用于鲁棒的特征聚合。其设计优先考虑推理速度和工程优化。该模型有多种尺寸（从 Nano 到超大），允许开发者根据其硬件限制完美适配模型，从嵌入式边缘设备到云端 GPU。

虽然YOLOv9推动学术边界，但YOLOv5在工程实用性方面表现出色。

以下比较突出了这些模型的不同作用。YOLOv9 通常能实现更高的 mAP（平均精度均值），尤其是在较大的模型尺寸（c 和 e）中。这使其在需要精细细节的任务中表现更出色。

相反，YOLOv5 提供了无与伦比的推理速度，尤其是在其 Nano (n) 和 Small (s) 变体中。对于 实时应用，在 NVIDIA Jetson 或 Raspberry Pi 等边缘硬件上，YOLOv5 因其轻量级特性和 TensorRT 优化成熟度而仍然是顶级竞争者。

为了最大限度地提高部署灵活性，两种模型都可以使用 Ultralytics 导出模式导出为 ONNX、TensorRT 和 Core ML 等格式。这确保了您的模型在任何目标硬件上高效运行。

训练方法在用户体验方面存在显著差异。Ultralytics YOLOv5 旨在实现 训练效率，提供开箱即用的强大预设，适用于自定义数据集。它具有自动锚点计算、超参数演进和丰富的日志集成功能。

YOLOv9 虽然功能强大，但可能需要更仔细地调整超参数以实现稳定性和收敛性，尤其是在较小的数据集上。然而，由于它已集成到 ultralytics Python 包，开发者现在可以使用与 YOLOv5 相同的简单语法训练 YOLOv9，弥合了可用性差距。

借助Ultralytics库，在这些架构之间切换就像更改模型名称一样简单。此代码片段演示了如何加载这两个模型并运行推理：

YOLOv9 和 YOLOv5 之间的选择取决于您的具体限制。YOLOv9 是最大化准确性的卓越选择，提供了尖端的架构改进。YOLOv5 仍然是多功能性和易用性的典范，提供了一个强大且支持良好的生态系统，简化了整个 AI 生命周期。

对于寻求两全其美——结合 YOLOv5 的易用性与超越 YOLOv9 的性能——的开发者而言，我们推荐探索 YOLO11。作为 Ultralytics 的最新迭代，YOLO11 在所有视觉任务中提供了最先进的速度和准确性，代表着 YOLO 家族的未来。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load the established industry standard YOLOv5 (nano version)
model_v5 = YOLO("yolov5nu.pt")

# Run inference on an image
results_v5 = model_v5("path/to/image.jpg")

# Load the high-accuracy YOLOv9 (compact version)
model_v9 = YOLO("yolov9c.pt")

# Run inference on the same image for comparison
results_v9 = model_v9("path/to/image.jpg")
```

---

## Reference for ultralytics/utils/tal.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/tal/

**Contents:**
- Reference for ultralytics/utils/tal.py
- class ultralytics.utils.tal.TaskAlignedAssigner
  - method ultralytics.utils.tal.TaskAlignedAssigner._forward
  - method ultralytics.utils.tal.TaskAlignedAssigner.forward
  - method ultralytics.utils.tal.TaskAlignedAssigner.get_box_metrics
  - method ultralytics.utils.tal.TaskAlignedAssigner.get_pos_mask
  - method ultralytics.utils.tal.TaskAlignedAssigner.get_targets
  - method ultralytics.utils.tal.TaskAlignedAssigner.iou_calculation
  - method ultralytics.utils.tal.TaskAlignedAssigner.select_candidates_in_gts
  - method ultralytics.utils.tal.TaskAlignedAssigner.select_highest_overlaps

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/tal.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A task-aligned assigner for object detection.

This class assigns ground-truth (gt) objects to anchors based on the task-aligned metric, which combines both classification and localization information.

Compute the task-aligned assignment.

Compute the task-aligned assignment.

Compute alignment metric given predicted and ground truth bounding boxes.

Get positive mask for each ground truth box.

Compute target labels, target bounding boxes, and target scores for the positive anchor points.

Calculate IoU for horizontal bounding boxes.

Select positive anchor centers within ground truth bounding boxes.

Select anchor boxes with highest IoU when assigned to multiple ground truths.

Select the top-k candidates based on the given metrics.

Bases: TaskAlignedAssigner

Assigns ground-truth objects to rotated bounding boxes using a task-aligned metric.

Calculate IoU for rotated bounding boxes.

Select the positive anchor center in gt for rotated bounding boxes.

Generate anchors from features.

Transform distance(ltrb) to box(xywh or xyxy).

Transform bbox(xyxy) to dist(ltrb).

Decode predicted rotated bounding box coordinates from anchor points and distribution.

**Examples:**

Example 1 (python):
```python
def __init__(self, topk: int = 13, num_classes: int = 80, alpha: float = 1.0, beta: float = 6.0, eps: float = 1e-9)
```

Example 2 (python):
```python
class TaskAlignedAssigner(nn.Module):
    """A task-aligned assigner for object detection.

    This class assigns ground-truth (gt) objects to anchors based on the task-aligned metric, which combines both
    classification and localization information.

    Attributes:
        topk (int): The number of top candidates to consider.
        num_classes (int): The number of object classes.
        alpha (float): The alpha parameter for the classification component of the task-aligned metric.
        beta (float): The beta parameter for the localization component of the task-aligned metric.
        eps (float): A small value to prevent division by zero.
    """

    def __init__(self, topk: int = 13, num_classes: int = 80, alpha: float = 1.0, beta: float = 6.0, eps: float = 1e-9):
        """Initialize a TaskAlignedAssigner object with customizable hyperparameters.

        Args:
            topk (int, optional): The number of top candidates to consider.
            num_classes (int, optional): The number of object classes.
            alpha (float, optional): The alpha parameter for the classification component of the task-aligned metric.
            beta (float, optional): The beta parameter for the localization component of the task-aligned metric.
            eps (float, optional): A small value to prevent division by zero.
        """
        super().__init__()
        self.topk = topk
        self.num_classes = num_classes
        self.alpha = alpha
        self.beta = beta
        self.eps = eps
```

Example 3 (python):
```python
def _forward(self, pd_scores, pd_bboxes, anc_points, gt_labels, gt_bboxes, mask_gt)
```

Example 4 (python):
```python
def _forward(self, pd_scores, pd_bboxes, anc_points, gt_labels, gt_bboxes, mask_gt):
    """Compute the task-aligned assignment.

    Args:
        pd_scores (torch.Tensor): Predicted classification scores with shape (bs, num_total_anchors, num_classes).
        pd_bboxes (torch.Tensor): Predicted bounding boxes with shape (bs, num_total_anchors, 4).
        anc_points (torch.Tensor): Anchor points with shape (num_total_anchors, 2).
        gt_labels (torch.Tensor): Ground truth labels with shape (bs, n_max_boxes, 1).
        gt_bboxes (torch.Tensor): Ground truth boxes with shape (bs, n_max_boxes, 4).
        mask_gt (torch.Tensor): Mask for valid ground truth boxes with shape (bs, n_max_boxes, 1).

    Returns:
        target_labels (torch.Tensor): Target labels with shape (bs, num_total_anchors).
        target_bboxes (torch.Tensor): Target bounding boxes with shape (bs, num_total_anchors, 4).
        target_scores (torch.Tensor): Target scores with shape (bs, num_total_anchors, num_classes).
        fg_mask (torch.Tensor): Foreground mask with shape (bs, num_total_anchors).
        target_gt_idx (torch.Tensor): Target ground truth indices with shape (bs, num_total_anchors).
    """
    mask_pos, align_metric, overlaps = self.get_pos_mask(
        pd_scores, pd_bboxes, gt_labels, gt_bboxes, anc_points, mask_gt
    )

    target_gt_idx, fg_mask, mask_pos = self.select_highest_overlaps(mask_pos, overlaps, self.n_max_boxes)

    # Assigned target
    target_labels, target_bboxes, target_scores = self.get_targets(gt_labels, gt_bboxes, target_gt_idx, fg_mask)

    # Normalize
    align_metric *= mask_pos
    pos_align_metrics = align_metric.amax(dim=-1, keepdim=True)  # b, max_num_obj
    pos_overlaps = (overlaps * mask_pos).amax(dim=-1, keepdim=True)  # b, max_num_obj
    norm_align_metric = (align_metric * pos_overlaps / (pos_align_metrics + self.eps)).amax(-2).unsqueeze(-1)
    target_scores = target_scores * norm_align_metric

    return target_labels, target_bboxes, target_scores, fg_mask.bool(), target_gt_idx
```

---

## YOLOv6-3.0 与 YOLOX：工业级速度与无锚框精度深度解析

**URL:** https://docs.ultralytics.com/zh/compare/yolov6-vs-yolox/

**Contents:**
- YOLOv6-3.0 与 YOLOX：工业级速度与无锚框精度深度解析
- YOLOv6-3.0：专为工业效率而设计
  - 架构和主要特性
  - 优势与劣势
- YOLOX：简洁与无锚框创新
  - 架构和主要特性
  - 优势与劣势
- 性能比较：指标与分析
  - 分析
- Ultralytics 的优势：为什么选择 YOLO11？

选择最佳物体检测架构是影响计算机视觉系统效率和能力的关键决策。本技术比较研究了YOLOv6.0和YOLOX 这两个在实时检测领域颇具影响力的模型。我们分析了它们的架构创新、基准性能指标以及对各种部署场景的适用性。

作者: Chuyi Li, Lulu Li, Yifei Geng, Hongliang Jiang, Meng Cheng, Bo Zhang, Zaidan Ke, Xiaoming Xu, and Xiangxiang Chu组织: Meituan日期: 2023-01-13Arxiv: YOLOv6 v3.0：全面重载GitHub: meituan/YOLOv6文档: Ultralytics YOLOv6 文档

由美团视觉AI部门开发的YOLOv6-3.0专为工业应用而设计，这些应用通常硬件资源受限，但实时速度不可妥协。它致力于在标准GPU硬件上最大化目标检测流水线的吞吐量。

YOLOv6-3.0引入了一系列“免费赠品”（bag-of-freebies）策略，以在不增加推理成本的情况下提高准确性。

YOLOv6-3.0 的主要优势在于其延迟优化。当使用TensorRT进行优化时，它在 NVIDIA GPU 上实现了卓越的推理速度，使其成为高吞吐量工厂自动化和智慧城市监控的有力候选。此外，它对量化感知训练 (QAT)的支持有助于将其部署到具有降低精度要求的边缘设备。

然而，该模型有些专业化。它缺乏更广泛框架中常见的多任务通用性，几乎完全专注于detect。此外，尽管其生态系统健壮，但与Ultralytics模型周围的社区相比规模较小，这可能会限制针对小众数据集的第三方教程和预训练权重的可用性。

作者: Zheng Ge, Songtao Liu, Feng Wang, Zeming Li, and Jian Sun组织: Megvii日期: 2021-07-18Arxiv: YOLOX：2021年超越YOLO系列GitHub: Megvii-BaseDetection/YOLOX文档: YOLOX 文档

YOLOX 通过将 无锚框检测器 引入主流 YOLO 系列，代表了一种范式转变。通过消除对预定义锚框的需求，它简化了设计过程，并提高了在各种目标形状上的泛化能力。

YOLOX集成了多项先进技术，在保持简洁架构的同时提升了性能：

YOLOX在精度和研究灵活性方面表现出色。其无锚框特性使其在detect具有异常长宽比的对象时特别有效，在这些场景中通常优于基于锚框的同类模型。YOLOX-Nano模型也以其轻量级著称（参数少于100万），使其成为超低功耗微控制器的理想选择。

不利的一面是，与 YOLOv6 或 YOLO11 等较新的模型相比，YOLOX 在相同精度水平下，FLOPs 方面的计算成本可能更高。其训练流程虽然有效，但由于复杂的动态标签分配计算，可能会更慢，并且与高度优化的 Ultralytics 实现相比，在训练期间通常需要更多的 GPU 内存。

下表直接比较了在COCO 数据集上的关键性能指标。

数据表明设计理念存在明显分歧。 YOLOv6-3.0 在硬件感知效率方面占据主导地位。例如， YOLOv6-3.0n 在 T4 GPU 上实现了惊人的 1.17 毫秒推理时间，显著快于同类模型的典型基准。该 YOLOv6-3.0l 也超越了最大的 YOLOX 模型（YOLOXx）的准确率（52.8 vs 51.1 mAP），同时使用的 FLOPs.

YOLOX，相反，在超轻量级类别中胜出。该 YOLOXnano 参数少于 100 万，这是少数现代检测器能够复制的壮举，使其特别适用于内存存储是主要瓶颈而非计算速度的特定物联网应用。然而，对于通用检测，YOLOX 往往需要更多参数才能达到与 YOLOv6 相当的准确性。

如果您的部署目标是现代NVIDIA GPU（例如，Jetson Orin, T4, A100），YOLOv6-3.0可能会提供更好的吞吐量，因为它专业的骨干网络。如果您针对的是通用CPU或存储限制非常严格的传统嵌入式系统，YOLOX Nano可能更适合。

尽管 YOLOv6 和 YOLOX 为特定细分市场提供了强大的解决方案，但Ultralytics YOLO11代表了最先进研究的结晶，在速度、准确性和可用性之间提供了卓越的平衡，适用于绝大多数开发者。

与通常只专注于边界框检测的竞争对手不同，YOLO11 原生支持广泛的计算机视觉任务，包括实例分割、姿势估计、旋转框检测 (OBB)和分类。这使得开发人员能够使用单一框架解决复杂的多阶段问题。

此外，Ultralytics 生态系统得到积极维护，确保与最新的 Python 版本、PyTorch 更新以及 CoreML、OpenVINO 和 ONNX 等部署目标兼容。

YOLO11 专为训练效率而设计，通常比基于 Transformer 的替代方案（如 RT-DETR）或旧版 YOLO 需要更少的 GPU 内存。这使得研究人员能够在消费级硬件上训练更大的模型。Python API 设计简洁，用户只需几行代码即可从安装到推理：

基准测试一致表明，YOLO11在与YOLOv6和YOLOX相当或更快的推理速度下，实现了更高的mAP分数。这种“帕累托最优”性能使其成为从自动驾驶汽车到医学图像分析等各种应用的推荐选择。

在比较 YOLOv6-3.0 和 YOLOX 时，选择很大程度上取决于您的具体限制。YOLOv6-3.0 是严格工业级 GPU 部署的首选，尤其是在毫秒级延迟至关重要的情况下。YOLOX 仍然是研究无锚点架构以及通过其 Nano 模型应对超受限存储环境的可靠选择。

然而，对于寻求兼具顶级性能和易用、功能丰富平台的未来就绪型解决方案的开发者而言，Ultralytics YOLO11是最终的赢家。它能够无缝处理多项任务，结合详尽的文档和广泛的部署支持，加速了从概念到生产的开发生命周期。

探索其他比较，了解 Ultralytics 模型如何与 RT-DETR 或 YOLOv7 进行对比。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load the YOLO11 model (n, s, m, l, or x)
model = YOLO("yolo11n.pt")

# Perform inference on an image
results = model("path/to/image.jpg")

# Export to ONNX for deployment
model.export(format="onnx")
```

---

## 脑肿瘤数据集 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/datasets/detect/brain-tumor/

**Contents:**
- 脑肿瘤数据集
- 数据集结构
- 应用
- 数据集 YAML
- 用法
- Sample Images 和注释
- 引用和致谢
- 常见问题
  - Ultralytics 文档中提供的大脑肿瘤数据集的结构是什么？
  - 如何使用 Ultralytics 在脑肿瘤数据集上训练 YOLO11 模型？

脑肿瘤检测数据集包含来自 MRI 或 CT 扫描的医学图像，其中包含有关脑肿瘤的存在、位置和特征的信息。该数据集对于训练计算机视觉算法以自动识别脑肿瘤至关重要，有助于在医疗保健应用中进行早期诊断和治疗计划。

观看： 使用 Ultralytics HUB 进行脑肿瘤检测

使用计算机视觉进行脑肿瘤检测的应用能够实现早期诊断、治疗计划和肿瘤进展监测。通过分析 MRI 或 CT 扫描等医学影像数据，计算机视觉系统可协助准确识别脑肿瘤，从而有助于及时的医疗干预和个性化治疗策略。

YAML（Yet Another Markup Language）文件用于定义数据集配置。它包含关于数据集的路径、类和其他相关信息。在脑肿瘤数据集的情况下， brain-tumor.yaml 文件保存在 https://github.com/ultralytics/ultralytics/blob/main/ultralytics/cfg/datasets/brain-tumor.yaml.

ultralytics/cfg/datasets/brain-tumor.yaml

要使用提供的代码片段在脑肿瘤数据集上训练 YOLO11 模型 100 个 epochs，图像大小为 640。有关可用参数的详细列表，请参阅模型的训练页面。

脑肿瘤数据集包含大量医学图像，其中包含有肿瘤和无肿瘤的脑部扫描。下面展示了数据集中图像的示例，并附有相应的注释。

此示例突出显示了脑肿瘤数据集中图像的多样性和复杂性，强调了在医学图像分析的训练阶段中加入镶嵌的优势。

该数据集已根据 AGPL-3.0 许可发布。

如果您在研究或开发工作中使用此数据集，请适当引用：

脑肿瘤数据集分为两个子集：训练集包含 893 张带有相应注释的图像，而测试集包含 223 张带有配对注释的图像。这种结构化的划分有助于开发用于检测脑肿瘤的强大而准确的计算机视觉模型。有关数据集结构的更多信息，请访问数据集结构部分。

您可以使用 Python 和 CLI 两种方法，在脑肿瘤数据集上训练 YOLO11 模型，训练 100 个 epoch，图像大小为 640px。以下是两种方法的示例：

在 AI 项目中使用脑肿瘤数据集能够实现脑肿瘤的早期诊断和治疗计划。它有助于通过计算机视觉自动识别脑肿瘤，从而促进准确及时的医疗干预，并支持个性化的治疗策略。该应用在改善患者预后和医疗效率方面具有巨大的潜力。有关 AI 在医疗保健领域的应用的更多见解，请参阅 Ultralytics' healthcare solutions。

可以使用 Python 或 CLI 方法，通过微调的 YOLO11 模型进行推理。以下是一些示例：

脑肿瘤数据集的 yaml 配置文件可在 brain-tumor.yaml 中找到。此文件包含路径、类别以及在此数据集上训练和评估模型所需的其他相关信息。

**Examples:**

Example 1 (yaml):
```yaml
# Ultralytics 🚀 AGPL-3.0 License - https://ultralytics.com/license

# Brain-tumor dataset by Ultralytics
# Documentation: https://docs.ultralytics.com/datasets/detect/brain-tumor/
# Example usage: yolo train data=brain-tumor.yaml
# parent
# ├── ultralytics
# └── datasets
#     └── brain-tumor ← downloads here (4.21 MB)

# Train/val/test sets as 1) dir: path/to/imgs, 2) file: path/to/imgs.txt, or 3) list: [path/to/imgs1, path/to/imgs2, ..]
path: brain-tumor # dataset root dir
train: images/train # train images (relative to 'path') 893 images
val: images/val # val images (relative to 'path') 223 images

# Classes
names:
  0: negative
  1: positive

# Download script/URL (optional)
download: https://github.com/ultralytics/assets/releases/download/v0.0.0/brain-tumor.zip
```

Example 2 (python):
```python
from ultralytics import YOLO

# Load a model
model = YOLO("yolo11n.pt")  # load a pretrained model (recommended for training)

# Train the model
results = model.train(data="brain-tumor.yaml", epochs=100, imgsz=640)
```

Example 3 (sql):
```sql
# Start training from a pretrained *.pt model
yolo detect train data=brain-tumor.yaml model=yolo11n.pt epochs=100 imgsz=640
```

Example 4 (python):
```python
from ultralytics import YOLO

# Load a model
model = YOLO("path/to/best.pt")  # load a brain-tumor fine-tuned model

# Inference using the model
results = model.predict("https://ultralytics.com/assets/brain-tumor-sample.jpg")
```

---

## YOLO11 与 YOLOv7：详细技术比较

**URL:** https://docs.ultralytics.com/zh/compare/yolo11-vs-yolov7/

**Contents:**
- YOLO11 与 YOLOv7：详细技术比较
- Ultralytics YOLO11：视觉 AI 的新标准
  - 架构与创新
- YOLOv7：高效训练的基准
  - 架构与创新
- 性能分析：速度、准确性和效率
  - 主要内容
- 生态系统与开发者体验
  - 简化工作流程
  - 多功能性与训练效率

选择合适的目标检测模型是一个关键决策，它会影响计算机视觉应用程序的速度、准确性和可扩展性。本指南对 Ultralytics YOLO11 和 YOLOv7 这两个 YOLO (You Only Look Once) 系列中的重要里程碑进行了深入的技术比较。YOLOv7 在 2022 年代表着一个重大飞跃，而最近发布的 YOLO11 则引入了架构改进，重新定义了现代 AI 开发的最先进性能。

Ultralytics YOLO11于2024年末发布，在其前代产品的坚实基础上，提供了无与伦比的效率和多功能性。它旨在在一个统一的框架内处理各种计算机视觉任务。

YOLO11 引入了精炼的架构，其特色是C3k2 块和C2PSA（跨阶段部分空间注意力）机制。这些增强功能使模型能够以更高的粒度提取特征，同时与前几代相比保持更低的参数数量。该架构针对速度进行了优化，确保即使是较大的模型变体也能在标准硬件上保持实时推理能力。

YOLO11 的一个显著特点是其原生支持除目标检测之外的多种任务，包括实例分割、姿势估计、旋转框检测 (OBB)和图像分类。

YOLO11 完全集成到 Ultralytics 生态系统中，为开发人员提供无缝访问数据管理、模型训练和部署工具的权限。这种集成显著降低了 MLOps 流水线的复杂性，使团队能够更快地从原型转向生产。

YOLOv7 于2022年中发布，重点优化了训练过程，以在不增加推理成本的情况下实现高准确性。它引入了多项新颖概念，对该领域的后续研究产生了影响。

YOLOv7的核心是E-ELAN（扩展高效层聚合网络），它在不破坏原始梯度路径的情况下提高了模型的学习能力。作者还引入了“可训练的免费包”，这是一系列优化策略——例如模型重参数化和辅助检测头——它们在训练期间提高准确性，但在推理期间被精简掉。

虽然YOLOv7在发布时设定了令人印象深刻的基准，但它主要是一个目标detect架构。将其适应于segment或姿势估计等其他任务通常需要代码库的特定分支或分叉，这与新模型的统一方法形成对比。

YOLOv7 依赖于基于锚框的检测方法和复杂的辅助头。尽管有效，但与现代 Ultralytics 模型中精简的无锚框设计相比，这些架构选择使得模型在边缘部署时更难定制和优化。

在比较技术指标时，YOLO11 架构的进步变得显而易见。新模型以显著更少的参数和更快的推理速度实现了可比或更优的精度。

除了原始指标之外，开发者体验是一个主要区别因素。Ultralytics YOLO模型以其易用性和强大的生态系统而闻名。

YOLOv7 通常需要克隆仓库并与复杂的 shell 脚本交互来进行训练和测试。相比之下，YOLO11 通过标准 python 包分发 (ultralytics）。这使开发人员只需几行代码即可将高级计算机视觉功能集成到他们的软件中。

YOLO11 开箱即用，支持广泛的任务。如果项目需求从简单的边界框转向 实例分割 或 姿势估计，开发人员只需切换模型权重文件（例如， yolo11n-seg.pt），而无需更改整个代码库或pipeline。YOLOv7 通常需要查找和配置特定的分支才能完成这些任务。

此外，YOLO11 受益于训练效率。这些模型利用现代优化技术，并附带高质量的预训练权重，通常比旧架构收敛更快。这种效率也体现在内存需求上；Ultralytics 模型经过优化，可在训练期间最大限度地减少 CUDA 内存使用，从而防止困扰旧版或基于 Transformer 的检测器的常见内存不足 (OOM) 错误。

Ultralytics 维护着详尽的文档和一个活跃的社区。用户受益于频繁的更新、错误修复以及清晰的企业支持路径。相比之下，YOLOv7 仓库虽然具有历史意义，但维护活跃度较低，这可能对长期生产部署构成风险。

虽然YOLOv7仍然是一个有能力的模型，也是2022年计算机视觉快速进步的证明，但Ultralytics YOLO11代表了现代AI开发的明确选择。它在性能、效率和可用性之间提供了卓越的平衡。

对于开发者和研究人员而言，转向 YOLO11 带来了即时的好处：更快的推理时间、更低的硬件成本以及针对各种视觉任务的统一工作流程。在活跃的 Ultralytics 生态系统支持下，YOLO11 不仅仅是一个模型，而是一个全面的解决方案，用于在现实世界中部署最先进的计算机视觉技术。

探索更多比较，以找到最适合您特定需求的模型：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a model (YOLO11n recommended for speed)
model = YOLO("yolo11n.pt")

# Train the model with a single command
train_results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference on an image
results = model("path/to/image.jpg")
```

---

## EfficientDet 与 DAMO-YOLO：技术对比

**URL:** https://docs.ultralytics.com/zh/compare/efficientdet-vs-damo-yolo/

**Contents:**
- EfficientDet 与 DAMO-YOLO：技术对比
- 性能分析：效率与延迟
  - 关键基准测试要点
- EfficientDet：可扩展且高效
  - 架构亮点
  - 优势与劣势
- DAMO-YOLO：速度导向创新
  - 架构亮点
  - 优势与劣势
- Ultralytics YOLO11：全面的替代方案

在快速发展的计算机视觉领域，选择合适的物体 detect 架构对于应用成功至关重要。塑造了该领域的两个著名架构是 Google Research 开发的 EfficientDet 和阿里巴巴达摩院开发的 DAMO-YOLO。尽管两者都旨在最大限度地提高性能，但它们的设计理念却大相径庭：一个侧重于参数效率和可扩展性，而另一个则针对工业硬件上的低延迟推理。

本指南对这两种模型进行了深入的技术分析，比较了它们的架构、性能指标和理想用例，旨在帮助开发人员做出明智的决策。

以下基准测试说明了EfficientDet和DAMO-YOLO之间明显的权衡。EfficientDet以其低参数量和FLOPs而闻名，使其在理论上高效，而DAMO-YOLO则针对GPU上的实际推理速度进行了优化。

EfficientDet 通过引入一种原则性的模型维度缩放方法，彻底改变了 目标检测领域。它基于 EfficientNet 主干网络构建，旨在实现高准确性，同时最大限度地减少理论计算成本 (FLOPs)。

EfficientDet 的核心创新在于两个主要组成部分：

EfficientDet 的主要优势在于其理论效率。它以远少于 YOLOv3 或 RetinaNet 等先前检测器的参数实现了最先进的准确性。然而，其大量使用深度可分离卷积以及 BiFPN 复杂的内存访问模式可能导致在现代 GPU 上的利用率较低，从而在 FLOPs 较低的情况下导致更高的延迟。

尽管EfficientDet的FLOPs较低，但“低FLOPs”并不总是等同于“快速推理”。在GPU或TPU等硬件上，内存带宽和内核启动开销往往更为重要。EfficientDet复杂的图结构有时可能成为实时推理场景中的瓶颈。

DAMO-YOLO 的设计旨在弥合工业硬件上高性能与低延迟之间的差距。它融合了尖端的神经架构搜索 (NAS) 技术，以寻找检测任务的最佳结构。

DAMO-YOLO 为 YOLO 家族引入了多项“新技术”组件：

DAMO-YOLO 在原始速度方面表现出色。通过优先考虑对硬件加速友好的结构（如 TensorRT），它实现了卓越的吞吐量。然而，与更简单、手工设计的架构相比，它对复杂的 NAS 生成架构的依赖可能会使其更难为定制研究目的进行修改或微调。此外，它缺乏更主流 YOLO 版本中普遍存在的广泛社区支持和多平台易用性。

尽管EfficientDet提供了参数效率，DAMO-YOLO提供了GPU速度，但Ultralytics YOLO11在开发者友好的生态系统中，提供了两者的卓越平衡。对于大多数实际应用——从边缘AI到云部署——YOLO11都是最佳选择。

使用 Ultralytics 实现最先进的 detect 非常简单。以下代码片段演示了如何加载预训练的 YOLO11 模型并对图像运行推理：

Ultralytics 模型可与流行的 MLOps 工具无缝集成。无论您是使用 MLflow 进行日志记录，还是使用 Ray Tune 进行超参数优化，这些功能都直接内置于库中。

在 EfficientDet 和YOLO 的比较中，选择主要取决于具体的硬件限制。对于理论效率和参数数量是主要瓶颈的应用场景，EfficientDet仍然是强有力的候选者。对于在现代 GPU 上运行的高吞吐量应用来说，YOLO-YOLO显然是赢家，因为在这些应用中，延迟是最重要的。

然而，对于一个兼具高性能、易用性和多任务能力的解决方案——Ultralytics YOLO11脱颖而出，成为行业标准。其强大的生态系统和持续改进确保开发者拥有最可靠的工具来构建可扩展的计算机视觉解决方案。

为了进一步了解目标检测模型的全貌，请探索这些额外的比较：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLO11n model
model = YOLO("yolo11n.pt")

# Run inference on a local image or URL
results = model.predict("https://ultralytics.com/images/bus.jpg")

# Display the results
results[0].show()

# Export the model to ONNX format for deployment
path = model.export(format="onnx")
```

---

## PP-YOLOE+ 对比 YOLOv9：技术对比 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/compare/pp-yoloe-vs-yolov9/

**Contents:**
- PP-YOLOE+ 对比 YOLOv9：技术对比
- PP-YOLOE+：PaddlePaddle 生态系统内的高精度
  - 架构和主要特性
  - 优势与劣势
- YOLOv9：用于增强学习的可编程梯度信息
  - 架构和主要特性
  - 优势与劣势
- 对比性能分析
- Ultralytics 优势
  - 简化的用户体验

为计算机视觉项目选择最优架构需要在快速发展的模型领域中进行探索。本页面详细比较了百度的PP-YOLOE+和YOLOv9，这两个复杂的单阶段对象detect器。我们分析了它们的架构创新、性能指标和生态系统集成，以帮助您做出明智的决策。虽然这两个模型都展现出强大的能力，但它们代表了不同的设计理念和框架依赖性。

PP-YOLOE+ 是 PP-YOLOE 的演进版本，由百度作为PaddleDetection套件的一部分开发。它旨在提供精度和推理速度之间的平衡权衡，并专门针对PaddlePaddle深度学习框架进行了优化。

作者： PaddlePaddle 作者机构：百度日期： 2022-04-02预印本：https://arxiv.org/abs/2203.16250GitHub：https://github.com/PaddlePaddle/PaddleDetection/文档：PaddleDetection PP-YOLOE+ README

PP-YOLOE+ 作为一个无锚框、单阶段检测器运行。它基于 CSPRepResNet 主干网络，并利用任务对齐学习 (TAL) 策略来改善分类和定位任务之间的对齐。一个关键特性是高效任务对齐头 (ET-Head)，它在保持精度的同时降低了计算开销。该模型使用 Varifocal Loss 损失函数来处理训练期间的类别不平衡问题。

PP-YOLOE+ 的主要优势在于其针对百度硬件和软件栈的优化。它提供了可扩展的模型（s、m、l、x），在标准物体检测基准测试中表现良好。

然而，它对PaddlePaddle生态系统的严重依赖对更广泛的AI社区来说是一个重大障碍，因为该社区主要青睐PyTorch。将现有的PyTorch工作流程迁移到PaddlePaddle可能需要大量资源。此外，与较新的架构相比，PP-YOLOE+需要更多参数才能达到相似的准确性，这会影响受限设备的存储和内存。

Ultralytics YOLOv9 通过解决深度神经网络固有的“信息瓶颈”问题，在实时目标检测领域引入了范式转变。

作者：王建尧、廖鸿源Chien-Yao Wang and Hong-Yuan Mark Liao组织：台湾中央研究院信息科学研究所日期：2024-02-21Arxiv:https://arxiv.org/abs/2402.13616 GitHub:https://github.com/WongKinYiu/yolov9Documentation:ultralytics

YOLOv9 集成了两项开创性概念：可编程梯度信息 (PGI) 和 广义高效层聚合网络 (GELAN)。

YOLOv9 的 PGI 技术解决了信息瓶颈问题，该问题此前需要繁琐的深度监督方法。这使得模型更轻量、更准确，显著提升了性能平衡。

YOLOv9 在训练效率和参数利用率方面表现出色。它在 COCO 数据集上取得了最先进的结果，在精度上超越了以前的版本，同时保持了实时速度。它集成到 Ultralytics 生态系统意味着它得益于一个维护良好的生态系统，包括通过 导出模式轻松部署到 ONNX 和 TensorRT 等格式。

一个潜在的考虑因素是，最大的变体 (YOLOv9-E) 在训练时需要大量的 GPU 资源。然而，推理内存占用仍然具有竞争力，避免了与 Transformer 模型相关的高成本。

通过直接比较，YOLOv9 展现出卓越的效率。例如，YOLOv9-C 模型实现了比 PP-YOLOE+l (52.9%) 更高的 mAP (53.0%)，同时参数量约为其一半 (25.3M 对比 52.2M)。这种在不牺牲精度的情况下大幅减小模型尺寸的特点，凸显了 GELAN 架构的有效性。

该表表明，对于相似的准确性目标，YOLOv9始终需要更少的计算资源。YOLOv9-E模型进一步突破了极限，实现了55.6%的mAP，明显优于最大的PP-YOLOE+变体。

尽管PP-YOLOE+是一个有能力的检测器，但通过Ultralytics框架选择YOLOv9在易用性和多功能性方面提供了显著优势。

Ultralytics 优先考虑开发者友好的体验。与 PaddleDetection 通常所需的复杂配置文件不同，Ultralytics 模型只需几行 python 代码即可加载、训练和部署。这显著降低了工程师和研究人员的入门门槛。

Ultralytics 支持除了简单检测之外的广泛任务，包括实例分割、姿势估计和旋转框检测 (obb)。这种多功能性使开发者能够使用单一、统一的 API 应对各种挑战。此外，活跃的社区和频繁的更新确保用户能够获取最新的优化以及与 TensorBoard 和 MLflow 等工具的集成。

以下示例演示了如何轻松地使用Ultralytics Python API运行YOLOv9推理。这种简洁性与PP-YOLOE+通常需要的更繁琐的设置形成了对比。

对于大多数开发者和组织，YOLOv9 是更优的选择，因为它拥有现代架构 (GELAN/PGI)、卓越的参数效率以及 Ultralytics 生态系统的强大支持。它提供了一个面向未来的解决方案，具有现成的预训练权重和无缝导出能力。

如果您正在寻找更高的通用性和速度，我们还推荐探索 YOLO11，它是 YOLO 系列的最新迭代。YOLO11 进一步优化了性能与延迟之间的平衡，在一个紧凑的包中提供了最先进的 detect、分割和分类任务能力。

对于那些对久经考验的主力感兴趣的用户，YOLOv8 仍然是一个高度可靠的选择，拥有丰富的社区资源和第三方集成。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv9 model
model = YOLO("yolov9c.pt")

# Run inference on an image
results = model("path/to/image.jpg")

# Display results
results[0].show()
```

---

## 了解计算机视觉项目的关键步骤

**URL:** https://docs.ultralytics.com/zh/guides/steps-of-a-cv-project/

**Contents:**
- 了解计算机视觉项目的关键步骤
- 简介
- 计算机视觉项目概述
- 步骤 1：定义项目目标
  - 步骤 1.5：选择正确的模型和训练方法
- 步骤 2：数据收集和数据标注
- 步骤 3：数据增强和拆分数据集
- 第四步：模型训练
- 第五步：模型评估和模型微调
- 第六步：模型测试

计算机视觉是人工智能 (AI) 的一个子领域，它帮助计算机像人类一样观察和理解世界。它处理和分析图像或视频，以提取信息、识别模式并根据这些数据做出决策。

观看： 操作方法 计算机视觉 项目 | 分步指南

诸如目标检测、图像分类和实例分割等计算机视觉技术可以应用于各个行业，从自动驾驶到医学影像，以获得有价值的见解。

开展自己的计算机视觉项目是理解和学习计算机视觉的绝佳方式。然而，一个计算机视觉项目可能包含许多步骤，初看起来可能会令人困惑。通过本指南，您将熟悉计算机视觉项目所涉及的步骤。我们将从项目的开始到结束，逐步讲解每个部分的重要性。

在讨论计算机视觉项目中每个步骤的细节之前，我们先来看看整个流程。如果您今天启动一个计算机视觉项目，您将采取以下步骤：

既然我们知道了预期情况，那么让我们直接进入步骤，推动您的项目向前发展。

任何计算机视觉项目的第一步都是明确定义您要解决的问题。了解最终目标有助于您开始构建解决方案。对于计算机视觉来说尤其如此，因为您项目的目标将直接影响您需要关注的计算机视觉任务。

以下是一些项目目标示例以及可用于实现这些目标的计算机视觉任务：

目标： 开发一种可以监控和管理高速公路上不同车辆类型流量的系统，从而改善交通管理和安全性。

目标： 开发一种工具，通过提供医学影像扫描中肿瘤的精确像素级轮廓来协助放射科医生。

目标： 创建一个对各种文档（例如，发票、收据、法律文件）进行分类的数字系统，以提高组织效率和文档检索。

在了解项目目标和合适的计算机视觉任务后，定义项目目标的一个重要部分是选择正确的模型和训练方法。

根据目标的不同，您可以选择先选择模型，或者在查看了您可以在第 2 步中收集到的数据之后再选择。例如，假设您的项目高度依赖于特定类型数据的可用性。在这种情况下，先收集和分析数据，然后再选择模型可能更实用。另一方面，如果您对模型的要求有清楚的了解，您可以先选择模型，然后再收集符合这些规范的数据。

选择从头开始训练还是使用 迁移学习会影响您准备数据的方式。从头开始训练需要多样化的数据集，以从零开始构建模型理解。另一方面，迁移学习允许您使用预训练模型，并用更小、更具体的数据集对其进行调整。此外，选择要训练的特定模型将决定您需要如何准备数据，例如根据模型的具体要求调整图像大小或添加标注。

注意：选择模型时，请考虑其部署，以确保兼容性和性能。 例如，轻量级模型非常适合边缘计算，因为它们在资源受限的设备上具有效率。 要了解有关定义项目的关键点的更多信息，请阅读我们的指南，了解如何定义项目的目标并选择合适的模型。

在开始计算机视觉项目的实际操作之前，重要的是要清楚地了解这些细节。在继续执行步骤 2 之前，请仔细检查您是否已考虑以下事项：

您的计算机视觉模型的质量取决于您数据集的质量。您可以从互联网收集图像、拍摄自己的照片或使用现有数据集。以下是一些下载高质量数据集的优秀资源：Google 数据集搜索引擎、加州大学欧文分校机器学习存储库和Kaggle 数据集。

一些库，例如 Ultralytics，提供对各种数据集的内置支持，从而更容易开始使用高质量的数据。这些库通常包含用于无缝使用流行数据集的实用程序，这可以为您节省项目初始阶段的大量时间和精力。

但是，如果您选择收集图像或拍摄自己的照片，则需要注释您的数据。数据注释是标记您的数据以将知识传授给您的模型的过程。您将使用的数据注释类型取决于您的特定计算机视觉技术。以下是一些例子：

数据收集和标注可能是一项耗时的手动工作。标注工具可以帮助简化此过程。以下是一些有用的开放标注工具：LabeI Studio、CVAT和Labelme。

在收集和标注图像数据后，首先将数据集拆分为训练集、验证集和测试集，然后再执行数据增强，这一点非常重要。在增强之前拆分数据集对于在原始的、未更改的数据上测试和验证模型至关重要。它有助于准确评估模型对新的、未见过的数据的泛化能力。

拆分数据后，您可以通过应用旋转、缩放和翻转图像等变换来执行数据增强，以人为方式增加数据集的大小。数据增强使您的模型对变化更加稳健，并提高其在未见过的图像上的性能。

像 OpenCV、Albumentations 和 TensorFlow 这样的库提供了灵活的数据增强功能供您使用。此外，一些库（例如 Ultralytics）在其模型训练函数中直接内置了数据增强设置，从而简化了此过程。

为了更好地理解您的数据，您可以使用 Matplotlib 或 Seaborn 等工具来可视化图像并分析其分布和特征。数据可视化有助于识别模式、异常以及数据增强技术的有效性。您还可以使用 Ultralytics Explorer，这是一个用于探索计算机视觉数据集的工具，支持语义搜索、SQL 查询和向量相似性搜索。

通过正确地理解、分割和增强您的数据，您可以开发出一个经过良好训练、验证和测试的模型，该模型在实际应用中表现良好。

一旦您的数据集准备好进行训练，您就可以专注于设置必要的环境、管理数据集和训练模型。

首先，您需要确保您的环境配置正确。通常，这包括以下内容：

然后，您可以将训练和验证数据集加载到您的环境中。通过调整大小、格式转换或增强来标准化和预处理数据。选择模型后，配置层并指定超参数。通过设置 损失函数、优化器和性能指标来编译模型。

像 Ultralytics 这样的库简化了训练过程。您可以通过最少的代码将数据馈送到模型中来开始训练。这些库会自动处理权重调整、反向传播和验证。它们还提供工具来轻松监控进度和调整超参数。训练后，使用几个命令保存模型及其权重。

重要的是要记住，正确的数据集管理对于高效训练至关重要。对数据集使用版本控制来跟踪更改并确保可重现性。像 DVC (数据版本控制) 这样的工具可以帮助管理大型数据集。

使用各种指标评估模型的性能并对其进行改进以提高准确性非常重要。评估有助于识别模型擅长的领域以及可能需要改进的领域。微调可确保模型针对最佳性能进行优化。

要更深入地了解模型评估和微调技术，请查看我们的模型评估见解指南。

在此步骤中，您可以确保您的模型在完全未见过的数据上表现良好，从而确认其已准备好进行部署。模型测试和模型评估之间的区别在于，它侧重于验证最终模型的性能，而不是迭代地改进它。

务必充分测试和调试可能出现的任何常见问题。在未在训练或验证期间使用的单独测试数据集上测试您的模型。此数据集应代表真实场景，以确保模型的性能一致且可靠。

此外，还要解决常见问题，例如过拟合、欠拟合 和数据泄露。使用 交叉验证 和 异常检测 等技术来识别和修复这些问题。有关全面的测试策略，请参阅我们的模型测试指南。

一旦您的模型经过彻底的测试，就可以部署它了。模型部署 涉及使您的模型可在生产环境中使用。以下是部署计算机视觉模型的步骤：

有关部署策略和最佳实践的更详细指导，请查看我们的模型部署实践指南。

模型部署完成后，持续监控其性能、维护它以处理任何问题以及记录整个过程以供将来参考和改进非常重要。

监控工具可以帮助您跟踪关键绩效指标 (KPI) 并检测异常或准确性下降。通过监控模型，您可以了解模型漂移，即由于输入数据变化导致模型性能随时间下降的情况。定期使用更新的数据重新训练模型，以保持准确性和相关性。

除了监控和维护之外，文档记录也很关键。全面记录整个过程，包括模型架构、训练程序、超参数、数据预处理步骤以及在部署和维护期间所做的任何更改。良好的文档记录可确保可重复性，并使未来的更新或故障排除更加容易。通过有效地监控、维护和记录您的模型，您可以确保它在其生命周期内保持准确、可靠且易于管理。

与计算机视觉爱好者社区建立联系，可以帮助您自信地解决在计算机视觉项目中所面临的任何问题。以下是一些有效学习、排除故障和建立联系的方式。

利用这些资源将帮助您克服挑战，并及时了解计算机视觉社区的最新趋势和最佳实践。

承担计算机视觉项目既令人兴奋又充满回报。通过遵循本指南中的步骤，您可以为成功打下坚实的基础。每一步对于开发一个满足您的目标并在实际场景中良好运行的解决方案都至关重要。随着经验的积累，您将发现更先进的技术和工具来改进您的项目。

选择正确的计算机视觉任务取决于项目的最终目标。例如，如果您想监控交通，对象检测是合适的，因为它可以实时定位和识别多种车辆类型。对于医学成像，图像分割非常适合提供肿瘤的详细边界，从而有助于诊断和治疗计划。了解更多关于对象检测、图像分类和实例分割等特定任务的信息。

数据标注对于训练模型识别模式至关重要。标注的类型随任务而异：

诸如 Label Studio、CVAT 和 Labelme 等工具可以协助完成此过程。有关更多详细信息，请参阅我们的数据收集和标注指南。

在增强之前拆分数据集有助于验证模型在原始、未更改数据上的性能。请按照以下步骤操作：

拆分后，应用旋转、缩放和翻转等数据增强技术以增加数据集多样性。Albumentations 和 OpenCV 等库可以提供帮助。Ultralytics 还提供内置增强设置以方便使用。

导出模型可确保与不同部署平台的兼容性。Ultralytics 提供了多种格式，包括 ONNX、TensorRT 和 CoreML。要导出您的 YOLO11 模型，请按照本指南操作:

持续监控和维护对于模型的长期成功至关重要。实施工具来跟踪关键绩效指标 (KPI) 并检测异常情况。定期使用更新的数据重新训练模型，以抵消模型漂移。记录整个过程，包括模型架构、超参数和更改，以确保可重复性和便于将来的更新。请在我们的监控和维护指南中了解更多信息。

---

## Reference for ultralytics/utils/export/tensorflow.py

**URL:** https://docs.ultralytics.com/zh/reference/utils/export/tensorflow/

**Contents:**
- Reference for ultralytics/utils/export/tensorflow.py
- function ultralytics.utils.export.tensorflow.tf_wrapper
- function ultralytics.utils.export.tensorflow._tf_inference
- function ultralytics.utils.export.tensorflow.tf_kpts_decode
- function ultralytics.utils.export.tensorflow.onnx2saved_model
- function ultralytics.utils.export.tensorflow.keras2pb
- function ultralytics.utils.export.tensorflow.tflite2edgetpu
- function ultralytics.utils.export.tensorflow.pb2tfjs
- function ultralytics.utils.export.tensorflow.gd_outputs

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/export/tensorflow.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A wrapper to add TensorFlow compatible inference methods to Detect and Pose layers.

Decode boxes and cls scores for tf object detection.

Decode keypoints for tf pose estimation.

Convert a ONNX model to TensorFlow SavedModel format via ONNX.

Convert a Keras model to TensorFlow GraphDef (.pb) format.

Creates a frozen graph by converting variables to constants for inference optimization.

Convert a TensorFlow Lite model to Edge TPU format using the Edge TPU compiler.

Requires the Edge TPU compiler to be installed. The function compiles the TFLite model for optimal performance on Google's Edge TPU hardware accelerator.

Convert a TensorFlow GraphDef (.pb) model to TensorFlow.js format.

Requires tensorflowjs package. Uses tensorflowjs_converter command-line tool for conversion. Handles spaces in file paths and warns if output directory contains spaces.

Return TensorFlow GraphDef model output node names.

**Examples:**

Example 1 (python):
```python
def tf_wrapper(model: torch.nn.Module) -> torch.nn.Module
```

Example 2 (python):
```python
def tf_wrapper(model: torch.nn.Module) -> torch.nn.Module:
    """A wrapper to add TensorFlow compatible inference methods to Detect and Pose layers."""
    for m in model.modules():
        if not isinstance(m, Detect):
            continue
        import types

        m._inference = types.MethodType(_tf_inference, m)
        if type(m) is Pose:
            m.kpts_decode = types.MethodType(tf_kpts_decode, m)
    return model
```

Example 3 (python):
```python
def _tf_inference(self, x: list[torch.Tensor]) -> tuple[torch.Tensor]
```

Example 4 (python):
```python
def _tf_inference(self, x: list[torch.Tensor]) -> tuple[torch.Tensor]:
    """Decode boxes and cls scores for tf object detection."""
    shape = x[0].shape  # BCHW
    x_cat = torch.cat([xi.view(x[0].shape[0], self.no, -1) for xi in x], 2)
    box, cls = x_cat.split((self.reg_max * 4, self.nc), 1)
    if self.dynamic or self.shape != shape:
        self.anchors, self.strides = (x.transpose(0, 1) for x in make_anchors(x, self.stride, 0.5))
        self.shape = shape
    grid_h, grid_w = shape[2], shape[3]
    grid_size = torch.tensor([grid_w, grid_h, grid_w, grid_h], device=box.device).reshape(1, 4, 1)
    norm = self.strides / (self.stride[0] * grid_size)
    dbox = self.decode_bboxes(self.dfl(box) * norm, self.anchors.unsqueeze(0) * norm[:, :2])
    return torch.cat((dbox, cls.sigmoid()), 1)
```

---

## YOLOv6-3.0 与 YOLOv9：工业级速度与最先进效率的结合

**URL:** https://docs.ultralytics.com/zh/compare/yolov6-vs-yolov9/

**Contents:**
- YOLOv6-3.0 与 YOLOv9：工业级速度与最先进效率的结合
- YOLOv6-3.0：为工业应用优化
  - 架构与设计理念
  - 优势与局限性
- YOLOv9：重新定义精度与信息流
  - 架构：PGI 与 GELAN
  - Ultralytics 生态系统的优势
- 对比性能分析
- 训练与部署工作流
  - 示例：使用Ultralytics训练YOLOv9

选择最优的对象detect模型是计算机视觉开发中的一个关键决策，需要在准确性、推理速度和计算效率之间进行战略性平衡。这项比较深入探讨了YOLOv6-3.0（美团为工业吞吐量设计的模型）和YOLOv9（通过信息保存重新定义效率的SOTA架构）的技术细微之处。

YOLOv6-3.0高度关注硬件延迟是主要瓶颈的实际部署场景。

YOLOv6-3.0 被设计为一种硬件感知型卷积神经网络 (CNN)。该架构利用了高效的重参数化骨干网络和混合块 (RepBi-PAN)，以最大限度地提高 GPU 上的吞吐量。通过根据特定硬件特性定制模型结构，YOLOv6 旨在提供高推理速度而不会严重损害准确性。它作为一种单阶段检测器，针对工业自动化和监控进行了优化，其中实时处理是不可或缺的。

YOLOv9 引入了新颖的架构概念，解决了深度网络中信息丢失的根本问题，实现了卓越的性能指标。

YOLOv9 凭借两项突破性创新脱颖而出：可编程梯度信息 (PGI) 和 广义高效层聚合网络 (GELAN)。

深度网络在数据通过连续层时常常丢失信息，这种现象被称为信息瓶颈。YOLOv9 的 PGI 作为一种辅助监督机制，确保学习目标对象的关键数据在整个网络深度中得以保留。这显著提高了收敛性和准确性，尤其对于难以 detect 的对象。

将 YOLOv9 集成到 Ultralytics 生态系统为开发者提供了独特的优势：

性能数据突显了明显的区别：YOLOv6-3.0 针对特定硬件优化原始速度，而 YOLOv9 在效率（每参数 accuracy）方面占据主导地位。

例如，YOLOv9c 仅用 25.3M 参数就达到了 53.0% mAP，优于 YOLOv6-3.0l (52.8% mAP)，后者需要两倍以上的参数 (59.6M) 和显著更高的 FLOPs。这表明 YOLOv9 的架构创新（GELAN 和 PGI）使其能够“以更少的资源学习更多”，使其成为对精度仍有高要求但资源受限环境的理想高效选择。

相反，YOLOv6-3.0n 提供了极低的延迟（1.17 毫秒），使其适用于超高速实时推理，即使精度有所下降（37.5% mAP）也是可接受的。

这两种模型的开发者体验差异显著。YOLOv6-3.0通常依赖于涉及shell脚本和手动配置文件的特定仓库工作流。尽管功能强大，但这可能会给新手带来更陡峭的学习曲线。

相比之下，YOLOv9 受益于精简的 Ultralytics 工作流程。训练最先进的模型只需极少的代码，并且该生态系统支持无缝导出到 ONNX、TensorRT 和 CoreML 等格式，以实现广泛的部署兼容性。

Ultralytics python 接口允许仅用几行代码启动训练运行，自动处理数据增强、日志记录和评估。

Ultralytics 模型，包括 YOLOv9，支持一键导出为适用于边缘 AI 和云部署的各种格式。这种灵活性简化了从研究到生产的过渡。

尽管YOLOv6-3.0仍然是优先考虑特定硬件原始吞吐量的专业工业应用的强大工具，但YOLOv9对于大多数现代计算机视觉项目而言，是更卓越的选择。

YOLOv9 创新的 PGI 和 GELAN 架构在准确性和效率之间实现了更好的平衡，在每参数性能指标上通常超越 YOLOv6。此外，与Ultralytics 生态系统的集成确保开发者受益于简化的工作流程、积极的维护以及一套加速从数据到部署过程的工具。对于那些寻求面向未来、多功能且高性能模型的人来说，YOLOv9 是推荐的前进方向。

如果您正在探索最先进的选项，请考虑 Ultralytics 库中的这些其他强大模型：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv9 model
model = YOLO("yolov9c.pt")

# Train the model on your custom dataset
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)
```

---

## Ultralytics YOLO11 支持的计算机视觉任务

**URL:** https://docs.ultralytics.com/zh/tasks/

**Contents:**
- Ultralytics YOLO11 支持的计算机视觉任务
- 检测
- 图像分割
- 分类
- 姿势估计
- OBB
- 结论
- 常见问题
  - Ultralytics YOLO11 可以执行哪些计算机视觉任务？
  - 如何使用 Ultralytics YOLO11 进行目标检测？

Ultralytics YOLO11 是一个多功能 AI 框架，支持多种计算机视觉任务。该框架可用于执行检测、分割、旋转框检测、分类和姿势估计。这些任务中的每一个都有不同的目标和用例，使您能够使用单个框架解决各种计算机视觉挑战。

观看： 探索 Ultralytics YOLO 任务： 目标检测、分割、OBB、跟踪和姿势估计。

检测是YOLO11支持的主要任务。它涉及识别图像或视频帧中的目标，并在其周围绘制边界框。检测到的目标根据其特征被分类到不同的类别。YOLO11能够以高准确性和速度在单个图像或视频帧中检测多个目标，使其成为监控系统和自动驾驶车辆等实时应用的理想选择。

分割通过为每个对象生成像素级掩码，进一步推动了对象detect技术的发展。这种精度对于医学影像、农业分析和制造业质量控制等应用非常有用。

分类涉及根据图像内容对整个图像进行归类。此任务对于电子商务中的产品分类、内容审核和野生动物监测等应用至关重要。

姿势估计在图像或视频帧中 detect 特定关键点，以跟踪运动或估计姿势。这些关键点可以代表人体的关节、面部特征或其他重要的兴趣点。YOLO11 在关键点 detect 方面表现出色，具有高精度和高速，使其在健身应用、体育分析和人机交互方面具有重要价值。

定向边界框 (OBB) 检测通过添加方向角来更好地定位旋转对象，从而增强了传统的对象检测。此功能对于航空图像分析、文档处理和工业应用尤其有价值，在这些应用中，对象以各种角度出现。YOLO11 在各种场景中提供高精度和速度来检测旋转对象。

Ultralytics YOLO11 支持多种计算机视觉任务，包括检测、分割、分类、定向对象检测和关键点检测。每项任务都解决了计算机视觉领域中的特定需求，从基本的对象识别到详细的姿势分析。通过了解每项任务的功能和应用，您可以为特定的计算机视觉挑战选择最合适的方法，并利用 YOLO11 的强大功能来构建有效的解决方案。

Ultralytics YOLO11 是一个多功能的 AI 框架，能够以高精度和高速度执行各种计算机视觉任务。这些任务包括：

要使用 Ultralytics YOLO11 进行目标检测，请按照以下步骤操作：

将 YOLO11 用于分割任务具有以下几个优势：

在图像分割部分了解更多关于 YOLO11 在分割方面的优势和用例。

是的，Ultralytics YOLO11 能够以高精度和高速度有效地执行姿势估计和关键点检测。此功能对于跟踪体育分析、医疗保健和人机交互应用中的运动尤其有用。YOLO11 检测图像或视频帧中的关键点，从而实现精确的姿势估计。

有关更多详细信息和实施技巧，请访问我们的姿势估计示例。

使用 YOLO11 进行旋转框检测 (OBB) 通过检测具有附加角度参数的对象来提供更高的精度。此功能对于需要精确定位旋转对象的应用非常有用，例如航空图像分析和仓库自动化。

查看定向对象检测部分，了解更多详细信息和示例。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pretrained YOLO model (adjust model type as needed)
model = YOLO("yolo11n.pt")  # n, s, m, l, x versions available

# Perform object detection on an image
results = model.predict(source="image.jpg")  # Can also use video, directory, URL, etc.

# Display the results
results[0].show()  # Show the first image results
```

Example 2 (sql):
```sql
# Run YOLO detection from the command line
yolo detect model=yolo11n.pt source="image.jpg" # Adjust model and source as needed
```

---

## YOLOv10 对比 YOLOv6-3.0：实时目标检测的演进

**URL:** https://docs.ultralytics.com/zh/compare/yolov10-vs-yolov6/

**Contents:**
- YOLOv10 对比 YOLOv6-3.0：实时目标检测的演进
- YOLOv10：免 NMS 检测前沿
  - 架构与创新
- YOLOv6-3.0：工业级优化
  - 架构与创新
- 性能分析
  - 主要内容
- 集成与生态系统
  - Ultralytics的易用性
  - 多功能性与面向未来

选择合适的计算机视觉架构是一个关键决策，它会影响您AI项目的效率、准确性和可扩展性。随着目标检测领域的发展加速，开发人员常常需要在既定的工业标准和前沿创新之间做出选择。本指南提供了YOLOv10和YOLOv6-3.0这两款专为高性能应用设计的杰出模型之间的全面技术比较。

YOLOv10 代表了 YOLO 系列中的范式转变，专注于消除部署流水线中的瓶颈，以实现真正的实时端到端效率。由清华大学研究人员开发，它引入了架构更改，消除了对非极大值抑制 (NMS) 的需求，NMS 是传统上会增加延迟的常见后处理步骤。

YOLOv10 通过以下几个关键机制优化了推理延迟和模型性能：

YOLOv6-3.0（通常简称为YOLOv6）于2023年初发布，由美团专门为工业应用而开发。它优先采用硬件友好的设计，以最大化GPU上的吞吐量，使其成为工厂自动化和大规模视频处理的有力候选者。

YOLOv6-3.0通过激进的结构调优，专注于优化速度和准确性之间的权衡：

YOLOv6被明确设计为“硬件友好型”，旨在NVIDIA GPU（如T4和V100）上实现优化性能。这使得它在具备特定硬件加速并经过调优的场景中尤其有效。

以下比较使用了来自COCO数据集的指标，这是一个目标检测的标准基准。该表格突出了YOLOv10在参数效率和准确性方面如何突破极限。

最显著的区别之一在于生态系统和易用性。尽管 YOLOv6 是一个强大的独立代码库，但 YOLOv10 受益于集成到 Ultralytics 生态系统中。这为开发者提供了从数据标注到部署的无缝工作流程。

使用 Ultralytics 模型可确保您能够访问标准化、简单的 Python API。您可以在YOLOv8和 YOLOv10 等模型之间进行切换，只需最少的代码更改，这种灵活性在不同框架之间切换时并不容易获得。

尽管 YOLOv6-3.0 主要侧重于 detect，但 Ultralytics 框架支持更广泛的计算机视觉任务，包括 segment、分类和姿势估计。对于需要多任务能力的用户，升级到YOLO11通常是推荐的途径，因为它在同一统一 API 中为所有这些模态提供了最先进的性能。

使用 Ultralytics 进行训练，您可以利用自动 超参数调整 等功能，以及通过 TensorBoard 或 Weights & Biases 进行实时日志记录，从而大幅加速从研究到生产的周期。

尽管YOLOv6-3.0在发布时是工业对象检测的强大基准，但YOLOv10代表了视觉AI演进的下一步。凭借其无NMS架构、大幅减少的参数数量和更高的精度，YOLOv10为现代计算机视觉挑战提供了更高效、更可扩展的解决方案。

对于寻求在 detect、segment 和姿势估计方面最新多功能性和性能的开发者而言，我们也推荐探索 YOLO11。作为活跃维护的 Ultralytics 生态系统的一部分，这些模型确保您在强大的社区支持和持续改进下，始终走在AI创新的前沿。

如需进一步了解模型比较，请查看我们对 YOLOv10 与 YOLOv8 的分析，或探索 RT-DETR 在基于 Transformer 的 detect 方面的能力。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv10 model
model = YOLO("yolov10n.pt")

# Train the model on your custom data
model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference on an image
results = model.predict("path/to/image.jpg")
```

---

## KITTI 数据集 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/datasets/detect/kitti/

**Contents:**
- KITTI 数据集
- 数据集结构
- 应用
- 数据集 YAML
- 用法
- Sample Images 和注释
- 引用和致谢
- 常见问题
  - kitti数据集用于什么？
  - KITTI数据集中包含多少图像？

KITTI 数据集是自动驾驶和计算机视觉领域最具影响力的基准数据集之一。它由卡尔斯鲁厄理工学院和芝加哥丰田技术研究所发布，包含从真实驾驶场景中收集的立体相机、激光雷达和 GPS/IMU 数据。

观看： 如何在 KITTI 数据集上训练 Ultralytics YOLO11 🚀

它广泛用于评估目标检测、深度估计、光流和视觉里程计中的算法。该数据集与 Ultralytics YOLO11 完全兼容，适用于 2D 目标检测任务，并且可以轻松集成到 Ultralytics 平台进行训练和评估。

Kitti 原始测试集在此处被排除，因为它不包含真实标注。

总计，该数据集包含7,481张图像，每张图像都配有详细的标注，用于汽车、行人、骑行者和其他道路元素等对象。该数据集分为两个主要子集：

KITTI 数据集推动了自动驾驶和机器人技术的发展，支持以下任务：

Ultralytics 使用 YAML 文件定义 kitti 数据集配置。该文件指定了训练所需的数据集路径、类别标签和元数据。配置文件可在 https://github.com/ultralytics/ultralytics/blob/main/ultralytics/cfg/datasets/kitti.yaml 获取。

Ultralytics/cfg/datasets/kitti.yaml

要在 kitti 数据集上训练 YOLO11n 模型，进行 100 个 epoch，图像尺寸为 640，请使用以下命令。有关更多详细信息，请参阅训练页面。

您还可以使用相同的配置文件，直接通过命令行或 Python API 执行评估、推理和导出任务。

kitti数据集提供了多样化的驾驶场景。每张图像都包含用于2D目标检测任务的边界框标注。这些示例展示了数据集的丰富多样性，使模型能够在各种真实世界条件下实现鲁棒的泛化。

如果您在研究中使用kitti数据集，请引用以下论文：

我们感谢 KITTI 视觉基准套件提供了这个全面的数据集，该数据集持续推动着计算机视觉、机器人技术和自主系统领域的发展。访问kitti 网站了解更多信息。

kitti数据集主要用于自动驾驶领域的计算机视觉研究，支持目标检测、深度估计、光流和3D定位等任务。

该数据集包含5,985张带标签的训练图像和1,496张验证图像，这些图像捕获自城市、乡村和高速公路场景。原始测试集在此处被排除，因为它不包含真实标注。

Kitti 包含对诸如汽车、行人、骑行者、卡车、有轨电车以及其他道路使用者等对象的标注。

是的，kitti 完全兼容 Ultralytics YOLO11。您可以直接使用提供的 YAML 配置文件训练和验证模型。

您可以访问位于https://github.com/ultralytics/ultralytics/blob/main/ultralytics/cfg/datasets/kitti.yaml的yaml文件。

**Examples:**

Example 1 (yaml):
```yaml
# Ultralytics 🚀 AGPL-3.0 License - https://ultralytics.com/license

# KITTI dataset by Karlsruhe Institute of Technology and Toyota Technological Institute at Chicago
# Documentation: https://docs.ultralytics.com/datasets/detect/kitti/
# Example usage: yolo train data=kitti.yaml
# parent
# ├── ultralytics
# └── datasets
#     └── kitti ← downloads here (390.5 MB)

# Train/val/test sets as 1) dir: path/to/imgs, 2) file: path/to/imgs.txt, or 3) list: [path/to/imgs1, path/to/imgs2, ..]
path: kitti # dataset root dir
train: images/train # train images (relative to 'path') 5985 images
val: images/val # val images (relative to 'path') 1496 images

names:
  0: car
  1: van
  2: truck
  3: pedestrian
  4: person_sitting
  5: cyclist
  6: tram
  7: misc

# Download script/URL (optional)
download: https://github.com/ultralytics/assets/releases/download/v0.0.0/kitti.zip
```

Example 2 (python):
```python
from ultralytics import YOLO

# Load a pretrained YOLO11 model
model = YOLO("yolo11n.pt")

# Train on kitti dataset
results = model.train(data="kitti.yaml", epochs=100, imgsz=640)
```

Example 3 (unknown):
```unknown
yolo detect train data=kitti.yaml model=yolo11n.pt epochs=100 imgsz=640
```

Example 4 (python):
```python
@article{Geiger2013IJRR,
  author = {Andreas Geiger and Philip Lenz and Christoph Stiller and Raquel Urtasun},
  title = {Vision meets Robotics: The KITTI Dataset},
  journal = {International Journal of Robotics Research (IJRR)},
  year = {2013}
}
```

---

## DAMO-YOLO vs. YOLOX：技术对比 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/compare/damo-yolo-vs-yolox/

**Contents:**
- DAMO-YOLO vs. YOLOX：技术对比
- DAMO-YOLO：高速推理优化
  - 架构与创新
  - 优势与理想用例
- YOLOX：无锚框先驱
  - 主要架构特性
  - 优势与理想用例
- 性能分析
  - 主要内容
- Ultralytics 优势

在快速发展的计算机视觉领域，选择合适的物体 detect 模型对于任何 AI 项目的成功都至关重要。本文深入比较了两种有影响力的架构：由阿里巴巴集团开发的 DAMO-YOLO 和由旷视科技创建的 YOLOX。这两个模型都为该领域做出了重大贡献，突破了速度和准确性的界限。我们将探讨它们独特的架构、性能指标和理想用例，以帮助您做出明智的决策。

DAMO-YOLO 代表了实时目标检测领域的一大飞跃，它优先考虑在 GPU 硬件上实现低延迟而不牺牲准确性。由阿里巴巴的研究人员开发，它集成了尖端神经网络设计原则，以实现令人印象深刻的速度-精度权衡。

DAMO-YOLO 的架构基于多项创新技术，旨在最大限度地提高效率：

DAMO-YOLO 在对 实时性能 有严格要求的场景中表现出色。其架构优化使其成为需要高吞吐量的工业应用的有力竞争者。

YOLOX通过摆脱基于锚框的机制，标志着YOLO系列的一个关键时刻。由旷视科技开发，它引入了无锚框设计，简化了detect流程并提高了泛化能力，为2021年的性能设定了新标准。

YOLOX以其稳健的设计理念而著称，该理念解决了早期YOLO版本中的常见问题：

YOLOX以其高准确性和稳定性而闻名，使其成为对精度要求极高的应用的可靠选择。

下表直接比较了不同模型尺寸的 DAMO-YOLO 和 YOLOX。这些指标突出了在 COCO 数据集上，模型复杂度（参数量和 FLOPs）、推理速度和检测准确性 (mAP) 之间的权衡。

DAMO-YOLO 大量采用重参数化和高效的颈部结构，使其特别适合在 NVIDIA GPU 上进行 TensorRT 部署，在此环境下，它能充分利用并行计算能力。

尽管DAMO-YOLO和YOLOX提供了强大的能力，但Ultralytics YOLO模型——特别是YOLO11——为现代计算机视觉开发提供了卓越的综合解决方案。Ultralytics构建了一个生态系统，不仅关注原始性能，还关注机器学习操作的整个生命周期。

开发者和研究人员正越来越多地转向Ultralytics模型，原因如下：

通过此 Python 示例体验 Ultralytics 工作流的简洁性：

DAMO-YOLO和YOLOX都在目标检测的历史上奠定了自己的地位。DAMO-YOLO是专门的高吞吐量GPU应用的绝佳选择，在这些应用中，每一毫秒的延迟都至关重要。YOLOX仍然是一个坚实、准确的无锚点检测器，在研究社区中广受理解。

然而，对于绝大多数实际应用而言，Ultralytics YOLO11 脱颖而出，成为首选。它结合了最先进的性能、多任务通用性以及用户友好且维护良好的生态系统，使开发人员能够更快、更高效地构建强大的解决方案。无论您是部署到云端还是边缘，Ultralytics 都提供了在当今竞争激烈的AI领域取得成功所需的工具。

为了进一步了解目标检测领域，请探索这些模型如何与其他最先进的架构进行比较：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load the YOLO11 model (pre-trained on COCO)
model = YOLO("yolo11n.pt")

# Train the model on your custom dataset
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference on an image
results = model("path/to/image.jpg")
```

---

## 使用 Ultralytics YOLO11 实现对象模糊 🚀 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/guides/object-blurring/

**Contents:**
- 使用 Ultralytics YOLO11 的对象模糊处理 🚀
- 什么是目标模糊处理？
- 对象模糊的优势
  - ObjectBlurrer 参数
- 真实世界的应用
  - 监控中的隐私保护
  - 医疗保健数据匿名化
  - 文档编辑
  - 媒体和内容创作
- 常见问题

使用 Ultralytics YOLO11 的对象模糊处理涉及对图像或视频中特定的检测到的对象应用模糊效果。这可以通过使用 YOLO11 模型的功能来识别和操作给定场景中的对象来实现。

观看： 使用 Ultralytics YOLO11 的对象模糊处理

使用 Ultralytics YOLO 进行对象模糊处理

这是一个包含以下内容的表格 ObjectBlurrer 参数：

字段 ObjectBlurrer 解决方案还支持一系列 track 参数：

安全摄像头 和监控系统可以使用 YOLO11 自动模糊面部、车牌或其他识别信息，同时仍然捕获重要的活动。这有助于在公共场所维护安全，同时尊重隐私权。

在医学影像中，患者信息常出现在扫描或照片中。YOLO11 可以 detect 并模糊这些信息，以便在为研究或教育目的共享医疗数据时遵守 HIPAA 等法规。

当共享包含敏感信息的文档时，YOLO11 可以自动 detect 并模糊特定元素，如签名、账号或个人详细信息，从而简化编辑过程，同时保持文档的完整性。

内容创作者可以使用 YOLO11 模糊视频和图像中的品牌徽标、受版权保护的材料或不当内容，从而帮助避免法律问题，同时保持整体内容质量。

使用 Ultralytics YOLO11 的对象模糊处理涉及自动检测并对图像或视频中的特定对象应用模糊效果。此技术通过隐藏敏感信息同时保留相关的视觉数据来增强隐私。YOLO11 的实时处理能力使其适用于需要即时隐私保护和选择性焦点调整的应用。

要使用 YOLO11 实现实时对象模糊，请遵循提供的 python 示例。这涉及使用 YOLO11 进行目标检测，并使用 OpenCV 应用模糊效果。以下是一个简化版本：

Ultralytics YOLO11 在对象模糊处理方面具有以下几个优势：

有关更详细的应用，请查看对象模糊处理部分的优势。

是的，Ultralytics YOLO11 可以配置为 detect 并模糊视频中的人脸以保护隐私。通过训练或使用预训练模型专门识别人脸，可以使用 OpenCV 处理 detect 结果以应用模糊效果。请参阅我们关于使用 YOLO11 进行目标 detect 的指南，并修改代码以针对人脸 detect。

Ultralytics YOLO11 通常在速度上优于 Faster R-CNN 等模型，使其更适合实时应用。虽然这两种模型都提供准确的检测，但 YOLO11 的架构经过优化，可实现快速推理，这对于实时对象模糊等任务至关重要。请在我们的YOLO11 文档中了解有关技术差异和性能指标的更多信息。

**Examples:**

Example 1 (markdown):
```markdown
# Blur the objects
yolo solutions blur show=True

# Pass a source video
yolo solutions blur source="path/to/video.mp4"

# Blur the specific classes
yolo solutions blur classes="[0, 5]"
```

Example 2 (python):
```python
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "Error reading video file"

# Video writer
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))
video_writer = cv2.VideoWriter("object_blurring_output.avi", cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))

# Initialize object blurrer
blurrer = solutions.ObjectBlurrer(
    show=True,  # display the output
    model="yolo11n.pt",  # model for object blurring, e.g., yolo11m.pt
    # line_width=2,  # width of bounding box.
    # classes=[0, 2],  # blur specific classes, e.g., person and car with the COCO pretrained model.
    # blur_ratio=0.5,  # adjust percentage of blur intensity, value in range 0.1 - 1.0
)

# Process video
while cap.isOpened():
    success, im0 = cap.read()

    if not success:
        print("Video frame is empty or processing is complete.")
        break

    results = blurrer(im0)

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
assert cap.isOpened(), "Error reading video file"
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))

# Video writer
video_writer = cv2.VideoWriter("object_blurring_output.avi", cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))

# Init ObjectBlurrer
blurrer = solutions.ObjectBlurrer(
    show=True,  # display the output
    model="yolo11n.pt",  # model="yolo11n-obb.pt" for object blurring using YOLO11 OBB model.
    blur_ratio=0.5,  # set blur percentage, e.g., 0.7 for 70% blur on detected objects
    # line_width=2,  # width of bounding box.
    # classes=[0, 2],  # count specific classes, e.g., person and car with the COCO pretrained model.
)

# Process video
while cap.isOpened():
    success, im0 = cap.read()
    if not success:
        print("Video frame is empty or processing is complete.")
        break
    results = blurrer(im0)
    video_writer.write(results.plot_im)

cap.release()
video_writer.release()
cv2.destroyAllWindows()
```

---

## VisDrone 数据集 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/datasets/detect/visdrone/

**Contents:**
- VisDrone 数据集
- 数据集结构
- 应用
- 数据集 YAML
- 用法
- 样本数据和注释
- 引用和致谢
- 常见问题
  - 什么是 VisDrone 数据集，它的主要功能是什么？
  - 如何使用 VisDrone 数据集通过 Ultralytics 训练 YOLO11 模型？

VisDrone 数据集 是由中国天津大学机器学习和数据挖掘实验室 AISKYEYE 团队创建的大规模基准。它包含用于与无人机图像和视频分析相关的各种计算机视觉任务的，经过仔细标注的真实数据。

观看： 如何在 VisDrone 数据集上训练 Ultralytics YOLO11 | 航空 detect | 完整教程 🚀

VisDrone 由 288 个视频片段（包含 261,908 帧）和 10,209 张静态图像组成，这些数据由各种无人机载摄像头拍摄。该数据集涵盖了广泛的方面，包括地点（中国 14 个不同的城市）、环境（城市和乡村）、物体（行人、车辆、自行车等）和密度（稀疏和拥挤的场景）。该数据集是在不同的场景以及天气和光照条件下，使用各种无人机平台收集的。这些帧通过手动方式进行了标注，包含超过 260 万个目标的边界框，例如行人、汽车、自行车和三轮车。此外，还提供了场景可见性、物体类别和遮挡等属性，以更好地利用数据。

VisDrone 数据集被组织成五个主要子集，每个子集都侧重于一个特定的任务：

VisDrone 数据集被广泛用于训练和评估基于无人机的计算机视觉任务（如物体检测、物体跟踪和人群计数）中的深度学习模型。该数据集包含各种传感器数据、物体注释和属性，使其成为无人机计算机视觉领域研究人员和从业人员的宝贵资源。

YAML（Yet Another Markup Language）文件用于定义数据集配置。它包含有关数据集路径、类别和其他相关信息。对于 Visdrone 数据集， VisDrone.yaml 文件保存在 https://github.com/ultralytics/ultralytics/blob/main/ultralytics/cfg/datasets/VisDrone.yaml.

Ultralytics/cfg/datasets/VisDrone.yaml

要在 VisDrone 数据集上训练 YOLO11n 模型 100 个 epochs，图像大小为 640，您可以使用以下代码片段。有关可用参数的完整列表，请参阅模型训练页面。

VisDrone 数据集包含由无人机载摄像头拍摄的各种图像和视频。以下是数据集中一些数据的示例，以及它们对应的注释：

该示例展示了 VisDrone 数据集中数据的多样性和复杂性，并强调了高质量传感器数据对于基于无人机的计算机视觉任务的重要性。

如果您在研究或开发工作中使用 VisDrone 数据集，请引用以下论文：

我们要感谢中国天津大学机器学习与数据挖掘实验室的 AISKYEYE 团队创建并维护 VisDrone 数据集，使其成为无人机计算机视觉研究领域的一项宝贵资源。有关 VisDrone 数据集及其创建者的更多信息，请访问 VisDrone 数据集 GitHub 存储库。

VisDrone 数据集是由中国天津大学 AISKYEYE 团队创建的大规模基准。它专为与基于无人机的图像和视频分析相关的各种计算机视觉任务而设计。主要功能包括：

要在 VisDrone 数据集上训练 YOLO11 模型 100 个 epochs，图像大小为 640，您可以按照以下步骤操作：

VisDrone 数据集分为五个主要子集，每个子集都针对特定的计算机视觉任务定制：

这些子集广泛用于训练和评估无人机应用（如监控、交通监测和公共安全）中的深度学习模型。

VisDrone数据集的配置文件， VisDrone.yaml可以在 Ultralytics 仓库中的以下链接找到： VisDrone.yaml.

如果您在研究或开发工作中使用 VisDrone 数据集，请引用以下论文：

**Examples:**

Example 1 (python):
```python
# Ultralytics 🚀 AGPL-3.0 License - https://ultralytics.com/license

# VisDrone2019-DET dataset https://github.com/VisDrone/VisDrone-Dataset by Tianjin University
# Documentation: https://docs.ultralytics.com/datasets/detect/visdrone/
# Example usage: yolo train data=VisDrone.yaml
# parent
# ├── ultralytics
# └── datasets
#     └── VisDrone ← downloads here (2.3 GB)

# Train/val/test sets as 1) dir: path/to/imgs, 2) file: path/to/imgs.txt, or 3) list: [path/to/imgs1, path/to/imgs2, ..]
path: VisDrone # dataset root dir
train: images/train # train images (relative to 'path') 6471 images
val: images/val # val images (relative to 'path') 548 images
test: images/test # test-dev images (optional) 1610 images

# Classes
names:
  0: pedestrian
  1: people
  2: bicycle
  3: car
  4: van
  5: truck
  6: tricycle
  7: awning-tricycle
  8: bus
  9: motor

# Download script/URL (optional) ---------------------------------------------------------------------------------------
download: |
  import os
  from pathlib import Path
  import shutil

  from ultralytics.utils.downloads import download
  from ultralytics.utils import ASSETS_URL, TQDM


  def visdrone2yolo(dir, split, source_name=None):
      """Convert VisDrone annotations to YOLO format with images/{split} and labels/{split} structure."""
      from PIL import Image

      source_dir = dir / (source_name or f"VisDrone2019-DET-{split}")
      images_dir = dir / "images" / split
      labels_dir = dir / "labels" / split
      labels_dir.mkdir(parents=True, exist_ok=True)

      # Move images to new structure
      if (source_images_dir := source_dir / "images").exists():
          images_dir.mkdir(parents=True, exist_ok=True)
          for img in source_images_dir.glob("*.jpg"):
              img.rename(images_dir / img.name)

      for f in TQDM((source_dir / "annotations").glob("*.txt"), desc=f"Converting {split}"):
          img_size = Image.open(images_dir / f.with_suffix(".jpg").name).size
          dw, dh = 1.0 / img_size[0], 1.0 / img_size[1]
          lines = []

          with open(f, encoding="utf-8") as file:
              for row in [x.split(",") for x in file.read().strip().splitlines()]:
                  if row[4] != "0":  # Skip ignored regions
                      x, y, w, h = map(int, row[:4])
                      cls = int(row[5]) - 1
                      # Convert to YOLO format
                      x_center, y_center = (x + w / 2) * dw, (y + h / 2) * dh
                      w_norm, h_norm = w * dw, h * dh
                      lines.append(f"{cls} {x_center:.6f} {y_center:.6f} {w_norm:.6f} {h_norm:.6f}\n")

          (labels_dir / f.name).write_text("".join(lines), encoding="utf-8")


  # Download (ignores test-challenge split)
  dir = Path(yaml["path"])  # dataset root dir
  urls = [
      f"{ASSETS_URL}/VisDrone2019-DET-train.zip",
      f"{ASSETS_URL}/VisDrone2019-DET-val.zip",
      f"{ASSETS_URL}/VisDrone2019-DET-test-dev.zip",
      # f"{ASSETS_URL}/VisDrone2019-DET-test-challenge.zip",
  ]
  download(urls, dir=dir, threads=4)

  # Convert
  splits = {"VisDrone2019-DET-train": "train", "VisDrone2019-DET-val": "val", "VisDrone2019-DET-test-dev": "test"}
  for folder, split in splits.items():
      visdrone2yolo(dir, split, folder)  # convert VisDrone annotations to YOLO labels
      shutil.rmtree(dir / folder)  # cleanup original directory
```

Example 2 (python):
```python
from ultralytics import YOLO

# Load a model
model = YOLO("yolo11n.pt")  # load a pretrained model (recommended for training)

# Train the model
results = model.train(data="VisDrone.yaml", epochs=100, imgsz=640)
```

Example 3 (sql):
```sql
# Start training from a pretrained *.pt model
yolo detect train data=VisDrone.yaml model=yolo11n.pt epochs=100 imgsz=640
```

Example 4 (python):
```python
@ARTICLE{9573394,
  author={Zhu, Pengfei and Wen, Longyin and Du, Dawei and Bian, Xiao and Fan, Heng and Hu, Qinghua and Ling, Haibin},
  journal={IEEE Transactions on Pattern Analysis and Machine Intelligence},
  title={Detection and Tracking Meet Drones Challenge},
  year={2021},
  volume={},
  number={},
  pages={1-1},
  doi={10.1109/TPAMI.2021.3119563}}
```

---

## YOLO11 与 YOLOv5：最先进目标检测的演进

**URL:** https://docs.ultralytics.com/zh/compare/yolo11-vs-yolov5/

**Contents:**
- YOLO11 与 YOLOv5：最先进目标检测的演进
- 性能分析
  - 关键性能指标
  - 精度与效率
- 模型架构与设计
  - YOLOv5：久经考验的标准
  - YOLO11：尖端技术
  - 比较表：技术规格
- 训练与生态系统
  - 训练效率

实时目标检测的发展深受 Ultralytics YOLO 系列的影响。YOLOv5 于 2020 年发布，在易用性、速度和可靠性方面树立了全球标准，成为历史上部署最广泛的视觉 AI 模型之一。最新迭代的 YOLO11 在这一传奇基础上进一步发展，提供了前所未有的准确性、效率和多功能性。

本指南对这两个强大的模型进行了详细的技术比较，帮助开发人员和研究人员理解各自的架构转变、性能提升和理想用例。

YOLO11 和 YOLOv5 之间的性能差距突显了神经网络设计的快速进步。虽然 YOLOv5 仍然是一个有能力的模型，但 YOLO11 在所有模型规模上始终优于它，尤其是在 CPU 推理速度和 detect accuracy 方面。

下表展示了在COCO数据集上的直接对比。一个关键的观察是YOLO11n的效率，它实现了39.5 mAP，显著超越了YOLOv5n的28.0 mAP，同时在CPU硬件上运行更快。

YOLO11 代表了“效率与准确性”权衡中的范式转变。

最显著的技术差异之一是检测头机制。YOLOv5 采用基于锚点的方法，这需要预定义的锚框，必须针对特定数据集进行调整才能实现最佳性能。

YOLO11采用了无锚点设计。这消除了手动计算锚框的需要，简化了训练流程，并在无需超参数调优的情况下提高了在多样化数据集上的泛化能力。

这两种模型之间的架构差异反映了 计算机视觉 研究在过去几年中的进展。

YOLOv5YOLOv5 引入了用户友好的PyTorch 实现，使大众也能进行对象检测。

YOLO11集成了最新的深度学习技术，以最大化特征重用并最小化计算开销。

两种模型都受益于强大的Ultralytics 生态系统，该系统提供无缝的数据管理、训练和部署工具。

YOLO11 的设计旨在比 YOLOv5 训练更快、收敛更快。

YOLO11 的训练通过...得到简化 ultralytics Python 包。以下示例演示了如何在 COCO8 数据集上训练 YOLO11n 模型。

尽管 YOLOv5 因其发布时间较早而拥有大量的第三方教程，但 YOLO11 原生集成到现代 Ultralytics 包中。这提供了对高级功能的即时访问：

选择合适的模型取决于您项目的具体限制和要求。

YOLO11 是95% 新项目的推荐选择。

YOLOv5 仍然是适用于特定传统场景的可行选择。

从 YOLOv5 迁移到 YOLO11 非常简单。数据集格式 (YOLO TXT) 保持不变，这意味着您可以无需修改地重用现有标注数据集。python API 结构也非常相似，通常只需更改模型名称字符串（例如，从 yolov5su.pt 到 yolo11n.pt 在 ultralytics 包)。

Ultralytics 支持除了 YOLO11 和 YOLOv5 之外的广泛模型。根据您的具体需求，您可以考虑：

从 YOLOv5 到 YOLO11 的转变标志着计算机视觉历史上的一个重要里程碑。YOLOv5 普及了人工智能，使目标检测人人可及。YOLO11 则完善了这一愿景，提供了一个更快、更轻、更准确的模型。

对于寻求最佳每瓦性能和最多功能特性集的开发者而言，YOLO11 显然是赢家。它与活跃的 Ultralytics 生态系统的集成确保您能够访问最新的工具、简单的API以及一个蓬勃发展的社区，以支持您的AI之旅。

准备升级？查阅YOLO11 文档或探索GitHub 仓库，立即开始。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLO11n model
model = YOLO("yolo11n.pt")

# Train the model
# The device argument can be 'cpu', 0 for GPU, or [0, 1] for multi-GPU
results = model.train(data="coco8.yaml", epochs=100, imgsz=640, device=0)
```

---

## EfficientDet 对比 YOLOv9：目标检测效率的演进

**URL:** https://docs.ultralytics.com/zh/compare/efficientdet-vs-yolov9/

**Contents:**
- EfficientDet 对比 YOLOv9：目标检测效率的演进
- EfficientDet：开创性的可扩展效率
  - 架构和主要特性
  - 优势与劣势
  - 应用案例
- YOLOv9：重新定义实时性能
  - 架构和主要特性
  - 优势与优点
- 性能分析
  - 关键基准洞察

在快节奏的计算机视觉领域，选择正确的模型架构对于平衡性能、速度和计算资源至关重要。本指南Google 研究院开发的具有里程碑意义的模型EfficientDet 和 YOLOv9 进行了全面的技术比较。 YOLOv9之间进行了全面Ultralytics 技术比较。我们将分析它们的架构创新、性能指标基准，并确定哪种模型最适合现代实时对象检测应用。

EfficientDet 于2019年末发布，引入了一种系统化的模型扩展方法，影响了后续多年的研究。该模型由谷歌研究团队开发，旨在不牺牲准确性的前提下优化效率。

EfficientDet 基于 EfficientNet 主干网络构建，并引入了 双向特征金字塔网络 (BiFPN)。与传统 FPN 不同，BiFPN 通过引入可学习的权重来学习不同输入特征的重要性，从而实现简单快速的多尺度特征融合。该模型采用 复合缩放方法，同时统一缩放所有主干网络、特征网络以及边界框/类别预测网络的分辨率、深度和宽度。

EfficientDet 因其能够以比 YOLOv3 等同期模型更少的参数实现高精度而具有革命性意义。其主要优势在于其可扩展性；模型家族（D0 到 D7）允许用户选择特定的资源权衡。

然而，以现代标准衡量，EfficientDet的推理速度较慢，尤其是在GPU硬件上。其复杂的特征融合层虽然准确，但不如新架构对硬件友好。此外，原始实现缺乏现代框架中常见的用户友好工具，使得训练和部署更加耗时耗力。

了解更多关于 EfficientDet 的信息

于 2024 年初推出，YOLOv9 代表了 YOLO 系列的飞跃，解决了深度学习信息瓶颈问题，以实现卓越的效率。它在Ultralytics python 包中得到全面支持，确保为开发者提供无缝体验。

YOLOv9 引入了两项开创性概念：可编程梯度信息 (PGI) 和 广义高效层聚合网络 (GELAN)。

YOLOv9 的GELAN 架构设计为与硬件无关，这意味着它能高效运行在各种推理设备上，从边缘 TPU 到高端 NVIDIA GPU，而无需像某些基于 Transformer 的模型那样进行特定的硬件优化。

以下比较突出了YOLOv9与EfficientDet系列相比，在推理速度和效率方面带来的显著改进。

这两种模型之间最显著的区别之一是它们所围绕的生态系统。尽管 EfficientDet 依赖于较旧的 TensorFlow 代码库，但 YOLOv9 是 Ultralytics 库中的一等公民。

将YOLOv9与Ultralytics结合使用，可提供一个维护良好的生态系统，从而简化整个机器学习生命周期。从标注数据集到部署到边缘设备，工作流程都得到了简化。

以下是一个实用示例，展示了使用Ultralytics Python API运行YOLOv9推理是多么容易：

尽管EfficientDet严格来说是一个目标检测器，但YOLOv9和Ultralytics框架背后的架构原则支持更广泛的视觉任务。用户可以在同一代码库中轻松切换目标检测、实例分割和姿势估计，从而减少复杂项目的技术债务。

在比较 EfficientDet 与 YOLOv9 时，现代计算机视觉开发的选择是明确的。虽然 EfficientDet 在定义模型缩放效率方面发挥了历史性作用，但 YOLOv9 在当今与开发者相关的几乎所有指标上都超越了它。

YOLOv9提供卓越的每参数精度、快几个数量级的推理速度，以及一个强大且开发者友好的生态系统。无论您是部署到受限的边缘设备，还是在云端处理高吞吐量视频流，YOLOv9都能提供成功所需的性能平衡。

对于那些开始新项目的人，我们强烈建议利用 YOLOv9 或最新的YOLO11，以确保您的应用程序受益于深度学习效率的最新进展。

如果您对探索 Ultralytics 系列中的更多选项感兴趣，可以考虑这些模型：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv9 compact model
model = YOLO("yolov9c.pt")

# Run inference on an image
results = model("path/to/image.jpg")

# Process results
for result in results:
    result.show()  # Display predictions
    result.save()  # Save image to disk
```

---

## Reference for ultralytics/utils/plotting.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/plotting/

**Contents:**
- Reference for ultralytics/utils/plotting.py
- class ultralytics.utils.plotting.Colors
- Ultralytics Color Palette
- Pose Color Palette
  - method ultralytics.utils.plotting.Colors.__call__
  - method ultralytics.utils.plotting.Colors.hex2rgb
- class ultralytics.utils.plotting.Annotator
  - method ultralytics.utils.plotting.Annotator.box_label
  - method ultralytics.utils.plotting.Annotator.fromarray
  - method ultralytics.utils.plotting.Annotator.get_bbox_dimension

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/plotting.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Ultralytics color palette for visualization and plotting.

This class provides methods to work with the Ultralytics color palette, including converting hex color codes to RGB values and accessing predefined color schemes for object detection and pose estimation.

Ultralytics Brand Colors

For Ultralytics brand colors see https://www.ultralytics.com/brand. Please use the official Ultralytics colors for all marketing materials.

Convert hex color codes to RGB values.

Convert hex color codes to RGB values (i.e. default PIL order).

Ultralytics Annotator for train/val mosaics and JPGs and predictions annotations.

Draw a bounding box on an image with a given label.

Update self.im from a NumPy array or PIL image.

Calculate the dimensions and area of a bounding box.

Assign text color based on background color.

Plot keypoints on the image.

Add rectangle to image (PIL-only).

Return annotated image as array or PIL image.

Save the annotated image to 'filename'.

Show the annotated image.

Add text to an image using PIL or cv2.

Plot training labels including class histograms and box statistics.

Save image crop as {file} with crop size multiple {gain} and {pad} pixels. Save and/or return crop.

This function takes a bounding box and an image, and then saves a cropped portion of the image according to the bounding box. Optionally, the crop can be squared, and the function allows for gain and padding adjustments to the bounding box.

Plot image grid with labels, bounding boxes, masks, and keypoints.

This function supports both tensor and numpy array inputs. It will automatically convert tensor inputs to numpy arrays for processing.

Channel Support: - 1 channel: Grayscale - 2 channels: Third channel added as zeros - 3 channels: Used as-is (standard RGB) - 4+ channels: Cropped to first 3 channels

Plot training results from a results CSV file. The function supports various types of data including

segmentation, pose estimation, and classification. Plots are saved as 'results.png' in the directory where the CSV is located.

Plot a scatter plot with points colored based on a 2D histogram.

Plot the evolution results stored in a 'tune_results.csv' file. The function generates a scatter plot for each

key in the CSV, color-coded based on fitness scores. The best-performing configurations are highlighted on the plots.

Visualize feature maps of a given model module during inference.

**Examples:**

Example 1 (unknown):
```unknown
Colors(self)
```

Example 2 (sql):
```sql
>>> from ultralytics.utils.plotting import Colors
>>> colors = Colors()
>>> colors(5, True)  # Returns BGR format: (221, 111, 255)
>>> colors(5, False)  # Returns RGB format: (255, 111, 221)
```

Example 3 (python):
```python
class Colors:
    """Ultralytics color palette for visualization and plotting.

    This class provides methods to work with the Ultralytics color palette, including converting hex color codes to RGB
    values and accessing predefined color schemes for object detection and pose estimation.

    ## Ultralytics Color Palette

    | Index | Color                                                             | HEX       | RGB               |
    |-------|-------------------------------------------------------------------|-----------|-------------------|
    | 0     | <i class="fa-solid fa-square fa-2xl" style="color: #042aff;"></i> | `#042aff` | (4, 42, 255)      |
    | 1     | <i class="fa-solid fa-square fa-2xl" style="color: #0bdbeb;"></i> | `#0bdbeb` | (11, 219, 235)    |
    | 2     | <i class="fa-solid fa-square fa-2xl" style="color: #f3f3f3;"></i> | `#f3f3f3` | (243, 243, 243)   |
    | 3     | <i class="fa-solid fa-square fa-2xl" style="color: #00dfb7;"></i> | `#00dfb7` | (0, 223, 183)     |
    | 4     | <i class="fa-solid fa-square fa-2xl" style="color: #111f68;"></i> | `#111f68` | (17, 31, 104)     |
    | 5     | <i class="fa-solid fa-square fa-2xl" style="color: #ff6fdd;"></i> | `#ff6fdd` | (255, 111, 221)   |
    | 6     | <i class="fa-solid fa-square fa-2xl" style="color: #ff444f;"></i> | `#ff444f` | (255, 68, 79)     |
    | 7     | <i class="fa-solid fa-square fa-2xl" style="color: #cced00;"></i> | `#cced00` | (204, 237, 0)     |
    | 8     | <i class="fa-solid fa-square fa-2xl" style="color: #00f344;"></i> | `#00f344` | (0, 243, 68)      |
    | 9     | <i class="fa-solid fa-square fa-2xl" style="color: #bd00ff;"></i> | `#bd00ff` | (189, 0, 255)     |
    | 10    | <i class="fa-solid fa-square fa-2xl" style="color: #00b4ff;"></i> | `#00b4ff` | (0, 180, 255)     |
    | 11    | <i class="fa-solid fa-square fa-2xl" style="color: #dd00ba;"></i> | `#dd00ba` | (221, 0, 186)     |
    | 12    | <i class="fa-solid fa-square fa-2xl" style="color: #00ffff;"></i> | `#00ffff` | (0, 255, 255)     |
    | 13    | <i class="fa-solid fa-square fa-2xl" style="color: #26c000;"></i> | `#26c000` | (38, 192, 0)      |
    | 14    | <i class="fa-solid fa-square fa-2xl" style="color: #01ffb3;"></i> | `#01ffb3` | (1, 255, 179)     |
    | 15    | <i class="fa-solid fa-square fa-2xl" style="color: #7d24ff;"></i> | `#7d24ff` | (125, 36, 255)    |
    | 16    | <i class="fa-solid fa-square fa-2xl" style="color: #7b0068;"></i> | `#7b0068` | (123, 0, 104)     |
    | 17    | <i class="fa-solid fa-square fa-2xl" style="color: #ff1b6c;"></i> | `#ff1b6c` | (255, 27, 108)    |
    | 18    | <i class="fa-solid fa-square fa-2xl" style="color: #fc6d2f;"></i> | `#fc6d2f` | (252, 109, 47)    |
    | 19    | <i class="fa-solid fa-square fa-2xl" style="color: #a2ff0b;"></i> | `#a2ff0b` | (162, 255, 11)    |

    ## Pose Color Palette

    | Index | Color                                                             | HEX       | RGB               |
    |-------|-------------------------------------------------------------------|-----------|-------------------|
    | 0     | <i class="fa-solid fa-square fa-2xl" style="color: #ff8000;"></i> | `#ff8000` | (255, 128, 0)     |
    | 1     | <i class="fa-solid fa-square fa-2xl" style="color: #ff9933;"></i> | `#ff9933` | (255, 153, 51)    |
    | 2     | <i class="fa-solid fa-square fa-2xl" style="color: #ffb266;"></i> | `#ffb266` | (255, 178, 102)   |
    | 3     | <i class="fa-solid fa-square fa-2xl" style="color: #e6e600;"></i> | `#e6e600` | (230, 230, 0)     |
    | 4     | <i class="fa-solid fa-square fa-2xl" style="color: #ff99ff;"></i> | `#ff99ff` | (255, 153, 255)   |
    | 5     | <i class="fa-solid fa-square fa-2xl" style="color: #99ccff;"></i> | `#99ccff` | (153, 204, 255)   |
    | 6     | <i class="fa-solid fa-square fa-2xl" style="color: #ff66ff;"></i> | `#ff66ff` | (255, 102, 255)   |
    | 7     | <i class="fa-solid fa-square fa-2xl" style="color: #ff33ff;"></i> | `#ff33ff` | (255, 51, 255)    |
    | 8     | <i class="fa-solid fa-square fa-2xl" style="color: #66b2ff;"></i> | `#66b2ff` | (102, 178, 255)   |
    | 9     | <i class="fa-solid fa-square fa-2xl" style="color: #3399ff;"></i> | `#3399ff` | (51, 153, 255)    |
    | 10    | <i class="fa-solid fa-square fa-2xl" style="color: #ff9999;"></i> | `#ff9999` | (255, 153, 153)   |
    | 11    | <i class="fa-solid fa-square fa-2xl" style="color: #ff6666;"></i> | `#ff6666` | (255, 102, 102)   |
    | 12    | <i class="fa-solid fa-square fa-2xl" style="color: #ff3333;"></i> | `#ff3333` | (255, 51, 51)     |
    | 13    | <i class="fa-solid fa-square fa-2xl" style="color: #99ff99;"></i> | `#99ff99` | (153, 255, 153)   |
    | 14    | <i class="fa-solid fa-square fa-2xl" style="color: #66ff66;"></i> | `#66ff66` | (102, 255, 102)   |
    | 15    | <i class="fa-solid fa-square fa-2xl" style="color: #33ff33;"></i> | `#33ff33` | (51, 255, 51)     |
    | 16    | <i class="fa-solid fa-square fa-2xl" style="color: #00ff00;"></i> | `#00ff00` | (0, 255, 0)       |
    | 17    | <i class="fa-solid fa-square fa-2xl" style="color: #0000ff;"></i> | `#0000ff` | (0, 0, 255)       |
    | 18    | <i class="fa-solid fa-square fa-2xl" style="color: #ff0000;"></i> | `#ff0000` | (255, 0, 0)       |
    | 19    | <i class="fa-solid fa-square fa-2xl" style="color: #ffffff;"></i> | `#ffffff` | (255, 255, 255)   |

    !!! note "Ultralytics Brand Colors"

        For Ultralytics brand colors see [https://www.ultralytics.com/brand](https://www.ultralytics.com/brand).
        Please use the official Ultralytics colors for all marketing materials.

    Attributes:
        palette (list[tuple]): List of RGB color tuples for general use.
        n (int): The number of colors in the palette.
        pose_palette (np.ndarray): A specific color palette array for pose estimation with dtype np.uint8.

    Examples:
        >>> from ultralytics.utils.plotting import Colors
        >>> colors = Colors()
        >>> colors(5, True)  # Returns BGR format: (221, 111, 255)
        >>> colors(5, False)  # Returns RGB format: (255, 111, 221)
    """

    def __init__(self):
        """Initialize colors as hex = matplotlib.colors.TABLEAU_COLORS.values()."""
        hexs = (
            "042AFF",
            "0BDBEB",
            "F3F3F3",
            "00DFB7",
            "111F68",
            "FF6FDD",
            "FF444F",
            "CCED00",
            "00F344",
            "BD00FF",
            "00B4FF",
            "DD00BA",
            "00FFFF",
            "26C000",
            "01FFB3",
            "7D24FF",
            "7B0068",
            "FF1B6C",
            "FC6D2F",
            "A2FF0B",
        )
        self.palette = [self.hex2rgb(f"#{c}") for c in hexs]
        self.n = len(self.palette)
        self.pose_palette = np.array(
            [
                [255, 128, 0],
                [255, 153, 51],
                [255, 178, 102],
                [230, 230, 0],
                [255, 153, 255],
                [153, 204, 255],
                [255, 102, 255],
                [255, 51, 255],
                [102, 178, 255],
                [51, 153, 255],
                [255, 153, 153],
                [255, 102, 102],
                [255, 51, 51],
                [153, 255, 153],
                [102, 255, 102],
                [51, 255, 51],
                [0, 255, 0],
                [0, 0, 255],
                [255, 0, 0],
                [255, 255, 255],
            ],
            dtype=np.uint8,
        )
```

Example 4 (typescript):
```typescript
def __call__(self, i: int | torch.Tensor, bgr: bool = False) -> tuple
```

---

## YOLOv8 与 EfficientDet：目标检测架构深度解析

**URL:** https://docs.ultralytics.com/zh/compare/yolov8-vs-efficientdet/

**Contents:**
- YOLOv8 与 EfficientDet：目标检测架构深度解析
- 性能正面交锋：速度、精度和效率
  - 基准测试的主要收获
- Ultralytics YOLOv8 概述
  - YOLOv8 的优势
- Google EfficientDet 概述
  - EfficientDet 的优势
- 架构比较
  - 骨干网络与特征融合
  - 检测头

在快速发展的计算机视觉领域，选择合适的物体 detect 模型对于构建成功的 AI 应用至关重要。在各自时代定义了最先进水平的两个著名架构是 Ultralytics 的 YOLOv8 和 Google Research 的 EfficientDet。本比较探讨了这两个模型的技术细微差别、性能指标和理想用例，帮助开发人员和研究人员为他们的项目做出明智的决策。

EfficientDet在发布时引入了模型缩放和效率方面的开创性概念，而Ultralytics YOLOv8则代表了更现代的演进，优先考虑实时推理速度、易用性和实际部署能力。

YOLOv8 和 EfficientDet 之间的比较突出了设计理念的根本转变。EfficientDet 侧重于最大限度地减少 FLOPs（浮点运算）和参数数量，理论上使其效率很高。相比之下，YOLOv8 旨在最大限度地提高现代硬件上的吞吐量，利用 GPU 并行性提供卓越的推理速度，同时不牺牲精度。

始终在目标硬件上对模型进行基准测试。理论 FLOPs 是衡量复杂度的有用指标，但通常无法预测 GPU 或 NPU 上的实际延迟，因为内存带宽和并行化能力在其中扮演着更重要的角色。使用 YOLO 基准测试模式 在您的特定设置上测试性能。

YOLOv8 是 YOLO（You Only Look Once）系列中由 Ultralytics 发布的最新主要迭代，旨在成为目标检测、实例分割和图像分类的统一框架。

YOLOv8 引入了关键的架构改进，包括无锚点检测头，这简化了训练过程并提高了对不同物体形状的泛化能力。它还利用了新的骨干网络和路径聚合网络 (PAN-FPN)，旨在实现更丰富的特征集成。

EfficientDet 由 Google Brain 团队开发，是一个目标检测模型家族，它将复合缩放的概念引入了目标检测领域。它同时缩放网络的分辨率、深度和宽度，以实现最佳性能。

EfficientDet 基于EfficientNet 骨干网络，并引入了BiFPN（双向特征金字塔网络），它允许轻松快速地进行多尺度特征融合。

了解更多关于 EfficientDet 的信息

YOLOv8 和 EfficientDet 之间的架构差异决定了它们的性能特征以及对不同任务的适用性。

最显著的差异化因素之一是模型所围绕的生态系统。Ultralytics 一直致力于普及 AI，确保 YOLOv8 对初学者和专家都易于访问。

The Ultralytics Python API允许用户通过几行代码加载、训练和部署模型。该生态系统无缝集成了Weights & Biases等工具用于实验跟踪，以及Roboflow用于数据集管理。

相比之下，EfficientDet 通常存在于面向研究的存储库中（例如原始的 TensorFlow 实现）。尽管功能强大，但这些实现通常需要更多的样板代码、复杂的配置文件，以及对底层框架 (TensorFlow/Keras) 更深入的了解才能在 自定义数据集 上进行训练。

Ultralytics 模型支持一键导出为多种格式，包括 ONNX、TensorRT、CoreML 和 TFLite。这种灵活性对于将模型部署到从云服务器到 Raspberry Pi 边缘设备等各种环境至关重要。

YOLOv8 是当今绝大多数计算机视觉应用的首选，因为它在速度和准确性之间取得了平衡。

EfficientDet 在小众场景中仍然具有相关性，特别是在学术研究或高度受限的 CPU 环境中。

尽管EfficientDet是高效神经网络设计领域的里程碑式成就，但YOLOv8和更新的YOLO11为现代AI开发提供了更优越的方案。YOLOv8的无锚框架构、GPU优化设计和强大的Ultralytics生态系统在开发速度、推理延迟和部署灵活性方面提供了显著优势。

对于希望构建快速且准确的最先进计算机视觉解决方案的开发者而言，Ultralytics YOLO 模型是明确的选择。

如果您有兴趣将这些架构与其他模型进行比较，请查看这些页面：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a YOLOv8 model
model = YOLO("yolov8n.pt")

# Train the model
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference on an image
results = model("path/to/image.jpg")
```

---

## 使用 Ultralytics YOLO11 🚀 进行速度估计 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/guides/speed-estimation/

**Contents:**
- 使用 Ultralytics YOLO11 进行速度估计 🚀
- 什么是速度估计？
- 速度估计的优势
- 实际应用
  - SpeedEstimator 参数
- 常见问题
  - 如何使用 Ultralytics YOLO11 估计目标速度？
  - 在交通管理中使用 Ultralytics YOLO11 进行速度估计有哪些好处？
  - YOLO11 是否可以与其他 AI 框架（如TensorFlow或PyTorch）集成？
  - 使用 Ultralytics YOLO11 进行速度估计的准确性如何？

速度估计是计算给定环境中物体移动速率的过程，常用于计算机视觉应用。使用 Ultralytics YOLO11，现在可以结合物体追踪以及距离和时间数据来计算物体的速度，这对于交通监控和监视等任务至关重要。速度估计的准确性直接影响各种应用的效率和可靠性，使其成为智能系统和实时决策过程发展中的关键组成部分。

观看： 使用 Ultralytics YOLO11 进行速度估计

要深入了解速度估计，请查看我们的博客文章：Ultralytics YOLO11 在计算机视觉项目中用于速度估计

速度将是一个估计值，可能不完全准确。此外，估计值可能因相机规格和相关因素而异。

使用 Ultralytics YOLO 进行速度估计

这是一个包含以下内容的表格 SpeedEstimator 参数：

字段 SpeedEstimator 解决方案允许使用 track 参数：

使用Ultralytics YOLO11估计物体速度涉及结合目标检测和跟踪技术。首先，您需要使用YOLO11模型检测每一帧中的物体。然后，跨帧跟踪这些物体，以计算它们随时间的变化。最后，利用物体在帧间移动的距离和帧率来估计其速度。

有关更多详细信息，请参阅我们的官方博客文章。

将 Ultralytics YOLO11 用于速度估计在交通管理方面具有显著优势：

是的，YOLO11 可以与其他 AI 框架（如 TensorFlow 和 PyTorch）集成。Ultralytics 提供对将 YOLO11 模型导出为各种格式（如 ONNX、TensorRT 和 CoreML）的支持，从而确保与其他 ML 框架的平滑互操作性。

要将 YOLO11 模型导出为 ONNX 格式，请执行以下操作：

在我们的导出指南中了解有关导出模型的更多信息。

使用 Ultralytics YOLO11 进行速度估计的准确性取决于多个因素，包括物体跟踪的质量、视频的分辨率和帧速率以及环境变量。虽然速度估计器提供可靠的估计，但由于帧处理速度和物体遮挡的差异，它可能不是 100% 准确。

注意：在可能的情况下，始终考虑误差范围并使用地面实况数据验证估计值。

有关更多提高准确性的技巧，请查看 参数 SpeedEstimator 部分.

**Examples:**

Example 1 (markdown):
```markdown
# Run a speed example
yolo solutions speed show=True

# Pass a source video
yolo solutions speed source="path/to/video.mp4"

# Adjust meter per pixel value based on camera configuration
yolo solutions speed meter_per_pixel=0.05
```

Example 2 (python):
```python
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "Error reading video file"

# Video writer
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))
video_writer = cv2.VideoWriter("speed_management.avi", cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))

# Initialize speed estimation object
speedestimator = solutions.SpeedEstimator(
    show=True,  # display the output
    model="yolo11n.pt",  # path to the YOLO11 model file.
    fps=fps,  # adjust speed based on frame per second
    # max_speed=120,  # cap speed to a max value (km/h) to avoid outliers
    # max_hist=5,  # minimum frames object tracked before computing speed
    # meter_per_pixel=0.05,  # highly depends on the camera configuration
    # classes=[0, 2],  # estimate speed of specific classes.
    # line_width=2,  # adjust the line width for bounding boxes
)

# Process video
while cap.isOpened():
    success, im0 = cap.read()

    if not success:
        print("Video frame is empty or processing is complete.")
        break

    results = speedestimator(im0)

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
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))
video_writer = cv2.VideoWriter("speed_estimation.avi", cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))

# Initialize SpeedEstimator
speedestimator = solutions.SpeedEstimator(
    model="yolo11n.pt",
    show=True,
)

while cap.isOpened():
    success, im0 = cap.read()
    if not success:
        break
    results = speedestimator(im0)
    video_writer.write(results.plot_im)

cap.release()
video_writer.release()
cv2.destroyAllWindows()
```

Example 4 (unknown):
```unknown
yolo export model=yolo11n.pt format=onnx
```

---

## 帮助 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/help/

**Contents:**
- 帮助
- 常见问题
  - 什么是 Ultralytics YOLO？它如何使我的机器学习项目受益？
  - 如何向 Ultralytics YOLO 存储库贡献代码？
  - 为什么我应该为我的机器学习项目使用 Ultralytics HUB？
  - 什么是 Ultralytics 中的持续集成 (CI)，它如何确保高质量的代码？
  - Ultralytics 如何处理数据隐私？
- 评论

欢迎访问 Ultralytics 帮助页面。本页面汇集了实用指南、政策和常见问题解答，以支持您使用 Ultralytics YOLO 模型和存储库。

我们鼓励您查阅这些资源，以获得流畅高效的体验。如果您需要额外支持，请通过 GitHub Issues 或 Ultralytics 社区联系我们。

Ultralytics YOLO (You Only Look Once) 是一种最先进的实时目标检测模型。其最新版本 YOLO11 增强了速度、准确性和多功能性，使其成为从实时视频分析到高级机器学习研究等各种应用的理想选择。YOLO 在检测图像和视频中的目标方面的效率使其成为希望将强大的计算机视觉功能集成到其项目中的企业和研究人员的首选解决方案。

有关 YOLO11 的更多详细信息，请访问 YOLO11 文档。

向 Ultralytics YOLO 仓库贡献代码非常简单。首先查看贡献指南，了解提交 Pull Request、报告错误等的协议。您还需要签署贡献者许可协议 (CLA)，以确保您的贡献在法律上得到认可。如需有效地报告错误，请参阅最小可复现示例 (MRE) 指南。

Ultralytics HUB 为您提供了一种无缝的、无需代码的解决方案来管理您的机器学习项目。它使您能够毫不费力地生成、训练和部署像 YOLO11 这样的 AI 模型。其独特的功能包括云训练、实时跟踪和直观的数据集管理。Ultralytics HUB 简化了从数据处理到模型部署的整个工作流程，使其成为初学者和高级用户不可或缺的工具。

要开始使用，请访问 Ultralytics HUB 快速入门。

Ultralytics 中的持续集成 (CI) 涉及自动化流程，旨在确保代码库的完整性和质量。我们的 CI 设置包括 Docker 部署、断链检查、CodeQL 分析和 PyPI 发布。这些流程通过自动对新提交的代码运行测试和检查，帮助维护稳定和安全的存储库。

在持续集成 (CI) 指南中了解更多信息。

Ultralytics 非常重视数据隐私。我们的 隐私政策 概述了我们如何收集和使用匿名数据来改进 YOLO 软件包，同时优先考虑用户隐私和控制。我们遵守严格的数据保护法规，以确保您的信息始终安全。

---

## YOLO11 与 YOLOX：全面技术比较

**URL:** https://docs.ultralytics.com/zh/compare/yolo11-vs-yolox/

**Contents:**
- YOLO11 与 YOLOX：全面技术比较
- Ultralytics YOLO11：视觉 AI 的新标准
  - 架构与核心能力
  - 主要优势
- YOLOX：无锚框先驱
  - 架构亮点
  - 优势与局限性
- 性能分析
  - 指标细分
- 技术深度解析

对于开发人员和研究人员来说，选择最佳物体检测模型是一项关键决策，其目的是在准确性、推理速度和易于部署之间取得平衡。本技术分析深入比较了 Ultralytics YOLO11和 Megvii 的开创性无锚检测器YOLOX 进行了深入比较。YOLOX 在 2021 年进行了重大创新，而YOLO11 则代表了下一代计算机视觉技术，具有更强的通用性、卓越的性能指标和统一的开发生态系统。

YOLO11 是备受赞誉的 YOLO 系列中最新的旗舰模型，由 Ultralytics 推出，旨在重新定义实时计算机视觉的可能性。YOLO11 继承了其前代模型的优势，引入了架构改进，显著提升了特征提取能力和处理效率。

YOLO11 采用尖端的无锚点架构，优化了计算成本与 detect 准确性之间的权衡。与仅依赖边界框回归的传统模型不同，YOLO11 是一个多任务框架。它原生支持广泛的视觉任务，包括目标检测、实例分割、姿势估计、图像分类和旋转框检测 (OBB)。

YOLO11 通过为所有支持的任务提供单一的 Python 接口，简化了开发工作流程。从 detect 切换到 segment 就像加载不同的模型权重文件一样简单（例如， yolo11n-seg.pt）。

由旷视科技于 2021 年发布，YOLOX 在目标检测领域是一个具有变革性的里程碑。它通过采用无锚机制和解耦头结构，与当时常见的基于锚的方法（如 YOLOv4 和 YOLOv5）有所不同。

YOLOX以其解耦头而著称，它将分类和回归任务分离到不同的分支中。这种设计，结合其SimOTA标签分配策略，使其无需手动调整锚框超参数的复杂性，即可实现强大的性能。

下表直接比较了 COCO 数据集上的关键性能指标。YOLO11 在效率方面表现出明显优势，以相当或更低的计算要求，提供了显著更高的准确性 (mAP)。

最显著的区别之一在于训练和开发体验。Ultralytics 优先提供简化的用户体验，提供了一个全面的生态系统，简化了机器学习生命周期的每个阶段。

尽管YOLOX是一个专用的目标detect器，但YOLO11则是一个全面的视觉平台。

YOLO11 因其性能平衡和生态系统支持，是绝大多数商业和研究应用的推荐选择。

YOLOX 在特定的利基场景中仍具有相关性：

尽管YOLOX在普及无锚点目标detect方面发挥了关键作用，但Ultralytics YOLO11是现代计算机视觉开发的卓越选择。

YOLO11 在所有关键指标上都超越了 YOLOX：它更准确、显著更快，并且参数效率更高。除了原始性能之外，Ultralytics 生态系统还为开发者提供了无与伦比的易用性、完善的文档和多功能任务处理能力。无论是用于快速原型开发还是大规模工业部署，YOLO11 都提供了构建尖端 AI 解决方案所需的工具和性能。

探索 YOLO11 如何与该领域的其他领先模型进行比较：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLO11 model
model = YOLO("yolo11n.pt")

# Train the model on a custom dataset
model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference on an image
results = model("https://ultralytics.com/images/bus.jpg")
```

---

## PP-YOLOE+ 对比 YOLOv10：全面技术对比

**URL:** https://docs.ultralytics.com/zh/compare/pp-yoloe-vs-yolov10/

**Contents:**
- PP-YOLOE+ 对比 YOLOv10：全面技术对比
- PP-YOLOE+：PaddlePaddle 生态系统中的精度
  - 主要架构特性
- YOLOv10：免 NMS 实时革命
  - 创新与生态系统集成
- 技术性能分析
  - 效率与速度解读
- 优势与劣势
  - YOLOv10：现代之选
  - PP-YOLOE+：PaddlePaddle专精模型

选择正确的物体检测模型是影响计算机视觉系统效率、准确性和可扩展性的关键决策。本详细对比分析了来自百度PaddlePaddle 生态系统的精制无锚检测器PP-YOLOE+ 和YOLOv10。 YOLOv10是清华大学推出的革命性实时端到端检测器，已完全集成到Ultralytics 生态系统中。

这些模型代表了解决速度与精度权衡的两种不同方法。通过研究它们的架构创新、性能指标和理想用例，我们为您选择最适合特定应用的工具提供所需见解。

PP-YOLOE+（实用PaddlePaddle单层高效增强版）是PP-YOLOE架构的演进，旨在提供高精度检测机制。由百度开发，它作为PaddlePaddle框架内的旗舰模型，强调针对硬件环境预定义的工业应用进行优化。

作者： PaddlePaddle 作者机构：百度日期： 2022-04-02预印本：https://arxiv.org/abs/2203.16250GitHub：PaddleDetection 仓库文档：PP-YOLOE+ 文档

PP-YOLOE+ 通过多项旨在优化特征表示和定位的结构增强而脱颖而出：

YOLOv10YOLO 代表了YOLO 系列的范式转变。它由清华大学的研究人员开发，通过为NMS 训练引入一致的双重分配，解决了非最大抑制（ NMS ）的历史瓶颈问题。这使得真正的端到端部署得以实现，并大大减少了推理延迟。

作者：Ao Wang, Hui Chen, Lihao Liu, et al.组织:清华大学日期:2024-05-23ArXiv:https://arxiv.org/abs/2405.14458 GitHub:YOLOv10 Repository文档:Ultralytics YOLOv10 文档

YOLOv10 不仅仅是架构上的更新；它是一种整体效率驱动的设计。

以下指标突出了两种模型之间的性能差异。YOLOv10 始终展现出卓越的效率，以更少的参数和更低的延迟提供更高的准确性。

数据揭示了 YOLOv10 在 性能平衡 方面的明显优势。

传统对象检测器需要非极大值抑制 (NMS) 来过滤重叠的框，这一步骤通常很慢，并且难以在硬件上优化。YOLOv10 完全消除了这一步骤，从而无论检测到多少对象，推理时间都保持不变。

对于需要即时响应时间的应用，例如自动驾驶汽车或高速生产线，YOLOv10是卓越的选择。其低延迟和移除的 NMS 步骤确保了确定性的推理速度，这对于安全关键系统至关重要。

对于寻求多功能解决方案的开发者而言，Ultralytics YOLO 模型 得益于其良好维护的生态系统而提供了一个独特的优势。能够轻松切换任务（detect、segment、姿势估计）并导出为 ONNX、TensorRT 和 CoreML 等格式，使得 YOLOv10 及其同类模型具有高度适应性。

如果您的现有基础设施完全基于百度的技术栈，PP-YOLOE+提供了一个原生解决方案，可与PaddlePaddle的其他工具良好集成。然而，对于新项目，YOLOv10的训练效率和较低的硬件成本通常能带来更好的投资回报。

体验 Ultralytics 模型易用性的特点。您只需几行 Python 代码即可加载 YOLOv10 并运行预测：

这个简单的API让研究人员能够专注于数据和结果，而不是样板代码。

尽管 PP-YOLOE+ 在其特定框架内仍是一个强劲的竞争者，但 YOLOv10 为更广泛的计算机视觉社区提供了更具吸引力的解决方案。其在消除 NMS 方面的架构突破，结合 Ultralytics 生态系统的强大功能，为开发者提供了一个不仅更快、更轻，而且更易于使用和维护的工具。

对于那些希望保持在绝对前沿的用户，我们还推荐探索 YOLO11，这是 Ultralytics 的最新旗舰模型，它进一步突破了在多个视觉任务中的多功能性和性能界限。

通过这些比较，拓宽您对目标 detect 领域的理解：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv10n model
model = YOLO("yolov10n.pt")

# Run inference on an image
results = model("path/to/image.jpg")

# Display the results
results[0].show()
```

---

## YOLOv9 与 YOLOv6-3.0：详细技术比较

**URL:** https://docs.ultralytics.com/zh/compare/yolov9-vs-yolov6/

**Contents:**
- YOLOv9 与 YOLOv6-3.0：详细技术比较
- YOLOv9：重新定义精度与效率
  - 架构创新
  - 优势
  - 弱点
  - 理想用例
- YOLOv6-3.0：为工业级速度而生
  - 架构特性
  - 优势
  - 弱点

选择理想的对象detect架构是开发强大计算机视觉解决方案的关键一步。该决策通常涉及在准确性、推理速度和计算资源消耗之间进行复杂的权衡。本指南对YOLOv9（一款以架构效率著称的SOTA模型）和YOLOv6-3.0（一款专门为工业部署速度优化的模型）进行了全面的技术比较。我们将分析它们的架构创新、性能指标和理想部署场景，以帮助您做出明智的选择。

YOLOv9 于 2024 年初推出，代表了实时目标检测领域的一个范式转变。它解决了深度神经网络中信息丢失的根本问题，实现了卓越的准确性，同时保持了出色的计算效率。

作者：王建尧、廖鸿源Chien-Yao Wang and Hong-Yuan Mark Liao组织：台湾中央研究院信息科学研究所日期：2024-02-21Arxiv:https://arxiv.org/abs/2402.13616 GitHub:https://github.com/WongKinYiu/yolov9Docs:ultralytics

YOLOv9的核心优势在于两个开创性概念：可编程梯度信息 (PGI)和广义高效层聚合网络 (GELAN)。随着网络变得更深，重要的特征信息在前向传播过程中经常丢失。PGI通过确保可靠的梯度信息被保留用于更新网络权重，从而解决了这个信息瓶颈。同时，GELAN优化了架构以最大化参数利用率，使模型能够以更少的参数和FLOPs实现比传统设计更高的准确性。

在 Ultralytics 生态系统中使用时，YOLOv9 提供无缝的开发体验。它得益于用户友好的 Python API、全面的文档和强大的支持，使其对研究人员和企业开发者都易于使用。

YOLOv9 在精度至关重要的场景中表现出色：

YOLOv6-3.0 是 YOLOv6 系列的第三次迭代，由美团视觉团队开发。它于2023年初发布，主要侧重于最大限度地提高工业应用的推理速度，尤其是在 GPU 硬件上。

作者: Chuyi Li, Lulu Li, Yifei Geng, 等。机构:美团日期: 2023-01-13Arxiv:https://arxiv.org/abs/2301.05586GitHub:https://github.com/meituan/YOLOv6文档:https://docs.ultralytics.com/models/yolov6/

YOLOv6-3.0采用了硬件感知型神经网络设计。它利用了高效的重参数化骨干网络（RepBackbone）以及由混合模块组成的颈部网络。这种结构经过专门调优，旨在充分利用GPU的并行计算能力，目标是在推理过程中提供尽可能低的延迟，同时保持具有竞争力的准确性。

YOLOv6-3.0 非常适合高吞吐量环境：

以下比较突出了两种模型的性能指标。尽管 YOLOv6-3.0 在其最小变体中提供了令人印象深刻的速度，YOLOv9 展示了卓越的效率，在可比较的范围内以更少的参数提供了更高的精度。

Ultralytics YOLO 模型，包括 YOLOv9，以其在训练期间优化的内存使用而闻名。与一些需要大量 GPU 显存的重型基于 Transformer 的模型不同，这些模型通常可以在消费级硬件上进行训练，使最先进的 AI 开发变得普及。

这两种模型之间的用户体验差异显著。YOLOv9 完全集成到 Ultralytics 生态系统中，提供了简化的工作流程。开发人员可以利用简单的 python 接口，仅用几行代码即可训练、验证和部署模型。

此集成提供了对高级功能的访问，例如自动超参数调优、使用TensorBoard或Weights & Biases进行实时日志记录，以及无缝导出到ONNX和TensorRT等格式。

相比之下，训练 YOLOv6-3.0 通常涉及浏览其特定的 GitHub 存储库和训练脚本，这对于习惯了 Ultralytics 库即插即用特性的人来说，可能会带来更陡峭的学习曲线。

尽管 YOLOv6-3.0 仍然是特定工业细分市场中，对 GPU 硬件要求最低延迟的有力竞争者，但YOLOv9 作为现代计算机视觉任务的卓越全能选择脱颖而出。

YOLOv9 实现了最先进的精度、卓越的参数效率以及 Ultralytics 生态系统的巨大优势的完美结合。它能够以更轻量级的模型实现更高的精度，这意味着在边缘部署场景中可以降低存储成本并加快传输速度。此外，Ultralytics 模型所具备的易用性、详尽的文档和活跃的社区支持显著加速了开发生命周期，使团队能够自信地从概念阶段迈向部署。

对于寻求下一代性能的开发者而言，我们也推荐探索 Ultralytics YOLO11，这是我们的最新模型，它进一步完善了这些功能，适用于更广泛的任务，包括 姿势估计 和 旋转框检测。您还可以在我们的 模型比较中心 将这些模型与 RT-DETR 等基于 Transformer 的方法进行比较。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv9 model
model = YOLO("yolov9c.pt")

# Train the model on your custom dataset
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference on an image
results = model.predict("image.jpg")
```

---

## YOLOX 对比 YOLO11：深入探讨目标检测技术演进

**URL:** https://docs.ultralytics.com/zh/compare/yolox-vs-yolo11/

**Contents:**
- YOLOX 对比 YOLO11：深入探讨目标检测技术演进
- YOLOX：弥合研究与工业
  - 架构与创新
  - 优势与劣势
- Ultralytics YOLO11：视觉 AI 的新标准
  - 架构与生态系统优势
  - 为什么选择YOLO11？
- 性能分析
  - 主要内容
- 真实世界的应用

选择最佳的对象检测架构对于旨在平衡准确性、延迟和计算效率的开发人员来说至关重要。本综合分析报告比较了 Megvii 首创的无锚点模型YOLOX 和 Ultralytics YOLO11进行了比较UltralyticsYOLOX 在 2021 年引入了重大创新，而YOLO11 则代表了 2024 年计算机视觉的最前沿，为从检测到实例分割的各种任务提供了统一的框架。

YOLOX于2021年发布，通过采用无锚点机制并解耦预测头，标志着YOLO系列的一个重大转变。它的设计旨在弥合学术研究与工业应用之间的鸿沟。

YOLOX与YOLOv5等早期版本不同，它移除了锚框，从而降低了设计复杂性并减少了启发式超参数的数量。其架构特点是采用解耦头，将分类和回归任务分离到不同的分支中，这提高了收敛速度和准确性。此外，它引入了SimOTA，这是一种先进的标签分配策略，能够动态分配正样本，进一步提升了性能。

Ultralytics YOLO11 改进了实时目标检测的传统，重点关注效率、灵活性和易用性。它旨在成为快速原型设计和大规模生产部署的首选解决方案。

YOLO11 采用高度优化的无锚点架构，在增强特征提取的同时，最大限度地减少了计算开销。与 YOLOX 不同，YOLO11 不仅仅是一个模型，它还是一个综合生态系统的一部分。它在一个用户友好的 API 中支持广泛的计算机视觉任务—包括分类、segmentation、姿势估计和 track—。

YOLO11 与 Ultralytics HUB 以及 Weights & Biases 和 Comet 等第三方工具无缝集成，让您轻松可视化实验并管理数据集。

下表突出显示了YOLOX和YOLO11之间的性能差异。YOLO11始终以更少的参数和FLOPs展现出更高的准确性（mAP），从而实现了更快的推理速度。

Ultralytics 优先考虑精简的用户体验。虽然 YOLOX 通常需要复杂的配置文件和手动设置，但 YOLO11 只需最少的代码即可使用。

开发者可以通过几行python代码加载预训练模型、运行推理，甚至在自定义数据上进行训练：

在自定义数据集上训练 YOLO11 模型同样简单。该库自动处理数据增强、超参数调整和日志记录。

尽管YOLOX在普及无锚点目标detect方面发挥了关键作用，但Ultralytics YOLO11代表了现代AI开发的卓越选择。

YOLO11 在准确性、速度和效率方面均优于 YOLOX，同时提供了一个健壮且维护良好的生态系统。其在多项视觉任务中的多功能性——无需为 detect、segment 和姿势估计任务切换不同的库——显著降低了开发复杂性。对于寻求由活跃社区支持和全面文档支持的、面向未来的高性能解决方案的开发者而言，YOLO11 是推荐的选择。

探索 YOLO11 如何与其他领先架构进行比较，以找到最适合您特定需求的模型：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLO11 model
model = YOLO("yolo11n.pt")

# Run inference on an image
results = model("https://ultralytics.com/images/bus.jpg")

# Display results
results[0].show()
```

Example 2 (markdown):
```markdown
# Train the model on a custom dataset
model.train(data="coco8.yaml", epochs=100, imgsz=640)
```

---

## Reference for ultralytics/solutions/security_alarm.py

**URL:** https://docs.ultralytics.com/zh/reference/solutions/security_alarm/

**Contents:**
- Reference for ultralytics/solutions/security_alarm.py
- class ultralytics.solutions.security_alarm.SecurityAlarm
  - method ultralytics.solutions.security_alarm.SecurityAlarm.authenticate
  - method ultralytics.solutions.security_alarm.SecurityAlarm.process
  - method ultralytics.solutions.security_alarm.SecurityAlarm.send_email

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/solutions/security_alarm.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class to manage security alarm functionalities for real-time monitoring.

This class extends the BaseSolution class and provides features to monitor objects in a frame, send email notifications when specific thresholds are exceeded for total detections, and annotate the output frame for visualization.

Authenticate the email server for sending alert notifications.

This method initializes a secure connection with the SMTP server and logs in using the provided credentials.

Monitor the frame, process object detections, and trigger alerts if thresholds are exceeded.

This method processes the input frame, extracts detections, annotates the frame with bounding boxes, and sends an email notification if the number of detected objects surpasses the specified threshold and an alert has not already been sent.

Send an email notification with an image attachment indicating the number of objects detected.

This method encodes the input image, composes the email message with details about the detection, and sends it to the specified recipient.

**Examples:**

Example 1 (rust):
```rust
SecurityAlarm(self, **kwargs: Any) -> None
```

Example 2 (python):
```python
>>> security = SecurityAlarm()
>>> security.authenticate("abc@gmail.com", "1111222233334444", "xyz@gmail.com")
>>> frame = cv2.imread("frame.jpg")
>>> results = security.process(frame)
```

Example 3 (python):
```python
class SecurityAlarm(BaseSolution):
    """A class to manage security alarm functionalities for real-time monitoring.

    This class extends the BaseSolution class and provides features to monitor objects in a frame, send email
    notifications when specific thresholds are exceeded for total detections, and annotate the output frame for
    visualization.

    Attributes:
        email_sent (bool): Flag to track if an email has already been sent for the current event.
        records (int): Threshold for the number of detected objects to trigger an alert.
        server (smtplib.SMTP): SMTP server connection for sending email alerts.
        to_email (str): Recipient's email address for alerts.
        from_email (str): Sender's email address for alerts.

    Methods:
        authenticate: Set up email server authentication for sending alerts.
        send_email: Send an email notification with details and an image attachment.
        process: Monitor the frame, process detections, and trigger alerts if thresholds are crossed.

    Examples:
        >>> security = SecurityAlarm()
        >>> security.authenticate("abc@gmail.com", "1111222233334444", "xyz@gmail.com")
        >>> frame = cv2.imread("frame.jpg")
        >>> results = security.process(frame)
    """

    def __init__(self, **kwargs: Any) -> None:
        """Initialize the SecurityAlarm class with parameters for real-time object monitoring.

        Args:
            **kwargs (Any): Additional keyword arguments passed to the parent class.
        """
        super().__init__(**kwargs)
        self.email_sent = False
        self.records = self.CFG["records"]
        self.server = None
        self.to_email = ""
        self.from_email = ""
```

Example 4 (python):
```python
def authenticate(self, from_email: str, password: str, to_email: str) -> None
```

---

## RTDETRv2 与 DAMO-YOLO：实时目标检测深度解析

**URL:** https://docs.ultralytics.com/zh/compare/rtdetr-vs-damo-yolo/

**Contents:**
- RTDETRv2 与 DAMO-YOLO：实时目标检测深度解析
- 性能基准：速度与准确性
- RTDETRv2：Transformer 强大模型
  - 架构与能力
- DAMO-YOLO：效率优化
  - 主要架构创新
- 实际应用场景
  - 何时选择 RTDETRv2
  - 何时选择 DAMO-YOLO
- Ultralytics 优势：为何 YOLO11 是最佳选择

计算机视觉领域正在快速发展，研究人员不断突破推理速度和检测准确性之间的界限。RTDETRv2（百度推出的基于Transformer的模型）和DAMO-YOLO（阿里巴巴推出的高度优化的卷积网络）是该领域的两个杰出竞争者。本文将对这些模型的独特架构理念、性能指标和理想应用场景进行技术比较。

在选择目标检测模型时，主要的权衡通常在于平均精度 (mAP) 和延迟之间。以下数据突出了 RTDETRv2 和 DAMO-YOLO 在 COCO 验证数据集上的性能差异。

数据揭示了设计理念上的明显区别。DAMO-YOLO 优先考虑原始速度和效率，其“Tiny”变体实现了极低的延迟，适用于受限的 边缘计算 环境。相反，RTDETRv2 追求最大 精度，其最大变体实现了显著的 54.3 mAP，使其在精度至关重要的任务中表现更优。

RTDETRv2 建立在 Detection Transformer (DETR) 架构的成功之上，解决了通常与视觉 Transformer 相关的高计算成本问题，同时保持其捕获全局上下文的能力。

RTDETRv2 采用混合编码器，可高效处理多尺度特征。与传统的基于 CNN 的 YOLO 模型不同，RTDETR 消除了对非极大值抑制 (NMS) 后处理的需求。这种端到端的方法简化了部署流程，并减少了拥挤场景中的延迟波动。

该模型利用了一种高效的混合编码器，该编码器解耦了尺度内交互和跨尺度融合，与标准 DETR 模型相比，显著降低了计算开销。这种设计使其在复杂环境中识别物体方面表现出色，在这些环境中，遮挡可能会使标准卷积检测器感到困惑。

尽管 RTDETRv2 提供了高精度，但值得注意的是，与 CNN 相比，Transformer 架构在训练期间通常会消耗显著更多的 CUDA 内存。GPU 显存有限的用户可能会发现，与 YOLO11 等高效替代方案相比，训练这些模型更具挑战性。

DAMO-YOLO 代表了一种严谨的架构优化方法，它利用神经网络架构搜索 (NAS) 来寻找最有效的特征提取和融合结构。

DAMO-YOLO 集成了多项先进技术以最大化速度-准确性权衡：

该模型在需要高吞吐量的场景中表现尤为出色，例如工业装配线或高速交通监控等毫秒必争的场景。

在这两个模型之间进行选择通常取决于部署环境的具体限制。

对于精度不可妥协且硬件资源充足的应用，RTDETRv2 是首选。

DAMO-YOLO 在资源受限环境或需要超低延迟的应用中表现出色。

尽管 RTDETRv2 和 DAMO-YOLO 提供了引人注目的功能，但 Ultralytics YOLO11 提供了一个全面的解决方案，它平衡了性能、可用性和生态系统支持，使其成为大多数开发者和研究人员的更优选择。

采用研究模型最显著的障碍之一是其代码库的复杂性。Ultralytics 通过统一、用户友好的 python API 消除了这一障碍。无论您是执行实例 segment、姿势估计还是分类，工作流程都保持一致且直观。

与主要专注于检测的 DAMO-YOLO 不同，YOLO11 是一个多功能平台。它开箱即用地支持广泛的计算机视觉任务，包括对于航空影像和文档分析至关重要的旋转框检测 (OBB)。这种多功能性使团队能够针对多个项目需求标准化使用单一框架。

YOLO11 专为效率而设计。与 RTDETRv2 等基于 Transformer 的模型相比，它在训练时通常需要更少的 GPU 内存 (VRAM)。这种效率降低了硬件门槛，使开发人员能够在消费级 GPU 上训练最先进的模型，或通过 Ultralytics 生态系统有效利用云资源。此外，庞大的预训练权重库确保了迁移学习的快速有效，显著缩短了 AI 解决方案的上市时间。

对于那些寻求一个稳健、维护良好、高性能且随行业发展而演进的解决方案的用户，Ultralytics YOLO11 仍然是推荐标准。

为了进一步了解这些模型如何融入更广泛的计算机视觉领域，请查阅这些相关的比较：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a model (YOLO11 offers various sizes: n, s, m, l, x)
model = YOLO("yolo11n.pt")

# Train the model with a single line of code
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference on an image
results = model("path/to/image.jpg")
```

---

## DAMO-YOLO 对比 YOLOv9：一项技术比较 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/compare/damo-yolo-vs-yolov9/

**Contents:**
- DAMO-YOLO 对比 YOLOv9：一项技术比较
- DAMO-YOLO：通过神经网络架构搜索实现的速度导向设计
  - 架构创新
  - 优势与局限性
- YOLOv9：用于实现最高效率的可编程梯度
  - 核心架构：PGI 和 GELAN
  - Ultralytics 优势
- 性能分析：准确性与效率
- 训练方法与可用性
  - 代码示例：训练 YOLOv9

在飞速发展的计算机视觉领域，选择最佳的物体检测模型是一项至关重要的决策，会影响到从系统延迟到检测精度的方方面面。本综合指南对阿里巴巴集团的高速检测器YOLO 和YOLOv9 进行了技术比较。 YOLOv9之间的技术比较。我们将分析它们的架构创新、性能指标和理想用例，帮助开发人员和研究人员做出明智的选择。

尽管这两个模型都比其前身有了显著改进，但 YOLOv9，尤其是在 Ultralytics 生态系统中利用时，提供了最先进的准确性、开发人员友好的工具和多功能部署选项的引人注目的融合。

DAMO-YOLO 是阿里巴巴开发的一种目标检测框架，采用“一次性”方法设计。它优先考虑低延迟和高吞吐量，使其成为需要对特定硬件施加严格速度限制的工业应用的有力竞争者。

作者： 徐贤哲、蒋一奇、陈卫华、黄一伦、张远、孙秀宇机构：阿里巴巴集团日期： 2022-11-23预印本：https://arxiv.org/abs/2211.15444GitHub：https://github.com/tinyvision/DAMO-YOLO

DAMO-YOLO 凭借自动化设计流程和高效组件而脱颖而出：

DAMO-YOLO 的主要优势在于其推理速度。该架构针对高 GPU 吞吐量进行了大量优化，使其适用于处理量至关重要的视频分析流水线。此外，蒸馏技术的使用增强了其较小模型的性能。

然而，DAMO-YOLO在生态系统成熟度方面面临挑战。与Ultralytics模型可用的强大工具相比，用户可能会发现部署、格式转换和社区支持方面的资源较少。其任务多功能性通常也仅限于目标detect，而现代框架通常原生支持segmentation和姿势估计。

YOLOv9通过解决深度神经网络中信息丢失的根本问题，代表着实时目标检测领域的范式转变。通过确保关键数据在整个网络深度中得以保留，YOLOv9以卓越的参数效率实现了更高的精度。

作者：王建尧、廖鸿源Chien-Yao Wang, Hong-Yuan Mark Liao组织：台湾中央研究院信息科学研究所日期：2024-02-21Arxiv:https://arxiv.org/abs/2402.13616 GitHub:https://github.com/WongKinYiu/yolov9Documentation:ultralytics

YOLOv9引入了两项使其脱颖而出的开创性技术：

在传统的深度学习模型中，输出层的损失函数通常缺乏足够的信息来有效指导浅层更新。PGI充当桥梁，保留输入信息并确保整个网络学习到鲁棒的特征，从而实现更好的收敛和更高的准确性。

在 Ultralytics 生态系统中使用 YOLOv9 时，开发者将获得相较于独立实现的显著优势：

以下比较突出了 DAMO-YOLO 和 YOLOv9 之间的权衡。尽管 DAMO-YOLO 在特定硬件上提供了具有竞争力的速度，YOLOv9 始终以更少的参数提供更高的 平均精度均值 (mAP)，展示了卓越的架构效率。

两种模型的训练体验差异显著。DAMO-YOLO 对 NAS 的依赖意味着需要复杂的搜索阶段来推导架构，或者使用预先搜索的骨干网络。如果需要定制骨干网络结构，其“一劳永逸”的方法可能会带来高昂的计算成本。

相比之下，由 Ultralytics 支持的 YOLOv9 提供了精简的 训练模式。用户可以通过最少的配置，在 Open Images V7 等自定义数据集或专业集合上微调模型。与 Ultralytics HUB 的集成支持云端训练、可视化和一键部署，从而普及了对高级 AI 的访问，而无需在 NAS 或超参数调优方面拥有深厚专业知识。

使用 Ultralytics Python 包实现 YOLOv9 非常简单。

DAMO-YOLO 和 YOLOv9 都展示了目标检测领域的快速创新。DAMO-YOLO 证明了神经架构搜索在榨取最大速度性能方面的价值。然而，YOLOv9 对大多数用户而言，是更通用、更强大的解决方案。

通过 PGI 解决深度监督信息瓶颈并使用 GELAN 优化层，YOLOv9 实现了 卓越的效率和最先进的准确性。当与 Ultralytics 生态系统结合时，它提供了一个强大、维护良好且用户友好的平台，加速了从概念到部署的进程。对于希望自信地构建尖端视觉应用的开发人员来说，Ultralytics YOLO 模型仍然是卓越的选择。

如果您对探索 Ultralytics 系列中的其他最先进选项或进行进一步比较感兴趣，可以参考这些资源：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv9 model
model = YOLO("yolov9c.pt")

# Train the model on the COCO8 dataset for 100 epochs
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference on an image
results = model("path/to/image.jpg")
```

---

## EfficientDet 对比 YOLOv5：详细技术比较

**URL:** https://docs.ultralytics.com/zh/compare/efficientdet-vs-yolov5/

**Contents:**
- EfficientDet 对比 YOLOv5：详细技术比较
- EfficientDet：可扩展且高效
  - 架构亮点
  - 优势与劣势
- Ultralytics YOLOv5：实际性能与可用性
  - 架构亮点
  - 优势与劣势
- 性能指标：速度对比准确性
- 理想用例
  - 何时选择 EfficientDet

目标检测领域发展迅速，其驱动力是不断平衡准确性和计算效率的需求。Google Brain团队开发的EfficientDet和Ultralytics创建的YOLOv5是显著影响该领域的两种架构。尽管这两种模型都旨在高效地检测图像中的目标，但它们以根本不同的设计理念和架构策略解决问题。

本指南提供了深入的技术比较，旨在帮助开发人员、研究人员和工程师为其特定的计算机视觉应用选择合适的工具。

EfficientDet于2019年末发布，源于同时优化准确性和效率的研究目标。它将“复合缩放”（Compound Scaling）的概念引入目标检测领域，这种方法可以统一缩放骨干网络的分辨率、深度和宽度。

EfficientDet 基于 EfficientNet 主干网络构建，并引入了一种新颖的特征融合网络，称为 BiFPN（双向特征金字塔网络）。与传统仅限于自上而下信息流的 特征金字塔网络 (FPN) 不同，BiFPN 允许在不同分辨率层之间进行复杂的双向信息流。

该模型还利用了复合缩放，这允许用户根据其资源限制从一系列模型（D0 到 D7）中进行选择。这确保了如果您有更多计算资源可用，您可以线性增加模型大小以获得更好的准确性。

EfficientDet 的主要优势在于其理论效率。它以极低的FLOPs（浮点运算）实现了高mAP分数。这使其成为参数效率作为关键指标的学术研究的有趣候选。

然而，EfficientDet存在一个实际缺点：推理延迟。BiFPN中复杂的连接以及大量使用的深度可分离卷积——尽管在数学上是高效的——但与标准卷积相比，在GPU硬件上通常未能得到充分优化。因此，尽管FLOPs较低，EfficientDet在GPU上的运行速度可能比理论计算成本更高的模型还要慢。

了解更多关于 EfficientDet 的信息

Ultralytics YOLOv5 在2020年发布时，代表着一场范式转变。与其前身不同，它是第一个原生在 PyTorch 中实现的 YOLO 模型，使其能够被庞大的开发者生态系统所使用。它在追求原始性能的同时，优先考虑了“部署友好性”。

YOLOv5 采用 CSPDarknet 主干网络，优化了梯度流并减少了计算量。它开创性地在训练期间使用了Mosaic 数据增强——一种将四张图像拼接在一起的技术——提高了模型 detect 小目标的能力，并减少了对大批量大小的需求。

该架构旨在实现高速。通过利用标准卷积和流线型头部结构，YOLOv5 最大限度地发挥了现代 GPU 的并行处理能力，从而实现极低的 推理延迟。

YOLOv5最显著的优势之一是其周围的生态系统。Ultralytics提供了一个无缝的工作流程，包括自动锚框生成、超参数演进，以及对ONNX、TensorRT、CoreML和TFLite的原生导出支持。这种“开箱即用”的方法大大缩短了从概念到生产的时间。

YOLOv5 在实时推理和易用性方面表现出色。其简单的 API 和完善的文档使开发者能够在几分钟内使用自己的数据训练自定义模型。它以一种对边缘 AI和云部署都最优的方式平衡了速度和准确性。虽然像YOLO11这样的新模型在准确性上已经超越了它，但 YOLOv5 仍然是一个可靠的、行业标准的“主力军”。

下表比较了 EfficientDet 和 YOLOv5 在COCO val2017 数据集上的性能。关键在于理论成本 (FLOPs) 和实际速度 (Latency) 之间的区别。

如所示， YOLOv5 在 GPU 延迟方面表现出色。例如， YOLOv5s （37.4 mAP）运行速度为 1.92 毫秒 在 T4 GPU 上，而 EfficientDet-d0 （34.6 mAP）需要 3.92 毫秒—使得 YOLOv5 大致 2x 速度更快 同时提供更高的精度。这种差异在更大模型上会进一步扩大； YOLOv5l （49.0 mAP）几乎是 速度提升5倍 比同类 EfficientDet-d4 （49.7 mAP）。

相反，EfficientDet 在纯 CPU 环境中表现出色，其中较低的 FLOPs 通常能更好地转化为性能，这在较小的 D0 变体的 ONNX CPU 速度中可见一斑。

在这些模型之间进行选择取决于您的具体限制：

Ultralytics模型的一个显著特点是易用性。虽然实现EfficientDet通常需要复杂的TensorFlow配置或特定的仓库克隆，但YOLOv5可以通过PyTorch Hub仅用几行Python代码即可加载和运行。

尽管EfficientDet通过证明复合缩放和高效特征融合的价值，在计算机视觉领域树立了重要的里程碑，但YOLOv5通过使高性能对象检测变得易于访问、快速和可部署，彻底改变了行业。

对于今天开始新项目的开发者而言，我们推荐关注 Ultralytics 系列的最新进展。YOLO11 建立在 YOLOv5 的坚实基础之上，提供了：

如需进一步了解 Ultralytics 模型与其他架构的比较，请查阅我们与 YOLOv8 和 RT-DETR 的比较。

**Examples:**

Example 1 (python):
```python
import torch

# Load the YOLOv5s model from the official Ultralytics repository
model = torch.hub.load("ultralytics/yolov5", "yolov5s", pretrained=True)

# Define an image (URL or local path)
img = "https://ultralytics.com/images/zidane.jpg"

# Perform inference
results = model(img)

# Display results
results.print()  # Print predictions to console
results.show()  # Show image with bounding boxes
```

---

## EfficientDet 对比 YOLOv10：目标检测效率的演进

**URL:** https://docs.ultralytics.com/zh/compare/efficientdet-vs-yolov10/

**Contents:**
- EfficientDet 对比 YOLOv10：目标检测效率的演进
- 模型来源与概述
  - EfficientDet
  - YOLOv10
- 架构深度解析
  - EfficientDet：复合缩放与BiFPN
  - YOLOv10：免 NMS 端到端检测
- 性能分析：速度 vs. 准确性
  - 主要内容
- 训练效率与易用性

在快速发展的计算机视觉领域，对计算效率和 detect 准确性之间最佳平衡的追求是持续的。定义了各自时代的两个架构是 EfficientDet（一个来自Google Research 的可扩展模型家族）和 YOLOv10（清华大学研究人员最新推出的实时端到端检测器）。

本比较探讨了两种模型的技术细微之处，考察了 YOLOv10 的现代设计理念如何改进 EfficientDet 引入的基础概念。我们将分析它们的架构、性能指标以及在实际部署中的适用性。

理解这些模型的历史背景有助于我们认识到近年来取得的技术飞跃。

EfficientDet 于 2019 年末推出，旨在解决目标检测模型缩放效率低下的问题。它提出了一种统一缩放分辨率、深度和宽度的复合缩放方法。

于2024年5月发布，YOLOv10通过消除后处理过程中对非极大值抑制（NMS）的需求，突破了实时检测的界限，从而降低了延迟并简化了部署。

这些模型的核心区别在于它们的特征融合和后处理方法。

EfficientDet 基于EfficientNet 骨干网络。其标志性特征是双向特征金字塔网络 (BiFPN)。与传统 FPN 简单地将不同尺度的特征相加不同，BiFPN 引入了可学习的权重，以在融合过程中强调更重要的特征。它还增加了自顶向下和自底向上的路径，以促进更好的信息流。

尽管它在FLOPs（每秒浮点运算次数）方面具有理论效率，但大量使用深度可分离卷积和复杂的BiFPN结构有时会导致在GPU硬件上的吞吐量低于更简单的架构。

YOLOv10 通过消除对 NMS 的依赖，引入了范式转变。传统的实时检测器会生成大量冗余预测，需要进行过滤，从而造成延迟瓶颈。YOLOv10 在训练过程中采用一致双重分配：一个用于丰富监督信号的一对多头部，以及一个用于精确、免 NMS 推理的一对一头部。

此外，YOLOv10 采用了一种整体效率-精度驱动的模型设计。这包括轻量级分类头、空间-通道解耦下采样和秩引导块设计，确保每个参数都有效地提升模型性能。

非极大值抑制 (NMS) 是一种用于过滤重叠边界框的后处理步骤。它按顺序执行且计算成本高昂，速度通常随检测到的对象数量而变化。通过设计一种自然地为每个对象预测一个边界框（端到端）的架构，YOLOv10 稳定了推理延迟，使其在边缘 AI 应用中具有高度可预测性。

在比较性能时，YOLOv10 在现代硬件，特别是 GPU 上展现出显著优势。EfficientDet 针对 FLOPs 进行了优化，而 YOLOv10 则针对实际延迟和吞吐量进行了优化。

对开发者而言，最关键的因素之一是将这些模型轻松集成到现有工作流程中。

YOLOv10 已集成到 Ultralytics 生态系统中，这在易用性和维护方面提供了显著优势。用户受益于统一的Python API，该 API 标准化了跨不同模型世代的训练、验证和部署。

使用 Ultralytics 训练 YOLOv10 非常简单。该框架会自动处理数据增强、超参数调整和日志记录。

相比之下，重现 EfficientDet 结果通常需要复杂的 TensorFlow 配置或特定版本的 AutoML 库，这对于快速原型开发来说可能不太友好。

两种模型各有其优点，但其理想应用领域因其架构特性而异。

由于其免NMS设计和低延迟，YOLOv10是时间敏感型任务的卓越选择。

EfficientDet 在特定背景下仍然具有相关性：

尽管EfficientDet是一种开创性的架构，引入了模型缩放的重要概念，但YOLOv10代表了现代对象检测的标准。向无NMS、端到端架构的转变使得YOLOv10能够提供卓越的性能，这对于当今的实时应用至关重要。

对于寻求构建强大、高性能视觉系统的开发人员和研究人员，YOLOv10——以及更广泛的 Ultralytics 生态系统——提供了速度、准确性和开发者体验的引人注目的结合。使用统一平台无缝训练、导出和部署模型的能力显著缩短了产品上市时间。

那些对最新进展感兴趣的人也应该探索Ultralytics YOLO11，它进一步完善了这些功能，以应对更广泛的计算机视觉任务，包括segmentation、姿势估计和定向object detection。

为了做出最明智的决策，请考虑查阅这些相关的技术比较：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv10n model
model = YOLO("yolov10n.pt")

# Train the model on your custom dataset
# efficiently using available GPU resources
model.train(data="coco8.yaml", epochs=100, imgsz=640, batch=16)

# Run inference on an image
results = model("path/to/image.jpg")
```

---

## 使用 Neural Magic 的 DeepSparse 引擎优化 YOLO11 推理

**URL:** https://docs.ultralytics.com/zh/integrations/neural-magic/

**Contents:**
- 使用 Neural Magic 的 DeepSparse 引擎优化 YOLO11 推理
- Neural Magic 的 DeepSparse
- 将 Neural Magic 的 DeepSparse 与 YOLO11 集成的优势
- Neural Magic 的 DeepSparse 技术如何工作？
- 创建在自定义数据集上训练的 YOLO11 稀疏版本
- 用法：使用 DeepSparse 部署 YOLO11
  - 步骤 1：安装
  - 步骤 2：将 YOLO11 导出为 ONNX 格式
  - 步骤 3：部署和运行推理
  - 第四步：性能基准测试

在各种硬件上部署目标检测模型（如Ultralytics YOLO11）时，您可能会遇到独特的优化问题。这时，YOLO11与Neural Magic的DeepSparse Engine的集成就派上用场了。它改变了YOLO11模型的执行方式，并能够在CPU上直接实现GPU级别的性能。

本指南向您展示了如何使用 Neural Magic 的 DeepSparse 部署 YOLO11，如何运行推理，以及如何对性能进行基准测试以确保其得到优化。

Neural Magic 是 于 2025 年 1 月被 Red Hat 收购，并且正在弃用他们的社区版本 deepsparse, sparseml, sparsezoo和 sparsify 库。更多信息，请参见发布的通知 在 Readme 文件中 sparseml GitHub 仓库.

Neural Magic's DeepSparse 是一种推理运行时，旨在优化神经网络在 CPU 上的执行。它应用稀疏性、剪枝和量化等先进技术，以显著降低计算需求，同时保持准确性。DeepSparse 为跨各种设备的高效且可扩展的 神经网络 执行提供了一种敏捷的解决方案。

在使用 DeepSparse 部署 YOLO11 之前，让我们先了解使用 DeepSparse 的优势。一些关键优势包括：

标准 CPU 上的高性能：在 CPU 上提供类似 GPU 的性能，为各种应用提供更易于访问且经济高效的选择。

简化的集成和部署：提供用户友好的工具，可轻松将 YOLO11 集成到应用程序中，包括图像和视频注释功能。

支持多种模型类型: 兼容标准和稀疏优化的 YOLO11 模型，增加了部署的灵活性。

经济高效且可扩展的解决方案: 降低运营费用，并提供高级对象检测模型的可扩展部署。

Neural Magic 的 DeepSparse 技术受人脑在神经网络计算效率方面的启发。它借鉴了大脑的两个关键原则，具体如下：

稀疏性: 稀疏化过程包括从深度学习网络中修剪冗余信息，从而在不影响准确性的前提下，生成更小、更快的模型。 这项技术显著降低了网络的规模和计算需求。

引用局部性：DeepSparse 使用独特的执行方法，将网络分解为 Tensor Columns。这些列按深度方向执行，完全适合 CPU 的缓存。这种方法模仿了大脑的效率，最大限度地减少了数据移动，并最大限度地利用了 CPU 的缓存。

SparseZoo是由 Neural Magic 提供的开源模型仓库，提供一系列预稀疏化的 YOLO11 模型检查点。通过与 Ultralytics 无缝集成的 SparseML，用户可以使用简单的命令行界面，轻松地在特定数据集上微调这些稀疏检查点。

查看Neural Magic的SparseML YOLO11文档以获取更多详细信息。

使用 Neural Magic 的 DeepSparse 部署 YOLO11 涉及几个简单的步骤。在深入了解使用说明之前，请务必查看 Ultralytics 提供的 YOLO11 模型系列。这将帮助您选择最适合您项目要求的模型。以下是如何开始。

DeepSparse Engine 需要 ONNX 格式的 YOLO11 模型。将您的模型导出为此格式对于与 DeepSparse 兼容至关重要。使用以下 CLI 命令导出 YOLO11 模型：

此命令将保存 yolo11n.onnx 模型到您的磁盘。

有了 ONNX 格式的 YOLO11 模型，您可以使用 DeepSparse 部署和运行推理。这可以通过其直观的 Python API 轻松完成：

重要的是检查您的 YOLO11 模型在 DeepSparse 上的性能是否达到最佳。您可以基准测试模型的性能，以分析吞吐量和延迟：

DeepSparse 提供了额外的功能，用于在应用程序中实际集成 YOLO11，例如图像注释和数据集评估。

运行 annotate 命令会处理您指定的图像，检测对象，并保存带有边界框和分类的注释图像。注释图像将存储在 annotation-results 文件夹中。这有助于提供模型检测能力的可视化表示。

运行 eval 命令后，您将收到详细的输出指标，例如精确率、召回率和mAP（平均精确率）。这提供了模型在数据集上性能的全面视图，对于针对特定用例微调和优化 YOLO11 模型、确保高准确性和效率特别有用。

本指南探讨了 Ultralytics 的 YOLO11 与 Neural Magic 的 DeepSparse Engine 的集成。它强调了这种集成如何增强 YOLO11 在 CPU 平台上的性能，从而提供 GPU 级别的效率和先进的神经网络稀疏技术。

有关更多详细信息和高级用法，请访问 Neural Magic 提供的 DeepSparse 文档。您还可以浏览 YOLO11 集成指南和在 YouTube 上观看演练会话。

此外，如需更广泛地了解各种 YOLO11 集成，请访问 Ultralytics 集成指南页面，您可以在其中发现各种其他令人兴奋的集成可能性。

Neural Magic 的 DeepSparse 引擎是一种推理运行时，旨在通过稀疏性、剪枝和量化等先进技术来优化神经网络在 CPU 上的执行。通过将 DeepSparse 与 YOLO11 集成，您可以在标准 CPU 上实现类似 GPU 的性能，从而显著提高推理速度、模型效率和整体性能，同时保持准确性。有关更多详细信息，请查看 Neural Magic 的 DeepSparse 部分。

安装部署 YOLO11 与 Neural Magic 的 DeepSparse 所需的软件包非常简单。您可以使用 CLI 轻松安装它们。以下是您需要运行的命令：

安装完成后，按照安装部分中提供的步骤设置您的环境，并开始将 DeepSparse 与 YOLO11 一起使用。

要将 YOLO11 模型转换为 ONNX 格式（与 DeepSparse 兼容所必需的格式），您可以使用以下 CLI 命令：

此命令将导出您的YOLO11模型（yolo11n.pt) 转换为 (yolo11n.onnx)，可以被 DeepSparse 引擎利用。有关模型导出的更多信息，请参见 模型导出章节.

在 DeepSparse 上对 YOLO11 性能进行基准测试可帮助您分析吞吐量和延迟，以确保您的模型得到优化。您可以使用以下 CLI 命令来运行基准测试：

此命令将为您提供重要的性能指标。 有关更多详细信息，请参见基准测试性能部分。

将 Neural Magic 的 DeepSparse 与 YOLO11 集成有以下几个好处：

要更深入地了解这些优势，请访问将 Neural Magic 的 DeepSparse 与 YOLO11 集成的好处部分。

**Examples:**

Example 1 (markdown):
```markdown
# Install the required packages
pip install deepsparse[yolov8]
```

Example 2 (markdown):
```markdown
# Export YOLO11 model to ONNX format
yolo task=detect mode=export model=yolo11n.pt format=onnx opset=13
```

Example 3 (python):
```python
from deepsparse import Pipeline

# Specify the path to your YOLO11 ONNX model
model_path = "path/to/yolo11n.onnx"

# Set up the DeepSparse Pipeline
yolo_pipeline = Pipeline.create(task="yolov8", model_path=model_path)

# Run the model on your images
images = ["path/to/image.jpg"]
pipeline_outputs = yolo_pipeline(images=images)
```

Example 4 (markdown):
```markdown
# Benchmark performance
deepsparse.benchmark model_path="path/to/yolo11n.onnx" --scenario=sync --input_shapes="[1,3,640,640]"
```

---

## YOLOX 对比 YOLOv8：深入探讨目标检测技术演进

**URL:** https://docs.ultralytics.com/zh/compare/yolox-vs-yolov8/

**Contents:**
- YOLOX 对比 YOLOv8：深入探讨目标检测技术演进
- 性能指标与基准
- YOLOX：无锚框先驱
  - 架构与优势
- YOLOv8：视觉 AI 的现代标准
  - 主要优势
- 架构比较与用例
  - 1. 训练效率和内存
  - 2. 生态系统和易用性
  - 3. 维护良好的生态系统

计算机视觉领域瞬息万变，新架构不断突破速度和准确性的极限。YOLOX和YOLOv8是这一发展历程中的两个重要里程碑。本文将比较YOLOX的无锚框创新与Ultralytics YOLOv8最先进的多功能性之间的技术细节。我们分析它们的架构、性能指标以及在实际应用中的适用性，以帮助您为机器学习项目选择合适的工具。

虽然YOLOv8是一个强大的模型，但该领域已进一步发展。请查看YOLO11，这是Ultralytics的最新迭代，它为detect、segment和姿势估计任务提供了更高的效率、更快的处理速度和更高的准确性。

评估目标检测模型时，推理速度与平均精度 (mAP) 之间的权衡至关重要。下表强调，Ultralytics YOLOv8 在可比模型尺寸下始终以更低的延迟实现更高的精度。

值得注意的是，YOLOv8 通过 ONNX 为 CPU 推理提供了透明的基准测试，这是在没有专用 GPU 的硬件上部署的关键指标。相比之下，标准的 YOLOX 基准测试主要关注 GPU 性能，这为针对标准处理器上的 边缘 AI 应用的用户留下了空白。

由旷视科技的研究人员于2021年发布，YOLOX通过采用无锚点机制，在YOLO系列中引入了重大转变。这一设计选择消除了对预定义锚框的需求，简化了训练过程，并提高了在特定场景下的性能。

YOLOX集成了解耦头，将分类和定位任务分离，以提高收敛速度和准确性。它利用SimOTA（简化最优传输分配）进行动态标签分配，将训练过程视为一个最优传输问题。尽管在当时具有革命性，但YOLOX主要是一个目标检测模型，在同一代码库中缺乏对segmentation或姿势估计等其他任务的原生支持。

Ultralytics于2023年初推出YOLOv8，它代表了对效率、准确性和可用性进行广泛研究的集大成者。它在无锚点（anchor-free）的传统基础上，通过最先进的任务对齐分配器（Task-Aligned Assigner）和现代化架构进行了改进，在各种硬件上均表现出色。

YOLOv8 不仅仅是一个检测模型；它是一个统一的框架。它原生支持图像分类、实例分割、姿势估计和旋转框检测 (OBB)。这种多功能性使开发人员能够使用单一、内聚的 API 解决复杂的多模态问题。

理解这些架构之间的技术差异有助于为实时推理和生产系统选择合适的工具。

Ultralytics YOLO模型的一个突出特点是其训练效率。YOLOv8实现了先进的数据增强策略，例如mosaic和MixUp，这些策略经过优化，旨在防止过拟合，同时保持高训练速度。

至关重要的是，与旧架构或基于重型 Transformer 的模型相比，YOLOv8 在训练和推理期间都表现出 更低的内存需求。这种效率使得在消费级 GPU 上训练自定义模型或将其部署到内存受限的 边缘设备 上成为可能。YOLOX 虽然高效，但通常需要更多手动调整超参数才能实现最佳稳定性。

对于开发人员和研究人员来说，模型周围的生态系统与架构本身同样重要。

使用YOLOv8运行预测非常简单，只需几行代码即可完成。

选择 YOLOv8 意味着可以访问一个维护良好的生态系统。Ultralytics 提供全面的文档、频繁的更新和活跃的社区支持。与更广泛的Ultralytics 生态系统集成简化了工作流程，包括数据标注、数据集管理以及模型部署到 TensorRT 和 OpenVINO 等格式。

YOLOX仍然是专注于无锚框detect头理论方面的学术研究的有力候选者。其代码库为研究2021年从基于锚框到无锚框方法过渡的研究人员提供了清晰的参考。

尽管YOLOX在普及无锚点detect方面发挥了关键作用，但Ultralytics YOLOv8代表了这项技术的自然演进。通过提供卓越的性能指标、多功能的任务学习框架以及无与伦比的用户体验，YOLOv8在现代AI开发中脱颖而出，成为卓越之选。

对于寻求强大、面向未来解决方案，能够从快速原型设计扩展到企业部署的开发者而言，Ultralytics YOLOv8——以及更新的 YOLO11——提供了成功所需的工具。

通过探索这些比较，拓宽您对目标 detect 领域的理解：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pretrained YOLOv8 model
model = YOLO("yolov8n.pt")

# Run inference on an image
results = model.predict("https://ultralytics.com/images/bus.jpg")

# Display the results
results[0].show()
```

---

## Reference for ultralytics/solutions/analytics.py

**URL:** https://docs.ultralytics.com/zh/reference/solutions/analytics/

**Contents:**
- Reference for ultralytics/solutions/analytics.py
- class ultralytics.solutions.analytics.Analytics
  - method ultralytics.solutions.analytics.Analytics.process
  - method ultralytics.solutions.analytics.Analytics.update_graph

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/solutions/analytics.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class for creating and updating various types of charts for visual analytics.

This class extends BaseSolution to provide functionality for generating line, bar, pie, and area charts based on object detection and tracking data.

Process image data and run object tracking to update analytics charts.

Update the graph with new data for single or multiple classes.

**Examples:**

Example 1 (rust):
```rust
Analytics(self, **kwargs: Any) -> None
```

Example 2 (unknown):
```unknown
>>> analytics = Analytics(analytics_type="line")
>>> frame = cv2.imread("image.jpg")
>>> results = analytics.process(frame, frame_number=1)
>>> cv2.imshow("Analytics", results.plot_im)
```

Example 3 (python):
```python
class Analytics(BaseSolution):
    """A class for creating and updating various types of charts for visual analytics.

    This class extends BaseSolution to provide functionality for generating line, bar, pie, and area charts based on
    object detection and tracking data.

    Attributes:
        type (str): The type of analytics chart to generate ('line', 'bar', 'pie', or 'area').
        x_label (str): Label for the x-axis.
        y_label (str): Label for the y-axis.
        bg_color (str): Background color of the chart frame.
        fg_color (str): Foreground color of the chart frame.
        title (str): Title of the chart window.
        max_points (int): Maximum number of data points to display on the chart.
        fontsize (int): Font size for text display.
        color_cycle (cycle): Cyclic iterator for chart colors.
        total_counts (int): Total count of detected objects (used for line charts).
        clswise_count (dict[str, int]): Dictionary for class-wise object counts.
        fig (Figure): Matplotlib figure object for the chart.
        ax (Axes): Matplotlib axes object for the chart.
        canvas (FigureCanvasAgg): Canvas for rendering the chart.
        lines (dict): Dictionary to store line objects for area charts.
        color_mapping (dict[str, str]): Dictionary mapping class labels to colors for consistent visualization.

    Methods:
        process: Process image data and update the chart.
        update_graph: Update the chart with new data points.

    Examples:
        >>> analytics = Analytics(analytics_type="line")
        >>> frame = cv2.imread("image.jpg")
        >>> results = analytics.process(frame, frame_number=1)
        >>> cv2.imshow("Analytics", results.plot_im)
    """

    @plt_settings()
    def __init__(self, **kwargs: Any) -> None:
        """Initialize Analytics class with various chart types for visual data representation."""
        super().__init__(**kwargs)

        import matplotlib.pyplot as plt  # scope for faster 'import ultralytics'
        from matplotlib.backends.backend_agg import FigureCanvasAgg
        from matplotlib.figure import Figure

        self.type = self.CFG["analytics_type"]  # Chart type: "line", "pie", "bar", or "area".
        self.x_label = "Classes" if self.type in {"bar", "pie"} else "Frame#"
        self.y_label = "Total Counts"

        # Predefined data
        self.bg_color = "#F3F3F3"  # background color of frame
        self.fg_color = "#111E68"  # foreground color of frame
        self.title = "Ultralytics Solutions"  # window name
        self.max_points = 45  # maximum points to be drawn on window
        self.fontsize = 25  # text font size for display
        figsize = self.CFG["figsize"]  # Output size, e.g. (12.8, 7.2) -> 1280x720.
        self.color_cycle = cycle(["#DD00BA", "#042AFF", "#FF4447", "#7D24FF", "#BD00FF"])

        self.total_counts = 0  # Stores total counts for line charts.
        self.clswise_count = {}  # dictionary for class-wise counts
        self.update_every = kwargs.get("update_every", 30)  # Only update graph every 30 frames by default
        self.last_plot_im = None  # Cache of the last rendered chart

        # Ensure line and area chart
        if self.type in {"line", "area"}:
            self.lines = {}
            self.fig = Figure(facecolor=self.bg_color, figsize=figsize)
            self.canvas = FigureCanvasAgg(self.fig)  # Set common axis properties
            self.ax = self.fig.add_subplot(111, facecolor=self.bg_color)
            if self.type == "line":
                (self.line,) = self.ax.plot([], [], color="cyan", linewidth=self.line_width)
        elif self.type in {"bar", "pie"}:
            # Initialize bar or pie plot
            self.fig, self.ax = plt.subplots(figsize=figsize, facecolor=self.bg_color)
            self.canvas = FigureCanvasAgg(self.fig)  # Set common axis properties
            self.ax.set_facecolor(self.bg_color)
            self.color_mapping = {}

            if self.type == "pie":  # Ensure pie chart is circular
                self.ax.axis("equal")
```

Example 4 (python):
```python
def process(self, im0: np.ndarray, frame_number: int) -> SolutionResults
```

---

## YOLOv10 对比 YOLOv8：实时目标检测技术比较

**URL:** https://docs.ultralytics.com/zh/compare/yolov10-vs-yolov8/

**Contents:**
- YOLOv10 对比 YOLOv8：实时目标检测技术比较
- 性能分析
- YOLOv10：架构创新提升效率
  - YOLOv10 的主要优势
  - 弱点
- Ultralytics YOLOv8：多功能行业标准
  - YOLOv8为何值得推荐
- 对比分析：架构与用例
  - 架构差异
  - 理想用例

YOLO（You Only Look Once）系列的发展不断推动计算机视觉的边界，为开发者提供了更快、更准确的 目标检测 工具。在 YOLOv10 和 YOLOv8 之间进行选择时，理解它们在架构、效率和生态系统支持方面的细微差别至关重要。尽管 YOLOv10 引入了新颖的架构改进以提高效率，但 YOLOv8 仍然是一个强大、多功能的标准，以其易用性和全面的功能集而闻名。

本指南提供了详细的技术比较，旨在帮助您为您的机器学习项目选择合适的模型。

在COCO dataset上的性能指标阐明了这些模型背后独特的设计理念。YOLOv10 着重于减少参数数量和浮点运算 (FLOPs)，通常在给定模型尺寸下实现更高的 mAP（平均精度）。然而，YOLOv8保持了极具竞争力的推理速度，尤其是在 CPU 上以及导出到TensorRT等优化格式时，平衡了原始速度与实际部署能力。

Authors：Ao Wang, Hui Chen, Lihao Liu, et al.组织:清华大学日期:2024-05-23Arxiv:YOLOv10:Real-TimeEnd-to-End Object Detection GitHub:THU-MIG/yolov10

YOLOv10 由清华大学研究人员开发，其主要目标是消除后处理过程中对非极大值抑制 (NMS) 的依赖。NMS 在对延迟敏感的应用中可能成为瓶颈。YOLOv10 在训练期间引入了一致的双重分配策略，使模型能够为每个对象预测一个最佳边界框，从而有效地使其成为一个端到端检测器。

作者: Glenn Jocher, Ayush Chaurasia, and Jing Qiu机构:Ultralytics日期: 2023-01-10文档:Ultralytics YOLOv8 文档GitHub:ultralytics/ultralytics

由 Ultralytics 推出，YOLOv8 代表了多年来在实用、用户友好型 AI 研究方面的集大成之作。它不仅旨在实现高性能，更旨在提供卓越的开发者体验。YOLOv8 采用无锚点 detect 机制和丰富的梯度流，以确保稳健的训练。其突出特点是原生支持广泛的任务——detect、segment、分类、姿势估计和 obb——所有这些都集成在一个统一的框架中。

Ultralytics 模型，例如 YOLOv8，旨在实现内存高效。这显著降低了训练自定义模型的门槛，因为与 RT-DETR 等大型 Transformer 模型相比，它们需要更少的 CUDA 内存，从而可以在消费级 GPU 上进行训练。

根本区别在于后处理和分配策略。YOLOv10 采用双头架构，其中一个头部在训练期间使用一对多分配（如传统 YOLO）以提供丰富的监督信号，而另一个头部在推理时使用一对一分配，从而消除了对 NMS 的需求。

YOLOv8则采用任务对齐分配器和无锚点耦合头结构。这种设计简化了检测头并提高了泛化能力。尽管它需要NMS，但该操作在ONNX和TensorRT等导出格式中经过高度优化，在稳健的部署流程中，实际的延迟差异通常可以忽略不计。

在这两者之间进行选择通常取决于您项目的具体限制：

高性能边缘 AI (YOLOv10)：如果您的应用程序运行在资源严重受限的硬件上，存储的每一兆字节都很重要，或者如果 NMS 操作在您的目标芯片上造成特定瓶颈，YOLOv10 是一个绝佳的选择。示例包括农业中的嵌入式传感器或轻型无人机。

通用多任务 AI (YOLOv8)：对于绝大多数商业和研究应用，YOLOv8是卓越的选择。它执行segment（例如，精确医学成像）和姿势估计（例如，体育分析）的能力使其具有令人难以置信的多功能性。此外，其广泛的文档和支持确保开发人员能够快速解决问题并更快地部署。

Ultralytics框架的一个主要优势是统一的API。无论您是使用YOLOv8还是探索更新的模型，工作流程都保持一致且直观。

以下是您如何轻松使用Python启动YOLOv8模型训练的方法：

对于 YOLOv10，Ultralytics 包也方便了访问，允许研究人员在熟悉的环境中试验该架构：

YOLOv10 和 YOLOv8 都是计算机视觉领域令人印象深刻的里程碑。YOLOv10 在架构效率方面取得了突破，为专业低延迟应用展示了 NMS-free 的未来潜力。

然而，Ultralytics YOLOv8仍然是开发者和组织推荐的首选模型。其强大的生态系统、经过验证的可靠性和多任务能力提供了一个超越简单detect的全面解决方案。借助Ultralytics YOLOv8，您获得的不仅是一个模型，而是一个完整的工具包，可用于高效构建、训练和部署世界级AI解决方案。

对于那些希望保持在绝对前沿的用户，务必关注 YOLO11，这是 Ultralytics 的最新迭代，相比 YOLOv8 提供了更高的性能和效率提升。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv8 model
model = YOLO("yolov8n.pt")

# Train the model on your custom dataset
# The system automatically handles data downloading and processing
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference on an image
results = model("path/to/image.jpg")
```

Example 2 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv10 model
model = YOLO("yolov10n.pt")

# Train the model using the same simple API
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)
```

---

## EfficientDet 与 PP-YOLOE+：技术对比

**URL:** https://docs.ultralytics.com/zh/compare/efficientdet-vs-pp-yoloe/

**Contents:**
- EfficientDet 与 PP-YOLOE+：技术对比
- 正面交锋性能分析
- EfficientDet：可扩展效率
  - 主要架构特性
  - 优势与劣势
- PP-YOLOE+：无锚点挑战者
  - 主要架构特性
  - 优势与劣势
- Ultralytics 优势：统一解决方案
  - 为什么选择 Ultralytics YOLO11？

在计算机视觉的发展过程中，很少有比较能像Google的EfficientDet和百度的PP-YOLOE+ 之间的对比那样清晰地突出设计理念的转变。EfficientDet 标志着通过复合缩放提高参数效率的里程碑，而 PP-YOLOE+ 则代表了为GPU 推理优化的高速、无锚检测的新时代。

本分析深入探讨了它们的架构、性能指标和实际应用，旨在帮助开发人员为其特定的目标检测需求选择合适的工具。

这两个模型发布之间，性能格局发生了显著变化。EfficientDet 专注于最小化FLOPs（浮点运算）和参数数量，使其在理论上是高效的。然而，PP-YOLOE+ 专为 GPU 等硬件加速器上的实际推理速度而设计，利用 TensorRT 优化。

数据揭示了一个关键见解：尽管 EfficientDet-d0 轻量，但其较大变体 (d5-d7) 存在显著的延迟问题。相反，PP-YOLOE+l 实现了与 EfficientDet-d6 (52.9 vs 52.6) 相当的 平均精度 (mAP)，但在 T4 GPU 上运行速度快了 10 倍以上 (8.36 毫秒 vs 89.29 毫秒)。

EfficientDet 由 Google Brain AutoML 团队推出，旨在打破以往检测器的效率限制。它基于 EfficientNet 主干网络构建，采用统一缩放分辨率、深度和宽度的复合缩放方法。

作者: Mingxing Tan, Ruoming Pang, and Quoc V. Le机构:Google日期: 2019-11-20Arxiv:1911.09070GitHub:google/automl文档:README

EfficientDet 大量使用深度可分离卷积，显著减少了参数数量，但与 YOLO 等模型中使用的标准卷积相比，可能导致 GPU 利用率较低。

了解更多关于 EfficientDet 的信息

PP-YOLOE+ 作为 PaddlePaddle 生态系统的一部分由百度发布，是 PP-YOLOv2 的演进版本。它旨在通过采用完全无锚机制和先进的训练策略，超越 YOLOv5 和 YOLOX 的性能。

作者： PaddlePaddle 作者机构：百度日期： 2022-04-02预印本：2203.16250GitHub：PaddlePaddle/PaddleDetection文档：PP-YOLOE+ 配置

尽管EfficientDet提供了理论效率，PP-YOLOE+提供了原始速度，但开发者通常需要一个在性能、可用性和生态系统支持之间取得平衡的解决方案。这正是Ultralytics YOLO11的优势所在。

与比较模型的专业化特性不同，Ultralytics 模型专为现代 MLOps 工作流设计，提供原生的 PyTorch 体验，易于训练和部署。

运行最先进的模型不应该复杂。以下是使用Ultralytics轻松实现对象检测的方法：

EfficientDet 和 PP-YOLOE+ 之间的选择主要取决于您的硬件限制和遗留要求。

然而，对于绝大多数实际应用——从智慧城市分析到农业监测——Ultralytics YOLO11 脱颖而出，成为最务实的选择。它将现代无锚点检测器的架构创新与无与伦比的用户体验相结合，使您能够专注于解决业务问题，而不是调试框架的复杂性。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLO11 model
model = YOLO("yolo11n.pt")

# Run inference on an image
results = model.predict("https://ultralytics.com/images/bus.jpg")

# Display the results
results[0].show()
```

---

## YOLOv7 对比 YOLOv9：全面技术比较

**URL:** https://docs.ultralytics.com/zh/compare/yolov7-vs-yolov9/

**Contents:**
- YOLOv7 对比 YOLOv9：全面技术比较
- 性能与效率分析
- YOLOv7: 优化可训练的免费赠品包
  - 架构亮点
- YOLOv9：解决信息瓶颈
  - 架构亮点
- 为什么Ultralytics模型（YOLO11和YOLOv8）是首选
  - 简化的用户体验
  - 维护良好的生态系统
  - 内存与训练效率

YOLO（You Only Look Once）系列的发展以神经网络架构的持续创新为标志，平衡了推理速度、准确性和计算效率之间的关键权衡。本次比较深入探讨了 YOLOv7（2022 年发布的里程碑版本，以其可训练的“免费包”而闻名）和 YOLOv9（2024 年架构，引入了可编程梯度信息（PGI）以克服深度网络中的信息瓶颈）。

从 YOLOv7 到 YOLOv9 的转变代表了参数效率的显著飞跃。尽管 YOLOv7 旨在通过扩展高效层聚合网络 (E-ELAN) 突破实时目标检测的极限，但 YOLOv9 引入了架构上的改变，使其能够以更少的参数和浮点运算 (FLOPs) 实现更高的平均精度 (mAP)。

对于专注于边缘AI部署的开发者而言，这种效率至关重要。如下表所示，YOLOv9e 取得了领先的 55.6% mAP，超越了更大的 YOLOv7x，同时保持了具有竞争力的计算开销。相反，较小的 YOLOv9t 为高度受限的设备提供了轻量级解决方案，这是 YOLOv7 未能以相同粒度明确针对的层级。

YOLOv7于2022年7月发布，对YOLO架构进行了一些结构性改革，重点在于优化训练过程，同时不增加推理成本。

YOLOv7 利用 E-ELAN（扩展高效层聚合网络），它控制最短和最长的梯度路径，以使网络能够有效学习更多特征。它还推广了基于拼接模型的模型缩放，允许同时缩放深度和宽度。一个关键创新是计划重参数化卷积，它在推理过程中简化了模型架构以提高速度。

虽然YOLOv7仍然是一个有能力的模型，但它缺乏Ultralytics生态系统中新优化方案的原生支持。与新版本相比，开发者可能会发现与现代MLOps工具的集成更具挑战性。

YOLOv9 于 2024 年初推出，解决了深度学习中的一个根本问题：即数据通过连续层时信息丢失的问题。

YOLOv9的核心创新是可编程梯度信息 (PGI)。在深度网络中，有用信息在前向传播过程中可能会丢失，导致梯度不可靠。PGI提供了一个辅助监督框架，确保关键信息为损失函数所保留。此外，广义高效层聚合网络 (GELAN)通过允许任意阻塞来扩展ELAN的能力，从而最大化参数和计算资源的利用。

这种架构使得 YOLOv9 在复杂检测任务中表现出色，例如在杂乱环境中检测小目标或进行高分辨率航空影像分析。

虽然YOLOv7和YOLOv9是令人印象深刻的学术成就，但Ultralytics YOLO系列——包括YOLOv8和最先进的YOLO11——专为实际、真实世界的应用开发而设计。这些模型优先考虑易用性、生态系统集成和操作效率，使其成为大多数工程团队的卓越选择。

Ultralytics 模型封装在一个统一的 Python API 中，抽象化了训练流程的复杂性。在目标检测、实例分割、姿势估计和旋转框检测 (OBB) 任务之间切换只需更改一个参数，这种多功能性是标准 YOLOv7 或 YOLOv9 实现所不具备的。

选择 Ultralytics 模型可获得一个强大的生态系统。这包括与 Ultralytics HUB（以及即将推出的 Ultralytics Platform）的无缝集成，用于云训练和数据集管理。此外，活跃的社区和频繁的更新确保了与最新硬件的兼容性，例如导出到 TensorRT 或 OpenVINO 以实现最佳推理速度。

Ultralytics 模型以其训练效率而闻名。与基于 Transformer 的模型（如RT-DETR）可能内存占用高且收敛缓慢不同，Ultralytics YOLO 模型利用优化的数据加载器和Mosaic 数据增强，以更低的 CUDA 内存需求实现快速训练。这使得开发者能够在消费级 GPU 上训练最先进的模型。

YOLOv7 和 YOLOv9 都代表了计算机视觉史上的重要里程碑。YOLOv9 凭借其 PGI 架构，在 v7 的基础上进行了引人注目的升级，提供了更好的效率和精度。然而，对于寻求 通用、易用且支持良好解决方案 的开发者而言，Ultralytics YOLO11 仍然是推荐的选择。它在性能、全面文档和多任务能力（detect、segment、classify、姿势估计）之间的平衡，提供了从概念到生产的最快路径。

要为您的特定计算机视觉任务找到最合适的模型，可以探索这些其他比较：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a model (YOLO11 automatically handles architecture)
model = YOLO("yolo11n.pt")  # Load a pretrained model

# Train the model with a single line of code
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Perform inference on an image
results = model("path/to/image.jpg")
```

---

## YOLOv5 对比 RTDETRv2：平衡实时速度与 Transformer 精度

**URL:** https://docs.ultralytics.com/zh/compare/yolov5-vs-rtdetr/

**Contents:**
- YOLOv5 对比 RTDETRv2：平衡实时速度与 Transformer 精度
- 模型规格与来源
- 架构与设计理念
  - Ultralytics YOLOv5
  - RTDETRv2
- 性能分析
  - 训练效率与内存使用
- Ultralytics YOLOv5 的主要优势
- 代码对比：易用性
- 理想用例

在快速发展的计算机视觉领域，选择合适的物体 detect 模型对于项目成功至关重要。这项全面的技术比较考察了两种不同的方法：YOLOv5（以其多功能性和速度而闻名的传奇 CNN-based 检测器）和 RTDETRv2（一个专注于高准确度的现代 Transformer-based 模型）。

尽管 RTDETRv2 利用 Vision Transformers (ViT) 捕获全局上下文，但 Ultralytics YOLOv5 仍然是需要低资源开销的强大、可部署解决方案的开发者的首选。

在深入探讨性能指标之前，了解每个模型的背景和架构理念至关重要。

这些模型之间的根本区别在于它们处理视觉数据的方式。

YOLOv5 采用高度优化的卷积神经网络（CNN）架构。它利用了改进的 CSPDarknet 主干网络和路径聚合网络（PANet）颈部网络来提取特征图。

RTDETRv2 (Real-Time Detection Transformer v2) 代表了向 Transformer 架构的转变。

下表直接比较了关键性能指标。尽管RTDETRv2在COCO数据集上显示出令人印象深刻的准确性（mAP），YOLOv5展现出卓越的推理速度，尤其是在Transformer模型通常表现不佳的CPU硬件上。

虽然 RTDETRv2 实现了更高的 mAP 值，但请注意 速度 和 FLOPs 列。YOLOv5n 在 CPU 上运行速度为 73.6 毫秒，使其在非加速硬件上进行实时应用成为可能。RTDETRv2 模型则显著更重，需要强大的 GPU 来维持实时帧率。

YOLOv5 的一个关键优势是其训练效率。像 RTDETRv2 这样的 Transformer 模型以高 VRAM 消耗和缓慢的收敛速度而闻名。

对于大多数开发者和商业应用，YOLOv5 提供了更平衡和实用的一系列优势：

如果您需要比 YOLOv5 更高的精度，同时保持这些生态系统优势，请考虑新的YOLO11。它融合了现代架构改进，以 YOLO 所期望的效率媲美甚至超越 Transformer 精度。

以下示例展示了使用 Ultralytics 包中 YOLOv5 的简便性。

尽管RTDETRv2展示了Transformer在计算机视觉中实现令人印象深刻的准确性的潜力，但它在硬件资源和训练复杂性方面带来了显著的成本。对于绝大多数实际应用而言，Ultralytics YOLOv5仍然是更优的选择。它将速度、精度和低内存使用完美结合，并辅以支持性生态系统和丰富的文档，确保开发者能够构建可扩展、高效且有效的AI解决方案。

对于那些寻求最新性能同时又不牺牲 Ultralytics 框架可用性的人，我们强烈推荐探索YOLO11，它弥合了 CNN 效率与 Transformer 级别准确性之间的鸿沟。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv5 model
model = YOLO("yolov5s.pt")

# Run inference on an image
results = model("https://ultralytics.com/images/bus.jpg")

# Display results
for result in results:
    result.show()  # show to screen
    result.save(filename="result.jpg")  # save to disk
```

---

## Reference for ultralytics/data/dataset.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/data/dataset/

**Contents:**
- Reference for ultralytics/data/dataset.py
- class ultralytics.data.dataset.YOLODataset
  - method ultralytics.data.dataset.YOLODataset.build_transforms
  - method ultralytics.data.dataset.YOLODataset.cache_labels
  - method ultralytics.data.dataset.YOLODataset.close_mosaic
  - method ultralytics.data.dataset.YOLODataset.collate_fn
  - method ultralytics.data.dataset.YOLODataset.get_labels
  - method ultralytics.data.dataset.YOLODataset.update_labels_info
- class ultralytics.data.dataset.YOLOMultiModalDataset
  - property ultralytics.data.dataset.YOLOMultiModalDataset.category_names

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/data/dataset.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Dataset class for loading object detection and/or segmentation labels in YOLO format.

This class supports loading data for object detection, segmentation, pose estimation, and oriented bounding box (OBB) tasks using the YOLO format.

Build and append transforms to the list.

Cache dataset labels, check images and read shapes.

Disable mosaic, copy_paste, mixup and cutmix augmentations by setting their probabilities to 0.0.

Collate data samples into batches.

Return dictionary of labels for YOLO training.

This method loads labels from disk or cache, verifies their integrity, and prepares them for training.

Update label format for different tasks.

cls is not with bboxes now, classification and semantic segmentation need an independent cls label Can also support classification and semantic segmentation by adding or removing dict keys there.

Dataset class for loading object detection and/or segmentation labels in YOLO format with multi-modal support.

This class extends YOLODataset to add text information for multi-modal model training, enabling models to process both image and text data.

Return category names for the dataset.

Return frequency of each category in the dataset.

Get negative text samples based on frequency threshold.

Enhance data transformations with optional text augmentation for multi-modal training.

Add text information for multi-modal model training.

Dataset class for object detection tasks using annotations from a JSON file in grounding format.

This dataset is designed for grounding tasks where annotations are provided in a JSON file rather than the standard YOLO format text files.

Return unique category names from the dataset.

Return frequency of each category in the dataset.

Get negative text samples based on frequency threshold.

Configure augmentations for training with optional text loading.

Load annotations from a JSON file, filter, and normalize bounding boxes for each image.

The image files would be read in get_labels function, return empty list here.

Load labels from cache or generate them from JSON file.

Verify the number of instances in the dataset matches expected counts.

This method checks if the total number of bounding box instances in the provided labels matches the expected count for known datasets. It performs validation against a predefined set of datasets with known instance counts.

For unrecognized datasets (those not in the predefined expected_counts), a warning is logged and verification is skipped.

Dataset as a concatenation of multiple datasets.

This class is useful to assemble different existing datasets for YOLO training, ensuring they use the same collation function.

Set mosaic, copy_paste and mixup options to 0.0 and build transformations.

Collate data samples into batches.

Semantic Segmentation Dataset.

Dataset class for image classification tasks extending torchvision ImageFolder functionality.

This class offers functionalities like image augmentation, caching, and verification. It's designed to efficiently handle large datasets for training deep learning models, with optional image transformations and caching mechanisms to speed up training.

Return subset of data and targets corresponding to given indices.

Return the total number of samples in the dataset.

Verify all images in dataset.

**Examples:**

Example 1 (typescript):
```typescript
YOLODataset(self, *args, data: dict | None = None, task: str = "detect", **kwargs)
```

Example 2 (json):
```json
>>> dataset = YOLODataset(img_path="path/to/images", data={"names": {0: "person"}}, task="detect")
>>> dataset.get_labels()
```

Example 3 (python):
```python
class YOLODataset(BaseDataset):
    """Dataset class for loading object detection and/or segmentation labels in YOLO format.

    This class supports loading data for object detection, segmentation, pose estimation, and oriented bounding box
    (OBB) tasks using the YOLO format.

    Attributes:
        use_segments (bool): Indicates if segmentation masks should be used.
        use_keypoints (bool): Indicates if keypoints should be used for pose estimation.
        use_obb (bool): Indicates if oriented bounding boxes should be used.
        data (dict): Dataset configuration dictionary.

    Methods:
        cache_labels: Cache dataset labels, check images and read shapes.
        get_labels: Return dictionary of labels for YOLO training.
        build_transforms: Build and append transforms to the list.
        close_mosaic: Set mosaic, copy_paste and mixup options to 0.0 and build transformations.
        update_labels_info: Update label format for different tasks.
        collate_fn: Collate data samples into batches.

    Examples:
        >>> dataset = YOLODataset(img_path="path/to/images", data={"names": {0: "person"}}, task="detect")
        >>> dataset.get_labels()
    """

    def __init__(self, *args, data: dict | None = None, task: str = "detect", **kwargs):
        """Initialize the YOLODataset.

        Args:
            data (dict, optional): Dataset configuration dictionary.
            task (str): Task type, one of 'detect', 'segment', 'pose', or 'obb'.
            *args (Any): Additional positional arguments for the parent class.
            **kwargs (Any): Additional keyword arguments for the parent class.
        """
        self.use_segments = task == "segment"
        self.use_keypoints = task == "pose"
        self.use_obb = task == "obb"
        self.data = data
        assert not (self.use_segments and self.use_keypoints), "Can not use both segments and keypoints."
        super().__init__(*args, channels=self.data.get("channels", 3), **kwargs)
```

Example 4 (python):
```python
def build_transforms(self, hyp: dict | None = None) -> Compose
```

---

## EfficientDet 对比 RTDETRv2：目标检测的技术比较

**URL:** https://docs.ultralytics.com/zh/compare/efficientdet-vs-rtdetr/

**Contents:**
- EfficientDet 对比 RTDETRv2：目标检测的技术比较
- 模型概述
- 架构分析
  - EfficientDet：通过复合缩放实现效率
  - RTDETRv2：实时 detect Transformer
- 性能指标
- Ultralytics 优势：卓越的替代方案
  - 为什么选择 Ultralytics YOLO11？
  - 代码示例：YOLO11快速入门
- 理想用例

目标检测领域已显著发展，从传统的卷积神经网络 (CNN) 转向现代的基于Transformer的架构。这一演进中的两个显著里程碑是Google的可扩展CNN架构EfficientDet和百度推出的实时检测TransformerRTDETRv2。

本指南对这两种模型进行了深入的技术比较，分析了它们的架构创新、性能指标和理想部署场景。我们还将探讨 Ultralytics YOLO11 如何作为一种强大的替代方案，为各种计算机视觉应用提供统一的生态系统。

在深入探讨架构细节之前，了解每个模型的起源和主要目标至关重要。

EfficientDet详情： 作者：Mingxing Tan, Ruoming Pang, and Quoc V. Le 机构：Google Research 日期：2019-11-20 Arxiv：https://arxiv.org/abs/1911.09070 GitHub：https://github.com/google/automl/tree/master/efficientdet 文档：https://github.com/google/automl/tree/master/efficientdet#readme

RTDETRv2 详情： 作者：Wenyu Lv, Yian Zhao, Qinyao Chang, Kui Huang, Guanzhong Wang, and Yi Liu 组织：百度 日期：2023-04-17 Arxiv：https://arxiv.org/abs/2304.08069 GitHub：https://github.com/lyuwenyu/RT-DETR/tree/main/rtdetrv2_pytorch 文档：https://github.com/lyuwenyu/RT-DETR/tree/main/rtdetrv2_pytorch#readme

EfficientDet和RTDETRv2的核心区别在于它们在特征提取和边界框预测方面的基本方法。

EfficientDet 旨在打破通过简单地增大模型来提高准确性的趋势。它采用 EfficientNet 主干网络，并引入了加权双向特征金字塔网络（BiFPN）。

RTDETRv2 建立在 DETR（Detection Transformer）的成功之上，但解决了其高计算成本和收敛速度慢的问题。它是一个无锚点模型，利用自注意力机制来建模全局上下文。

Transformer 与 CNN 内存使用对比

尽管像 RTDETRv2 这样的 Transformer 模型擅长捕获全局上下文，但由于注意力机制的二次复杂度，它们在训练期间通常需要比 EfficientDet 或 YOLO 等基于 CNN 的架构显著更多的 CUDA 内存。

在选择用于部署的模型时，开发人员必须权衡精度 (mAP)、速度（延迟）和模型大小（参数）之间的利弊。下表比较了 EfficientDet 变体与 RTDETRv2 的性能。

尽管EfficientDet和RTDETRv2是强大的模型，但寻求平衡性能、可用性和多功能性的整体解决方案的开发者应考虑Ultralytics YOLO系列。诸如最新的YOLO11等模型为从研究到生产部署的广泛应用提供了引人注目的选择。

以下示例演示了使用Ultralytics YOLO11运行推理是多么容易，展示了与旧框架相比，API的简洁性。

选择合适的模型在很大程度上取决于您的具体硬件限制和项目需求。

EfficientDet 和 RTDETRv2 都代表了计算机视觉领域的重大成就。EfficientDet 通过缩放突破了效率的边界，而 RTDETRv2 则证明了 Transformer 可以足够快以满足实时应用的需求。

然而，对于绝大多数开发人员和企业而言，Ultralytics YOLO 模型代表了最实用的解决方案。通过将最先进的性能与无与伦比的开发人员体验和丰富的生态系统相结合，Ultralytics 使您能够更快、更可靠地构建强大的AI 解决方案。

为了进一步帮助您做出决定，请探索以下其他比较：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLO11 model
model = YOLO("yolo11n.pt")  # 'n' for nano, or try 's', 'm', 'l', 'x'

# Run inference on an image
results = model("path/to/image.jpg")

# Display the results
results[0].show()
```

---

## YOLOv5 vs. DAMO-YOLO：详细技术对比

**URL:** https://docs.ultralytics.com/zh/compare/yolov5-vs-damo-yolo/

**Contents:**
- YOLOv5 vs. DAMO-YOLO：详细技术对比
- 性能指标与基准
  - 结果分析
- Ultralytics YOLOv5：多功能行业标准
  - 主要优势
- DAMO-YOLO：研究驱动的精度
  - 主要特点
- 架构与操作差异
  - 架构：简洁性与复杂性
  - 训练效率与内存

在快速发展的计算机视觉领域，选择合适的物体 detect 架构对于项目成功至关重要。本比较探讨了两个重要模型：Ultralytics YOLOv5（一个以其可靠性和速度而闻名的全球通用行业标准）和 DAMO-YOLO（一个来自阿里巴巴集团的以研究为重点的模型，引入了新颖的架构搜索技术）。

尽管这两个模型都旨在解决 object detection 任务，但它们迎合了不同的需求。YOLOv5 优先考虑易用性、部署多功能性和实际性能平衡，而 DAMO-YOLO 则侧重于通过 Neural Architecture Search (NAS) 和重型特征融合机制来推动学术界限。

在选择用于生产的模型时，理解推理速度和检测精度之间的权衡至关重要。以下数据突出了这些模型在COCO 数据集上的表现，COCO 是目标检测的标准基准。

数据揭示了设计理念上的明显二分法。YOLOv5n (Nano) 在速度和效率方面是无可争议的冠军，在 GPU 上提供了令人难以置信的 1.12 毫秒 推理时间，并具有广泛可用的 CPU 性能。这使其成为对低延迟要求极高的 边缘 AI 应用的理想选择。

DAMO-YOLO 模型，例如 DAMO-YOLOl，实现了略高的 平均精度 (mAP)，峰值为 50.8，但以 CPU 性能指标的不透明性为代价。DAMO-YOLO 缺乏报告的 CPU 速度表明它主要针对高端 GPU 环境进行了优化，从而限制了其在更广泛的部署场景（如移动应用程序或嵌入式系统）中的灵活性。

作者：Glenn JocherGlenn Jocher组织：Ultralytics日期：2020-06-26 GitHubyolov5文档yolov5

自发布以来，YOLOv5 已成为计算机视觉领域的基石。它原生构建于PyTorch之上，在复杂性与易用性之间取得了平衡，提供“开箱即用”的体验。其架构采用CSPDarknet骨干网络和PANet颈部，可有效聚合不同尺度的特征，以检测各种大小的目标。

YOLOv5 与流行的 MLOps 工具无缝集成。您可以使用Weights & Biases 或 Comet 通过一条命令跟踪您的实验，确保您的训练运行可复现且易于分析。

作者： 徐贤哲、蒋一奇、陈卫华、黄一伦、张远、孙秀宇机构： 阿里巴巴集团日期： 2022-11-23预印本：https://arxiv.org/abs/2211.15444v2GitHub：https://github.com/tinyvision/DAMO-YOLO

DAMO-YOLO 是阿里巴巴达摩院开发的一种方法。它引入了一系列先进技术，包括神经网络架构搜索 (NAS) 以自动设计高效骨干网络 (MAE-NAS)、一种称为 RepGFPN（重参数化广义特征金字塔网络）的重型颈部结构，以及一个名为 ZeroHead 的轻量级头部。

YOLOv5和DAMO-YOLO之间的差异不仅限于简单的指标，还延伸到它们的核心设计理念和操作要求。

YOLOv5 采用手工设计、直观的架构。其基于锚框的方法易于理解和调试。相比之下，DAMO-YOLO 依赖于大量的重参数化和自动化搜索（NAS）。虽然 NAS 可以产生高效的结构，但它通常会导致“黑盒”模型，使得开发者难以定制或解释。此外，DAMO-YOLO 中沉重的颈部网络（RepGFPN）增加了训练期间的计算负载，与 YOLOv5 高效的 CSP 设计相比，需要更多的GPU 内存。

Ultralytics 模型以其训练效率而闻名。YOLOv5 通常需要更少的 CUDA 内存，使其可以在消费级 GPU 上进行训练。DAMO-YOLO 凭借其复杂的重参数化和蒸馏过程，通常需要高端硬件才能有效训练。此外，Ultralytics 提供了庞大的预训练权重库和自动超参数调优功能，以加速收敛过程。

也许最显著的区别在于生态系统。YOLOv5 不仅仅是一个模型；它是一套全面的工具套件的一部分。

DAMO-YOLO 主要是一个研究型代码库，缺乏这种程度的完善支持，这使得其集成到商业产品中更具挑战性。

这些模型之间的选择通常取决于具体的部署环境。

得益于Ultralytics Python包，运行YOLOv5非常简单。以下示例演示了如何加载预训练模型并在图像上运行推理。

YOLOv5 和 DAMO-YOLO 都对目标 detect 领域做出了重大贡献。DAMO-YOLO 展示了神经架构搜索和高级特征融合在实现高精度基准方面的潜力。

然而，对于绝大多数开发人员、工程师和企业而言，Ultralytics YOLOv5 仍然是卓越之选。其无与伦比的易用性、强大的性能平衡以及维护良好的生态系统的保障，确保项目从原型到生产的过渡阻力最小。其在CPU和GPU上高效部署的能力，结合更低的训练内存需求，使YOLOv5成为实际应用中高度实用的解决方案。

对于那些希望利用计算机视觉技术绝对最新成果的用户，Ultralytics 持续创新，推出了 YOLOv8 和最先进的 YOLO11。这些新模型在 YOLOv5 的坚实基础上，提供了更高的速度、精度和任务多功能性。

为了进一步了解这些模型如何融入更广泛的生态系统，请查阅这些详细的比较：

**Examples:**

Example 1 (python):
```python
import torch

# Load a pre-trained YOLOv5s model from PyTorch Hub
model = torch.hub.load("ultralytics/yolov5", "yolov5s", pretrained=True)

# Define an image URL or local path
img = "https://ultralytics.com/images/zidane.jpg"

# Run inference
results = model(img)

# Print results to the console
results.print()

# Show the image with bounding boxes
results.show()
```

---

## Ultralytics YOLOv5 综合指南 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/yolov5/

**Contents:**
- Ultralytics YOLOv5 综合指南
- 探索与学习
- 支持的环境
- 项目状态
- 连接与贡献
- 常见问题
  - Ultralytics YOLOv5 的主要特点是什么？
  - 如何在我的数据集上训练自定义 YOLOv5 模型？
  - 与其他目标检测模型（如 RCNN）相比，我为什么要使用 Ultralytics YOLOv5？
  - 如何在训练期间优化 YOLOv5 模型性能？

欢迎来到 Ultralytics YOLOv5🚀 文档！Ultralytics YOLOv5 是革命性的“You Only Look Once”目标检测模型的第五次迭代，旨在实时提供高速、高精度的结果。虽然 YOLOv5 仍然是一个强大的工具，但请考虑探索它的继任者 Ultralytics YOLOv8，以获得最新的进展。

基于PyTorch构建，这个强大的深度学习框架因其多功能性、易用性和高性能而获得了极高的人气。我们的文档将指导您完成安装过程，解释模型的架构细节，展示各种用例，并提供一系列详细教程。这些资源将帮助您充分发挥 YOLOv5 在您的计算机视觉项目中的潜力。让我们开始吧！

这是一个综合教程汇编，将指导您了解 YOLOv5 的不同方面。

Ultralytics 提供了一系列即用型环境，每个环境都预装了必要的依赖项，例如 CUDA、CuDNN、Python 和 PyTorch，以快速启动您的项目。您还可以使用 Ultralytics HUB 管理您的模型和数据集。

此徽章表示所有 YOLOv5 GitHub Actions 持续集成 (CI) 测试均已成功通过。这些 CI 测试严格检查 YOLOv5 在各个关键方面的功能和性能：训练、验证、推理、导出 和 基准测试。它们确保在 macOS、Windows 和 Ubuntu 上运行的一致性和可靠性，测试每 24 小时进行一次，并在每次提交新内容时进行。

您使用 YOLOv5 的旅程不必是孤单的。加入我们在 GitHub 上的活跃社区，与 LinkedIn 上的专业人士联系，在 Twitter 上分享您的结果，并在 YouTube 上查找教育资源。在 TikTok 和 BiliBili 上关注我们，获取更多引人入胜的内容。

有兴趣参与贡献吗？我们欢迎各种形式的贡献，从代码改进和错误报告到文档更新。请查看我们的贡献指南以获取更多信息。

我们很高兴看到您使用 YOLOv5 的创新方式。深入研究、实验并彻底改变您的计算机视觉项目！🚀

Ultralytics YOLOv5 以其高速高精度的目标检测能力而闻名。基于 PyTorch 构建，它功能多样且用户友好，适用于各种计算机视觉项目。主要功能包括实时推理、支持多种训练技巧（如测试时增强 (TTA) 和模型集成），以及兼容 TFLite、ONNX、CoreML 和 TensorRT 等导出格式。要深入了解 Ultralytics YOLOv5 如何提升您的项目，请查阅我们的 TFLite、ONNX、CoreML、TensorRT 导出指南。

在您自己的数据集上训练自定义 YOLOv5 模型包括几个关键步骤。首先，准备所需格式并使用标签进行注释的数据集。然后，配置 YOLOv5 训练参数，并使用 train.py 脚本。有关此过程的深入教程，请查阅我们的 训练自定义数据指南。它提供了逐步指导，以确保为您的特定用例获得最佳结果。

由于 Ultralytics YOLOv5 在实时目标检测中具有卓越的速度和准确性，因此它比 R-CNN 等模型更受欢迎。YOLOv5 一次性处理整个图像，与 RCNN 基于区域的方法（涉及多次传递）相比，速度明显更快。此外，YOLOv5 与各种导出格式的无缝集成以及广泛的文档使其成为初学者和专业人士的绝佳选择。请在我们的架构总结中了解更多关于架构优势的信息。

优化 YOLOv5 模型性能包括调整各种超参数，并结合数据增强和迁移学习等技术。Ultralytics 提供了关于超参数进化和剪枝/稀疏化的全面资源，以提高模型效率。您可以在我们的最佳训练结果技巧指南中找到实用技巧，该指南提供了在训练期间实现最佳性能的可操作见解。

Ultralytics YOLOv5 支持多种环境，包括 Gradient、Google Colab 和 Kaggle 上的免费 GPU Notebook，以及 Google Cloud、Amazon AWS 和 Azure 等主要云平台。还提供 Docker 镜像，方便设置。有关设置这些环境的详细指南，请查看我们的 Supported Environments 部分，其中包含每个平台的逐步说明。

---

## EfficientDet 对比 YOLOv8：目标检测巨头技术比较

**URL:** https://docs.ultralytics.com/zh/compare/efficientdet-vs-yolov8/

**Contents:**
- EfficientDet 对比 YOLOv8：目标检测巨头技术比较
- 性能指标：速度、准确性与效率
  - 数据分析
- 架构与设计理念
  - EfficientDet 详情
  - YOLOv8 详情
- Ultralytics 优势
  - 1. 易用性和实施
  - 2. 跨任务的通用性
  - 3. 训练和内存效率

在快速发展的计算机视觉领域，选择正确的架构是项目成功的关键。本分析对比了两种有影响力的模型：EfficientDet 是Google 的一个研究里程碑，重点关注参数效率，而 YOLOv8是Ultralytics 为实时应用和易用性而设计的最先进模型。

EfficientDet在模型缩放方面引入了开创性概念，但此后，YOLOv8和尖端的YOLO11等新架构重新定义了速度、精度和部署多功能性的标准。

在选择用于生产的模型时，开发人员必须权衡推理延迟和检测精度之间的利弊。下表直接比较了 COCO 数据集上的性能指标。

这些指标突出了设计理念上的显著差异。EfficientDet最小化FLOPs（浮点运算），这在历史上与理论效率相关。然而，在实际的实时推理场景中——尤其是在GPU上——YOLOv8展现出显著优势。

FLOPs计数低并不总是意味着执行速度快。EfficientDet在理论计算成本上进行了高度优化，但YOLOv8更有效地利用了现代GPU（如NVIDIA T4/A100）的并行处理能力，从而实现了更低的实际延迟。

理解架构的细微差别有助于解释上述性能差异。

EfficientDet 基于复合缩放原则构建，该原则统一缩放网络分辨率、深度和宽度。它采用 EfficientNet 主干网络，并引入了 BiFPN（双向特征金字塔网络）。BiFPN 实现了加权特征融合，学习哪些特征最为重要。虽然这带来了高参数效率，但 BiFPN 复杂而不规则的连接在偏好规则内存访问模式的硬件上执行时，计算成本可能很高。

了解更多关于 EfficientDet 的信息

YOLOv8 代表了向无锚框检测机制的转变，通过消除手动计算锚框的需要，简化了训练过程。它采用经过C2f模块修改的CSPDarknet主干网络，与以前的版本相比，这改善了梯度流和特征丰富度。头部采用解耦结构，独立处理分类和回归任务，并采用任务对齐分配进行动态标签分配。这种架构经过专门设计，旨在最大限度地提高 GPU 硬件上的吞吐量。

尽管EfficientDet是一项卓越的学术成就，但围绕YOLOv8和YOLO11的Ultralytics生态系统为专注于产品交付和MLOps的开发者提供了实实在在的益处。

实现 EfficientDet 通常需要在 TensorFlow 生态系统中处理复杂的配置文件和依赖项。相比之下，Ultralytics 模型优先考虑开发者体验。只需几行 Python 代码即可加载、训练和部署模型。

EfficientDet 主要是一种 目标检测架构。Ultralytics YOLOv8 远不止简单的边界框。在同一框架内，用户可以执行：

训练现代 Transformer 模型或复杂的多尺度架构可能需要大量资源。Ultralytics YOLO 模型以其内存效率而闻名。

Ultralytics 模型可与 Weights & Biases、Comet 和 ClearML 等工具无缝集成，用于实验跟踪，以及与 Roboflow 集成，用于数据集管理。

这些模型之间的选择通常决定了在特定环境中部署的可行性。

EfficientDet 仍然是对 深度学习领域的一项重要贡献，证明了智能缩放可以生成紧凑的模型。然而，对于当今绝大多数实际应用而言，Ultralytics YOLOv8（以及更新的 YOLO11）提供了更优越的解决方案。

现代硬件上极快的推理速度、全面的 Python SDK 以及处理多种视觉任务的能力相结合，使 Ultralytics 模型成为开发人员的首选。无论您是构建安全警报系统还是分析卫星图像，Ultralytics 生态系统都提供了将您的项目从概念高效地推向生产的工具。

要更广泛地了解目标 detect 选择，可以考虑这些比较：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv8 model
model = YOLO("yolov8n.pt")

# Train on a custom dataset with a single command
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference on an image
detection = model("https://ultralytics.com/images/bus.jpg")
```

---

## 使用 Ultralytics 进行 K 折交叉验证 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/guides/kfold-cross-validation/

**Contents:**
- 使用 Ultralytics 进行 K 折交叉验证
- 简介
- 设置
- 为对象检测数据集生成特征向量
- K 折数据集分割
- 保存记录（可选）
- 使用 K 折数据分割训练 YOLO
- 结论
- 常见问题
  - 什么是 K 折交叉验证？为什么它在目标检测中很有用？

本综合指南阐述了如何在 Ultralytics 生态系统中为目标检测数据集实现 K 折交叉验证。我们将利用 YOLO detect 格式和关键的 Python 库，如 sklearn、Pandas 和 PyYAML，引导您完成必要的设置、特征向量生成过程以及 K 折数据集划分的执行。

无论您的项目涉及 Fruit Detection 数据集还是自定义数据源，本教程旨在帮助您理解和应用 K-Fold 交叉验证，以增强您的 机器学习 模型。当我们应用 k=5 此教程的 folds，请记住，最佳 folds 数量可能因您的数据集和项目的具体情况而异。

在我们的演示中，我们使用 水果检测 数据集。

本教程适用于 k=5 folds。但是，您应该确定最适合您特定数据集的 folds 数量。

启动一个新的 python 虚拟环境（venv)，并激活它。使用 pip （或您喜欢的包管理器）来安装：

首先创建一个新的 example.py 以下步骤的 Python 文件。

现在，读取数据集 YAML 文件的内容并提取类标签的索引。

初始化一个空的 pandas DataFrame。

以下是已填充 DataFrame 的示例视图：

行索引标签文件，每个文件对应于数据集中的一个图像，列对应于您的类标签索引。每一行代表一个伪特征向量，其中包含数据集中存在的每个类标签的计数。这种数据结构使得可以将K-Fold 交叉验证应用于目标检测数据集。

现在我们将使用 KFold 类，来自 sklearn.model_selection 生成 k 数据集的分割。

数据集现已拆分为 k folds，每个 fold 都有一个列表，其中包含 train 和 val 索引。我们将构建一个DataFrame来更清晰地展示这些结果。

现在，我们将计算每个fold的类标签分布，作为类中存在的类的比率 val 到那些存在于 train.

理想情况下，所有类别的比例在每个分割中以及不同类别之间都应合理地相似。然而，这会受到数据集具体情况的影响。

接下来，我们为每个拆分创建目录和数据集 YAML 文件。

最后，将图像和标签复制到每个分割的相应目录（'train' 或 'val'）中。

作为可选项，您可以将 K-Fold 拆分和标签分布 DataFrame 的记录保存为 CSV 文件，以供将来参考。

接下来，遍历数据集 YAML 文件以运行训练。结果将保存到由以下内容指定的目录中 project 和 name 参数。默认情况下，此目录是'runs/detect/train#'，其中#是一个整数索引。

您还可以使用 Ultralytics data.utils.autosplit 函数来自动分割数据集：

在本指南中，我们探讨了使用 K-Fold 交叉验证来训练 YOLO 对象检测模型的过程。我们学习了如何将我们的数据集分成 K 个分区，确保不同折叠之间的平衡类分布。

我们还探讨了创建报告 DataFrame 的过程，以可视化数据分割以及这些分割中的标签分布，从而使我们能够清楚地了解训练集和验证集的结构。

作为可选项，我们保存了记录以供将来参考，这在大型项目中或在排除模型性能故障时特别有用。

最后，我们使用循环中的每个分割实现了实际的模型训练，保存我们的训练结果以供进一步分析和比较。

K折交叉验证是一种充分利用现有数据的可靠方法，它有助于确保您的模型性能在不同的数据子集上保持可靠和一致。 这样可以得到一个更具泛化能力和更可靠的模型，从而降低模型过拟合特定数据模式的可能性。

请记住，尽管本指南中使用了YOLO，但这些步骤大多可迁移到其他机器学习模型。理解这些步骤将使您能够在自己的机器学习项目中有效地应用交叉验证。

K-折交叉验证是一种将数据集分成“k”个子集（折叠）以更可靠地评估模型性能的技术。每个折叠都用作训练和 验证数据。在对象检测的上下文中，使用 K-折交叉验证有助于确保您的 Ultralytics YOLO 模型的性能在不同的数据分割中是稳健且可推广的，从而提高其可靠性。有关使用 Ultralytics YOLO 设置 K-折交叉验证的详细说明，请参阅 使用 Ultralytics 的 K-折交叉验证。

要使用 Ultralytics YOLO 实施 K 折交叉验证，您需要按照以下步骤操作：

有关全面的指南，请参阅我们文档中的K-Fold 数据集分割部分。

Ultralytics YOLO 提供最先进的实时目标检测，具有高精度和效率。它用途广泛，支持多种计算机视觉任务，例如检测、分割和分类。此外，它还与 Ultralytics HUB 等工具无缝集成，用于无代码模型训练和部署。有关更多详细信息，请浏览我们的 Ultralytics YOLO 页面，了解其优势和功能。

您的标注应遵循 YOLO 检测格式。每个标注文件必须列出对象类别，以及其在图像中的边界框坐标。YOLO 格式确保了用于训练对象检测模型的精简和标准化数据处理。有关正确标注格式的更多信息，请访问YOLO 检测格式指南。

是的，只要标注采用 YOLO 检测格式，您就可以将 K-Fold 交叉验证用于任何自定义数据集。将数据集路径和类别标签替换为您自定义数据集的特定路径和标签。这种灵活性确保了任何对象检测项目都可以从使用 K-Fold 交叉验证的强大模型评估中受益。有关实际示例，请查看我们的Generating Feature Vectors部分。

**Examples:**

Example 1 (python):
```python
from pathlib import Path

dataset_path = Path("./Fruit-detection")  # replace with 'path/to/dataset' for your custom data
labels = sorted(dataset_path.rglob("*labels/*.txt"))  # all data in 'labels'
```

Example 2 (typescript):
```typescript
import yaml

yaml_file = "path/to/data.yaml"  # your data YAML with data directories and names dictionary
with open(yaml_file, encoding="utf8") as y:
    classes = yaml.safe_load(y)["names"]
cls_idx = sorted(classes.keys())
```

Example 3 (typescript):
```typescript
import pandas as pd

index = [label.stem for label in labels]  # uses base filename as ID (no extension)
labels_df = pd.DataFrame([], columns=cls_idx, index=index)
```

Example 4 (typescript):
```typescript
from collections import Counter

for label in labels:
    lbl_counter = Counter()

    with open(label) as lf:
        lines = lf.readlines()

    for line in lines:
        # classes for YOLO label uses integer at first position of each line
        lbl_counter[int(line.split(" ", 1)[0])] += 1

    labels_df.loc[label.stem] = lbl_counter

labels_df = labels_df.fillna(0.0)  # replace `nan` values with `0.0`
```

---

## YOLOv7 对比 EfficientDet：实时目标detect架构的技术比较

**URL:** https://docs.ultralytics.com/zh/compare/yolov7-vs-efficientdet/

**Contents:**
- YOLOv7 对比 EfficientDet：实时目标detect架构的技术比较
- 架构设计与理念
  - EfficientDet：复合缩放与BiFPN
  - YOLOv7：E-ELAN 与模型重参数化
- 性能分析：指标与基准
  - 主要内容
- 训练方法与优化
- Ultralytics 优势
  - 易用性与生态系统
  - 多功能性与性能平衡

对象检测仍然是计算机视觉的基石，推动了从自动驾驶到医学成像等领域的创新。选择正确的架构对于平衡准确性、速度和计算资源至关重要。本分析深入探讨了YOLOv7 和 EfficientDet，这两个对实时检测领域产生深远影响的模型。

这两种架构的根本区别在于它们的优化目标。由 Google Brain 团队开发的EfficientDet优先考虑参数效率和浮点运算 (FLOPs)。它利用可扩展架构，允许用户线性地权衡资源以获得准确性。相比之下，由 YOLOv4 作者（Chien-Yao Wang 等人）创建的YOLOv7则专注于在 GPU 硬件上最大化推理速度，同时保持最先进的准确性。

EfficientDet 基于EfficientNet 骨干网络，该网络利用复合缩放方法统一缩放网络分辨率、深度和宽度。EfficientDet 的一个关键创新是双向特征金字塔网络 (BiFPN)。与传统 FPN 不同，BiFPN 通过引入可学习的权重来学习不同输入特征的重要性，从而实现轻松快速的多尺度特征融合。这种设计使得 EfficientDet 对于内存和 FLOPs 严格受限的边缘计算应用非常有效。

了解更多关于 EfficientDet 的信息

YOLOv7 引入了扩展高效层聚合网络（E-ELAN）。这种架构通过控制最短和最长的梯度路径，在不破坏原始梯度路径的情况下提高了网络的学习能力。此外，YOLOv7 采用了模型重参数化技术，该技术将复杂的训练结构简化为流线型的推理结构。这使得模型在训练期间具有鲁棒性，但在 GPU 上部署时速度极快。

在比较性能时，选择通常取决于部署硬件。EfficientDet 在低功耗环境 (CPU) 中表现出色，而 YOLOv7 则专为高吞吐量 GPU 推理而设计。

如果您的部署目标是通用CPU或移动处理器，EfficientDet模型（特别是d0-d2）较低的FLOPs通常能带来更好的电池续航和热管理。对于边缘GPU（如NVIDIA Jetson）或云推理服务器，YOLOv7能为实时视频分析提供显著更高的帧率。

YOLOv7YOLOv7 采用了一种 "无用包 "方法，在不影响推理速度的情况下，采用了一些增加训练成本但提高准确性的方法。主要技术包括

EfficientDet严重依赖AutoML来寻找最佳的骨干网络和特征网络架构。其训练通常包括：

尽管YOLOv7和EfficientDet都很强大，但计算机视觉领域发展迅速。Ultralytics生态系统提供了诸如YOLO11等现代替代方案，它们融合了先前架构的最佳特性，同时提升了开发者体验。

研究型代码库（如原始 EfficientDet codebase）的主要挑战之一是集成复杂性。Ultralytics 通过统一的 python 包解决了这个问题。开发者只需几行代码即可训练、验证和部署模型，并获得全面的文档和活跃的社区支持。

Ultralytics 模型不限于边界框。它们原生支持实例分割、姿势估计、分类和旋转目标检测 (OBB)。在性能方面，现代 YOLO 版本（如 YOLOv8 和 YOLO11）通常比 EfficientDet 实现更高的每参数准确性，并且比 YOLOv7 具有更快的推理速度，为实际部署达到了理想的平衡。

Ultralytics YOLO 模型以其内存效率而闻名。与基于 Transformer 的检测器或旧的可扩展架构相比，它们在训练期间通常需要更少的 CUDA 内存。这使得研究人员能够在消费级硬件上训练最先进的模型。此外，高质量的预训练权重可立即下载，简化了迁移学习过程。

EfficientDet 仍然是 嵌入式系统的有力候选，尤其是在 GPU 加速不可用的情况下。

两种架构都代表了计算机视觉领域的重大里程碑。EfficientDet 展示了复合缩放对参数效率的强大作用，而YOLOv7 则突破了 GPU 延迟优化所能达到的极限。

然而，对于寻求最现代化、可维护且多功能解决方案的开发者而言，Ultralytics YOLO11模型系列是推荐的选择。它提供了卓越的精度-速度权衡、更简单的工作流程和强大的生态系统，简化了从数据集整理到部署的整个过程。

如果您有兴趣比较其他目标 detect 架构，请考虑这些资源：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load the latest YOLO11 model
model = YOLO("yolo11n.pt")

# Train on a custom dataset with a single command
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference with high speed
predictions = model("https://ultralytics.com/images/bus.jpg")
```

---

## DAMO-YOLO 与 EfficientDet 的技术对比

**URL:** https://docs.ultralytics.com/zh/compare/damo-yolo-vs-efficientdet/

**Contents:**
- DAMO-YOLO 与 EfficientDet 的技术对比
- 模型概述
  - DAMO-YOLO
  - EfficientDet
- 性能分析：速度、准确性和效率
  - 主要内容
- 架构深度解析
  - EfficientDet：复合缩放的力量
  - DAMO-YOLO：速度导向创新
- 应用案例与应用

在快速发展的计算机视觉领域，选择合适的物体 detect 架构对于应用成功至关重要。这项全面分析对比了来自阿里巴巴的高性能模型 DAMO-YOLO 与来自 Google 的可扩展且高效架构 EfficientDet。这两个模型都为该领域带来了重大创新，解决了速度、准确性和计算成本之间的永恒权衡。

在深入探讨性能指标之前，了解每个模型的渊源和架构理念至关重要。

由阿里巴巴集团开发的DAMO-YOLO（蒸馏增强型神经架构搜索YOLO）致力于在不牺牲准确性的前提下最大化推理速度。它引入了神经架构搜索（NAS）用于骨干网络、高效的RepGFPN（重参数化广义特征金字塔网络）以及名为ZeroHead的轻量级检测头等技术。

EfficientDet 由 Google Brain 团队创建，通过提出复合缩放方法，彻底改变了目标检测领域。这种方法统一缩放了主干网络、特征网络和预测网络的分辨率、深度和宽度。它采用了 BiFPN（双向特征金字塔网络），实现了简便快速的特征融合。

了解更多关于 EfficientDet 的信息

以下图表和表格提供了EfficientDet和DAMO-YOLO模型在COCO数据集上的定量比较。这些基准测试突出了每种架构独特的优化目标。

从数据中，我们可以观察到每个模型系列都有其独特的优势：

DAMO-YOLO的速度优势源于其使用神经架构搜索（NAS）对硬件延迟进行的特定优化，而EfficientDet则优化理论FLOPs，这并不总是与实际延迟线性相关。

EfficientDet 基于EfficientNet 骨干网络，该网络利用移动倒置残差瓶颈卷积 (MBConv)。其标志性特征是BiFPN，一个加权双向特征金字塔网络。与传统 FPN 仅自顶向下汇总特征不同，BiFPN 允许信息自顶向下和自底向上双向流动，并对每个特征层使用可学习的权重。这使得网络能够理解不同输入特征的重要性。

该模型使用复合系数 phi 进行缩放，该系数统一增加了网络宽度、深度和分辨率，因此更大的模型（例如 d7）在准确性和效率之间保持平衡。

DAMO-YOLO 采取了不同的方法，专注于实时延迟。它采用MAE-NAS（自动化架构搜索方法）在特定延迟约束下寻找最优骨干网络结构。

架构差异决定了每种模型在实际场景中的优势所在。

尽管DAMO-YOLO和EfficientDet是有能力的模型，但Ultralytics生态系统为现代AI开发提供了更全面的解决方案。诸如最先进的YOLO11和多功能的YOLOv8等模型在可用性、性能和功能集方面提供了显著优势。

易用性： 秉持“开箱即用”的理念，Ultralytics 提供了简单的 python API 和强大的 命令行界面 (CLI)。开发者可以在几分钟内从安装到训练。

良好维护的生态系统：与许多发布后被放弃的研究模型不同，Ultralytics 维护着一个活跃的仓库，提供频繁的更新、错误修复以及通过 GitHub issues 和讨论提供的社区支持。

DAMO-YOLO和EfficientDet都代表着计算机视觉历史上的重要里程碑。EfficientDet展示了原则性扩展和高效特征融合的力量，而DAMO-YOLO则突破了延迟感知架构搜索的界限。

然而，对于寻求兼具高性能和卓越开发者体验的生产就绪型解决方案的开发者而言，Ultralytics YOLO11是推荐的选择。它集成到强大的生态系统、对多种计算机视觉任务的支持以及持续改进，使其成为将视觉数据转化为可操作洞察的最实用工具。

为了进一步协助您的模型选择过程，请查阅 Ultralytics 文档中这些相关的比较：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLO11 model
model = YOLO("yolo11n.pt")

# Run inference on an image
results = model("path/to/image.jpg")
```

---

## RTDETRv2 与 YOLOv8：技术比较 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/compare/rtdetr-vs-yolov8/

**Contents:**
- RTDETRv2 与 YOLOv8：技术比较
- 性能指标：速度、准确性与效率
  - 结果分析
- RTDETRv2：基于 Transformer 的实时检测
  - 架构
  - 优势与劣势
- Ultralytics YOLOv8：速度、多功能性与生态系统
  - 架构
  - 为开发者带来的主要优势
- 深入探讨：架构与用例

在快速发展的计算机视觉领域，选择合适的物体 detect 模型对于项目成功至关重要。两种截然不同的架构理念目前主导着该领域：以 RTDETRv2 为代表的基于 Transformer 的方法和以 Ultralytics YOLOv8 为例的高度优化的卷积神经网络 (CNN) 设计。

尽管 RTDETRv2 利用视觉 Transformer 突破了精度极限，但 YOLOv8 则在速度、精度和部署便捷性之间取得了更好的平衡。本比较将探讨技术规格、架构差异和实际性能指标，以帮助开发者和研究人员为其应用选择最佳解决方案。

性能格局突显了明显的权衡。RTDETRv2 通过复杂的注意力机制专注于最大化平均精度 (mAP)，而 YOLOv8 则优先考虑实时推理速度和高 accuracy 的多功能平衡，适用于边缘和云部署。

如果您的部署目标涉及标准CPU（如Intel处理器）或嵌入式设备（如Raspberry Pi），YOLOv8的基于CNN的架构在延迟方面具有显著优势，优于RTDETRv2的Transformer密集型操作。

RTDETRv2 (Real-Time Detection Transformer v2) 代表了将视觉 Transformer (ViT) 应用于目标检测的持续演进。由百度研究人员开发，它旨在解决传统上与基于 DETR 的模型相关的延迟问题，同时保留其理解全局上下文的能力。

作者： 吕文宇、赵一安、常钦尧、黄奎、王冠中、刘毅机构：百度日期： 2024-07-24 (v2 发布)预印本：https://arxiv.org/abs/2304.08069GitHub：https://github.com/lyuwenyu/RT-DETR/tree/main/rtdetrv2_pytorch

RTDETRv2 采用混合架构，结合了骨干网络（通常是 ResNet 等 CNN）和高效的 Transformer 编码器-解码器。一个关键特性是尺度内交互和跨尺度融合的解耦，这有助于模型捕获图像中的长距离依赖关系。这使得模型能够同时“关注”场景的不同部分，从而可能提高在杂乱环境中的性能。

RTDETRv2 的主要优势在于其在全局上下文至关重要的复杂数据集上的高准确性。通过摒弃锚框而采用物体查询，它通过消除对非极大值抑制 (NMS) 的需求来简化后处理流水线。

Ultralytics YOLOv8 是一种最先进的、无锚框的目标检测模型，为行业中的多功能性和易用性树立了标准。它建立在 YOLO 系列的传统之上，引入了架构改进，在保持 YOLO 成名的实时速度的同时，提高了性能。

作者: Glenn Jocher, Ayush Chaurasia, and Jing Qiu机构:Ultralytics日期: 2023-01-10GitHub:https://github.com/ultralytics/ultralytics文档:https://docs.ultralytics.com/models/yolov8/

YOLOv8 采用 CSP (Cross Stage Partial) Darknet 骨干网络和 PANet (Path Aggregation Network) 颈部，最终形成解耦的检测头。这种架构是无锚点的，这意味着它直接预测物体中心，从而简化了设计并提高了泛化能力。该模型针对张量处理单元和 GPU 进行了高度优化，确保了最大吞吐量。

这两种模型之间的选择通常取决于应用程序环境的具体要求。

YOLOv8 依赖于卷积神经网络 (CNN)，它擅长高效处理局部特征和空间层次结构。这使得它们本质上更快，内存占用更少。RTDETRv2 对Transformer的依赖使其能够有效地建模全局关系，但引入了与图像尺寸相关的二次复杂度，导致更高的延迟和内存使用，尤其是在高分辨率下。

尽管 RTDETRv2 在将 Transformer 应用于 detect 方面取得了有趣的学术进展，但 YOLOv8 仍然是大多数实际应用的更优选择。它在 速度、精度和效率 方面的平衡是无与伦比的。此外，在单个用户友好的库中执行多个计算机视觉任务的能力，使其成为现代 AI 开发的多功能工具。

对于寻求性能和功能集最新进展的开发者而言，关注像 YOLO11 这样的新迭代，相较于 YOLOv8 和 RTDETRv2，能带来更高的效率和准确性提升。

将 YOLOv8 集成到您的工作流中非常简单。以下是一个 python 示例，演示如何加载预训练模型、运行推理并将其导出以进行部署。

要更广泛地了解目标 detect 架构，可以考虑查阅这些相关比较：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv8 model
model = YOLO("yolov8n.pt")

# Train the model on the COCO8 dataset
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference on a local image
# Ensure the image path is correct or use a URL
results = model("path/to/image.jpg")

# Export the model to ONNX format for deployment
success = model.export(format="onnx")
```

---

## 数据集概览 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/datasets/

**Contents:**
- 数据集概览
- 目标检测
- 实例分割
- 姿势估计
- 分类
- 旋转框检测 (OBB)
- 多目标跟踪
- 贡献新的数据集
  - 贡献新数据集的步骤
  - 优化和压缩数据集的示例代码

Ultralytics 支持各种数据集，以促进计算机视觉任务，例如检测、实例分割、姿势估计、分类和多目标跟踪。以下是主要的 Ultralytics 数据集列表，后跟每个计算机视觉任务和相应数据集的摘要。

观看： Ultralytics 数据集概览

边界框目标检测是一种计算机视觉技术，它通过在每个对象周围绘制一个边界框来检测和定位图像中的对象。

实例分割是一种计算机视觉技术，涉及在像素级别识别和定位图像中的对象。与仅对每个像素进行分类的语义分割不同，实例分割区分同一类的不同实例。

姿势估计是一种用于确定对象相对于相机或世界坐标系的姿势的技术。这涉及识别对象（特别是人或动物）上的关键点或关节。

图像分类是一种计算机视觉任务，涉及根据图像的视觉内容将图像分类为一个或多个预定义的类别。

旋转框检测 (OBB) 是计算机视觉中的一种方法，用于检测图像中倾斜角度的物体，它使用旋转的边界框，通常应用于航空和卫星图像。与传统的边界框不同，OBB 可以更好地拟合各种方向的物体。

多目标跟踪是一种计算机视觉技术，涉及在视频序列中检测和跟踪多个物体随时间的变化。这项任务通过在帧之间保持物体身份的一致性来扩展物体检测。

贡献新的数据集涉及多个步骤，以确保它与现有基础设施良好地结合。以下是必要的步骤：

观看： 如何为 Ultralytics 数据集贡献

组织数据集：将您的数据集整理成正确的文件夹结构。您应该有 images/ 和 labels/ 顶级目录，以及每个目录中的 train/ 和 val/ 子目录。

创建一个 data.yaml 文件：在数据集的根目录中，创建一个 data.yaml 描述数据集、类别和其他必要信息的文件。

通过遵循这些步骤，您可以贡献一个新的数据集，使其与 Ultralytics 的现有结构良好地集成。

Ultralytics 支持各种用于对象检测的数据集，包括：

这些数据集有助于训练强大的 Ultralytics YOLO 模型，以用于各种对象检测应用。

Ultralytics HUB为数据集管理和分析提供了强大的功能，包括：

该平台简化了从数据集管理到模型训练的过渡，从而提高了整个过程的效率。了解更多关于 Ultralytics HUB 数据集的信息。

Ultralytics YOLO 模型为 计算机视觉 任务提供了几个独特的功能：

在 Ultralytics Models 页面上了解更多关于 YOLO 模型的信息。

要使用 Ultralytics 工具优化和压缩数据集，请按照以下示例代码操作：

此过程有助于减小数据集大小，从而实现更高效的存储和更快的下载速度。请访问优化和压缩数据集，了解更多信息。

**Examples:**

Example 1 (unknown):
```unknown
dataset/
├── images/
│   ├── train/
│   └── val/
└── labels/
    ├── train/
    └── val/
```

Example 2 (python):
```python
from pathlib import Path

from ultralytics.data.utils import compress_one_image
from ultralytics.utils.downloads import zip_directory

# Define dataset directory
path = Path("path/to/dataset")

# Optimize images in dataset (optional)
for f in path.rglob("*.jpg"):
    compress_one_image(f)

# Zip dataset into 'path/to/dataset.zip'
zip_directory(path)
```

Example 3 (python):
```python
from pathlib import Path

from ultralytics.data.utils import compress_one_image
from ultralytics.utils.downloads import zip_directory

# Define dataset directory
path = Path("path/to/dataset")

# Optimize images in dataset (optional)
for f in path.rglob("*.jpg"):
    compress_one_image(f)

# Zip dataset into 'path/to/dataset.zip'
zip_directory(path)
```

---

## YOLOv7 对比 YOLOX：详细技术比较

**URL:** https://docs.ultralytics.com/zh/compare/yolov7-vs-yolox/

**Contents:**
- YOLOv7 对比 YOLOX：详细技术比较
- 性能指标：速度与准确性
  - 结果分析
- YOLOv7：优化的“免费赠品”
  - 架构亮点
- YOLOX：无锚框演进
  - 架构亮点
- 为什么Ultralytics模型是首选
  - 1. 统一的生态系统和易用性
  - 2. 优越的性能平衡

在快速发展的计算机视觉领域，YOLO（You Only Look Once）模型家族持续为实时目标检测设定标准。历史上的两个重要里程碑是YOLOv7和YOLOX。尽管这两个模型都旨在平衡速度和准确性，但它们在架构理念上存在显著差异——特别是在基于锚点与无锚点方法论方面。

本指南提供了深入的技术比较，旨在帮助研究人员和工程师为其特定的计算机视觉应用选择合适的工具。我们将分析它们的架构、基准性能，并探讨为什么 Ultralytics YOLO11 等现代替代方案通常能提供卓越的开发体验。

评估目标检测器时，推理延迟与平均精度 (mAP) 之间的权衡至关重要。下表直接比较了 YOLOv7 和 YOLOX 变体在COCO 数据集上的表现。

数据突出显示了不同模型系列在不同部署约束下的独特优势。YOLOv7 在高性能领域展现出卓越的效率。例如，YOLOv7l 仅用 36.9M 参数就达到了 51.4% mAP，优于 YOLOXx (51.1% mAP，99.1M 参数)，同时使用的计算资源显著更少。这使得 YOLOv7 成为 GPU 效率 至关重要但内存受限场景的有力候选。

相反，YOLOX 在轻量级类别中表现出色。YOLOX-Nano 模型（0.91M 参数）为超低功耗边缘设备提供了可行的解决方案，即使是最小的标准 YOLO 模型也可能过于笨重。其可扩展的深度-宽度乘数允许在各种硬件配置文件上进行精细调优。

YOLOv7于2022年7月发布，引入了几项架构创新，旨在优化训练过程，同时不增加推理成本。

YOLOv7专注于“可训练的免费包”——这些优化方法在训练期间提高精度，但在推理时被移除或合并。主要特点包括：

YOLOv7 利用计划重参数化，其中不同的训练模块在数学上合并为一个卷积层用于推理。这显著降低了推理延迟，而不牺牲训练期间获得的特征学习能力。

YOLOX于2021年发布，标志着YOLO范式的一次转变，它从锚框转向了无锚框机制，类似于语义分割方法。

YOLOX 通过消除手动调整锚框的需求，简化了检测流程，这在 YOLOv4 和 YOLOv5 等早期版本中是一个常见的痛点。

虽然YOLOv7和YOLOX在架构上有所不同，但它们在可用性和生态系统支持方面都被现代Ultralytics YOLO模型超越。对于寻求强大、面向未来的解决方案的开发者来说，转向YOLO11具有明显的优势。

YOLOv7和YOLOX通常需要克隆特定的GitHub仓库、管理复杂的依赖项要求以及使用不同的数据格式。相比之下，Ultralytics提供了一个可pip安装的软件包，可统一所有任务。

如基准测试所示，现代Ultralytics模型在速度和准确性之间取得了更好的平衡。YOLO11采用优化的无锚点架构，它借鉴了YOLOX（无锚点设计）和YOLOv7（梯度路径优化）的进步。这使得模型不仅在CPU推理上更快，而且在训练期间需要更少的CUDA内存，使它们可以在更广泛的硬件上使用。

YOLOv7和YOLOX主要设计用于目标检测。Ultralytics模型将此功能原生扩展到一系列计算机视觉任务，而无需更改API：

使用旧框架将模型从研究阶段推向生产环境极具挑战性。Ultralytics 生态系统内置了针对 ONNX、TensorRT、CoreML 和 OpenVINO 的导出模式，简化了模型部署。此外，与Ultralytics HUB的集成支持基于网络的 datasets 管理、远程训练以及一键部署到边缘设备。

YOLOv7 和 YOLOX 都对计算机视觉领域做出了重大贡献。YOLOv7 优化了架构，以在 GPU 设备上实现峰值性能，最大限度地提高了“免费赠品包”方法的效率。YOLOX 成功展示了无锚点 detect 的可行性，简化了管道并提高了泛化能力。

然而，对于现代开发工作流，Ultralytics YOLO11 脱颖而出，成为卓越之选。它结合了其前身的架构优势，拥有无与伦比的Python API、更低的内存需求，并支持全面的视觉任务。无论您是部署到边缘设备还是云服务器，Ultralytics 生态系统活跃的社区和详尽的文档都能确保更顺畅的生产路径。

如果您对进一步的技术比较感兴趣，请查阅这些资源：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a model (YOLO11n recommended for speed)
model = YOLO("yolo11n.pt")

# Train on a custom dataset with a single line
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference on an image
results = model("path/to/image.jpg")
```

---

## YOLOv6-3.0 与 YOLOv5：目标检测技术比较

**URL:** https://docs.ultralytics.com/zh/compare/yolov6-vs-yolov5/

**Contents:**
- YOLOv6-3.0 与 YOLOv5：目标检测技术比较
- 性能指标分析
- 美团 YOLOv6-3.0：工业级精度
  - 架构和主要特性
  - 优势与劣势
- Ultralytics YOLOv5：生态系统标准
  - 架构和主要特性
  - 为什么选择YOLOv5？
- 详细比较
  - 架构与设计理念

为您的计算机视觉项目选择合适的架构是一个关键决策，它会影响性能、部署便捷性和长期维护。在实时目标检测领域，美团的YOLOv6-3.0和Ultralytics的YOLOv5是两个杰出的竞争者。本指南提供了详细的技术比较，旨在帮助开发人员和研究人员选择最符合其特定需求的模型，无论是优先考虑原始 GPU 吞吐量还是多功能、易用的生态系统。

下表直接比较了COCO数据集上的性能指标。尽管YOLOv6-3.0在GPU设备上将峰值准确性推向了极限，Ultralytics YOLOv5以其卓越的效率（尤其是在CPU上）和显著更低的模型复杂度（参数和FLOPs）而闻名，特别是其轻量级变体。

分析： 数据突出表明，YOLOv5n (Nano) 模型是资源受限环境中的佼佼者，它拥有最低的参数计数 (2.6M) 和 FLOP (7.7B)，这转化为卓越的 CPU 推理速度。这使其非常适合内存和功率稀缺的 边缘 AI 应用。相反，YOLOv6-3.0 以增加模型大小为代价，瞄准更高的 mAPval，使其成为具有专用 GPU 硬件的工业设置的有力候选者。

作者: Chuyi Li, Lulu Li, Yifei Geng, Hongliang Jiang, Meng Cheng, Bo Zhang, Zaidan Ke, Xiaoming Xu, and Xiangxiang Chu组织: Meituan日期: 2023-01-13Arxiv: https://arxiv.org/abs/2301.05586GitHub: https://github.com/meituan/YOLOv6文档: https://docs.ultralytics.com/models/yolov6/

由美团开发的YOLOv6-3.0是一个专为工业应用量身定制的目标检测框架。它致力于在推理速度和准确性之间取得良好的权衡，特别针对GPU上的硬件感知性能进行了优化。

YOLOv6 融合了高效的骨干网络设计和可重参数化结构（RepVGG 风格），在推理时简化模型，同时在训练时保持复杂的特征提取能力。3.0 版本引入了自蒸馏和锚点辅助训练等技术，以进一步提升性能。

作者: Glenn Jocher组织: Ultralytics日期: 2020-06-26GitHub: https://github.com/ultralytics/yolov5文档: https://docs.ultralytics.com/models/yolov5/

Ultralytics YOLOv5是计算机视觉领域的一个传奇模型，以其以用户为中心的设计、可靠性以及完善的生态系统而闻名。因为它在速度、精度和易用性之间取得了平衡，它仍然是全球部署最广泛的模型之一。

YOLOv5 利用 CSPDarknet 主干网络结合 PANet 颈部网络，用于鲁棒的特征融合。它采用基于锚框的检测机制，该机制在不同数据集上均表现出高度稳定性。该架构高度模块化，提供五种尺寸 (n, s, m, l, x)，以适应从嵌入式设备到云服务器的各种需求。

使用YOLOv5的最大优势之一是Ultralytics生态系统。与Ultralytics HUB等工具的集成实现了无代码模型训练和预览，而通过Comet和MLflow进行实验跟踪的内置支持则简化了MLOps工作流程。

YOLOv6-3.0 严重依赖硬件感知的神经架构搜索和重参数化，以最大限度地提高特定 GPU 架构（如 Tesla T4）上的吞吐量。相比之下，YOLOv5 专注于通用设计，可在 CPU、GPU 和 NPU 上可靠运行。与一些无锚框方法相比，YOLOv5 的基于锚框的检测器通常更容易针对包含小对象的自定义数据集进行调整。

Ultralytics 模型被设计为“开箱即训”。借助 YOLOv5，AutoAnchor 等功能会自动根据您的数据集标签调整锚框，智能超参数演进有助于找到最佳训练设置。YOLOv6 则需要更传统研究仓库特有的手动设置，这可能给新用户带来更高的学习曲线。

YOLOv5 的简洁性最好地体现在它能够使用 PyTorch Hub 仅用几行代码运行推理。这消除了复杂的安装步骤，并使开发人员能够立即测试模型。

这种易于访问性是Ultralytics理念的标志，使计算机视觉从业者能够专注于解决问题，而不是调试环境问题。

两种架构在现代视觉领域扮演着重要角色。美团 YOLOv6-3.0 对于严格专注于在 GPU 硬件上最大化检测准确性的用户来说，提供了一个引人注目的选择。

然而，Ultralytics YOLOv5因其无与伦比的多功能性、训练效率和强大的生态系统，仍然是大多数开发者的卓越选择。它能够轻松部署到边缘设备，并支持segmentation和分类，使YOLOv5成为解决现实世界AI挑战的全面解决方案。

对于那些寻求最先进性能方面绝对最新成果的用户，我们推荐探索 Ultralytics YOLO11。YOLO11 在 YOLOv5 的传承基础上，实现了更高的精度、速度和丰富的功能，代表着视觉 AI 的未来。其他专业模型，如 RT-DETR，也适用于基于 Transformer 的应用。

在Ultralytics模型文档中探索所有工具和模型。

**Examples:**

Example 1 (python):
```python
import torch

# Load the YOLOv5s model from the official Ultralytics Hub
model = torch.hub.load("ultralytics/yolov5", "yolov5s", pretrained=True)

# Define an image URL (or local path)
img = "https://ultralytics.com/images/zidane.jpg"

# Perform inference
results = model(img)

# Display results
results.show()

# Print detailed results regarding detected objects
results.print()
```

---

## 使用 Ultralytics YOLO11 进行对象计数 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/guides/object-counting/

**Contents:**
- 使用 Ultralytics YOLO11 进行目标计数
- 什么是目标计数？
- 对象计数的优势
- 实际应用
  - ObjectCounter 参数
- 常见问题
  - 如何使用 Ultralytics YOLO11 在视频中计数目标？
  - 使用 Ultralytics YOLO11 进行目标计数有哪些优势？
  - 如何使用 Ultralytics YOLO11 计数特定类别的目标？
  - 对于实时应用，为什么我应该选择 YOLO11 而不是其他对象检测模型？

使用 Ultralytics YOLO11 进行目标计数包括准确识别和计数视频及摄像头流中的特定目标。YOLO11 在实时应用中表现出色，凭借其最先进的算法和深度学习能力，为人群分析和监控等各种场景提供高效而精确的目标计数。

观看： 如何使用 Ultralytics YOLO11 执行实时对象计数 🍏

使用 Ultralytics YOLO 进行对象计数

字段 region 参数接受两个点（用于一条线）或一个包含三个或更多点的多边形。按照它们应该连接的顺序定义坐标，以便计数器准确知道入口和出口发生的位置。

这是一个包含以下内容的表格 ObjectCounter 参数：

字段 ObjectCounter 解决方案允许使用多个 track 参数：

要使用 Ultralytics YOLO11 计数视频中的对象，您可以按照以下步骤操作：

如需更高级的配置和选项，请查看 RegionCounter 解决方案，以同时计算多个区域中的对象。

使用 Ultralytics YOLO11 进行对象计数具有以下优势：

有关实施示例和实际应用，请浏览 TrackZone 解决方案，了解如何在特定区域中跟踪对象。

要使用 Ultralytics YOLO11 计数特定类别的对象，您需要在跟踪阶段指定您感兴趣的类别。以下是一个 Python 示例：

在这个例子中， classes_to_count=[0, 2] 表示它计算类别的对象 0 和 2 （例如，COCO 数据集中的人和汽车）。您可以在以下位置找到有关类索引的更多信息 COCO 数据集文档.

与其他对象检测模型（如 Faster R-CNN、SSD 和以前的 YOLO 版本）相比，Ultralytics YOLO11 具有以下几个独特的优势：

查看 Ultralytics YOLO11 文档，以更深入地了解其功能和性能比较。

是的，由于 Ultralytics YOLO11 具有实时检测能力、可扩展性和集成灵活性，因此非常适合用于人群分析和交通管理等高级应用。其高级功能允许在动态环境中进行高精度对象跟踪、计数和分类。用例包括：

对于更专业的应用，请浏览 Ultralytics 解决方案，获取一套为应对现实世界计算机视觉挑战而设计的综合工具。

**Examples:**

Example 1 (markdown):
```markdown
# Run a counting example
yolo solutions count show=True

# Pass a source video
yolo solutions count source="path/to/video.mp4"

# Pass region coordinates
yolo solutions count region="[(20, 400), (1080, 400), (1080, 360), (20, 360)]"
```

Example 2 (python):
```python
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "Error reading video file"

# region_points = [(20, 400), (1080, 400)]                                      # line counting
region_points = [(20, 400), (1080, 400), (1080, 360), (20, 360)]  # rectangular region
# region_points = [(20, 400), (1080, 400), (1080, 360), (20, 360), (20, 400)]   # polygon region

# Video writer
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))
video_writer = cv2.VideoWriter("object_counting_output.avi", cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))

# Initialize object counter object
counter = solutions.ObjectCounter(
    show=True,  # display the output
    region=region_points,  # pass region points
    model="yolo11n.pt",  # model="yolo11n-obb.pt" for object counting with OBB model.
    # classes=[0, 2],  # count specific classes, e.g., person and car with the COCO pretrained model.
    # tracker="botsort.yaml",  # choose trackers, e.g., "bytetrack.yaml"
)

# Process video
while cap.isOpened():
    success, im0 = cap.read()

    if not success:
        print("Video frame is empty or processing is complete.")
        break

    results = counter(im0)

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


def count_objects_in_region(video_path, output_video_path, model_path):
    """Count objects in a specific region within a video."""
    cap = cv2.VideoCapture(video_path)
    assert cap.isOpened(), "Error reading video file"
    w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))
    video_writer = cv2.VideoWriter(output_video_path, cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))

    region_points = [(20, 400), (1080, 400), (1080, 360), (20, 360)]
    counter = solutions.ObjectCounter(show=True, region=region_points, model=model_path)

    while cap.isOpened():
        success, im0 = cap.read()
        if not success:
            print("Video frame is empty or processing is complete.")
            break
        results = counter(im0)
        video_writer.write(results.plot_im)

    cap.release()
    video_writer.release()
    cv2.destroyAllWindows()


count_objects_in_region("path/to/video.mp4", "output_video.avi", "yolo11n.pt")
```

Example 4 (python):
```python
import cv2

from ultralytics import solutions


def count_specific_classes(video_path, output_video_path, model_path, classes_to_count):
    """Count specific classes of objects in a video."""
    cap = cv2.VideoCapture(video_path)
    assert cap.isOpened(), "Error reading video file"
    w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))
    video_writer = cv2.VideoWriter(output_video_path, cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))

    line_points = [(20, 400), (1080, 400)]
    counter = solutions.ObjectCounter(show=True, region=line_points, model=model_path, classes=classes_to_count)

    while cap.isOpened():
        success, im0 = cap.read()
        if not success:
            print("Video frame is empty or processing is complete.")
            break
        results = counter(im0)
        video_writer.write(results.plot_im)

    cap.release()
    video_writer.release()
    cv2.destroyAllWindows()


count_specific_classes("path/to/video.mp4", "output_specific_classes.avi", "yolo11n.pt", [0, 2])
```

---

## Roboflow 100 数据集 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/datasets/detect/roboflow-100/

**Contents:**
- Roboflow 100数据集
- 主要功能
- 数据集结构
- 基准测试
- 应用
- 用法
- 样本数据和注释
- 引用和致谢
- 常见问题
  - 什么是 Roboflow 100 数据集，为什么它对对象检测如此重要？

Roboflow 100 由 Intel 赞助，是一个突破性的 目标检测 基准数据集。它包括从 Roboflow Universe 上超过 90,000 个公共数据集中抽样的 100 个不同的数据集。此基准专门用于测试 计算机视觉 模型（如 Ultralytics YOLO 模型）对各种领域的适应性，包括医疗保健、航空图像和视频游戏。

Ultralytics 提供了两种许可选项，以适应不同的用例：

Roboflow 100 数据集分为七个类别，每个类别都包含数据集、图像和类的独特集合：

这种结构为目标检测模型提供了多样化和广泛的测试平台，反映了各种Ultralytics解决方案中广泛的实际应用场景。

数据集基准测试涉及使用标准化指标评估机器学习模型在特定数据集上的性能。常用指标包括准确率、平均精度均值 (mAP) 和F1-分数。您可以在我们的YOLO 性能指标指南中了解更多信息。

使用提供的脚本进行基准测试的结果将存储在 ultralytics-benchmarks/ 目录，特别是在 evaluation.txt.

以下脚本展示了如何使用 Roboflow 100 基准测试中的所有 100 个数据集，以编程方式对 Ultralytics YOLO 模型（例如 YOLO11n）进行基准测试，使用 RF100Benchmark 类。

Roboflow 100 对于与 计算机视觉 和 深度学习 相关的各种应用都非常宝贵。研究人员和工程师可以利用此基准来：

如需更多关于实际应用的灵感，请浏览我们的实践项目指南，或查看Ultralytics HUB，以简化模型训练和部署。

Roboflow 100 数据集，包括元数据和下载链接，可在官方网站上找到 Roboflow 100 GitHub 仓库。您可以直接从那里访问和使用该数据集，以满足您的基准测试需求。 Ultralytics RF100Benchmark 实用程序简化了下载和准备这些数据集以供 Ultralytics 模型使用的过程。

Roboflow 100 包含从不同角度和领域捕获的多样化图像数据集。以下是 RF100 基准测试中包含的带注释图像示例，展示了各种对象和场景。诸如 数据增强 等技术可以进一步增强训练期间的多样性。

Roboflow 100 基准测试中看到的多样性代表了相对于传统基准测试的一项重大进步，传统基准测试通常侧重于在有限的领域内优化单个指标。这种全面的方法有助于开发更强大、更通用的计算机视觉模型，这些模型能够在多种不同的场景中表现良好。

如果您在研究或开发工作中使用 Roboflow 100 数据集，请引用原始论文：

我们感谢 Roboflow 团队和所有贡献者为创建和维护 Roboflow 100 数据集所做的重大努力，使其成为计算机视觉社区的宝贵资源。

如果您有兴趣探索更多数据集以增强您的对象检测和机器学习项目，请随时访问我们全面的数据集集合，其中包括各种其他检测数据集。

Roboflow 100 数据集是目标检测模型的基准。它包含来自 Roboflow Universe 的 100 个不同的数据集，涵盖医疗保健、航空图像和视频游戏等领域。它的重要性在于提供了一种标准化的方法来测试模型在各种真实场景中的适应性和鲁棒性，从而超越了传统的、通常是领域受限的基准。

Roboflow 100 数据集跨越七个不同的领域，为目标检测模型提供了独特的挑战：

这种多样性使RF100成为评估计算机视觉模型泛化能力的绝佳资源。

当使用 Roboflow 100 数据集时，请引用原始论文以感谢创建者。以下是推荐的 BibTeX 引用：

为了进一步探索，请考虑访问我们的综合数据集集合或浏览其他与 Ultralytics 模型兼容的检测数据集。

**Examples:**

Example 1 (python):
```python
import os
import shutil
from pathlib import Path

from ultralytics.utils.benchmarks import RF100Benchmark

# Initialize RF100Benchmark and set API key
benchmark = RF100Benchmark()
benchmark.set_key(api_key="YOUR_ROBOFLOW_API_KEY")

# Parse dataset and define file paths
names, cfg_yamls = benchmark.parse_dataset()
val_log_file = Path("ultralytics-benchmarks") / "validation.txt"
eval_log_file = Path("ultralytics-benchmarks") / "evaluation.txt"

# Run benchmarks on each dataset in RF100
for ind, path in enumerate(cfg_yamls):
    path = Path(path)
    if path.exists():
        # Fix YAML file and run training
        benchmark.fix_yaml(str(path))
        os.system(f"yolo detect train data={path} model=yolo11s.pt epochs=1 batch=16")

        # Run validation and evaluate
        os.system(f"yolo detect val data={path} model=runs/detect/train/weights/best.pt > {val_log_file} 2>&1")
        benchmark.evaluate(str(path), str(val_log_file), str(eval_log_file), ind)

        # Remove the 'runs' directory
        runs_dir = Path.cwd() / "runs"
        shutil.rmtree(runs_dir)
    else:
        print("YAML file path does not exist")
        continue

print("RF100 Benchmarking completed!")
```

Example 2 (css):
```css
@misc{rf100benchmark,
    Author = {Floriana Ciaglia and Francesco Saverio Zuppichini and Paul Guerrie and Mark McQuade and Jacob Solawetz},
    Title = {Roboflow 100: A Rich, Multi-Domain Object Detection Benchmark},
    Year = {2022},
    Eprint = {arXiv:2211.13523},
    url = {https://arxiv.org/abs/2211.13523}
}
```

Example 3 (css):
```css
@misc{rf100benchmark,
    Author = {Floriana Ciaglia and Francesco Saverio Zuppichini and Paul Guerrie and Mark McQuade and Jacob Solawetz},
    Title = {Roboflow 100: A Rich, Multi-Domain Object Detection Benchmark},
    Year = {2022},
    Eprint = {arXiv:2211.13523},
    url = {https://arxiv.org/abs/2211.13523}
}
```

---

## 用于 YOLO 的 OpenVINO 推理优化 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/guides/optimizing-openvino-latency-vs-throughput-modes/

**Contents:**
- 用于 YOLO 的 OpenVINO 推理优化
- 简介
- 针对延迟进行优化
  - 延迟优化的关键策略：
  - 管理首次推理延迟：
- 优化吞吐量
  - 吞吐量优化方法：
  - 面向吞吐量应用的设计：
  - 多设备执行：
- 真实世界的性能提升

在部署深度学习模型时，特别是用于目标检测的模型（如 Ultralytics YOLO 模型），实现最佳性能至关重要。本指南深入探讨了如何利用 Intel 的 OpenVINO 工具包来优化推理，重点关注延迟和吞吐量。无论您是从事消费级应用还是大规模部署，理解和应用这些优化策略都将确保您的模型在各种设备上高效运行。

对于需要单个模型根据单个输入立即做出响应的应用（在消费场景中很常见），延迟优化至关重要。目标是最大限度地减少输入和推理结果之间的延迟。但是，实现低延迟需要仔细考虑，尤其是在运行并发推理或管理多个模型时。

吞吐量优化对于同时服务于大量推理请求的场景至关重要，可以在不显著牺牲单个请求性能的情况下，最大限度地提高资源利用率。

OpenVINO 性能提示： 一种高级的、面向未来的方法，可以使用性能提示来增强各种设备的吞吐量。

显式批处理和流： 一种更精细的方法，涉及显式批处理和使用流进行高级性能调优。

OpenVINO 的多设备模式简化了吞吐量的扩展，它可以在无需应用程序级别的设备管理的情况下，自动平衡跨设备的推理请求。

使用 Ultralytics YOLO 模型实施 OpenVINO 优化可以显著提高性能。正如基准测试中所示，用户可以在 Intel CPU 上体验到高达 3 倍的推理速度提升，并且在 Intel 的硬件范围内（包括集成 GPU、独立 GPU 和 VPU）可以实现更大的加速。

例如，在 Intel Xeon CPUs 上运行 YOLOv8 模型时，OpenVINO 优化版本在每张图片的推理时间上始终优于 PyTorch 版本，且不会影响准确率。

要导出和优化您的 Ultralytics YOLO 模型以用于 OpenVINO，您可以使用 export 功能：

通过 OpenVINO 优化 Ultralytics YOLO 模型以实现低延迟和高吞吐量，可以显著提高应用程序的性能。通过仔细应用本指南中概述的策略，开发人员可以确保其模型高效运行，满足各种部署场景的需求。请记住，针对延迟或吞吐量进行优化之间的选择取决于您的具体应用程序需求和部署环境的特性。

有关更详细的技术信息和最新更新，请参阅 OpenVINO 文档 和 Ultralytics YOLO 仓库。这些资源提供了深入的指南、教程和社区支持，以帮助您充分利用深度学习模型。

确保您的模型达到最佳性能不仅仅是调整配置；而是要了解您的应用程序的需求并做出明智的决策。无论您是为实时响应进行优化，还是为大规模处理最大化吞吐量，Ultralytics YOLO 模型和 OpenVINO 的结合都为开发人员提供了一个强大的工具包，可以部署高性能 AI 解决方案。

优化 Ultralytics YOLO 模型以实现低延迟涉及几个关键策略：

有关优化延迟的更多实用技巧，请查看我们指南的延迟优化部分。

OpenVINO 通过最大限度地利用设备资源而不牺牲性能来提高 Ultralytics YOLO 模型的吞吐量。主要优势包括：

在我们的详细指南的吞吐量优化部分中了解更多关于吞吐量优化的信息。

有关管理首次推理延迟的详细策略，请参阅管理首次推理延迟部分。

平衡延迟和吞吐量优化需要了解您的应用需求：

使用 OpenVINO 的高级性能提示和多设备模式可以帮助达到适当的平衡。根据您的具体要求选择合适的OpenVINO 性能提示。

是的，Ultralytics YOLO 模型具有高度的通用性，可以与各种 AI 框架集成。选项包括：

在 Ultralytics 集成页面上探索更多集成。

**Examples:**

Example 1 (typescript):
```typescript
import openvino.properties.hint as hints

config = {hints.performance_mode: hints.PerformanceMode.THROUGHPUT}
compiled_model = core.compile_model(model, "GPU", config)
```

Example 2 (python):
```python
from ultralytics import YOLO

# Load a model
model = YOLO("yolov8n.pt")

# Export the model to OpenVINO format
model.export(format="openvino", half=True)  # Export with FP16 precision
```

Example 3 (markdown):
```markdown
# Load the OpenVINO model
ov_model = YOLO("yolov8n_openvino_model/")

# Run inference with performance hints for latency
results = ov_model("path/to/image.jpg", verbose=True)
```

Example 4 (typescript):
```typescript
import openvino.properties.hint as hints

config = {hints.performance_mode: hints.PerformanceMode.THROUGHPUT}
compiled_model = core.compile_model(model, "GPU", config)
```

---

## 交互式目标检测：Gradio & Ultralytics YOLO11 🚀

**URL:** https://docs.ultralytics.com/zh/integrations/gradio/

**Contents:**
- 交互式目标检测：Gradio & Ultralytics YOLO11 🚀
- 交互式对象检测简介
- 为什么要将 Gradio 用于目标检测？
- 如何安装 Gradio
- 如何使用界面
- 用例示例
- 使用示例
- 参数说明
  - Gradio 界面组件
- 常见问题

此Gradio界面提供了一种简单且交互式的方式，使用Ultralytics YOLO11模型执行目标检测。用户可以上传图像并调整参数，例如置信度阈值和交并比（IoU）阈值，以获得实时检测结果。

观看： Gradio 与 Ultralytics YOLO11 的集成

本节提供了使用Ultralytics YOLO11模型创建Gradio界面的python代码。该代码支持分类任务、detect任务、segment任务和关键点任务。

要将 Gradio 与 Ultralytics YOLO11 结合使用以进行对象检测，您可以按照以下步骤操作：

使用 Gradio 进行 Ultralytics YOLO11 目标检测有以下几个优点：

有关更多详细信息，您可以阅读这篇关于放射学中的 AI 的博客文章，其中展示了类似的交互式可视化技术。

是的，Gradio 和 Ultralytics YOLO11 可以有效地结合用于教育目的。Gradio 直观的 Web 界面使学生和教育工作者可以轻松地与 Ultralytics YOLO11 等最先进的 深度学习 模型进行交互，而无需高级编程技能。此设置非常适合演示对象检测和 计算机视觉 中的关键概念，因为 Gradio 提供了即时的视觉反馈，有助于理解不同参数对检测性能的影响。

在YOLO11 的 Gradio 界面中，您可以使用提供的滑块调整置信度和IoU 。这些阈值有助于控制预测精度和对象分离度：

有关这些参数的更多信息，请访问参数解释部分。

将 Ultralytics YOLO11 与 Gradio 结合的实际应用包括：

如需类似用例的示例，请查看 Ultralytics 关于动物行为监测的博客，其中展示了交互式可视化如何加强野生动物保护工作。

**Examples:**

Example 1 (unknown):
```unknown
pip install gradio
```

Example 2 (python):
```python
import gradio as gr
import PIL.Image as Image

from ultralytics import ASSETS, YOLO

model = YOLO("yolo11n.pt")


def predict_image(img, conf_threshold, iou_threshold):
    """Predicts objects in an image using a YOLO11 model with adjustable confidence and IoU thresholds."""
    results = model.predict(
        source=img,
        conf=conf_threshold,
        iou=iou_threshold,
        show_labels=True,
        show_conf=True,
        imgsz=640,
    )

    for r in results:
        im_array = r.plot()
        im = Image.fromarray(im_array[..., ::-1])

    return im


iface = gr.Interface(
    fn=predict_image,
    inputs=[
        gr.Image(type="pil", label="Upload Image"),
        gr.Slider(minimum=0, maximum=1, value=0.25, label="Confidence threshold"),
        gr.Slider(minimum=0, maximum=1, value=0.45, label="IoU threshold"),
    ],
    outputs=gr.Image(type="pil", label="Result"),
    title="Ultralytics Gradio",
    description="Upload images for inference. The Ultralytics YOLO11n model is used by default.",
    examples=[
        [ASSETS / "bus.jpg", 0.25, 0.45],
        [ASSETS / "zidane.jpg", 0.25, 0.45],
    ],
)

if __name__ == "__main__":
    iface.launch()
```

Example 3 (python):
```python
import gradio as gr

from ultralytics import YOLO

model = YOLO("yolo11n.pt")


def predict_image(img, conf_threshold, iou_threshold):
    results = model.predict(
        source=img,
        conf=conf_threshold,
        iou=iou_threshold,
        show_labels=True,
        show_conf=True,
    )
    return results[0].plot() if results else None


iface = gr.Interface(
    fn=predict_image,
    inputs=[
        gr.Image(type="pil", label="Upload Image"),
        gr.Slider(minimum=0, maximum=1, value=0.25, label="Confidence threshold"),
        gr.Slider(minimum=0, maximum=1, value=0.45, label="IoU threshold"),
    ],
    outputs=gr.Image(type="pil", label="Result"),
    title="Ultralytics Gradio YOLO11",
    description="Upload images for YOLO11 object detection.",
)
iface.launch()
```

---

## EfficientDet 对比 YOLO11：平衡效率与实时性能

**URL:** https://docs.ultralytics.com/zh/compare/efficientdet-vs-yolo11/

**Contents:**
- EfficientDet 对比 YOLO11：平衡效率与实时性能
- Google 的 EfficientDet
  - 架构与关键创新
  - 优势与劣势
  - 理想用例
- Ultralytics YOLO11
  - 架构与特性
  - 优势与劣势
  - 理想用例
- 性能对比

目标检测领域发展迅速，其驱动力是对既准确又足以高效部署于实际场景的模型的需求。Google的EfficientDet和Ultralytics YOLO11是这一演进中的两个重要里程碑。尽管这两种架构都旨在优化速度和准确性之间的权衡，但它们以不同的设计理念解决问题，并针对不同的主要用例。

EfficientDet 通过引入一种系统化的模型维度缩放方法，彻底改变了该领域，并高度关注参数效率和理论计算成本 (FLOPs)。相比之下，YOLO11 代表了实时计算机视觉的前沿，优先考虑现代硬件上的实际推理速度、跨任务的多功能性以及以开发者为中心的体验。这份全面的比较将深入探讨它们的技术规范、架构创新和性能基准，以帮助您为您的项目选择合适的工具。

EfficientDet 是由 Google Brain 团队开发的一系列目标 detect 模型。它于 2019 年末发布，旨在解决以前最先进的 detect 器效率低下的问题，这些 detect 器通常依赖于庞大的骨干网络或未经优化的特征融合网络。

EfficientDet的成功之处在于两大架构贡献，它们协同工作以最大化效率：

EfficientDet 使用 EfficientNet 作为其主干网络，EfficientNet 也是由 Google 开发的分类网络。EfficientNet 使用 神经架构搜索 (NAS) 进行优化，以找到最有效的网络结构，并大量利用深度可分离卷积来减少计算量。

EfficientDet 以其 高参数效率而闻名，与许多同期模型相比，它以显著更少的参数实现了具有竞争力的 mAPval 分数。其可扩展性使研究人员能够选择精确符合其理论计算预算的模型尺寸。

然而，理论效率并非总能转化为实际速度。深度可分离卷积的广泛使用以及BiFPN的复杂连接可能导致较低的GPU利用率。因此，与为并行处理优化的模型（如YOLO系列）相比，GPU上的推理延迟通常更高。此外，EfficientDet严格来说是一个目标detect器，在同一代码库中缺乏对其他计算机视觉任务（如实例segment或姿势估计）的原生支持。

了解更多关于 EfficientDet 的信息

Ultralytics YOLO11 是广受好评的 YOLO (You Only Look Once) 系列的最新迭代。它建立在实时性能的传统之上，引入了架构改进，在保持开发人员期望的闪电般快速的推理速度的同时，突破了准确性的界限。

YOLO11 采用最先进的无锚点检测头，消除了手动配置锚框的需要，并简化了训练过程。其骨干网络和颈部架构已得到优化，以增强特征提取能力，从而在小目标 detect 和复杂场景等挑战性任务上提升了性能。

与 EfficientDet 主要侧重于 FLOP 减少不同，YOLO11 专为硬件感知效率而设计。这意味着其层和操作经过选择，以最大限度地提高 GPU 和 NPU 加速器上的吞吐量。

单一的 YOLO11 模型架构支持广泛的视觉任务。在同一框架内，您可以执行目标检测、实例分割、图像分类、姿势估计和旋转框检测 (OBB)。

YOLO11 的主要优势在于其卓越的速度-精度平衡。它提供了与大型模型媲美甚至超越的最先进精度，同时以极低的延迟运行。这使其成为实时推理应用的理想选择。此外，Ultralytics 生态系统通过统一的 API 确保了易用性，使训练和部署无缝衔接。

一个需要考虑的因素是，最小的YOLO11变体虽然速度极快，但与学术界中最大、计算量最重的模型相比，可能会牺牲一小部分精度。然而，对于实际部署而言，这种权衡几乎总是有利的。

在比较 EfficientDet 和 YOLO11 时，最显著的区别在于推理速度，尤其是在 GPU 硬件上。虽然 EfficientDet 模型 (D0-D7) 表现出良好的参数效率，但其复杂的操作（如 BiFPN）阻碍了它们充分利用并行处理能力。

如下表所示，YOLO11n实现了比EfficientDet-d0（34.6）更高的mAP（39.5），同时显著更快。更令人印象深刻的是，YOLO11m达到了与更重的EfficientDet-d5（51.5 mAP）相同的准确性，但在T4 GPU上运行速度大约快14倍（4.7 ms vs 67.86 ms）。这种巨大的速度优势使得YOLO11能够实时处理高分辨率视频流，这对于更高级别的EfficientDet模型来说是一项挑战。

尽管技术指标至关重要，但开发者体验和生态系统支持对于项目成功同样重要。Ultralytics 提供了一套全面的工具，可简化整个 MLOps 生命周期，与以研究为中心的 EfficientDet 仓库相比，具有明显的优势。

体验 Ultralytics API 的简洁性。以下示例演示了如何加载预训练的 YOLO11 模型并对图像运行推理：

EfficientDet 和 YOLO11 都是计算机视觉领域的里程碑式成就。EfficientDet 仍然是可扩展架构设计的宝贵参考，适用于理论 FLOPs 是主要限制的利基应用。

然而，对于绝大多数现代计算机视觉应用而言，Ultralytics YOLO11 是卓越之选。其架构在准确性和速度之间提供了更好的平衡，尤其是在大多数生产环境中使用的GPU硬件上。结合多功能的多任务框架、强大的生态系统和无与伦比的易用性，YOLO11 使开发人员能够自信地构建和部署高性能AI解决方案。

为了进一步了解目标检测模型的全貌，可以探索这些额外的比较：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLO11n model
model = YOLO("yolo11n.pt")

# Run inference on an image source
results = model("path/to/image.jpg")

# Display the results
results[0].show()
```

---

## YOLOv9 与 YOLOv7：深入探讨目标检测演进

**URL:** https://docs.ultralytics.com/zh/compare/yolov9-vs-yolov7/

**Contents:**
- YOLOv9 与 YOLOv7：深入探讨目标检测演进
- 性能指标与效率
- YOLOv9：可编程梯度信息
  - 架构创新
  - 优势与劣势
- YOLOv7: 免费赠品包标准
  - 架构概述
  - 优势与劣势
- 理想用例和应用
  - 何时选择 YOLOv9

计算机视觉领域以快速创新为特征，架构突破不断重新定义速度和准确性的界限。YOLOv9和YOLOv7是这一发展历程中的两个重要里程碑。这两种模型都源于王建尧及其同事的研究，代表着“You Only Look Once”系列的不同世代。

尽管YOLOv7在2022年发布时设定了实时对象检测的标准，但YOLOv9于2024年问世，引入了新颖的机制来解决深度网络中的信息丢失问题。本比较将探讨它们的技术规格、架构差异和实际应用，以帮助开发者选择最适合其需求的模型。

从YOLOv7到YOLOv9的演变在计算成本和检测性能之间的权衡中最为明显。YOLOv9引入了显著的效率提升，使其能够以更少的参数实现比其前身更高的平均精度均值 (mAP)。

例如，YOLOv9m 模型达到了与 YOLOv7l 相同的 51.4% mAPval，但参数量几乎减半（20.0M 对 36.9M），并且 FLOPs 显著减少。这种效率使得 YOLOv9 对于硬件资源受限的 边缘 AI 应用特别有吸引力。

YOLOv9代表着深度neural networks处理数据在层间传输方式的范式转变。该模型于2024年初发布，专门针对“信息瓶颈”问题，即数据在通过深度网络的连续层时会丢失。

作者：王建尧、廖鸿源Chien-Yao Wang, Hong-Yuan Mark Liao组织：中央研究院信息科学研究所日期：2024-02-21Arxiv:2402.13616 GitHub:WongKinYiu/yolov9文档：Ultralytics YOLOv9

YOLOv9的核心创新是引入了可编程梯度信息 (PGI)。PGI提供了一个辅助监督框架，确保梯度可靠地反向传播到初始层，从而保留了否则可能在特征提取过程中丢失的重要输入信息。

与 PGI 相辅相成的是 广义高效层聚合网络 (GELAN)。这种架构允许开发者灵活堆叠各种计算块（如 CSP 或 ResBlocks），从而在不牺牲准确性的前提下，针对特定的硬件限制优化 模型权重。

在 YOLOv9 之前，YOLOv7 是 YOLO 家族的卫冕冠军。它引入了架构改进，专注于优化训练过程而不增加推理成本，这一概念被称为“免费赠品包”（bag-of-freebies）。

作者： Chien-Yao Wang、Alexey Bochkovskiy、Hong-Yuan Mark LiaoChien-Yao Wang, Alexey Bochkovskiy, Hong-Yuan Mark LiaoOrganization：中央研究院信息科学研究所日期：2022-07-06Arxiv:2207.02696 GitHub:WongKinYiu/yolov7文档：Ultralytics YOLOv7

YOLOv7引入了E-ELAN（扩展高效层聚合网络），它通过控制最短和最长梯度路径来提高网络的学习能力。它还利用模型缩放技术，同时修改网络的深度和宽度，确保针对不同目标设备的最佳架构。

这两种模型之间的选择通常取决于部署环境的具体限制和任务所需的精度。

YOLOv9非常适合需要最高精度效率比的场景。

YOLOv7 对于已针对其架构进行优化的现有管道仍然适用。

虽然YOLOv9和YOLOv7功能强大，但寻求速度、准确性和开发者体验终极平衡的开发者应考虑Ultralytics YOLO11。YOLO11将前几代的最佳特性与简化的API集成，在单一框架中支持detect、segment、姿势估计和分类。

在Ultralytics 生态系统中使用这些模型比使用原始研究存储库具有显著优势。Ultralytics Python API 抽象化了复杂的样板代码，使研究人员和工程师能够专注于数据和结果。

使用Ultralytics库运行这些模型非常简单。以下代码片段演示了如何加载预训练模型并在图像上运行推理。

对于那些对在自定义数据集上进行训练感兴趣的人，该过程同样简单，利用了框架内置的强大超参数调优和数据增强策略。

YOLOv9 和 YOLOv7 都代表了计算机视觉领域的重大成就。YOLOv9 是明确的技术继承者，通过其创新的 PGI 和 GELAN 架构提供了卓越的参数效率和准确性。对于寻求特定 Wang et al. 研究谱系高性能的用户来说，它是推荐的选择。

然而，对于寻求最全面的AI开发体验的开发者而言，Ultralytics YOLO11仍然是首要推荐。凭借其积极的维护、详尽的文档和对多模态任务的广泛支持，YOLO11确保您的项目面向未来且生产就绪。

为了进一步拓宽您对目标检测领域的理解，可以探索这些相关的模型和比较：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv9 model
model = YOLO("yolov9c.pt")

# Run inference on a local image
results = model.predict("path/to/image.jpg", save=True, conf=0.5)

# Process results
for result in results:
    result.show()  # Display predictions
```

Example 2 (markdown):
```markdown
# Train the model on a custom dataset
model.train(data="coco8.yaml", epochs=100, imgsz=640)
```

---

## YOLOX 对比 YOLOv10：技术比较 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/compare/yolox-vs-yolov10/

**Contents:**
- YOLOX 对比 YOLOv10：技术比较
- YOLOX：无锚框先驱
  - 架构和主要特性
  - 优势与劣势
- YOLOv10：实时端到端检测
  - 架构与创新
  - 优势与益处
- 性能分析
- 训练方法与生态系统
  - 代码示例：使用 YOLOv10

目标检测领域经历了快速发展，这得益于对兼顾高精度和实时推理速度模型的需求。YOLOX和YOLOv10代表了这一时间线上的两个重要里程碑。YOLOX于2021年发布，通过引入无锚点架构重振了YOLO系列；而YOLOv10于2024年发布，通过消除对非极大值抑制（NMS）的需求，树立了新标准，显著降低了推理延迟。

这项综合分析探讨了两种模型的架构创新、性能指标和理想用例，旨在帮助开发人员和研究人员为其计算机视觉应用选择最佳工具。

YOLOX于2021年由旷视推出，标志着其摆脱了早期YOLO版本中主导的基于锚框的设计。通过采用无锚框机制并集成解耦头和SimOTA等先进技术，YOLOX取得了具有竞争力的性能，并弥合了研究框架与工业应用之间的鸿沟。

技术细节：作者：葛正、刘松涛、王峰、李泽明、孙剑组织：旷视日期：2021-07-18Arxiv：https://arxiv.org/abs/2107.08430GitHub：https://github.com/Megvii-BaseDetection/YOLOX文档：https://yolox.readthedocs.io/en/latest/

YOLOX与其前身，如YOLOv4和YOLOv5不同，它通过实施几项关键的架构更改，旨在提高泛化能力并简化训练流程。

由清华大学研究人员于2024年5月发布，YOLOv10代表了实时目标检测领域的一个范式转变。通过消除对非极大值抑制（NMS）的需求并优化模型组件以提高效率，YOLOv10以显著降低的计算开销实现了卓越的速度和准确性。

技术细节：作者：王傲、陈辉、刘力豪 等组织：清华大学日期：2024-05-23Arxiv：https://arxiv.org/abs/2405.14458GitHub：https://github.com/THU-MIG/yolov10文档：https://docs.ultralytics.com/models/yolov10/

YOLOv10 专注于整体效率-精度驱动的模型设计，同时解决了架构和后处理流程的问题。

下表比较了 YOLOX 和 YOLOv10 在COCO 基准数据集上的性能。这些指标突出了新模型在效率方面的显著改进。

分析： 数据清楚地表明了 YOLOv10 在效率方面的优势。例如，与 YOLOX-s (40.5%) 相比，YOLOv10-s 实现了明显更高的 mAP，达到 46.7%，同时使用的参数更少（7.2M 对 9.0M）。值得注意的是，YOLOv10-x 在精度上超过了 YOLOX-x（54.4% 对 51.1%），同时速度更快（12.2 毫秒对 16.1 毫秒），并且需要的参数几乎减少了一半（56.9M 对 99.1M）。这种效率使 YOLOv10 成为实时系统的更好选择。

YOLOv10 消除了 NMS 后处理，这意味着推理时间更加稳定和可预测，这对于 自动驾驶汽车和工业机器人等安全关键型应用来说是一个关键因素。

尽管YOLOX引入了现在已成为标准的先进数据增强技术，但YOLOv10受益于成熟且用户友好的Ultralytics训练流程。

以下示例演示了开发人员如何轻松加载预训练的YOLOv10模型，并使用Ultralytics库对图像运行推理。

两种模型各有其适用之处，但 YOLOv10 的现代架构使其适用于更广泛的现代应用。

尽管YOLOX在普及无锚点detect方面发挥了关键作用，但YOLOv10在现代开发中脱颖而出，成为卓越之选。其创新的NMS-free架构，结合全面的Ultralytics生态系统，提供了一个既快速又准确的强大解决方案。

对于寻求最佳性能平衡、易用性和长期支持的开发者而言，YOLOv10 强烈推荐。此外，对于那些在诸如 姿势估计 或 实例分割 等任务中需要更多多功能性的人来说，强大的 YOLO11 模型在同一用户友好的框架内提供了一个出色的替代方案。

选择 Ultralytics 模型，您可以确保您的项目建立在前沿研究、活跃社区支持和生产级可靠性的基础之上。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv10n model
model = YOLO("yolov10n.pt")

# Run inference on a local image
results = model("path/to/image.jpg")

# Display the results
results[0].show()
```

---

## YOLOX 对比 YOLOv9：技术比较 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/compare/yolox-vs-yolov9/

**Contents:**
- YOLOX 对比 YOLOv9：技术比较
- YOLOX：无锚框先驱
  - 架构亮点
  - 优势与劣势
- YOLOv9：可编程梯度信息
  - 架构亮点
  - 优势与劣势
- 性能对比
- Ultralytics 优势
  - 易用性与集成

为目标检测选择合适的架构是一个关键决策，它会影响计算机视觉项目的速度、准确性和部署可行性。本分析比较了 YOLOX（一个于 2021 年发布的关键无锚点模型）和 YOLOv9（一个于 2024 年推出的、利用可编程梯度信息 (PGI) 的最先进架构）。

尽管YOLOX将范式转向了无锚点detect，但YOLOv9引入了新颖的机制来保留深度网络中的信息，提供了卓越的性能指标。本指南将详细分析它们的架构、基准测试和理想用例，以帮助您选择最适合您需求的模型。

YOLOX的发布旨在通过简化检测头并消除对预定义锚框的依赖，弥合研究界与工业应用之间的鸿沟。

作者: Zheng Ge, Songtao Liu, Feng Wang, Zeming Li, and Jian Sun组织:Megvii日期: 2021-07-18Arxiv:arXiv:2107.08430GitHub:Megvii-BaseDetection/YOLOX文档:YOLOX 文档

YOLOX引入了解耦头架构，将分类和回归任务分离。这种分离使模型能够更快收敛并获得更高的准确性。它还采用了无锚框机制，消除了通过聚类分析确定最佳锚框尺寸的需要，使模型对各种对象形状更具鲁棒性。此外，YOLOX利用SimOTA进行标签分配，将该过程视为一个最优传输问题，以提高训练稳定性。

YOLOv9 代表了一个重大飞跃，解决了深度神经网络固有的“信息瓶颈”问题。

作者: Chien-Yao Wang, Hong-Yuan Mark Liao机构:中央研究院资讯科学研究所日期: 2024-02-21Arxiv:arXiv:2402.13616GitHub:WongKinYiu/yolov9文档:Ultralytics YOLOv9 文档

YOLOv9 引入了可编程梯度信息 (PGI) 和 广义高效层聚合网络 (GELAN)。PGI 防止数据通过深层时关键输入信息的丢失，确保为模型更新生成可靠的梯度。GELAN 优化了参数利用率，使模型轻量化且准确。这些创新使 YOLOv9 在效率和 平均精度 (mAP) 方面显著超越了前代产品。

下表对比了 YOLOX 和 YOLOv9 在COCO 数据集上的性能。YOLOv9 始终以更少的参数展现出更高的 mAP 分数，突出了 GELAN 架构的效率。

分析： YOLOv9 在性能密度方面提供了实质性的升级。例如，YOLOv9c 仅用 25.3M 参数 实现了 53.0% mAP，而 YOLOX-L 需要 54.2M 参数 才能实现较低的 49.7% mAP 分数。这表明，对于此精度等级，YOLOv9 在参数使用方面大约有效两倍。

部署到边缘设备时，FLOPs 和参数与 mAP 同等重要。YOLOv9 的 GELAN 架构显著降低了计算开销，从而使设备运行更凉爽，并延长了移动部署中的电池寿命。

尽管YOLOX是一个强大的独立代码库，但在Ultralytics生态系统中使用YOLOv9为开发者和研究人员提供了独特的优势。

Ultralytics 框架统一了模型交互。您可以使用简单直观的 Python API 来训练、验证和部署 YOLOv9。这与 YOLOX 代码库形成对比，后者通常需要更多手动配置环境变量和数据集路径。

Ultralytics 模型受益于持续更新、错误修复和社区支持。与 Ultralytics HUB 的集成实现了无缝的 MLOps，使团队能够管理数据集、跟踪实验，并将模型部署到各种格式（ONNX、TensorRT、CoreML），而无需编写复杂的导出脚本。

Ultralytics YOLO 模型旨在实现速度与准确性之间的实用平衡。此外，与旧架构或基于重型 Transformer 的模型相比，它们在训练期间通常表现出较低的内存需求。这种效率降低了云计算成本，并使消费级GPUs上的训练成为可能。

尽管YOLOX主要是一个目标detect器，但Ultralytics框架扩展了其支持模型的功能。用户可以使用相似的语法和工作流程，轻松地在实例segment、姿势估计和旋转框检测等任务之间切换，这种多功能性是独立研究代码库通常缺乏的。

两种架构都在计算机视觉史上占据一席之地。YOLOX 在 2021 年成功挑战了基于锚点的现状。然而，YOLOv9 代表了现代标准，融合了梯度流优化和层聚合方面的多年进步。

对于大多数新开发项目，YOLOv9 是推荐的选择。其卓越的每参数性能，结合 Ultralytics 生态系统提供的易用性、训练效率 和强大部署选项，确保了从概念到生产的更快路径。

探索生态系统中的其他现代选项，例如 YOLO11 和 YOLOv8，以找到最适合您特定应用约束的模型。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv9 model
model = YOLO("yolov9c.pt")

# Run inference on an image
results = model("path/to/image.jpg")

# Train the model on a custom dataset with a single line of code
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)
```

---

## 使用 Ultralytics YOLO11 实现对象裁剪 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/guides/object-cropping/

**Contents:**
- 使用 Ultralytics YOLO11 进行目标裁剪
- 什么是对象裁剪？
- 对象裁剪的优势
- 视觉效果
  - ObjectCropper 参数
- 常见问题
  - Ultralytics YOLO11 中的目标裁剪是什么，它是如何工作的？
  - 为什么我应该使用 Ultralytics YOLO11 进行目标裁剪，而不是其他解决方案？
  - 如何使用目标裁剪来减少数据集的数据量？
  - 我可以使用 Ultralytics YOLO11 进行实时视频分析和目标裁剪吗？

使用 Ultralytics YOLO11 进行目标裁剪包括从图像或视频中隔离和提取特定的检测到的目标。YOLO11 模型的功能用于准确识别和描绘目标，从而实现精确裁剪，以进行进一步的分析或操作。

观看： 使用 Ultralytics YOLO 进行目标裁剪

使用 Ultralytics YOLO 进行目标裁剪

当您提供可选的 crop_dir 参数，每个裁剪的对象都会写入该文件夹，文件名包含源图像名称和类别。这使得检查检测结果或构建下游数据集变得容易，无需编写额外代码。

这是一个包含以下内容的表格 ObjectCropper 参数：

使用 Ultralytics YOLO11 进行目标裁剪包括基于 YOLO11 的检测能力从图像或视频中隔离和提取特定目标。此过程允许通过利用 YOLO11 以高精度识别目标并相应地裁剪它们，从而实现专注分析、减少数据量和增强精度。有关深入的教程，请参阅目标裁剪示例。

Ultralytics YOLO11 以其精度、速度和易用性而著称。它允许详细而准确的对象检测和裁剪，这对于重点分析和需要高数据完整性的应用至关重要。此外，YOLO11 与 OpenVINO 和 TensorRT 等工具无缝集成，适用于需要实时功能和在各种硬件上进行优化的部署。请在模型导出指南中了解其优势。

通过使用 Ultralytics YOLO11 仅裁剪图像或视频中的相关物体，您可以显著减小数据大小，使其更高效地进行存储和处理。此过程涉及训练模型以 detect 特定物体，然后使用结果仅裁剪并保存这些部分。有关利用 Ultralytics YOLO11 功能的更多信息，请访问我们的 快速入门指南。

是的，Ultralytics YOLO11 可以处理实时视频流以动态 detect 和裁剪对象。该模型的高速推理能力使其成为实时应用的理想选择，例如监控、体育分析和自动化检测系统。查看track和预测模式，了解如何实现实时处理。

Ultralytics YOLO11 针对 CPU 和 GPU 环境进行了优化，但为了获得最佳性能，特别是对于实时或高吞吐量推理，建议使用专用 GPU（例如 NVIDIA Tesla、RTX 系列）。对于轻量级设备的部署，可以考虑使用 CoreML 适用于 iOS 或 TFLite 适用于 Android。有关支持的设备和格式的更多详细信息，请参阅我们的模型部署选项。

**Examples:**

Example 1 (markdown):
```markdown
# Crop the objects
yolo solutions crop show=True

# Pass a source video
yolo solutions crop source="path/to/video.mp4"

# Crop specific classes
yolo solutions crop classes="[0, 2]"
```

Example 2 (python):
```python
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "Error reading video file"

# Initialize object cropper
cropper = solutions.ObjectCropper(
    show=True,  # display the output
    model="yolo11n.pt",  # model for object cropping, e.g., yolo11x.pt.
    classes=[0, 2],  # crop specific classes such as person and car with the COCO pretrained model.
    # conf=0.5,  # adjust confidence threshold for the objects.
    # crop_dir="cropped-detections",  # set the directory name for cropped detections
)

# Process video
while cap.isOpened():
    success, im0 = cap.read()

    if not success:
        print("Video frame is empty or processing is complete.")
        break

    results = cropper(im0)

    # print(results)  # access the output

cap.release()
cv2.destroyAllWindows()  # destroy all opened windows
```

---

## YOLOv6-3.0 与 YOLOv10：详细技术比较

**URL:** https://docs.ultralytics.com/zh/compare/yolov6-vs-yolov10/

**Contents:**
- YOLOv6-3.0 与 YOLOv10：详细技术比较
- YOLOv6-3.0：工业级速度与精度
  - 架构和主要特性
  - 优势与劣势
- YOLOv10：端到端效率前沿
  - 架构和主要特性
  - 优势与劣势
- 性能分析：指标与基准
  - 关键见解
- 使用与实现

选择最优的计算机视觉模型对于AI项目的成功至关重要，它需要在推理延迟、准确性和计算效率等因素之间取得平衡。这项全面的技术比较考察了两种著名的对象detect架构：YOLOv6-3.0（为工业速度而设计）和YOLOv10（以其实时端到端效率而闻名）。我们分析了它们的架构创新、基准指标和理想用例，以指导您的选择过程。

YOLOv6-3.0 由美团视觉智能部开发，是一个专门针对工业应用进行优化的单阶段目标检测框架。该模型于2023年初发布，优先考虑硬件友好的设计，以最大限度地提高 GPU 和边缘设备上的吞吐量，从而满足制造和物流领域实时推理的严苛需求。

YOLOv6-3.0对其架构进行了“全面重载”，融入了多项先进技术，以增强特征提取和收敛速度：

优势： YOLOv6-3.0 在需要高吞吐量的场景中表现出色。它对模型量化的支持使其能够在移动平台和嵌入式系统上有效部署。“精简版”变体对于 CPU 受限的环境特别有用。

缺点： 作为一个严格专注于目标检测的模型，它缺乏对实例分割或姿势估计等更广泛任务的原生支持，而这些任务在YOLO11等统一框架中可以找到。此外，与较新的模型相比，其参数效率较低，在相似的精度水平下需要更多内存。

YOLOv6-3.0 是工业自动化的有力竞争者，在工业自动化中，装配线上的摄像头必须快速处理高分辨率视频流以检测缺陷或分拣物品。

由清华大学研究人员于 2024 年 5 月推出，YOLOv10 通过消除后处理过程中对非极大值抑制 (NMS)的需求，突破了 YOLO 家族的界限。这一创新使其成为面向延迟敏感型应用的下一代模型。

YOLOv10采用了整体效率-精度驱动的设计策略：

优势： YOLOv10 提供了卓越的速度-精度权衡，通常以比前代少得多的参数实现更高的mAP。它集成到 Ultralytics Python 生态系统，使其与其他模型一起训练和部署变得异常容易。

缺点： 作为一个相对较新的项目，其社区资源和第三方工具仍在发展中。像 YOLOv6 一样，它专注于 detect，而需要多任务能力的用户可能更喜欢YOLO11。

移除NMS使YOLOv10能够实现稳定的推理延迟，这对于自动驾驶车辆等安全关键系统至关重要，因为这些系统的处理时间必须是确定性的。

下表比较了 YOLOv6-3.0 和 YOLOv10 在 COCO 数据集上的性能。关键指标包括模型大小、平均精度均值 (mAP) 以及在 CPU 和 GPU 上的推理速度。

Ultralytics 为使用这些模型提供了精简的体验。YOLOv10 在 ultralytics 包，从而实现无缝 训练 和预测。

您只需几行代码即可使用Python API运行YOLOv10。这突显了Ultralytics生态系统固有的易用性。

YOLOv6-3.0 通常需要克隆美团官方仓库进行训练和推理，因为它遵循不同的代码库结构。

两种模型都代表了计算机视觉领域的重大成就。YOLOv6-3.0 对于专门为其架构优化的传统工业系统来说，仍然是一个可靠的选择。然而，YOLOv10 由于其 NMS-free 架构、卓越的参数效率和更高的精度，通常能为新项目带来更好的投资回报。

对于寻求极致多功能性和生态系统支持的开发者而言，Ultralytics YOLO11 强烈推荐。YOLO11 不仅提供了最先进的 detect 性能，而且在一个单一、维护良好的软件包中原生支持 姿势估计、obb 和 分类。Ultralytics 生态系统确保了高效的 训练过程、低内存使用和轻松导出到 ONNX 和 TensorRT 等格式，使您能够自信地部署强大的AI解决方案。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv10n model
model = YOLO("yolov10n.pt")

# Run inference on an image
results = model.predict("path/to/image.jpg", save=True)

# Train the model on a custom dataset
# model.train(data="coco8.yaml", epochs=100, imgsz=640)
```

Example 2 (julia):
```julia
# Clone the YOLOv6 repository
git clone https://github.com/meituan/YOLOv6
cd YOLOv6
pip install -r requirements.txt

# Inference using the official script
python tools/infer.py --weights yolov6s.pt --source path/to/image.jpg
```

---

## EfficientDet 对比 YOLOv7：全面技术比较

**URL:** https://docs.ultralytics.com/zh/compare/efficientdet-vs-yolov7/

**Contents:**
- EfficientDet 对比 YOLOv7：全面技术比较
- 性能指标与分析
  - 主要内容
- EfficientDet 概述
  - 架构亮点
- YOLOv7 概述
  - 架构亮点
- 理想用例
  - 何时选择 EfficientDet
  - 何时选择 YOLOv7

在快速发展的计算机视觉领域，选择合适的物体 detect 架构对于项目成功至关重要。本分析比较了 EfficientDet（一个专注于效率的可扩展架构）和 YOLOv7（一个专为 GPU 硬件上的速度和准确性设计的实时检测器）。尽管这两个模型在各自发布时都代表了最先进的性能，但了解它们的技术细微差别有助于开发人员为现代部署做出明智的决策。

下表详细比较了关键性能指标，包括平均精度均值 (mAP)、在不同硬件上的推理速度以及计算复杂度（参数量和 FLOPs）。

EfficientDet 在 2019 年通过提出一种可扩展的架构，同时优化准确性和效率，从而引入了范式转变。它基于 EfficientNet 骨干网络，并引入了 BiFPN（双向特征金字塔网络）。

EfficientDet详情： 作者：Mingxing Tan, Ruoming Pang, and Quoc V. Le 机构：Google 日期：2019-11-20 Arxiv：https://arxiv.org/abs/1911.09070 GitHub：https://github.com/google/automl/tree/master/efficientdet

EfficientDet的核心创新是BiFPN，它实现了简单快速的多尺度特征融合。与传统FPN不同，BiFPN使用加权特征融合来学习不同输入特征的重要性。结合统一缩放分辨率、深度和宽度的复合缩放，EfficientDet提供了一系列（D0到D7）模型，以适应各种资源限制。

了解更多关于 EfficientDet 的信息

YOLOv7 于2022年发布，通过专注于优化训练过程和架构以提高推理速度，突破了实时目标detect的界限。它引入了多项“免费赠品”（Bag-of-Freebies），在不增加推理成本的情况下提高了准确性。

YOLOv7 详情： 作者： Chien-Yao Wang、Alexey Bochkovskiy 和 Hong-Yuan Mark LiaoChien-Yao Wang、Alexey Bochkovskiy 和 Hong-Yuan Mark Liao 机构： 台湾中央研究院信息科学研究所台湾中央研究院资讯科学研究所 日期： 2022-07-06 Arxiv:https://arxiv.org/abs/2207.02696 GitHub:https://github.com/WongKinYiu/yolov7

YOLOv7 利用 E-ELAN（扩展高效层聚合网络），它控制最短和最长的梯度路径，以使网络能够学习更多样化的特征。它还对基于拼接的模型采用模型缩放，使其能够在不同尺寸下保持最佳结构。该架构专门针对 GPU 效率进行了调整，避免了尽管 FLOP 计数低但内存访问成本高的操作。

在这些架构之间进行选择在很大程度上取决于部署硬件和具体的应用需求。

EfficientDet 非常适合 CPU 密集型环境或内存带宽和存储严格受限的边缘设备。其低参数量使其适用于：

YOLOv7在高性能GPU环境中表现出色，在这些环境中低延迟是不可妥协的。它是以下场景的首选：

尽管EfficientDet和YOLOv7都很强大，但该领域已经取得了进展。对于新项目，通常推荐使用YOLO11。它结合了现代主干网络的效率概念与YOLO系列的实时速度，在精度和部署便捷性方面通常优于两者。

尽管EfficientDet和YOLOv7仍然是计算机视觉领域的重要贡献，但Ultralytics生态系统——其中包含YOLOv8和尖端YOLO11等模型——为开发者和研究人员提供了独特的优势。

传统模型通常需要复杂的安装步骤、特定的 CUDA 版本或分散的代码库。相比之下，Ultralytics 专注于提供统一、精简的用户体验。凭借简单的 pip install ultralytics，用户可以访问强大的 python API 和 CLI 命令 标准化训练、验证和部署。该 维护良好的生态系统 确保频繁更新、广泛的硬件支持以及与诸如...等工具的集成 Ultralytics HUB 实现无缝MLOps。

Ultralytics 模型在工程上旨在实现最佳的性能平衡。它们在保持卓越推理速度的同时提供最先进的准确性，使其适用于从边缘部署到云 API 的各种场景。此外，训练 Ultralytics YOLO 模型的内存需求通常低于基于 Transformer 的架构或旧版 ConvNet，从而可以在消费级 GPU 上进行高效训练。

与许多特定检测器不同，Ultralytics 模型高度通用。单一框架支持：

这种多功能性，结合训练效率（得益于优化的数据加载器和COCO上现成的预训练权重），显著缩短了 AI 解决方案的上市时间。

以下是一个示例，展示了现代 Ultralytics 模型如何轻松用于推理，这与旧架构通常所需的样板代码形成了鲜明对比。

EfficientDet 和 YOLOv7 代表了计算机视觉历史上的两种不同理念：一种优化理论效率（FLOPs/Params），另一种优化实际硬件延迟。EfficientDet 仍然是参数受限的 CPU 应用的有力参考，而 YOLOv7 则很好地服务于高速 GPU 工作负载。

然而，对于寻求兼具速度、准确性和无摩擦开发体验的开发者而言——像YOLO11这样的Ultralytics模型是卓越的选择。它们简化了训练和部署的复杂流程，同时提供满足现代计算机视觉应用严苛要求的性能。

探索更多技术比较，以找到最适合您特定需求的模型：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load the latest YOLO11 model (pre-trained on COCO)
model = YOLO("yolo11n.pt")

# Perform inference on an image
results = model("https://ultralytics.com/images/bus.jpg")

# Process results
for result in results:
    result.save()  # Save the annotated image to disk
    print(f"Detected {len(result.boxes)} objects.")
```

---

## DAMO-YOLO 与 YOLO11 的技术对比 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/compare/damo-yolo-vs-yolo11/

**Contents:**
- DAMO-YOLO 与 YOLO11 的技术对比
- DAMO-YOLO
  - 架构与创新
- Ultralytics YOLO11
  - 架构与生态系统
- 性能对比
  - 结果分析
- 主要区别因素和用例
  - Ultralytics YOLO11 的优势
  - DAMO-YOLO 的优势

在快速发展的计算机视觉领域，选择正确的物体检测模型对于应用的成功至关重要。本篇综合比较分析了两种重要的架构：阿里巴巴集团开发的YOLO 和 Ultralytics YOLO11是Ultralytics 最新推出的最先进模型。虽然这两种模型都旨在优化速度和准确性之间的权衡，但它们的主要目的各不相同，并根据部署场景的不同而具有不同的优势。

本指南深入探讨了它们的架构、性能指标和理想用例，旨在帮助开发人员和研究人员做出明智的决策。

作者： 徐贤哲、蒋一奇、陈卫华、黄一伦、张远、孙秀宇机构：阿里巴巴集团日期： 2022-11-23预印本：https://arxiv.org/abs/2211.15444v2GitHub：https://github.com/tinyvision/DAMO-YOLO文档：https://github.com/tinyvision/DAMO-YOLO/blob/master/README.md

DAMO-YOLO 是一个目标检测框架，它集成了多项尖端技术以实现高性能。通过阿里巴巴研究驱动的一系列架构创新，它致力于在保持竞争性准确性的同时降低延迟。

DAMO-YOLO 引入了“蒸馏与选择”方法，并整合了以下关键组件：

尽管DAMO-YOLO展现了令人印象深刻的理论进展，但它主要是一个以研究为导向的框架，专注于对象检测。它通常缺乏更全面的生态系统中原生的多任务支持。

作者: Glenn Jocher, Jing Qiu机构:Ultralytics日期: 2024-09-27GitHub:https://github.com/ultralytics/ultralytics文档:https://docs.ultralytics.com/models/yolo11/

Ultralytics YOLO11 代表了实时计算机视觉的巅峰，通过在架构、效率和易用性方面的显著改进，进一步完善了 YOLO 系列的传承。它不仅仅被设计为一个模型，更是一个多功能工具，适用于在各种硬件环境中进行实际的、真实世界的部署。

YOLO11 在之前的成功基础上，采用了精炼的无锚点架构。它具有改进的主干网络，可实现卓越的特征提取，以及改进的颈部设计，增强了不同尺度信息流的传输。

Ultralytics YOLO11 框架的主要优势包括：

YOLO11 旨在 边缘 AI 设备上实现高效率。其优化的架构确保在 NVIDIA Jetson 和 Raspberry Pi 等硬件上实现低内存使用和高推理速度，使其成为嵌入式应用优于更重型 Transformer 模型的一个卓越选择。

以下图表和表格说明了DAMO-YOLO和YOLO11之间的性能差异。Ultralytics YOLO11始终表现出卓越的准确性（mAP）和理想的推理速度，尤其是在DAMO-YOLO缺乏官方基准测试的CPU硬件上。

利用Ultralytics YOLO11的开发者可以获得一个强大的生产级环境。

DAMO-YOLO 在学术研究领域是一个强有力的竞争者。

尽管DAMO-YOLO引入了阿里巴巴研究团队提出的新颖概念，但Ultralytics YOLO11脱颖而出，成为绝大多数开发者和企业的卓越选择。其主导地位不仅体现在更高的mAP分数和更快的推理速度，更在于其背后全面的生态系统支持。

从易用性和多功能性到良好维护的代码库和活跃的社区支持，YOLO11 降低了创建高级 AI 解决方案的门槛。无论是部署在云服务器还是资源受限的边缘设备上，YOLO11 都为现代计算机视觉应用提供了必要的可靠性和性能。

要更好地了解 Ultralytics 模型与其他架构的比较，请查阅我们的详细比较页面：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pretrained YOLO11 model
model = YOLO("yolo11n.pt")

# Train on a custom dataset
model.train(data="coco8.yaml", epochs=50, imgsz=640)

# Run inference on an image
results = model("path/to/image.jpg")
```

---

## YOLOv9 vs YOLO11：架构演进与性能分析

**URL:** https://docs.ultralytics.com/zh/compare/yolov9-vs-yolo11/

**Contents:**
- YOLOv9 vs YOLO11：架构演进与性能分析
- 性能指标：速度与准确性
- YOLOv9：解决信息瓶颈问题
  - 主要架构特性
- Ultralytics YOLO11：多功能性与效率兼备
  - 主要架构特性
- 详细比较点
  - 易用性与生态系统
  - 内存要求与训练效率
  - 任务多样性

计算机视觉领域以快速创新为特征，模型不断突破准确性、速度和效率的极限。本文将探讨目标检测领域的两个重要里程碑：YOLOv9（引入新颖架构概念的研究型模型）和Ultralytics YOLO11（为实际多功能性设计的最新生产就绪演进）。

尽管YOLOv9通过理论突破专注于解决深度学习信息瓶颈，但Ultralytics YOLO11则侧重于可用性、效率以及与Ultralytics生态系统的无缝集成，从而提升了最先进 (SOTA) 性能。

下表直接比较了在COCO 数据集上评估的关键性能指标。在选择模型时，平衡平均精度均值 (mAP) 与推理速度和计算成本 (FLOPs) 至关重要。

数据表明，YOLO11展现出卓越的效率。例如，YOLO11n模型实现了比YOLOv9t（38.3%）更高的mAP（39.5%），同时使用的FLOPs更少，并在GPU上运行速度显著更快。尽管最大的YOLOv9e模型在原始准确性上略占优势，但它所需的推理时间几乎是YOLO11l的两倍，这使得YOLO11成为实时推理场景中更实用的选择。

YOLOv9 发布时有一个特定的学术目标：解决数据通过深度神经网络时信息丢失的问题。其架构深受在训练过程中保留梯度信息的需求影响。

技术细节：作者：王建尧、廖弘源组织：台湾中央研究院信息科学研究所日期：2024-02-21Arxiv：https://arxiv.org/abs/2402.13616GitHub：https://github.com/WongKinYiu/yolov9文档：https://docs.ultralytics.com/models/yolov9/

YOLOv9的核心创新是可编程梯度信息 (PGI)和广义高效层聚合网络 (GELAN)。

YOLOv9 为对深度学习理论感兴趣的研究人员提供了一个极佳的案例研究，特别是在卷积神经网络中的梯度流和信息保存方面。

在YOLOv8的传承之上，YOLO11 代表了面向生产的计算机视觉的巅峰。它的设计目的不仅是为了基准分数，更是为了实际可部署性、易用性和多任务能力。

技术细节：作者：Glenn Jocher、Jing Qiu组织：Ultralytics日期：2024-09-27GitHub：https://github.com/ultralytics/ultralytics文档：https://docs.ultralytics.com/models/yolo11/

YOLO11 引入了精炼的架构，旨在最大限度地提高特征提取效率，同时最大限度地减少计算开销。它采用增强的骨干网络和颈部结构，改善了跨不同尺度的特征集成，这对于 detect 小目标至关重要。

该模型还具有改进的头部设计，可在训练期间实现更快的收敛。与以研究为中心的模型不同，YOLO11 构建在一个统一的框架中，原生支持detect、segment、分类、姿势估计和旋转框检测 (OBB)。

最显著的区别之一在于用户体验。Ultralytics YOLO11 以“开发者优先”的理念设计。它与更广泛的 Ultralytics 生态系统无缝集成，该生态系统包括用于数据标注、数据集管理和模型导出的工具。

高效的资源利用是 Ultralytics 模型的一个标志。YOLO11 经过优化，在训练期间需要更低的 CUDA 内存，相比许多基于 Transformer 的替代方案或旧版 YOLO 迭代。这使得开发人员能够在消费级硬件上训练更大的批次大小，从而加速开发周期。

此外，YOLO11 为所有任务提供现成的高质量预训练权重，确保迁移学习既快速又有效。这与研究模型形成对比，后者可能只提供主要专注于 COCO detect 的有限预训练检查点。

虽然YOLOv9主要因其在目标detect方面的成就而闻名，但YOLO11在单一框架内为广泛的计算机视觉任务提供原生支持：

在 YOLO11 中，切换任务就像更改模型权重文件一样简单（例如，从 yolo11n.pt 用于 detect 到 yolo11n-seg.pt 用于segmentation)。

以下 Python 代码展示了如何在 Ultralytics 框架内轻松加载和利用这两种模型，突出了统一的 API，该 API 简化了不同架构的测试。

YOLOv9是学术研究以及在不考虑计算成本的情况下，静态图像最大准确性为唯一优先级的场景的绝佳选择。

YOLO11 是商业应用、边缘计算和多任务系统的推荐选择。

虽然YOLOv9和YOLO11是强大的竞争者，但Ultralytics库支持各种针对特定需求定制的其他模型：

两种架构都代表了计算机视觉领域的重大成就。YOLOv9 为深度网络训练提供了宝贵的理论见解，而Ultralytics YOLO11 则将这些进步综合成一个稳健、多功能且高效的工具。对于大多数寻求构建可扩展实时应用的开发者和研究人员来说，YOLO11 在性能、易用性和全面的生态系统支持方面的平衡使其成为更优越的选择。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load the research-focused YOLOv9 model (compact version)
model_v9 = YOLO("yolov9c.pt")

# Load the production-optimized YOLO11 model (medium version)
model_11 = YOLO("yolo11m.pt")

# Run inference on a local image
# YOLO11 provides a balance of speed and accuracy ideal for real-time apps
results_11 = model_11("path/to/image.jpg")

# Display results
results_11[0].show()
```

---

## Reference for ultralytics/solutions/object_cropper.py

**URL:** https://docs.ultralytics.com/zh/reference/solutions/object_cropper/

**Contents:**
- Reference for ultralytics/solutions/object_cropper.py
- class ultralytics.solutions.object_cropper.ObjectCropper
  - method ultralytics.solutions.object_cropper.ObjectCropper.process

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/solutions/object_cropper.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class to manage the cropping of detected objects in a real-time video stream or images.

This class extends the BaseSolution class and provides functionality for cropping objects based on detected bounding boxes. The cropped images are saved to a specified directory for further analysis or usage.

Crop detected objects from the input image and save them as separate images.

**Examples:**

Example 1 (rust):
```rust
ObjectCropper(self, **kwargs: Any) -> None
```

Example 2 (json):
```json
>>> cropper = ObjectCropper()
>>> frame = cv2.imread("frame.jpg")
>>> processed_results = cropper.process(frame)
>>> print(f"Total cropped objects: {cropper.crop_idx}")
```

Example 3 (python):
```python
class ObjectCropper(BaseSolution):
    """A class to manage the cropping of detected objects in a real-time video stream or images.

    This class extends the BaseSolution class and provides functionality for cropping objects based on detected bounding
    boxes. The cropped images are saved to a specified directory for further analysis or usage.

    Attributes:
        crop_dir (str): Directory where cropped object images are stored.
        crop_idx (int): Counter for the total number of cropped objects.
        iou (float): IoU (Intersection over Union) threshold for non-maximum suppression.
        conf (float): Confidence threshold for filtering detections.

    Methods:
        process: Crop detected objects from the input image and save them to the output directory.

    Examples:
        >>> cropper = ObjectCropper()
        >>> frame = cv2.imread("frame.jpg")
        >>> processed_results = cropper.process(frame)
        >>> print(f"Total cropped objects: {cropper.crop_idx}")
    """

    def __init__(self, **kwargs: Any) -> None:
        """Initialize the ObjectCropper class for cropping objects from detected bounding boxes.

        Args:
            **kwargs (Any): Keyword arguments passed to the parent class and used for configuration including:
                - crop_dir (str): Path to the directory for saving cropped object images.
        """
        super().__init__(**kwargs)

        self.crop_dir = self.CFG["crop_dir"]  # Directory for storing cropped detections
        Path(self.crop_dir).mkdir(parents=True, exist_ok=True)
        if self.CFG["show"]:
            self.LOGGER.warning(f"show=True is not supported for ObjectCropper; saving crops to '{self.crop_dir}'.")
            self.CFG["show"] = False
        self.crop_idx = 0  # Initialize counter for total cropped objects
        self.iou = self.CFG["iou"]
        self.conf = self.CFG["conf"]
```

Example 4 (python):
```python
def process(self, im0) -> SolutionResults
```

---

## Reference for ultralytics/solutions/vision_eye.py

**URL:** https://docs.ultralytics.com/zh/reference/solutions/vision_eye/

**Contents:**
- Reference for ultralytics/solutions/vision_eye.py
- class ultralytics.solutions.vision_eye.VisionEye
  - method ultralytics.solutions.vision_eye.VisionEye.process

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/solutions/vision_eye.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class to manage object detection and vision mapping in images or video streams.

This class extends the BaseSolution class and provides functionality for detecting objects, mapping vision points, and annotating results with bounding boxes and labels.

Perform object detection, vision mapping, and annotation on the input image.

**Examples:**

Example 1 (rust):
```rust
VisionEye(self, **kwargs: Any) -> None
```

Example 2 (json):
```json
>>> vision_eye = VisionEye()
>>> frame = cv2.imread("frame.jpg")
>>> results = vision_eye.process(frame)
>>> print(f"Total detected instances: {results.total_tracks}")
```

Example 3 (python):
```python
class VisionEye(BaseSolution):
    """A class to manage object detection and vision mapping in images or video streams.

    This class extends the BaseSolution class and provides functionality for detecting objects, mapping vision points,
    and annotating results with bounding boxes and labels.

    Attributes:
        vision_point (tuple[int, int]): Coordinates (x, y) where vision will view objects and draw tracks.

    Methods:
        process: Process the input image to detect objects, annotate them, and apply vision mapping.

    Examples:
        >>> vision_eye = VisionEye()
        >>> frame = cv2.imread("frame.jpg")
        >>> results = vision_eye.process(frame)
        >>> print(f"Total detected instances: {results.total_tracks}")
    """

    def __init__(self, **kwargs: Any) -> None:
        """Initialize the VisionEye class for detecting objects and applying vision mapping.

        Args:
            **kwargs (Any): Keyword arguments passed to the parent class and for configuring vision_point.
        """
        super().__init__(**kwargs)
        # Set the vision point where the system will view objects and draw tracks
        self.vision_point = self.CFG["vision_point"]
```

Example 4 (python):
```python
def process(self, im0) -> SolutionResults
```

---

## YOLOv5 对比 YOLOv9：一项全面的技术比较

**URL:** https://docs.ultralytics.com/zh/compare/yolov5-vs-yolov9/

**Contents:**
- YOLOv5 对比 YOLOv9：一项全面的技术比较
- Ultralytics YOLOv5：多功能性行业标准
  - 架构与设计
  - 主要优势
- YOLOv9：实现最高精度的架构创新
  - 架构与创新
  - 主要优势
- 性能对比
  - 训练与资源
- 代码示例：统一接口

实时目标检测的发展以精度和效率的快速进步为标志。这一旅程中的两个重要里程碑是Ultralytics YOLOv5，一个为可用性和部署设定行业标准的模型，以及YOLOv9，一个专注于研究并推动深度学习理论边界的架构。

这项技术比较分析了它们的架构、性能指标和理想用例，以帮助开发人员和研究人员为其计算机视觉项目选择合适的工具。

自发布以来，YOLOv5 已成为全球最受欢迎的视觉 AI 模型之一。由 Ultralytics 开发，它优先考虑卓越的工程、易用性和真实世界性能。它在平衡速度和准确性的同时，通过强大的生态系统提供无缝的用户体验。

YOLOv5 利用CSPDarknet 主干网络结合PANet 颈部网络，用于高效的特征提取和聚合。其基于锚框的检测头经过高度优化以提高速度，使其适用于各种硬件。与纯粹的学术模型不同，YOLOv5 在设计时就考虑了部署，提供对iOS、Android和边缘设备的原生支持。

YOLOv9于2024年发布，专注于解决深度网络中的信息丢失问题。它引入了新颖的概念来改进数据在模型中的传播方式，在COCO等基准测试中取得了最先进的结果。

比较这两种模型时，权衡通常在于速度和绝对精度之间。YOLOv9 在 COCO 数据集上取得了更高的 mAPval 分数，这表明 PGI 和 GELAN 的有效性。然而，Ultralytics YOLOv5 在推理速度方面仍然是一个强大的竞争者，特别是在 CPU 和边缘设备上，其优化的架构表现出色。

虽然YOLOv9在准确性排行榜上名列前茅，但YOLOv5通常为实时应用提供更实用的平衡，在标准硬件上提供显著更快的推理速度（毫秒），同时保持强大的detect能力。

对于开发者而言，训练效率 往往与推理速度同等重要。Ultralytics YOLOv5 以其“即训即用”的简洁性而闻名。与更新、更复杂的架构，特别是基于 Transformer 的模型（如 RT-DETR）相比，它在训练期间通常需要更少的内存。这种较低的入门门槛使用户能够在适度的硬件配置上训练自定义模型。

YOLOv9 虽然参数效率高，但在训练时可能更耗费资源，因为用于 PGI 的辅助分支结构复杂，这些分支在推理时会被移除，但在训练时会增加开销。

Ultralytics生态系统的一个主要优势是统一的Python API。您可以通过一行代码在YOLOv5和YOLOv9之间切换，这使得在您的特定数据集上对两者进行基准测试变得异常容易。

在这些模型之间进行选择取决于您的项目优先级：

两种模型都代表了计算机视觉领域的卓越成就。Ultralytics YOLOv5 仍然是大多数开发者的务实选择，它提供了速度、可靠性和生态系统支持的无与伦比的组合。它是经过实战检验的、适用于实际部署的主力模型。另一方面，YOLOv9 展现了未来架构效率的潜力，为需要顶级精度的用户提供了解决方案。

对于那些寻求性能和多功能性方面绝对最新成果的用户，我们还推荐探索 YOLO11，它在 YOLOv5 和 YOLOv8 的优势基础上，在所有指标上均提供最先进的结果。

如果您有兴趣进一步探索，请查看 Ultralytics 生态系统中的这些相关模型：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load an Ultralytics YOLOv5 model (pre-trained on COCO)
model_v5 = YOLO("yolov5su.pt")

# Train the model on your custom data
results_v5 = model_v5.train(data="coco8.yaml", epochs=100, imgsz=640)

# Load a YOLOv9 model for comparison
model_v9 = YOLO("yolov9c.pt")

# Train YOLOv9 using the exact same API
results_v9 = model_v9.train(data="coco8.yaml", epochs=100, imgsz=640)
```

---

## YOLOX 对比 RTDETRv2：目标检测技术比较

**URL:** https://docs.ultralytics.com/zh/compare/yolox-vs-rtdetr/

**Contents:**
- YOLOX 对比 RTDETRv2：目标检测技术比较
- 性能分析：速度 vs. 准确性
- YOLOX：无锚框效率
  - 主要优势
  - 弱点
- RTDETRv2：Transformer 强大模型
  - 主要优势
  - 弱点
- 理想用例
- 为什么Ultralytics YOLO模型是卓越选择

在快速发展的计算机视觉领域，为您的项目选择合适的架构通常需要在推理速度、准确性和计算资源效率之间进行复杂的权衡。本比较探讨了两种截然不同的物体 detect 方法：YOLOX（一个高性能的无锚点 CNN）和 RTDETRv2（一个尖端的实时 detect Transformer）。

尽管YOLOX代表了YOLO家族中向无锚点方法学的重大转变，但RTDETRv2利用视觉Transformer（ViTs）的力量来捕获全局上下文，挑战了传统的卷积神经网络（CNNs）。本指南分析了它们的架构、性能指标和理想用例，以帮助您做出明智的决策。

下面的性能指标说明了这两种模型的基本设计理念。RTDETRv2 通常通过利用注意力机制来理解复杂场景，从而实现更高的平均精度 (mAP)。然而，这种 accuracy 通常伴随着更高的计算成本。YOLOX，尤其是在其较小的变体中，优先考虑低推理延迟和在标准硬件上的高效执行。

如表中所示，RTDETRv2-x实现了54.3的mAP，达到最高准确性，性能优于最大的YOLOX变体。相反，YOLOX-s在GPU硬件上展现出卓越的速度，使其在延迟敏感型应用中非常有效。

YOLOX通过转向无锚框机制和解耦detect头来改进YOLO系列。通过消除对预定义锚框的需求，YOLOX简化了训练过程，并提高了对不同对象形状的泛化能力。

作者: Zheng Ge, Songtao Liu, Feng Wang, Zeming Li, and Jian Sun组织:Megvii日期: 2021-07-18Arxiv:YOLOX：2021年超越YOLO系列

RTDETRv2 (Real-Time Detection Transformer version 2) 代表了将Transformer架构应用于实时目标检测的飞跃。它通过引入高效的混合编码器，解决了通常与 Transformer 相关的高计算成本问题。

作者： 吕文宇、赵一安、常钦尧、黄奎、王冠中、刘毅机构：百度日期： 2023-04-17 (v1), 2024-07 (v2)预印本：RT-DETRv2：带有免费增强的改进基线

这些模型之间的选择通常取决于部署环境的具体限制。

无论选择哪种模型，利用 TensorRT 或 OpenVINO 等优化框架对于在生产环境中实现实时速度至关重要。两种模型都能从 FP16 或 INT8 量化中显著受益。

尽管YOLOX和RTDETRv2表现出色，但以YOLO11为核心的Ultralytics YOLO生态系统为开发者和研究人员提供了更全面的解决方案。Ultralytics优先考虑用户体验，确保最先进的AI技术易于访问、高效且多功能。

与主要是一个检测模型的 YOLOX 不同，Ultralytics YOLO11 原生支持广泛的计算机视觉任务，包括实例分割、姿势估计、分类和旋转框检测 (OBB)。这使您能够通过单一、统一的 API 解决多个问题。

Ultralytics 软件包简化了复杂的 MLOps 世界。凭借维护良好的代码库、频繁的更新和详尽的 文档，用户可以在几分钟内完成从安装到训练的过程。

Ultralytics YOLO模型的一个关键优势是其效率。RTDETRv2等Transformer基模型以数据需求大和内存密集型著称，通常需要配备大容量显存的高端GPU进行训练。相比之下，Ultralytics YOLO模型经过优化，可以在更广泛的硬件（包括消费级GPU）上高效训练，同时占用更少的CUDA内存。这种训练效率普及了高性能AI的访问。

Ultralytics 模型在工程上旨在实现速度和准确性之间的“最佳平衡点”。对于大多数实际应用——从零售分析到安全监控——YOLO11 提供了与 Transformer 模型相当的准确性，同时保持了实时视频流所需的极快推理速度。

YOLOX 和 RTDETRv2 都为计算机视觉领域做出了重大贡献。YOLOX 对于严格受限的传统嵌入式系统来说，仍然是一个可靠的选择，而RTDETRv2 则为高端硬件突破了准确性的界限。

然而，对于大多数寻求面向未来、多功能且易于使用的解决方案的开发人员而言，Ultralytics YOLO11 脱颖而出，成为首选。它结合了低内存需求、广泛的任务支持和蓬勃发展的社区，确保您的项目建立在可靠性和性能的基础之上。

为了进一步完善您的模型选择，可以探索这些相关的技术比较：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a COCO-pretrained YOLO11n model
model = YOLO("yolo11n.pt")

# Train the model on a custom dataset
train_results = model.train(
    data="coco8.yaml",  # path to dataset YAML
    epochs=100,  # number of training epochs
    imgsz=640,  # training image size
    device="cpu",  # device to run on, i.e. device=0 or device=0,1,2,3 or device="cpu"
)

# Evaluate model performance on the validation set
metrics = model.val()
```

---

## YOLOv8 与 DAMO-YOLO：全面技术对比

**URL:** https://docs.ultralytics.com/zh/compare/yolov8-vs-damo-yolo/

**Contents:**
- YOLOv8 与 DAMO-YOLO：全面技术对比
- 执行摘要
- 架构创新
  - Ultralytics YOLOv8：精炼与统一
  - DAMO-YOLO：研究驱动的优化
- 性能指标
  - 分析
- 可用性与生态系统
  - Ultralytics 生态系统优势
  - DAMO-YOLO 可用性

在快速发展的计算机视觉领域，选择合适的物体 detect 模型对于项目成功至关重要。本比较深入探讨了 Ultralytics YOLOv8 和 DAMO-YOLO 之间的技术细微差别，这两个对该领域产生重大影响的杰出架构。尽管这两个模型都突破了速度和准确性的界限，但它们迎合了不同的需求和用户群体，从学术研究到生产级部署。

YOLOv8由Ultralytics开发，代表了YOLO家族中一个多功能、以用户为中心的演进。它于2023年初推出，优先考虑一个支持多任务（detect、segment、分类、姿势估计和旋转框检测）的统一框架，并由一个强大且维护良好的生态系统提供支持。

DAMO-YOLO，由阿里巴巴集团于 2022 年底发布，重点关注源自神经架构搜索 (NAS) 和高级特征融合技术的架构创新。它主要设计用于在 GPU 上进行高吞吐量目标检测。

这两种模型的核心区别在于它们的设计理念。YOLOv8强调易用性和泛化能力，而DAMO-YOLO则针对特定性能指标进行架构优化。

YOLOv8 在其前辈的成功基础上，引入了最先进的无锚点检测头。这种解耦头独立处理目标性、分类和回归任务，从而提高了收敛速度和准确性。

DAMO-YOLO（“发现、冒险、动量和展望”）集成了多项先进研究概念，以从架构中榨取最大性能。

性能通常是工程师的决定性因素。下表详细比较了 COCO 数据集上的关键指标。

YOLOv8 模型可以使用以下方式轻松导出为多种格式，包括 ONNX、TensorRT、CoreML 和 TFLite yolo export 命令。这 模型部署 此功能确保了在多样化生产环境中的无缝集成。

研究模型与生产工具之间的差距通常由其生态系统和易用性决定。

YOLOv8 不仅仅是一个模型；它是一个综合平台的一部分。Ultralytics 生态系统提供：

DAMO-YOLO 主要是一个研究型代码库。尽管它提供了令人印象深刻的技术，但学习曲线较陡峭。用户通常需要手动配置环境并深入复杂的代码库，才能将模型适应自定义数据集。它缺乏 Ultralytics 框架中广泛的多任务支持（如 segment、姿势估计等）。

体验 Ultralytics API 的简洁性。以下代码片段演示了如何加载预训练的 YOLOv8 模型并在自定义数据集上对其进行微调。

这种直接的工作流程与DAMO-YOLO等研究型模型通常所需的配置繁琐的设置形成了对比。

两种架构都代表了计算机视觉领域的重大成就。DAMO-YOLO 引入了 ZeroHead 和 MAE-NAS 等引人注目的创新，使其成为特定高性能 GPU 任务的有力竞争者。

然而，对于绝大多数开发人员和组织而言，Ultralytics YOLOv8 仍然是卓越之选。其无与伦比的多功能性、全面的文档和充满活力的生态系统减少了采用AI的阻力。无论您是在高速公路上优化速度估算，还是在实验室中执行精细的组织分割，YOLOv8 都提供了平衡的性能和必要的工具，以高效地将您的解决方案投入生产。

比较模型是找到满足您特定需求的合适工具的最佳方式。查看以下其他比较：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLOv8 model
model = YOLO("yolov8n.pt")

# Train the model on your custom data
# The data argument points to a YAML file describing your dataset
results = model.train(data="coco8.yaml", epochs=100, imgsz=640)

# Run inference on an image
results = model("path/to/image.jpg")
```

---

## 使用 Ultralytics YOLO11 的安全警报系统项目

**URL:** https://docs.ultralytics.com/zh/guides/security-alarm-system/

**Contents:**
- 使用 Ultralytics YOLO11 的安全警报系统项目
    - 收到的邮件示例
  - SecurityAlarm 参数
- 工作原理
- 常见问题
  - Ultralytics YOLO11 如何提高安全警报系统的准确性？
  - 是否可以将 Ultralytics YOLO11 与我现有的安全基础设施集成？
  - 运行 Ultralytics YOLO11 需要哪些存储要求？
  - 与其他目标检测模型（如 Faster R-CNN 或 SSD）相比，Ultralytics YOLO11 有何不同？
  - 如何使用 Ultralytics YOLO11 降低安全系统中误报的频率？

安全警报系统项目采用 Ultralytics YOLO11，集成了先进的计算机视觉功能，以加强安全措施。YOLO11 由 Ultralytics 开发，提供实时目标检测，使系统能够迅速识别并响应潜在的安全威胁。该项目具有以下几个优点：

观看： 使用 Ultralytics YOLO11 的安全警报系统 + 解决方案 目标检测

使用 Ultralytics YOLO 的安全警报系统

当您运行代码时，如果检测到任何对象，您将收到一封电子邮件通知。通知会立即发送，不会重复。您可以根据项目需求自定义代码。

这是一个包含以下内容的表格 SecurityAlarm 参数：

字段 SecurityAlarm 解决方案支持各种 track 参数：

安全警报系统使用 对象跟踪 监控视频流并 detect 潜在的安全威胁。当系统 detect 到超出指定阈值（由 records 参数），它会自动发送一封带有图像附件的电子邮件通知，显示检测到的对象。

该系统利用 SecurityAlarm 类，该类提供以下方法：

此实现非常适合家庭安全、零售监控和其他监控应用，在这些应用中，立即通知检测到的对象至关重要。

Ultralytics YOLO11 通过提供高精度、实时的对象检测来增强安全警报系统。其先进的算法显著减少了误报，确保系统仅响应真正的威胁。这种更高的可靠性可以与现有的安全基础设施无缝集成，从而提升整体监控质量。

是的，Ultralytics YOLO11 可以与您现有的安全基础设施无缝集成。该系统支持各种模式，并为定制提供了灵活性，使您可以通过高级对象检测功能增强现有设置。有关在您的项目中集成 YOLO11 的详细说明，请访问 集成部分。

在标准设置上运行 Ultralytics YOLO11 通常需要大约 5GB 的可用磁盘空间。这包括存储 YOLO11 模型和任何其他依赖项的空间。对于基于云的解决方案，Ultralytics HUB 提供高效的项目管理和数据集处理，从而可以优化存储需求。了解有关专业计划的更多信息，以获取包括扩展存储在内的增强功能。

Ultralytics YOLO11 凭借其实时检测能力和更高的准确性，优于 Faster R-CNN 或 SSD 等模型。其独特的架构使其能够更快地处理图像，而不会影响 精度，使其成为安全警报系统等时间敏感型应用的理想选择。有关对象检测模型的全面比较，您可以浏览我们的 指南。

为了减少误报，请确保您的 Ultralytics YOLO11 模型经过充分训练，并使用多样化且注释良好的数据集。微调超参数并定期使用新数据更新模型可以显着提高检测准确性。详细的 超参数调整 技术可以在我们的 超参数调整指南 中找到。

**Examples:**

Example 1 (python):
```python
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "Error reading video file"

# Video writer
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))
video_writer = cv2.VideoWriter("security_output.avi", cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))

from_email = "abc@gmail.com"  # the sender email address
password = "---- ---- ---- ----"  # 16-digits password generated via: https://myaccount.google.com/apppasswords
to_email = "xyz@gmail.com"  # the receiver email address

# Initialize security alarm object
securityalarm = solutions.SecurityAlarm(
    show=True,  # display the output
    model="yolo11n.pt",  # e.g., yolo11s.pt, yolo11m.pt
    records=1,  # total detections count to send an email
)

securityalarm.authenticate(from_email, password, to_email)  # authenticate the email server

# Process video
while cap.isOpened():
    success, im0 = cap.read()

    if not success:
        print("Video frame is empty or video processing has been successfully completed.")
        break

    results = securityalarm(im0)

    # print(results)  # access the output

    video_writer.write(results.plot_im)  # write the processed frame.

cap.release()
video_writer.release()
cv2.destroyAllWindows()  # destroy all opened windows
```

---

## Reference for ultralytics/trackers/utils/gmc.py

**URL:** https://docs.ultralytics.com/zh/reference/trackers/utils/gmc/

**Contents:**
- Reference for ultralytics/trackers/utils/gmc.py
- class ultralytics.trackers.utils.gmc.GMC
  - method ultralytics.trackers.utils.gmc.GMC.apply
  - method ultralytics.trackers.utils.gmc.GMC.apply_ecc
  - method ultralytics.trackers.utils.gmc.GMC.apply_features
  - method ultralytics.trackers.utils.gmc.GMC.apply_sparseoptflow
  - method ultralytics.trackers.utils.gmc.GMC.reset_params

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/trackers/utils/gmc.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Generalized Motion Compensation (GMC) class for tracking and object detection in video frames.

This class provides methods for tracking and detecting objects based on several tracking algorithms including ORB, SIFT, ECC, and Sparse Optical Flow. It also supports downscaling of frames for computational efficiency.

Estimate a 2×3 motion compensation warp for a frame.

Apply the ECC (Enhanced Correlation Coefficient) algorithm to a raw frame for motion compensation.

Apply feature-based methods like ORB or SIFT to a raw frame.

Apply Sparse Optical Flow method to a raw frame.

Reset the internal parameters including previous frame, keypoints, and descriptors.

**Examples:**

Example 1 (typescript):
```typescript
GMC(self, method: str = "sparseOptFlow", downscale: int = 2) -> None
```

Example 2 (unknown):
```unknown
Create a GMC object and apply it to a frame
>>> gmc = GMC(method="sparseOptFlow", downscale=2)
>>> frame = np.random.randint(0, 255, (480, 640, 3), dtype=np.uint8)
>>> warp = gmc.apply(frame)
>>> print(warp.shape)
(2, 3)
```

Example 3 (python):
```python
class GMC:
    """Generalized Motion Compensation (GMC) class for tracking and object detection in video frames.

    This class provides methods for tracking and detecting objects based on several tracking algorithms including ORB,
    SIFT, ECC, and Sparse Optical Flow. It also supports downscaling of frames for computational efficiency.

    Attributes:
        method (str): The tracking method to use. Options include 'orb', 'sift', 'ecc', 'sparseOptFlow', 'none'.
        downscale (int): Factor by which to downscale the frames for processing.
        prevFrame (np.ndarray): Previous frame for tracking.
        prevKeyPoints (list): Keypoints from the previous frame.
        prevDescriptors (np.ndarray): Descriptors from the previous frame.
        initializedFirstFrame (bool): Flag indicating if the first frame has been processed.

    Methods:
        apply: Apply the chosen method to a raw frame and optionally use provided detections.
        apply_ecc: Apply the ECC algorithm to a raw frame.
        apply_features: Apply feature-based methods like ORB or SIFT to a raw frame.
        apply_sparseoptflow: Apply the Sparse Optical Flow method to a raw frame.
        reset_params: Reset the internal parameters of the GMC object.

    Examples:
        Create a GMC object and apply it to a frame
        >>> gmc = GMC(method="sparseOptFlow", downscale=2)
        >>> frame = np.random.randint(0, 255, (480, 640, 3), dtype=np.uint8)
        >>> warp = gmc.apply(frame)
        >>> print(warp.shape)
        (2, 3)
    """

    def __init__(self, method: str = "sparseOptFlow", downscale: int = 2) -> None:
        """Initialize a Generalized Motion Compensation (GMC) object with tracking method and downscale factor.

        Args:
            method (str): The tracking method to use. Options include 'orb', 'sift', 'ecc', 'sparseOptFlow', 'none'.
            downscale (int): Downscale factor for processing frames.
        """
        super().__init__()

        self.method = method
        self.downscale = max(1, downscale)

        if self.method == "orb":
            self.detector = cv2.FastFeatureDetector_create(20)
            self.extractor = cv2.ORB_create()
            self.matcher = cv2.BFMatcher(cv2.NORM_HAMMING)

        elif self.method == "sift":
            self.detector = cv2.SIFT_create(nOctaveLayers=3, contrastThreshold=0.02, edgeThreshold=20)
            self.extractor = cv2.SIFT_create(nOctaveLayers=3, contrastThreshold=0.02, edgeThreshold=20)
            self.matcher = cv2.BFMatcher(cv2.NORM_L2)

        elif self.method == "ecc":
            number_of_iterations = 5000
            termination_eps = 1e-6
            self.warp_mode = cv2.MOTION_EUCLIDEAN
            self.criteria = (cv2.TERM_CRITERIA_EPS | cv2.TERM_CRITERIA_COUNT, number_of_iterations, termination_eps)

        elif self.method == "sparseOptFlow":
            self.feature_params = dict(
                maxCorners=1000, qualityLevel=0.01, minDistance=1, blockSize=3, useHarrisDetector=False, k=0.04
            )

        elif self.method in {"none", "None", None}:
            self.method = None
        else:
            raise ValueError(f"Unknown GMC method: {method}")

        self.prevFrame = None
        self.prevKeyPoints = None
        self.prevDescriptors = None
        self.initializedFirstFrame = False
```

Example 4 (python):
```python
def apply(self, raw_frame: np.ndarray, detections: list | None = None) -> np.ndarray
```

---

## RTDETRv2 与 YOLOv6-3.0：高精度 Transformer 兼顾工业速度

**URL:** https://docs.ultralytics.com/zh/compare/rtdetr-vs-yolov6/

**Contents:**
- RTDETRv2 与 YOLOv6-3.0：高精度 Transformer 兼顾工业速度
- RTDETRv2：以视觉 Transformer 突破界限
  - 架构创新
  - 优势与劣势
- YOLOv6-3.0：工业速度先锋
  - 针对效率进行优化
  - 优势与劣势
- 性能分析：速度与精度
  - 训练与资源需求
- 理想平衡：Ultralytics 的优势

选择最佳物体检测架构通常需要在绝对精度和推理延迟之间权衡利弊。本技术比较探讨了RTDETRv2 和YOLOv6.0，前者是专为高精度任务设计的Transformer视觉Transformer的模型，后者是专为工业速度和效率设计的基于 CNN 的检测器。通过分析它们的架构、性能指标和部署特点，我们可以帮助您确定计算机视觉应用的最佳解决方案。

RTDETRv2 (Real-Time Detection Transformer v2) 代表了目标检测领域的一个重大演进，利用 Transformer 的强大能力来捕获图像中的全局上下文。与处理局部特征的传统 CNN 不同，RTDETRv2 利用自注意力机制来理解远距离对象之间的关系，使其在复杂场景中非常有效。

作者： 吕文宇、赵一安、常钦尧、黄奎、王冠中、刘毅机构：百度日期： 2023-04-17 (初版), 2024-07-24 (v2)预印本：RT-DETR：DETR 在实时目标 detect 方面超越 YOLOGitHub：RT-DETR 仓库文档：RTDETRv2 文档

RTDETRv2 的架构是一种混合设计。它采用标准的 CNN 主干网络（通常是 ResNet 或 HGNet）进行初始特征提取，然后是一个 Transformer 编码器-解码器。这种结构使模型能够有效处理多尺度特征，同时消除了对锚框和 非极大值抑制 (NMS) 等手工设计组件的需求。

The Vision Transformer (ViT)组件在RTDETRv2中擅长解决拥挤场景中的歧义。通过同时分析整个图像上下文，模型减少了由遮挡或背景杂乱引起的误报。

YOLOv6-3.0 由美团开发，专注于工业应用的需求：即低延迟和高吞吐量。它改进了经典的单阶段目标检测器范式，以最大限度地提高从边缘设备到 GPU 等硬件上的效率。

作者: Chuyi Li, Lulu Li, Yifei Geng, Hongliang Jiang, Meng Cheng, Bo Zhang, Zaidan Ke, Xiaoming Xu, and Xiangxiang Chu机构:美团日期: 2023-01-13Arxiv:YOLOv6 v3.0：全面重载GitHub:YOLOv6 仓库文档:Ultralytics YOLOv6 文档

YOLOv6-3.0融合了“硬件感知”的设计理念。它采用高效的重参数化骨干网络（RepVGG风格），在推理时将网络简化为简单的3x3卷积堆栈，消除了多分支的复杂性。此外，它在训练期间采用自蒸馏技术，以提高准确性而不会增加推理成本。

RTDETRv2 和 YOLOv6-3.0 之间的选择通常取决于部署环境的具体限制。RTDETRv2 在需要最高准确性的场景中占据主导地位，而 YOLOv6-3.0 则在原始速度和效率方面更胜一筹。

下表对比了关键指标。请注意，YOLOv6-3.0 在相似的模型规模下实现了更低的延迟（更快的速度），而 RTDETRv2 则以计算强度 (FLOPs) 为代价追求更高的 mAP 分数。

尽管 RTDETRv2 和 YOLOv6-3.0 在各自的细分领域表现出色，但 Ultralytics YOLO11 提供了一个统一的解决方案，解决了两者的局限性。它结合了 CNN 的易用性和速度，以及可与 Transformer 精度媲美的架构改进。

为什么开发者和研究人员越来越青睐Ultralytics模型：

体验 Ultralytics API 的简洁性。以下示例演示了如何加载预训练模型并对图像运行推理：

RTDETRv2 和 YOLOv6-3.0 都是计算机视觉史上的重要里程碑。RTDETRv2 是研究和将 精度 作为绝对优先事项（不考虑计算成本）的场景的绝佳选择。YOLOv6-3.0 很好地服务于工业领域，为受控环境提供了极高的速度。

然而，对于大多数需要强大、多功能且易于部署的解决方案的实际应用而言，Ultralytics YOLO11 脱颖而出，成为卓越之选。它结合了领先的性能、低内存占用和蓬勃发展的生态系统，使开发人员能够自信、快速地从原型阶段迈向生产阶段。

探索不同架构的比较，以找到最适合您项目的方案：

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a pre-trained YOLO11 model (n=nano, s=small, m=medium, l=large, x=xlarge)
model = YOLO("yolo11n.pt")

# Run inference on a local image
results = model("path/to/image.jpg")

# Process results
for result in results:
    result.show()  # Display results on screen
    result.save(filename="result.jpg")  # Save results to disk
```

---
