# Yolo - Segmentation

**Pages:** 4

---

## Reference for ultralytics/data/augment.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/data/augment/

**Contents:**
- Reference for ultralytics/data/augment.py
- class ultralytics.data.augment.BaseTransform
  - method ultralytics.data.augment.BaseTransform.__call__
  - method ultralytics.data.augment.BaseTransform.apply_image
  - method ultralytics.data.augment.BaseTransform.apply_instances
  - method ultralytics.data.augment.BaseTransform.apply_semantic
- class ultralytics.data.augment.Compose
  - method ultralytics.data.augment.Compose.__call__
  - method ultralytics.data.augment.Compose.__getitem__
  - method ultralytics.data.augment.Compose.__repr__

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/data/augment.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Base class for image transformations in the Ultralytics library.

This class serves as a foundation for implementing various image processing operations, designed to be compatible with both classification and semantic segmentation tasks.

This constructor sets up the base transformation object, which can be extended for specific image processing tasks. It is designed to be compatible with both classification and semantic segmentation.

Apply all label transformations to an image, instances, and semantic masks.

This method orchestrates the application of various transformations defined in the BaseTransform class to the input labels. It sequentially calls the apply_image and apply_instances methods to process the image and object instances, respectively.

Apply image transformations to labels.

This method is intended to be overridden by subclasses to implement specific image transformation logic. In its base form, it returns the input labels unchanged.

Apply transformations to object instances in labels.

This method is responsible for applying various transformations to object instances within the given labels. It is designed to be overridden by subclasses to implement specific instance transformation logic.

Apply semantic segmentation transformations to an image.

This method is intended to be overridden by subclasses to implement specific semantic segmentation transformations. In its base form, it does not perform any operations.

A class for composing multiple image transformations.

Apply a series of transformations to input data.

This method sequentially applies each transformation in the Compose object's transforms to the input data.

Retrieve a specific transform or a set of transforms using indexing.

Return a string representation of the Compose object.

Set one or more transforms in the composition using indexing.

Append a new transform to the existing list of transforms.

Insert a new transform at a specified index in the existing list of transforms.

Convert the list of transforms to a standard Python list.

Base class for mix transformations like Cutmix, MixUp and Mosaic.

This class provides a foundation for implementing mix transformations on datasets. It handles the probability-based application of transforms and manages the mixing of multiple images and labels.

This class serves as a base for implementing mix transformations in image processing pipelines.

Apply pre-processing transforms and cutmix/mixup/mosaic transforms to labels data.

This method determines whether to apply the mix transform based on a probability factor. If applied, it selects additional images, applies pre-transforms if specified, and then performs the mix transform.

Apply CutMix, MixUp or Mosaic augmentation to the label dictionary.

This method should be implemented by subclasses to perform specific mix transformations like CutMix, MixUp or Mosaic. It modifies the input label dictionary in-place with the augmented data.

Update label text and class IDs for mixed labels in image augmentation.

This method processes the 'texts' and 'cls' fields of the input labels dictionary and any mixed labels, creating a unified set of text labels and updating class IDs accordingly.

Get a list of shuffled indexes for mosaic augmentation.

Bases: BaseMixTransform

Mosaic augmentation for image datasets.

This class performs mosaic augmentation by combining multiple (4 or 9) images into a single mosaic image. The augmentation is applied to a dataset with a given probability.

This class performs mosaic augmentation by combining multiple (4 or 9) images into a single mosaic image. The augmentation is applied to a dataset with a given probability.

Concatenate and process labels for mosaic augmentation.

This method combines labels from multiple images used in mosaic augmentation, clips instances to the mosaic border, and removes zero-area boxes.

Apply mosaic augmentation to the input image and labels.

This method combines multiple images (3, 4, or 9) into a single mosaic image based on the 'n' attribute. It ensures that rectangular annotations are not present and that there are other images available for mosaic augmentation.

Create a 1x3 image mosaic by combining three images.

This method arranges three images in a horizontal layout, with the main image in the center and two additional images on either side. It's part of the Mosaic augmentation technique used in object detection.

Create a 2x2 image mosaic from four input images.

This method combines four images into a single mosaic image by placing them in a 2x2 grid. It also updates the corresponding labels for each image in the mosaic.

Create a 3x3 image mosaic from the input image and eight additional images.

This method combines nine images into a single mosaic image. The input image is placed at the center, and eight additional images from the dataset are placed around it in a 3x3 grid pattern.

Update label coordinates with padding values.

This method adjusts the bounding box coordinates of object instances in the labels by adding padding values. It also denormalizes the coordinates if they were previously normalized.

Return a list of random indexes from the dataset for mosaic augmentation.

This method selects random image indexes either from a buffer or from the entire dataset, depending on the 'buffer' parameter. It is used to choose images for creating mosaic augmentations.

Bases: BaseMixTransform

Apply MixUp augmentation to image datasets.

This class implements the MixUp augmentation technique as described in the paper mixup: Beyond Empirical Risk Minimization. MixUp combines two images and their labels using a random weight.

MixUp is an image augmentation technique that combines two images by taking a weighted sum of their pixel values and labels. This implementation is designed for use with the Ultralytics YOLO framework.

Apply MixUp augmentation to the input labels.

This method implements the MixUp augmentation technique as described in the paper "mixup: Beyond Empirical Risk Minimization" (https://arxiv.org/abs/1710.09412).

Bases: BaseMixTransform

Apply CutMix augmentation to image datasets as described in the paper https://arxiv.org/abs/1905.04899.

CutMix combines two images by replacing a random rectangular region of one image with the corresponding region from another image, and adjusts the labels proportionally to the area of the mixed region.

Apply CutMix augmentation to the input labels.

Generate random bounding box coordinates for the cut region.

Implement random perspective and affine transformations on images and corresponding annotations.

This class applies random rotations, translations, scaling, shearing, and perspective transformations to images and their associated bounding boxes, segments, and keypoints. It can be used as part of an augmentation pipeline for object detection and instance segmentation tasks.

This class implements random perspective and affine transformations on images and corresponding bounding boxes, segments, and keypoints. Transformations include rotation, translation, scaling, and shearing.

Apply random perspective and affine transformations to an image and its associated labels.

This method performs a series of transformations including rotation, translation, scaling, shearing, and perspective distortion on the input image and adjusts the corresponding bounding boxes, segments, and keypoints accordingly.

'labels' arg must include: - 'img' (np.ndarray): The input image. - 'cls' (np.ndarray): Class labels. - 'instances' (Instances): Object instances with bounding boxes, segments, and keypoints. May include: - 'mosaic_border' (tuple[int, int]): Border size for mosaic augmentation.

Apply a sequence of affine transformations centered around the image center.

This function performs a series of geometric transformations on the input image, including translation, perspective change, rotation, scaling, and shearing. The transformations are applied in a specific order to maintain consistency.

Apply affine transformation to bounding boxes.

This function applies an affine transformation to a set of bounding boxes using the provided transformation matrix.

Apply affine transformation to keypoints.

This method transforms the input keypoints using the provided affine transformation matrix. It handles perspective rescaling if necessary and updates the visibility of keypoints that fall outside the image boundaries after transformation.

Apply affine transformations to segments and generate new bounding boxes.

This function applies affine transformations to input segments and generates new bounding boxes based on the transformed segments. It clips the transformed segments to fit within the new bounding boxes.

Compute candidate boxes for further processing based on size and aspect ratio criteria.

This method compares boxes before and after augmentation to determine if they meet specified thresholds for width, height, aspect ratio, and area. It's used to filter out boxes that have been overly distorted or reduced by the augmentation process.

Randomly adjust the Hue, Saturation, and Value (HSV) channels of an image.

This class applies random HSV augmentation to images within predefined limits set by hgain, sgain, and vgain.

This class applies random adjustments to the HSV channels of an image within specified limits.

Apply random HSV augmentation to an image within predefined limits.

This method modifies the input image by randomly adjusting its Hue, Saturation, and Value (HSV) channels. The adjustments are made within the limits set by hgain, sgain, and vgain during initialization.

Apply a random horizontal or vertical flip to an image with a given probability.

This class performs random image flipping and updates corresponding instance annotations such as bounding boxes and keypoints.

This class applies a random horizontal or vertical flip to an image with a given probability. It also updates any instances (bounding boxes, keypoints, etc.) accordingly.

Apply random flip to an image and update any instances like bounding boxes or keypoints accordingly.

This method randomly flips the input image either horizontally or vertically based on the initialized probability and direction. It also updates the corresponding instances (bounding boxes, keypoints) to match the flipped image.

Resize image and padding for detection, instance segmentation, pose.

This class resizes and pads images to a specified shape while preserving aspect ratio. It also updates corresponding labels and bounding boxes.

This class is designed to resize and pad images for object detection, instance segmentation, and pose estimation tasks. It supports various resizing modes including auto-sizing, scale-fill, and letterboxing.

Resize and pad an image for object detection, instance segmentation, or pose estimation tasks.

This method applies letterboxing to the input image, which involves resizing the image while maintaining its aspect ratio and adding padding to fit the new shape. It also updates any associated labels accordingly.

Update labels after applying letterboxing to an image.

This method modifies the bounding box coordinates of instances in the labels to account for resizing and padding applied during letterboxing.

Bases: BaseMixTransform

CopyPaste class for applying Copy-Paste augmentation to image datasets.

This class implements the Copy-Paste augmentation technique as described in the paper "Simple Copy-Paste is a Strong Data Augmentation Method for Instance Segmentation" (https://arxiv.org/abs/2012.07177). It combines objects from different images to create new training samples.

Apply Copy-Paste augmentation to an image and its labels.

Apply Copy-Paste augmentation to combine objects from another image into the current image.

Apply Copy-Paste augmentation to combine objects from another image into the current image.

Albumentations transformations for image augmentation.

This class applies various image transformations using the Albumentations library. It includes operations such as Blur, Median Blur, conversion to grayscale, Contrast Limited Adaptive Histogram Equalization (CLAHE), random changes in brightness and contrast, RandomGamma, and image quality reduction through compression.

This class applies various image augmentations using the Albumentations library, including Blur, Median Blur, conversion to grayscale, Contrast Limited Adaptive Histogram Equalization, random changes of brightness and contrast, RandomGamma, and image quality reduction through compression.

Apply Albumentations transformations to input labels.

This method applies a series of image augmentations using the Albumentations library. It can perform both spatial and non-spatial transformations on the input image and its corresponding labels.

A class for formatting image annotations for object detection, instance segmentation, and pose estimation tasks.

This class standardizes image and instance annotations to be used by the collate_fn in PyTorch DataLoader.

This class standardizes image and instance annotations for object detection, instance segmentation, and pose estimation tasks, preparing them for use in PyTorch DataLoader's collate_fn.

Format image annotations for object detection, instance segmentation, and pose estimation tasks.

This method standardizes the image and instance annotations to be used by the collate_fn in PyTorch DataLoader. It processes the input labels dictionary, converting annotations to the specified format and applying normalization if required.

Format an image for YOLO from a Numpy array to a PyTorch tensor.

This function performs the following operations: 1. Ensures the image has 3 dimensions (adds a channel dimension if needed). 2. Transposes the image from HWC to CHW format. 3. Optionally flips the color channels from RGB to BGR. 4. Converts the image to a contiguous array. 5. Converts the Numpy array to a PyTorch tensor.

Convert polygon segments to bitmap masks.

Create visual prompts from bounding boxes or masks for model input.

Process labels to create visual prompts.

Generate visual masks based on bounding boxes or masks.

Create binary masks from bounding boxes.

Randomly sample positive and negative texts and update class indices accordingly.

This class is responsible for sampling texts from a given set of class texts, including both positive (present in the image) and negative (not present in the image) samples. It updates the class indices to reflect the sampled texts and can optionally pad the text list to a fixed length.

This class is designed to randomly sample positive texts and negative texts, and update the class indices accordingly to the number of samples. It can be used for text-based object detection tasks.

Randomly sample positive and negative texts and update class indices accordingly.

This method samples positive texts based on the existing class labels in the image, and randomly selects negative texts from the remaining classes. It then updates the class indices to match the new sampled text order.

A class for resizing and padding images for classification tasks.

This class is designed to be part of a transformation pipeline, e.g., T.Compose([LetterBox(size), ToTensor()]). It resizes and pads images to a specified size while maintaining the original aspect ratio.

This class is designed to be part of a transformation pipeline for image classification tasks. It resizes and pads images to a specified size while maintaining the original aspect ratio.

Resize and pad an image using the letterbox method.

This method resizes the input image to fit within the specified dimensions while maintaining its aspect ratio, then pads the resized image to match the target size.

Apply center cropping to images for classification tasks.

This class performs center cropping on input images, resizing them to a specified size while maintaining the aspect ratio. It is designed to be part of a transformation pipeline, e.g., T.Compose([CenterCrop(size), ToTensor()]).

This class is designed to be part of a transformation pipeline, e.g., T.Compose([CenterCrop(size), ToTensor()]). It performs a center crop on input images to a specified size.

Apply center cropping to an input image.

This method resizes and crops the center of the image using a letterbox method. It maintains the aspect ratio of the original image while fitting it into the specified dimensions.

Convert an image from a numpy array to a PyTorch tensor.

This class is designed to be part of a transformation pipeline, e.g., T.Compose([LetterBox(size), ToTensor()]).

This class is designed to be used as part of a transformation pipeline for image preprocessing in the Ultralytics YOLO framework. It converts numpy arrays or PIL Images to PyTorch tensors, with an option for half-precision (float16) conversion.

The input image is expected to be in BGR format with shape (H, W, C). The output tensor will be in RGB format with shape (C, H, W), normalized to [0, 1].

Transform an image from a numpy array to a PyTorch tensor.

This method converts the input image from a numpy array to a PyTorch tensor, applying optional half-precision conversion and normalization. The image is transposed from HWC to CHW format and the color channels are reversed from BGR to RGB.

Apply a series of image transformations for training.

This function creates a composition of image augmentation techniques to prepare images for YOLO training. It includes operations such as mosaic, copy-paste, random perspective, mixup, and various color adjustments.

Create a composition of image transforms for classification tasks.

This function generates a sequence of torchvision transforms suitable for preprocessing images for classification models during evaluation or inference. The transforms include resizing, center cropping, conversion to tensor, and normalization.

Create a composition of image augmentation transforms for classification tasks.

This function generates a set of image transformations suitable for training classification models. It includes options for resizing, flipping, color jittering, auto augmentation, and random erasing.

**Examples:**

Example 1 (rust):
```rust
BaseTransform(self) -> None
```

Example 2 (json):
```json
>>> transform = BaseTransform()
>>> labels = {"image": np.array(...), "instances": [...], "semantic": np.array(...)}
>>> transformed_labels = transform(labels)
```

Example 3 (python):
```python
class BaseTransform:
    """Base class for image transformations in the Ultralytics library.

    This class serves as a foundation for implementing various image processing operations, designed to be compatible
    with both classification and semantic segmentation tasks.

    Methods:
        apply_image: Apply image transformations to labels.
        apply_instances: Apply transformations to object instances in labels.
        apply_semantic: Apply semantic segmentation to an image.
        __call__: Apply all label transformations to an image, instances, and semantic masks.

    Examples:
        >>> transform = BaseTransform()
        >>> labels = {"image": np.array(...), "instances": [...], "semantic": np.array(...)}
        >>> transformed_labels = transform(labels)
    """

    def __init__(self) -> None:
        """Initialize the BaseTransform object.

        This constructor sets up the base transformation object, which can be extended for specific image processing
        tasks. It is designed to be compatible with both classification and semantic segmentation.
        """
        pass
```

Example 4 (python):
```python
def __call__(self, labels)
```

---

## Reference for ultralytics/solutions/instance_segmentation.py

**URL:** https://docs.ultralytics.com/zh/reference/solutions/instance_segmentation/

**Contents:**
- Reference for ultralytics/solutions/instance_segmentation.py
- class ultralytics.solutions.instance_segmentation.InstanceSegmentation
  - method ultralytics.solutions.instance_segmentation.InstanceSegmentation.process

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/solutions/instance_segmentation.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class to manage instance segmentation in images or video streams.

This class extends the BaseSolution class and provides functionality for performing instance segmentation, including drawing segmented masks with bounding boxes and labels.

Perform instance segmentation on the input image and annotate the results.

**Examples:**

Example 1 (rust):
```rust
InstanceSegmentation(self, **kwargs: Any) -> None
```

Example 2 (json):
```json
>>> segmenter = InstanceSegmentation()
>>> frame = cv2.imread("frame.jpg")
>>> results = segmenter.process(frame)
>>> print(f"Total segmented instances: {results.total_tracks}")
```

Example 3 (python):
```python
class InstanceSegmentation(BaseSolution):
    """A class to manage instance segmentation in images or video streams.

    This class extends the BaseSolution class and provides functionality for performing instance segmentation, including
    drawing segmented masks with bounding boxes and labels.

    Attributes:
        model (str): The segmentation model to use for inference.
        line_width (int): Width of the bounding box and text lines.
        names (dict[int, str]): Dictionary mapping class indices to class names.
        clss (list[int]): List of detected class indices.
        track_ids (list[int]): List of track IDs for detected instances.
        masks (list[np.ndarray]): List of segmentation masks for detected instances.
        show_conf (bool): Whether to display confidence scores.
        show_labels (bool): Whether to display class labels.
        show_boxes (bool): Whether to display bounding boxes.

    Methods:
        process: Process the input image to perform instance segmentation and annotate results.
        extract_tracks: Extract tracks including bounding boxes, classes, and masks from model predictions.

    Examples:
        >>> segmenter = InstanceSegmentation()
        >>> frame = cv2.imread("frame.jpg")
        >>> results = segmenter.process(frame)
        >>> print(f"Total segmented instances: {results.total_tracks}")
    """

    def __init__(self, **kwargs: Any) -> None:
        """Initialize the InstanceSegmentation class for detecting and annotating segmented instances.

        Args:
            **kwargs (Any): Keyword arguments passed to the BaseSolution parent class including:
                - model (str): Model name or path, defaults to "yolo11n-seg.pt".
        """
        kwargs["model"] = kwargs.get("model", "yolo11n-seg.pt")
        super().__init__(**kwargs)

        self.show_conf = self.CFG.get("show_conf", True)
        self.show_labels = self.CFG.get("show_labels", True)
        self.show_boxes = self.CFG.get("show_boxes", True)
```

Example 4 (python):
```python
def process(self, im0) -> SolutionResults
```

---

## 使用 Ultralytics YOLO11 🚀 进行实例分割和追踪

**URL:** https://docs.ultralytics.com/zh/guides/instance-segmentation-and-tracking/

**Contents:**
- 使用 Ultralytics YOLO11 🚀 进行实例分割和追踪
- 什么是实例分割？
- 示例
  - InstanceSegmentation 参数
- 实例分割的应用
  - 废物管理与回收
  - 自动驾驶车辆
  - 医学影像
  - 建筑工地监控
- 注意

实例分割是一项计算机视觉任务，涉及在像素级别识别和勾勒图像中的各个对象。与仅按类别对像素进行分类的语义分割不同，实例分割会唯一地标记并精确地描绘每个对象实例，这对于需要详细空间理解的应用（如医学成像、自动驾驶和工业自动化）至关重要。

Ultralytics YOLO11 提供了强大的实例分割功能，可以在保持 YOLO 模型的速度和效率的同时，实现精确的目标边界检测。

Ultralytics 软件包中有两种类型的实例分割跟踪可用：

使用类别对象进行实例分割： 每个类别对象都分配有唯一的颜色，以便清晰地进行视觉分离。

带有对象 track 的实例分割：每个 track 都由独特的颜色表示，便于在视频帧中轻松识别和 track。

观看： 使用 Ultralytics YOLO11 进行带有对象追踪的实例分割

使用 Ultralytics YOLO 进行实例分割

这是一个包含以下内容的表格 InstanceSegmentation 参数：

您还可以利用 track 内的参数 InstanceSegmentation 解决方案：

使用 YOLO11 进行实例分割在各个行业中都有许多实际应用：

YOLO11 可用于废物管理设施，以识别和分类不同类型的材料。该模型可以高精度地 segment 塑料垃圾、纸板、金属和其他可回收物，使自动化分拣系统能够更高效地处理废物。考虑到全球产生的 70 亿吨塑料垃圾中只有约 10% 得到回收，这一点尤为重要。

在自动驾驶汽车中，实例 segment 有助于在像素级别识别和 track 行人、车辆、交通标志及其他道路元素。这种对环境的精确理解对于导航和安全决策至关重要。YOLO11 的实时性能使其成为这些时间敏感型应用的理想选择。

实例分割可以识别并勾勒出医学扫描中的肿瘤、器官或细胞结构。YOLO11 精确描绘对象边界的能力使其对于 医学诊断 和治疗计划非常有价值。

在建筑工地，实例分割可以跟踪重型机械、工人及物料。这有助于通过监控设备位置和检测工人进入危险区域的情况来确保安全，同时优化工作流程和资源分配。

如有任何疑问，请随时在 Ultralytics Issue Section 或下面提到的讨论区中发布您的问题。

要使用 Ultralytics YOLO11 执行实例分割，请使用 YOLO11 的分割版本初始化 YOLO 模型，并通过它处理视频帧。 这是一个简化的代码示例：

了解更多关于 Ultralytics YOLO11 指南 中的实例分割。

实例分割识别并勾勒出图像中的各个对象，为每个对象提供唯一的标签和掩码。对象跟踪通过为视频帧中的对象分配一致的 ID 来扩展此功能，从而有助于持续跟踪同一对象随时间的推移。当像 YOLO11 的实现中那样组合使用时，您将获得强大的功能来分析视频中对象的运动和行为，同时保持精确的边界信息。

与其他模型（如 Mask R-CNN 或 Faster R-CNN）相比，Ultralytics YOLO11 具有实时性能、卓越的准确性和易用性。YOLO11 通过单次处理图像（单阶段检测），在保持高精度的同时显著提高了速度。它还与 Ultralytics HUB 无缝集成，使用户能够高效地管理模型、数据集和训练流程。对于需要速度和准确性的应用，YOLO11 提供了最佳平衡。

是的，Ultralytics 提供了多个适用于训练 YOLO11 实例分割模型的 datasets，包括 COCO-Seg、COCO8-Seg（用于快速测试的较小子集）、Package-Seg 和 Crack-Seg。这些数据集附带实例分割任务所需的像素级标注。对于更专业的应用，您还可以按照 Ultralytics 格式创建自定义数据集。完整的 dataset 信息和使用说明可在 Ultralytics Datasets 文档中找到。

**Examples:**

Example 1 (julia):
```julia
# Instance segmentation using Ultralytics YOLO11
yolo solutions isegment show=True

# Pass a source video
yolo solutions isegment source="path/to/video.mp4"

# Monitor the specific classes
yolo solutions isegment classes="[0, 5]"
```

Example 2 (python):
```python
import cv2

from ultralytics import solutions

cap = cv2.VideoCapture("path/to/video.mp4")
assert cap.isOpened(), "Error reading video file"

# Video writer
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))
video_writer = cv2.VideoWriter("isegment_output.avi", cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))

# Initialize instance segmentation object
isegment = solutions.InstanceSegmentation(
    show=True,  # display the output
    model="yolo11n-seg.pt",  # model="yolo11n-seg.pt" for object segmentation using YOLO11.
    # classes=[0, 2],  # segment specific classes, e.g., person and car with the pretrained model.
)

# Process video
while cap.isOpened():
    success, im0 = cap.read()

    if not success:
        print("Video frame is empty or video processing has been successfully completed.")
        break

    results = isegment(im0)

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

# Video writer
w, h, fps = (int(cap.get(x)) for x in (cv2.CAP_PROP_FRAME_WIDTH, cv2.CAP_PROP_FRAME_HEIGHT, cv2.CAP_PROP_FPS))
video_writer = cv2.VideoWriter("instance-segmentation.avi", cv2.VideoWriter_fourcc(*"mp4v"), fps, (w, h))

# Init InstanceSegmentation
isegment = solutions.InstanceSegmentation(
    show=True,  # display the output
    model="yolo11n-seg.pt",  # model="yolo11n-seg.pt" for object segmentation using YOLO11.
)

# Process video
while cap.isOpened():
    success, im0 = cap.read()
    if not success:
        print("Video frame is empty or processing is complete.")
        break
    results = isegment(im0)
    video_writer.write(results.plot_im)

cap.release()
video_writer.release()
cv2.destroyAllWindows()
```

---

## Reference for ultralytics/nn/modules/block.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/nn/modules/block/

**Contents:**
- Reference for ultralytics/nn/modules/block.py
- class ultralytics.nn.modules.block.DFL
  - method ultralytics.nn.modules.block.DFL.forward
- class ultralytics.nn.modules.block.Proto
  - method ultralytics.nn.modules.block.Proto.forward
- class ultralytics.nn.modules.block.HGStem
  - method ultralytics.nn.modules.block.HGStem.forward
- class ultralytics.nn.modules.block.HGBlock
  - method ultralytics.nn.modules.block.HGBlock.forward
- class ultralytics.nn.modules.block.SPP

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/nn/modules/block.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Integral module of Distribution Focal Loss (DFL).

Proposed in Generalized Focal Loss https://ieeexplore.ieee.org/document/9792391

Apply the DFL module to input tensor and return transformed output.

Ultralytics YOLO models mask Proto module for segmentation models.

Perform a forward pass through layers using an upsampled input image.

StemBlock of PPHGNetV2 with 5 convolutions and one maxpool2d.

https://github.com/PaddlePaddle/PaddleDetection/blob/develop/ppdet/modeling/backbones/hgnet_v2.py

Forward pass of a PPHGNetV2 backbone layer.

HG_Block of PPHGNetV2 with 2 convolutions and LightConv.

https://github.com/PaddlePaddle/PaddleDetection/blob/develop/ppdet/modeling/backbones/hgnet_v2.py

Forward pass of a PPHGNetV2 backbone layer.

Spatial Pyramid Pooling (SPP) layer https://arxiv.org/abs/1406.4729.

Forward pass of the SPP layer, performing spatial pyramid pooling.

Spatial Pyramid Pooling - Fast (SPPF) layer for YOLOv5 by Glenn Jocher.

This module is equivalent to SPP(k=(5, 9, 13)).

Apply sequential pooling operations to input and return concatenated feature maps.

CSP Bottleneck with 1 convolution.

Apply convolution and residual connection to input tensor.

CSP Bottleneck with 2 convolutions.

Forward pass through the CSP bottleneck with 2 convolutions.

Faster Implementation of CSP Bottleneck with 2 convolutions.

Forward pass through C2f layer.

Forward pass using split() instead of chunk().

CSP Bottleneck with 3 convolutions.

Forward pass through the CSP bottleneck with 3 convolutions.

C3 module with cross-convolutions.

Forward pass of RepC3 module.

C3 module with TransformerBlock().

C3 module with GhostBottleneck().

Ghost Bottleneck https://github.com/huawei-noah/Efficient-AI-Backbones.

Apply skip connection and concatenation to input tensor.

Apply bottleneck with optional shortcut connection.

CSP Bottleneck https://github.com/WongKinYiu/CrossStagePartialNetworks.

Apply CSP bottleneck with 3 convolutions.

ResNet block with standard convolution layers.

Forward pass through the ResNet block.

ResNet layer with multiple ResNet blocks.

Forward pass through the ResNet layer.

Max Sigmoid attention block.

Forward pass of MaxSigmoidAttnBlock.

C2f module with an additional attn module.

Forward pass through C2f layer with attention.

Forward pass using split() instead of chunk().

ImagePoolingAttn: Enhance the text embeddings with image-aware information.

Forward pass of ImagePoolingAttn.

Implements contrastive learning head for region-text similarity in vision-language models.

Forward function of contrastive learning.

Batch Norm Contrastive Head using batch norm instead of l2-normalization.

Forward function of contrastive learning with batch normalization.

Passes input out unchanged.

Fuse the batch normalization layer in the BNContrastiveHead module.

Repeatable Cross Stage Partial Network (RepCSP) module for efficient feature extraction.

Forward pass through RepNCSPELAN4 layer.

Forward pass using split() instead of chunk().

ELAN1 module with 4 convolutions.

Forward pass through AConv layer.

Forward pass through ADown layer.

Forward pass through SPPELAN layer.

Forward pass through CBLinear layer.

Forward pass through CBFuse layer.

Faster Implementation of CSP Bottleneck with 2 convolutions.

Forward pass through C3f layer.

Faster Implementation of CSP Bottleneck with 2 convolutions.

C3k is a CSP bottleneck module with customizable kernel sizes for feature extraction in neural networks.

Bases: torch.nn.Module

RepVGGDW is a class that represents a depth wise separable convolutional block in RepVGG architecture.

Perform a forward pass of the RepVGGDW block.

Perform a forward pass of the RepVGGDW block without fusing the convolutions.

Fuse the convolutional layers in the RepVGGDW block.

This method fuses the convolutional layers and updates the weights and biases accordingly.

Compact Inverted Block (CIB) module.

Forward pass of the CIB module.

C2fCIB class represents a convolutional block with C2f and CIB modules.

Attention module that performs self-attention on the input tensor.

Forward pass of the Attention module.

PSABlock class implementing a Position-Sensitive Attention block for neural networks.

This class encapsulates the functionality for applying multi-head attention and feed-forward neural network layers with optional shortcut connections.

Execute a forward pass through PSABlock.

PSA class for implementing Position-Sensitive Attention in neural networks.

This class encapsulates the functionality for applying position-sensitive attention and feed-forward networks to input tensors, enhancing feature extraction and processing capabilities.

Execute forward pass in PSA module.

C2PSA module with attention mechanism for enhanced feature extraction and processing.

This module implements a convolutional block with attention mechanisms to enhance feature extraction and processing capabilities. It includes a series of PSABlock modules for self-attention and feed-forward operations.

This module essentially is the same as PSA module, but refactored to allow stacking more PSABlock modules.

Process the input tensor through a series of PSA blocks.

C2fPSA module with enhanced feature extraction using PSA blocks.

This class extends the C2f module by incorporating PSA blocks for improved attention mechanisms and feature extraction.

SCDown module for downsampling with separable convolutions.

This module performs downsampling using a combination of pointwise and depthwise convolutions, which helps in efficiently reducing the spatial dimensions of the input tensor while maintaining the channel information.

Apply convolution and downsampling to the input tensor.

TorchVision module to allow loading any torchvision model.

This class provides a way to load a model from the torchvision library, optionally load pre-trained weights, and customize the model by truncating or unwrapping layers.

Forward pass through the model.

Area-attention module for YOLO models, providing efficient attention mechanisms.

This module implements an area-based attention mechanism that processes input features in a spatially-aware manner, making it particularly effective for object detection tasks.

Process the input tensor through the area-attention.

Area-attention block module for efficient feature extraction in YOLO models.

This module implements an area-attention mechanism combined with a feed-forward network for processing feature maps. It uses a novel area-based attention approach that is more efficient than traditional self-attention while maintaining effectiveness.

Initialize weights using a truncated normal distribution.

Forward pass through ABlock.

Area-Attention C2f module for enhanced feature extraction with area-based attention mechanisms.

This module extends the C2f architecture by incorporating area-attention and ABlock layers for improved feature processing. It supports both area-attention and standard convolution modes.

Forward pass through A2C2f layer.

SwiGLU Feed-Forward Network for transformer-based architectures.

Apply SwiGLU transformation to input features.

Residual connection wrapper for neural network modules.

Apply residual connection to input features.

Spatial-Aware Visual Prompt Embedding module for feature enhancement.

Process input features and visual prompts to generate enhanced embeddings.

**Examples:**

Example 1 (typescript):
```typescript
DFL(self, c1: int = 16)
```

Example 2 (python):
```python
class DFL(nn.Module):
    """Integral module of Distribution Focal Loss (DFL).

    Proposed in Generalized Focal Loss https://ieeexplore.ieee.org/document/9792391
    """

    def __init__(self, c1: int = 16):
        """Initialize a convolutional layer with a given number of input channels.

        Args:
            c1 (int): Number of input channels.
        """
        super().__init__()
        self.conv = nn.Conv2d(c1, 1, 1, bias=False).requires_grad_(False)
        x = torch.arange(c1, dtype=torch.float)
        self.conv.weight.data[:] = nn.Parameter(x.view(1, c1, 1, 1))
        self.c1 = c1
```

Example 3 (python):
```python
def forward(self, x: torch.Tensor) -> torch.Tensor
```

Example 4 (python):
```python
def forward(self, x: torch.Tensor) -> torch.Tensor:
    """Apply the DFL module to input tensor and return transformed output."""
    b, _, a = x.shape  # batch, channels, anchors
    return self.conv(x.view(b, 4, self.c1, a).transpose(2, 1).softmax(1)).view(b, 4, a)
```

---
