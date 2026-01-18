# Yolo - Prediction

**Pages:** 37

---

## Reference for ultralytics/models/fastsam/predict.py

**URL:** https://docs.ultralytics.com/zh/reference/models/fastsam/predict/

**Contents:**
- Reference for ultralytics/models/fastsam/predict.py
- class ultralytics.models.fastsam.predict.FastSAMPredictor
  - method ultralytics.models.fastsam.predict.FastSAMPredictor._clip_inference
  - method ultralytics.models.fastsam.predict.FastSAMPredictor.postprocess
  - method ultralytics.models.fastsam.predict.FastSAMPredictor.prompt
  - method ultralytics.models.fastsam.predict.FastSAMPredictor.set_prompts

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/models/fastsam/predict.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Bases: SegmentationPredictor

FastSAMPredictor is specialized for fast SAM (Segment Anything Model) segmentation prediction tasks.

This class extends the SegmentationPredictor, customizing the prediction pipeline specifically for fast SAM. It adjusts post-processing steps to incorporate mask prediction and non-maximum suppression while optimizing for single-class segmentation.

This initializes a predictor specialized for Fast SAM (Segment Anything Model) segmentation tasks. The predictor extends SegmentationPredictor with custom post-processing for mask prediction and non-maximum suppression optimized for single-class segmentation.

Perform CLIP inference to calculate similarity between images and text prompts.

Apply postprocessing to FastSAM predictions and handle prompts.

Perform image segmentation inference based on cues like bounding boxes, points, and text prompts.

Set prompts to be used during inference.

**Examples:**

Example 1 (rust):
```rust
FastSAMPredictor(self, cfg = DEFAULT_CFG, overrides = None, _callbacks = None)
```

Example 2 (python):
```python
class FastSAMPredictor(SegmentationPredictor):
    """FastSAMPredictor is specialized for fast SAM (Segment Anything Model) segmentation prediction tasks.

    This class extends the SegmentationPredictor, customizing the prediction pipeline specifically for fast SAM. It
    adjusts post-processing steps to incorporate mask prediction and non-maximum suppression while optimizing for
    single-class segmentation.

    Attributes:
        prompts (dict): Dictionary containing prompt information for segmentation (bboxes, points, labels, texts).
        device (torch.device): Device on which model and tensors are processed.
        clip (Any, optional): CLIP model used for text-based prompting, loaded on demand.

    Methods:
        postprocess: Apply postprocessing to FastSAM predictions and handle prompts.
        prompt: Perform image segmentation inference based on various prompt types.
        set_prompts: Set prompts to be used during inference.
    """

    def __init__(self, cfg=DEFAULT_CFG, overrides=None, _callbacks=None):
        """Initialize the FastSAMPredictor with configuration and callbacks.

        This initializes a predictor specialized for Fast SAM (Segment Anything Model) segmentation tasks. The predictor
        extends SegmentationPredictor with custom post-processing for mask prediction and non-maximum suppression
        optimized for single-class segmentation.

        Args:
            cfg (dict): Configuration for the predictor.
            overrides (dict, optional): Configuration overrides.
            _callbacks (list, optional): List of callback functions.
        """
        super().__init__(cfg, overrides, _callbacks)
        self.prompts = {}
```

Example 3 (python):
```python
def _clip_inference(self, images, texts)
```

Example 4 (python):
```python
def _clip_inference(self, images, texts):
    """Perform CLIP inference to calculate similarity between images and text prompts.

    Args:
        images (list[PIL.Image]): List of source images, each should be PIL.Image with RGB channel order.
        texts (list[str]): List of prompt texts, each should be a string object.

    Returns:
        (torch.Tensor): Similarity matrix between given images and texts with shape (M, N).
    """
    from ultralytics.nn.text_model import CLIP

    if not hasattr(self, "clip"):
        self.clip = CLIP("ViT-B/32", device=self.device)
    images = torch.stack([self.clip.image_preprocess(image).to(self.device) for image in images])
    image_features = self.clip.encode_image(images)
    text_features = self.clip.encode_text(self.clip.tokenize(texts))
    return text_features @ image_features.T  # (M, N)
```

---

## Reference for ultralytics/models/yolo/segment/val.py

**URL:** https://docs.ultralytics.com/zh/reference/models/yolo/segment/val/

**Contents:**
- Reference for ultralytics/models/yolo/segment/val.py
- class ultralytics.models.yolo.segment.val.SegmentationValidator
  - method ultralytics.models.yolo.segment.val.SegmentationValidator._prepare_batch
  - method ultralytics.models.yolo.segment.val.SegmentationValidator._process_batch
  - method ultralytics.models.yolo.segment.val.SegmentationValidator.eval_json
  - method ultralytics.models.yolo.segment.val.SegmentationValidator.get_desc
  - method ultralytics.models.yolo.segment.val.SegmentationValidator.init_metrics
  - method ultralytics.models.yolo.segment.val.SegmentationValidator.plot_predictions
  - method ultralytics.models.yolo.segment.val.SegmentationValidator.postprocess
  - method ultralytics.models.yolo.segment.val.SegmentationValidator.pred_to_json

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/models/yolo/segment/val.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Bases: DetectionValidator

A class extending the DetectionValidator class for validation based on a segmentation model.

This validator handles the evaluation of segmentation models, processing both bounding box and mask predictions to compute metrics such as mAP for both detection and segmentation tasks.

Prepare a batch for training or inference by processing images and targets.

Compute correct prediction matrix for a batch based on bounding boxes and optional masks.

Return COCO-style instance segmentation evaluation metrics.

Return a formatted description of evaluation metrics.

Initialize metrics and select mask processing function based on save_json flag.

Plot batch predictions with masks and bounding boxes.

Post-process YOLO predictions and return output detections with proto.

Save one JSON result for COCO evaluation.

Preprocess batch of images for YOLO segmentation validation.

Save YOLO detections to a txt file in normalized coordinates in a specific format.

Scales predictions to the original image size.

**Examples:**

Example 1 (rust):
```rust
SegmentationValidator(self, dataloader = None, save_dir = None, args = None, _callbacks = None) -> None
```

Example 2 (sql):
```sql
>>> from ultralytics.models.yolo.segment import SegmentationValidator
>>> args = dict(model="yolo11n-seg.pt", data="coco8-seg.yaml")
>>> validator = SegmentationValidator(args=args)
>>> validator()
```

Example 3 (python):
```python
class SegmentationValidator(DetectionValidator):
    """A class extending the DetectionValidator class for validation based on a segmentation model.

    This validator handles the evaluation of segmentation models, processing both bounding box and mask predictions to
    compute metrics such as mAP for both detection and segmentation tasks.

    Attributes:
        plot_masks (list): List to store masks for plotting.
        process (callable): Function to process masks based on save_json and save_txt flags.
        args (namespace): Arguments for the validator.
        metrics (SegmentMetrics): Metrics calculator for segmentation tasks.
        stats (dict): Dictionary to store statistics during validation.

    Examples:
        >>> from ultralytics.models.yolo.segment import SegmentationValidator
        >>> args = dict(model="yolo11n-seg.pt", data="coco8-seg.yaml")
        >>> validator = SegmentationValidator(args=args)
        >>> validator()
    """

    def __init__(self, dataloader=None, save_dir=None, args=None, _callbacks=None) -> None:
        """Initialize SegmentationValidator and set task to 'segment', metrics to SegmentMetrics.

        Args:
            dataloader (torch.utils.data.DataLoader, optional): DataLoader to use for validation.
            save_dir (Path, optional): Directory to save results.
            args (namespace, optional): Arguments for the validator.
            _callbacks (list, optional): List of callback functions.
        """
        super().__init__(dataloader, save_dir, args, _callbacks)
        self.process = None
        self.args.task = "segment"
        self.metrics = SegmentMetrics()
```

Example 4 (python):
```python
def _prepare_batch(self, si: int, batch: dict[str, Any]) -> dict[str, Any]
```

---

## Reference for ultralytics/models/nas/val.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/models/nas/val/

**Contents:**
- Reference for ultralytics/models/nas/val.py
- class ultralytics.models.nas.val.NASValidator
  - method ultralytics.models.nas.val.NASValidator.postprocess

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/models/nas/val.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Bases: DetectionValidator

Ultralytics YOLO NAS Validator for object detection.

Extends DetectionValidator from the Ultralytics models package and is designed to post-process the raw predictions generated by YOLO NAS models. It performs non-maximum suppression to remove overlapping and low-confidence boxes, ultimately producing the final detections.

This class is generally not instantiated directly but is used internally within the NAS class.

Apply Non-maximum suppression to prediction outputs.

**Examples:**

Example 1 (unknown):
```unknown
NASValidator()
```

Example 2 (python):
```python
>>> from ultralytics import NAS
>>> model = NAS("yolo_nas_s")
>>> validator = model.validator
>>> # Assumes that raw_preds are available
>>> final_preds = validator.postprocess(raw_preds)
```

Example 3 (php):
```php
class NASValidator(DetectionValidator):
```

Example 4 (python):
```python
def postprocess(self, preds_in)
```

---

## Reference for ultralytics/models/yolo/segment/predict.py

**URL:** https://docs.ultralytics.com/zh/reference/models/yolo/segment/predict/

**Contents:**
- Reference for ultralytics/models/yolo/segment/predict.py
- class ultralytics.models.yolo.segment.predict.SegmentationPredictor
  - method ultralytics.models.yolo.segment.predict.SegmentationPredictor.construct_result
  - method ultralytics.models.yolo.segment.predict.SegmentationPredictor.construct_results
  - method ultralytics.models.yolo.segment.predict.SegmentationPredictor.postprocess

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/models/yolo/segment/predict.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Bases: DetectionPredictor

A class extending the DetectionPredictor class for prediction based on a segmentation model.

This class specializes in processing segmentation model outputs, handling both bounding boxes and masks in the prediction results.

This class specializes in processing segmentation model outputs, handling both bounding boxes and masks in the prediction results.

Construct a single result object from the prediction.

Construct a list of result objects from the predictions.

Apply non-max suppression and process segmentation detections for each image in the input batch.

**Examples:**

Example 1 (rust):
```rust
SegmentationPredictor(self, cfg = DEFAULT_CFG, overrides = None, _callbacks = None)
```

Example 2 (sql):
```sql
>>> from ultralytics.utils import ASSETS
>>> from ultralytics.models.yolo.segment import SegmentationPredictor
>>> args = dict(model="yolo11n-seg.pt", source=ASSETS)
>>> predictor = SegmentationPredictor(overrides=args)
>>> predictor.predict_cli()
```

Example 3 (python):
```python
class SegmentationPredictor(DetectionPredictor):
    """A class extending the DetectionPredictor class for prediction based on a segmentation model.

    This class specializes in processing segmentation model outputs, handling both bounding boxes and masks in the
    prediction results.

    Attributes:
        args (dict): Configuration arguments for the predictor.
        model (torch.nn.Module): The loaded YOLO segmentation model.
        batch (list): Current batch of images being processed.

    Methods:
        postprocess: Apply non-max suppression and process segmentation detections.
        construct_results: Construct a list of result objects from predictions.
        construct_result: Construct a single result object from a prediction.

    Examples:
        >>> from ultralytics.utils import ASSETS
        >>> from ultralytics.models.yolo.segment import SegmentationPredictor
        >>> args = dict(model="yolo11n-seg.pt", source=ASSETS)
        >>> predictor = SegmentationPredictor(overrides=args)
        >>> predictor.predict_cli()
    """

    def __init__(self, cfg=DEFAULT_CFG, overrides=None, _callbacks=None):
        """Initialize the SegmentationPredictor with configuration, overrides, and callbacks.

        This class specializes in processing segmentation model outputs, handling both bounding boxes and masks in the
        prediction results.

        Args:
            cfg (dict): Configuration for the predictor.
            overrides (dict, optional): Configuration overrides that take precedence over cfg.
            _callbacks (list, optional): List of callback functions to be invoked during prediction.
        """
        super().__init__(cfg, overrides, _callbacks)
        self.args.task = "segment"
```

Example 4 (python):
```python
def construct_result(self, pred, img, orig_img, img_path, proto)
```

---

## 高级自定义 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/usage/engine/

**Contents:**
- 高级自定义
- BaseTrainer
- DetectionTrainer
  - 自定义 DetectionTrainer
- 其他引擎组件
- 将 YOLO 与自定义训练器结合使用
- 常见问题
  - 如何为特定任务自定义 Ultralytics YOLO DetectionTrainer？
  - Ultralytics YOLO 中 BaseTrainer 的关键组件是什么？
  - 如何向 Ultralytics YOLO DetectionTrainer 添加回调？

Ultralytics YOLO 命令行和 python 接口都是构建在基础引擎执行器之上的高级抽象。本指南侧重于 Trainer 引擎，解释如何根据您的特定需求进行定制。

观看： 掌握 Ultralytics YOLO：高级自定义

字段 BaseTrainer 类提供了一个通用的训练程序，适用于各种任务。通过覆盖特定的函数或操作来定制它，同时遵守所需的格式。例如，通过覆盖以下函数来集成您自己的自定义模型和数据加载器：

有关更多详细信息和源代码，请参见 BaseTrainer 参考.

以下是如何使用和自定义 Ultralytics YOLO DetectionTrainer:

要训练未直接支持的自定义检测模型，请重载现有的 get_model 功能来实现：

通过修改损失函数或添加回调以每 10 个epochs将模型上传到 Google Drive，从而进一步自定义训练器。 这是一个例子：

有关回调触发事件和入口点的更多信息，请参阅回调指南。

自定义其他组件，例如 Validators 和 Predictors 同样。有关更多信息，请参阅以下文档： 验证器 和 预测器.

字段 YOLO model 类为 Trainer 类提供了一个高级封装器。您可以利用此架构在机器学习工作流程中获得更大的灵活性：

这种方法允许您保持 YOLO 接口的简单性，同时自定义底层训练过程以满足您的特定需求。

自定义 DetectionTrainer 针对特定任务，通过重写其方法来适应您的自定义模型和数据加载器。首先从以下继承： DetectionTrainer 并重新定义方法，例如 get_model 以实现自定义功能。这是一个例子：

如需进一步的自定义，例如更改损失函数或添加回调，请参阅回调指南。

字段 BaseTrainer 是训练程序的基础，通过覆盖其通用方法，可以针对各种任务进行定制。主要组件包括：

有关自定义和源代码的更多详细信息，请参见 BaseTrainer 参考.

在以下位置添加回调以监控和修改训练过程 DetectionTrainer。以下是如何添加回调以在每次训练后记录模型权重： epoch:

有关回调事件和入口点的更多详细信息，请参阅回调指南。

Ultralytics YOLO 在强大的引擎执行器上提供了一个高级抽象，使其成为快速开发和自定义的理想选择。主要优势包括：

通过浏览主要的 Ultralytics YOLO 页面，了解更多关于 YOLO 功能的信息。

是的， DetectionTrainer 对于非标准模型，具有高度的灵活性和可定制性。继承自 DetectionTrainer 并重载方法以支持您的特定模型的需求。 这是一个简单的例子：

有关全面的说明和示例，请查看 DetectionTrainer 参考.

**Examples:**

Example 1 (sql):
```sql
from ultralytics.models.yolo.detect import DetectionTrainer

trainer = DetectionTrainer(overrides={...})
trainer.train()
trained_model = trainer.best  # Get the best model
```

Example 2 (python):
```python
from ultralytics.models.yolo.detect import DetectionTrainer


class CustomTrainer(DetectionTrainer):
    def get_model(self, cfg, weights):
        """Loads a custom detection model given configuration and weight files."""
        ...


trainer = CustomTrainer(overrides={...})
trainer.train()
```

Example 3 (python):
```python
from ultralytics.models.yolo.detect import DetectionTrainer
from ultralytics.nn.tasks import DetectionModel


class MyCustomModel(DetectionModel):
    def init_criterion(self):
        """Initializes the loss function and adds a callback for uploading the model to Google Drive every 10 epochs."""
        ...


class CustomTrainer(DetectionTrainer):
    def get_model(self, cfg, weights):
        """Returns a customized detection model instance configured with specified config and weights."""
        return MyCustomModel(...)


# Callback to upload model weights
def log_model(trainer):
    """Logs the path of the last model weight used by the trainer."""
    last_weight_path = trainer.last
    print(last_weight_path)


trainer = CustomTrainer(overrides={...})
trainer.add_callback("on_train_epoch_end", log_model)  # Adds to existing callbacks
trainer.train()
```

Example 4 (python):
```python
from ultralytics import YOLO
from ultralytics.models.yolo.detect import DetectionTrainer


# Create a custom trainer
class MyCustomTrainer(DetectionTrainer):
    def get_model(self, cfg, weights):
        """Custom code implementation."""
        ...


# Initialize YOLO model
model = YOLO("yolo11n.pt")

# Train with custom trainer
results = model.train(trainer=MyCustomTrainer, data="coco8.yaml", epochs=3)
```

---

## Reference for ultralytics/utils/callbacks/comet.py

**URL:** https://docs.ultralytics.com/zh/reference/utils/callbacks/comet/

**Contents:**
- Reference for ultralytics/utils/callbacks/comet.py
- function ultralytics.utils.callbacks.comet._get_comet_mode
- function ultralytics.utils.callbacks.comet._get_comet_model_name
- function ultralytics.utils.callbacks.comet._get_eval_batch_logging_interval
- function ultralytics.utils.callbacks.comet._get_max_image_predictions_to_log
- function ultralytics.utils.callbacks.comet._scale_confidence_score
- function ultralytics.utils.callbacks.comet._should_log_confusion_matrix
- function ultralytics.utils.callbacks.comet._should_log_image_predictions
- function ultralytics.utils.callbacks.comet._resume_or_create_experiment
- function ultralytics.utils.callbacks.comet._fetch_trainer_metadata

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/callbacks/comet.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Return the Comet mode from environment variables, defaulting to 'online'.

Return the Comet model name from environment variable or default to 'Ultralytics'.

Get the evaluation batch logging interval from environment variable or use default value 1.

Get the maximum number of image predictions to log from environment variables.

Scale the confidence score by a factor specified in environment variable.

Determine if the confusion matrix should be logged based on environment variable settings.

Determine whether to log image predictions based on environment variable.

Resume CometML experiment or create a new experiment based on args.

Ensures that the experiment object is only created in a single process during distributed training.

Return metadata for YOLO training including epoch and asset saving status.

Scale bounding box from resized image coordinates to original image coordinates.

YOLO resizes images during training and the label values are normalized based on this resized shape. This function rescales the bounding box labels to the original image shape.

Format ground truth annotations for object detection.

This function processes ground truth annotations from a batch of images for object detection tasks. It extracts bounding boxes, class labels, and other metadata for a specific image in the batch, and formats them for visualization or evaluation.

Format YOLO predictions for object detection visualization.

Extract segmentation annotation from compressed segmentations as list of polygons.

Join the ground truth and prediction annotations if they exist.

Create metadata map for model predictions by grouping them based on image ID.

Log the confusion matrix to Comet experiment.

Log images to the experiment with optional annotations.

This function logs images to a Comet ML experiment, optionally including annotation data for visualization such as bounding boxes or segmentation masks.

Log predicted boxes for a single image during training.

This function logs image predictions to a Comet ML experiment during model validation. It processes validation data and formats both ground truth and prediction annotations for visualization in the Comet dashboard. The function respects configured limits on the number of images to log.

This function uses global state to track the number of logged predictions across calls. It only logs predictions for supported tasks defined in COMET_SUPPORTED_TASKS. The number of logged images is limited by the COMET_MAX_IMAGE_PREDICTIONS environment variable.

Log evaluation plots and label plots for the experiment.

This function logs various evaluation plots and confusion matrices to the experiment tracking system. It handles different types of metrics (SegmentMetrics, PoseMetrics, DetMetrics, OBBMetrics) and logs the appropriate plots for each type.

Log the best-trained model to Comet.ml.

Log samples of image batches for train, validation, and test.

Logs a specific asset file to the given experiment.

This function facilitates logging an asset, such as a file, to the provided experiment. It enables integration with experiment tracking platforms.

Logs a table to the provided experiment.

This function is used to log a table file to the given experiment. The table is identified by its file path.

Create or resume a CometML experiment at the start of a YOLO pre-training routine.

Log metrics and save batch images at the end of training epochs.

Log model assets at the end of each epoch during training.

This function is called at the end of each training epoch to log metrics, learning rates, and model information to a Comet ML experiment. It also logs model assets, confusion matrices, and image predictions based on configuration settings.

The function retrieves the current Comet ML experiment and logs various training metrics. If it's the first epoch, it also logs model information. On specified save intervals, it logs the model, confusion matrix (if enabled), and image predictions (if enabled).

Perform operations at the end of training.

**Examples:**

Example 1 (python):
```python
def _get_comet_mode() -> str
```

Example 2 (python):
```python
def _get_comet_mode() -> str:
    """Return the Comet mode from environment variables, defaulting to 'online'."""
    comet_mode = os.getenv("COMET_MODE")
    if comet_mode is not None:
        LOGGER.warning(
            "The COMET_MODE environment variable is deprecated. "
            "Please use COMET_START_ONLINE to set the Comet experiment mode. "
            "To start an offline Comet experiment, use 'export COMET_START_ONLINE=0'. "
            "If COMET_START_ONLINE is not set or is set to '1', an online Comet experiment will be created."
        )
        return comet_mode

    return "online"
```

Example 3 (python):
```python
def _get_comet_model_name() -> str
```

Example 4 (python):
```python
def _get_comet_model_name() -> str:
    """Return the Comet model name from environment variable or default to 'Ultralytics'."""
    return os.getenv("COMET_MODEL_NAME", "Ultralytics")
```

---

## Reference for ultralytics/models/yolo/detect/val.py

**URL:** https://docs.ultralytics.com/zh/reference/models/yolo/detect/val/

**Contents:**
- Reference for ultralytics/models/yolo/detect/val.py
- class ultralytics.models.yolo.detect.val.DetectionValidator
  - method ultralytics.models.yolo.detect.val.DetectionValidator._prepare_batch
  - method ultralytics.models.yolo.detect.val.DetectionValidator._prepare_pred
  - method ultralytics.models.yolo.detect.val.DetectionValidator._process_batch
  - method ultralytics.models.yolo.detect.val.DetectionValidator.build_dataset
  - method ultralytics.models.yolo.detect.val.DetectionValidator.coco_evaluate
  - method ultralytics.models.yolo.detect.val.DetectionValidator.eval_json
  - method ultralytics.models.yolo.detect.val.DetectionValidator.finalize_metrics
  - method ultralytics.models.yolo.detect.val.DetectionValidator.gather_stats

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/models/yolo/detect/val.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class extending the BaseValidator class for validation based on a detection model.

This class implements validation functionality specific to object detection tasks, including metrics calculation, prediction processing, and visualization of results.

Prepare a batch of images and annotations for validation.

Prepare predictions for evaluation against ground truth.

Return correct prediction matrix.

Evaluate COCO/LVIS metrics using faster-coco-eval library.

Performs evaluation using the faster-coco-eval library to compute mAP metrics for object detection. Updates the provided stats dictionary with computed metrics including mAP50, mAP50-95, and LVIS-specific metrics if applicable.

Evaluate YOLO output in JSON format and return performance statistics.

Set final values for metrics speed and confusion matrix.

Gather stats from all GPUs.

Construct and return dataloader.

Return a formatted string summarizing class metrics of YOLO model.

Calculate and return metrics statistics.

Initialize evaluation metrics for YOLO detection validation.

Plot predicted bounding boxes on input images and save the result.

Plot validation image samples.

Apply Non-maximum suppression to prediction outputs.

Serialize YOLO predictions to COCO json format.

Preprocess batch of images for YOLO validation.

Print training/validation set metrics per class.

Save YOLO detections to a txt file in normalized coordinates in a specific format.

Scales predictions to the original image size.

Update metrics with new predictions and ground truth.

**Examples:**

Example 1 (rust):
```rust
DetectionValidator(self, dataloader = None, save_dir = None, args = None, _callbacks = None) -> None
```

Example 2 (sql):
```sql
>>> from ultralytics.models.yolo.detect import DetectionValidator
>>> args = dict(model="yolo11n.pt", data="coco8.yaml")
>>> validator = DetectionValidator(args=args)
>>> validator()
```

Example 3 (python):
```python
class DetectionValidator(BaseValidator):
    """A class extending the BaseValidator class for validation based on a detection model.

    This class implements validation functionality specific to object detection tasks, including metrics calculation,
    prediction processing, and visualization of results.

    Attributes:
        is_coco (bool): Whether the dataset is COCO.
        is_lvis (bool): Whether the dataset is LVIS.
        class_map (list[int]): Mapping from model class indices to dataset class indices.
        metrics (DetMetrics): Object detection metrics calculator.
        iouv (torch.Tensor): IoU thresholds for mAP calculation.
        niou (int): Number of IoU thresholds.
        lb (list[Any]): List for storing ground truth labels for hybrid saving.
        jdict (list[dict[str, Any]]): List for storing JSON detection results.
        stats (dict[str, list[torch.Tensor]]): Dictionary for storing statistics during validation.

    Examples:
        >>> from ultralytics.models.yolo.detect import DetectionValidator
        >>> args = dict(model="yolo11n.pt", data="coco8.yaml")
        >>> validator = DetectionValidator(args=args)
        >>> validator()
    """

    def __init__(self, dataloader=None, save_dir=None, args=None, _callbacks=None) -> None:
        """Initialize detection validator with necessary variables and settings.

        Args:
            dataloader (torch.utils.data.DataLoader, optional): DataLoader to use for validation.
            save_dir (Path, optional): Directory to save results.
            args (dict[str, Any], optional): Arguments for the validator.
            _callbacks (list[Any], optional): List of callback functions.
        """
        super().__init__(dataloader, save_dir, args, _callbacks)
        self.is_coco = False
        self.is_lvis = False
        self.class_map = None
        self.args.task = "detect"
        self.iouv = torch.linspace(0.5, 0.95, 10)  # IoU vector for mAP@0.5:0.95
        self.niou = self.iouv.numel()
        self.metrics = DetMetrics()
```

Example 4 (python):
```python
def _prepare_batch(self, si: int, batch: dict[str, Any]) -> dict[str, Any]
```

---

## Reference for ultralytics/engine/model.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/engine/model/

**Contents:**
- Reference for ultralytics/engine/model.py
- class ultralytics.engine.model.Model
  - property ultralytics.engine.model.Model.names
  - property ultralytics.engine.model.Model.device
  - property ultralytics.engine.model.Model.transforms
  - property ultralytics.engine.model.Model.task_map
  - method ultralytics.engine.model.Model.__call__
  - method ultralytics.engine.model.Model.__getattr__
  - method ultralytics.engine.model.Model._apply
  - method ultralytics.engine.model.Model._check_is_pytorch_model

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/engine/model.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Bases: torch.nn.Module

A base class for implementing YOLO models, unifying APIs across different model types.

This class provides a common interface for various operations related to YOLO models, such as training, validation, prediction, exporting, and benchmarking. It handles different types of models, including those loaded from local files, Ultralytics HUB, or Triton Server.

This constructor sets up the model based on the provided model path or name. It handles various types of model sources, including local files, Ultralytics HUB models, and Triton Server models. The method initializes several important attributes of the model and prepares it for operations like training, prediction, or export.

Retrieve the class names associated with the loaded model.

This property returns the class names if they are defined in the model. It checks the class names for validity using the 'check_class_names' function from the ultralytics.nn.autobackend module. If the predictor is not initialized, it sets it up before retrieving the names.

Get the device on which the model's parameters are allocated.

This property determines the device (CPU or GPU) where the model's parameters are currently stored. It is applicable only to models that are instances of torch.nn.Module.

Retrieve the transformations applied to the input data of the loaded model.

This property returns the transformations if they are defined in the model. The transforms typically include preprocessing steps like resizing, normalization, and data augmentation that are applied to input data before it is fed into the model.

Provide a mapping from model tasks to corresponding classes for different modes.

This property method returns a dictionary that maps each supported task (e.g., detect, segment, classify) to a nested dictionary. The nested dictionary contains mappings for different operational modes (model, trainer, validator, predictor) to their respective class implementations.

The mapping allows for dynamic loading of appropriate classes based on the model's task and the desired operational mode. This facilitates a flexible and extensible architecture for handling various tasks and modes within the Ultralytics framework.

Alias for the predict method, enabling the model instance to be callable for predictions.

This method simplifies the process of making predictions by allowing the model instance to be called directly with the required arguments.

Enable accessing model attributes directly through the Model class.

This method provides a way to access attributes of the underlying model directly through the Model class instance. It first checks if the requested attribute is 'model', in which case it returns the model from the module dictionary. Otherwise, it delegates the attribute lookup to the underlying model.

Apply a function to model tensors that are not parameters or registered buffers.

This method extends the functionality of the parent class's _apply method by additionally resetting the predictor and updating the device in the model's overrides. It's typically used for operations like moving the model to a different device or changing its precision.

Check if the model is a PyTorch model and raise TypeError if it's not.

This method verifies that the model is either a PyTorch module or a .pt file. It's used to ensure that certain operations that require a PyTorch model are only performed on compatible model types.

Load a model from a checkpoint file or initialize it from a weights file.

This method handles loading models from either .pt checkpoint files or other weight file formats. It sets up the model, task, and related attributes based on the loaded weights.

Initialize a new model and infer the task type from model definitions.

Creates a new model instance based on the provided configuration file. Loads the model configuration, infers the task type if not specified, and initializes the model using the appropriate class from the task map.

Reset specific arguments when loading a PyTorch model checkpoint.

This method filters the input arguments dictionary to retain only a specific set of keys that are considered important for model loading. It's used to ensure that only relevant arguments are preserved when loading a model from a checkpoint, discarding any unnecessary or potentially conflicting settings.

Intelligently load the appropriate module based on the model task.

This method dynamically selects and returns the correct module (model, trainer, validator, or predictor) based on the current task of the model and the provided key. It uses the task_map dictionary to determine the appropriate module to load for the specific task.

Add a callback function for a specified event.

This method allows registering custom callback functions that are triggered on specific events during model operations such as training or inference. Callbacks provide a way to extend and customize the behavior of the model at various stages of its lifecycle.

Benchmark the model across various export formats to evaluate performance.

This method assesses the model's performance in different export formats, such as ONNX, TorchScript, etc. It uses the 'benchmark' function from the ultralytics.utils.benchmarks module. The benchmarking is configured using a combination of default configuration values, model-specific arguments, method-specific defaults, and any additional user-provided keyword arguments.

Clear all callback functions registered for a specified event.

This method removes all custom and default callback functions associated with the given event. It resets the callback list for the specified event to an empty list, effectively removing all registered callbacks for that event.

Generate image embeddings based on the provided source.

This method is a wrapper around the 'predict()' method, focusing on generating embeddings from an image source. It allows customization of the embedding process through various keyword arguments.

Sets the model to evaluation mode.

This method changes the model's mode to evaluation, which affects layers like dropout and batch normalization that behave differently during training and evaluation. In evaluation mode, these layers use running statistics rather than computing batch statistics, and dropout layers are disabled.

Export the model to a different format suitable for deployment.

This method facilitates the export of the model to various formats (e.g., ONNX, TorchScript) for deployment purposes. It uses the 'Exporter' class for the export process, combining model-specific overrides, method defaults, and any additional arguments provided.

Fuse Conv2d and BatchNorm2d layers in the model for optimized inference.

This method iterates through the model's modules and fuses consecutive Conv2d and BatchNorm2d layers into a single layer. This fusion can significantly improve inference speed by reducing the number of operations and memory accesses required during forward passes.

The fusion process typically involves folding the BatchNorm2d parameters (mean, variance, weight, and bias) into the preceding Conv2d layer's weights and biases. This results in a single Conv2d layer that performs both convolution and normalization in one step.

Display model information.

This method provides an overview or detailed information about the model, depending on the arguments passed. It can control the verbosity of the output and return the information as a list.

Check if the provided model is an Ultralytics HUB model.

This static method determines whether the given model string represents a valid Ultralytics HUB model identifier.

Check if the given model string is a Triton Server URL.

This static method determines whether the provided model string represents a valid Triton Server URL by parsing its components using urllib.parse.urlsplit().

Load parameters from the specified weights file into the model.

This method supports loading weights from a file or directly from a weights object. It matches parameters by name and shape and transfers them to the model.

Perform predictions on the given image source using the YOLO model.

This method facilitates the prediction process, allowing various configurations through keyword arguments. It supports predictions with custom predictors or the default predictor method. The method handles different types of image sources and can operate in a streaming mode.

Reset all callbacks to their default functions.

This method reinstates the default callback functions for all events, removing any custom callbacks that were previously added. It iterates through all default callback events and replaces the current callbacks with the default ones.

The default callbacks are defined in the 'callbacks.default_callbacks' dictionary, which contains predefined functions for various events in the model's lifecycle, such as on_train_start, on_epoch_end, etc.

This method is useful when you want to revert to the original set of callbacks after making custom modifications, ensuring consistent behavior across different runs or experiments.

Reset the model's weights to their initial state.

This method iterates through all modules in the model and resets their parameters if they have a 'reset_parameters' method. It also ensures that all parameters have 'requires_grad' set to True, enabling them to be updated during training.

Save the current model state to a file.

This method exports the model's checkpoint (ckpt) to the specified filename. It includes metadata such as the date, Ultralytics version, license information, and a link to the documentation.

Conduct object tracking on the specified input source using the registered trackers.

This method performs object tracking using the model's predictors and optionally registered trackers. It handles various input sources such as file paths or video streams, and supports customization through keyword arguments. The method registers trackers if not already present and can persist them between calls.

Train the model using the specified dataset and training configuration.

This method facilitates model training with a range of customizable settings. It supports training with a custom trainer or the default training approach. The method handles scenarios such as resuming training from a checkpoint, integrating with Ultralytics HUB, and updating model and configuration after training.

When using Ultralytics HUB, if the session has a loaded model, the method prioritizes HUB training arguments and warns if local arguments are provided. It checks for pip updates and combines default configurations, method-specific defaults, and user-provided arguments to configure the training process.

Conduct hyperparameter tuning for the model, with an option to use Ray Tune.

This method supports two modes of hyperparameter tuning: using Ray Tune or a custom tuning method. When Ray Tune is enabled, it leverages the 'run_ray_tune' function from the ultralytics.utils.tuner module. Otherwise, it uses the internal 'Tuner' class for tuning. The method combines default, overridden, and custom arguments to configure the tuning process.

Validate the model using a specified dataset and validation configuration.

This method facilitates the model validation process, allowing for customization through various settings. It supports validation with a custom validator or the default validation approach. The method combines default configurations, method-specific defaults, and user-provided arguments to configure the validation process.

**Examples:**

Example 1 (typescript):
```typescript
Model(self, model: str | Path | Model = "yolo11n.pt", task: str | None = None, verbose: bool = False) -> None
```

Example 2 (python):
```python
>>> from ultralytics import YOLO
>>> model = YOLO("yolo11n.pt")
>>> results = model.predict("image.jpg")
>>> model.train(data="coco8.yaml", epochs=3)
>>> metrics = model.val()
>>> model.export(format="onnx")
```

Example 3 (python):
```python
class Model(torch.nn.Module):
    """A base class for implementing YOLO models, unifying APIs across different model types.

    This class provides a common interface for various operations related to YOLO models, such as training, validation,
    prediction, exporting, and benchmarking. It handles different types of models, including those loaded from local
    files, Ultralytics HUB, or Triton Server.

    Attributes:
        callbacks (dict): A dictionary of callback functions for various events during model operations.
        predictor (BasePredictor): The predictor object used for making predictions.
        model (torch.nn.Module): The underlying PyTorch model.
        trainer (BaseTrainer): The trainer object used for training the model.
        ckpt (dict): The checkpoint data if the model is loaded from a *.pt file.
        cfg (str): The configuration of the model if loaded from a *.yaml file.
        ckpt_path (str): The path to the checkpoint file.
        overrides (dict): A dictionary of overrides for model configuration.
        metrics (dict): The latest training/validation metrics.
        session (HUBTrainingSession): The Ultralytics HUB session, if applicable.
        task (str): The type of task the model is intended for.
        model_name (str): The name of the model.

    Methods:
        __call__: Alias for the predict method, enabling the model instance to be callable.
        _new: Initialize a new model based on a configuration file.
        _load: Load a model from a checkpoint file.
        _check_is_pytorch_model: Ensure that the model is a PyTorch model.
        reset_weights: Reset the model's weights to their initial state.
        load: Load model weights from a specified file.
        save: Save the current state of the model to a file.
        info: Log or return information about the model.
        fuse: Fuse Conv2d and BatchNorm2d layers for optimized inference.
        predict: Perform object detection predictions.
        track: Perform object tracking.
        val: Validate the model on a dataset.
        benchmark: Benchmark the model on various export formats.
        export: Export the model to different formats.
        train: Train the model on a dataset.
        tune: Perform hyperparameter tuning.
        _apply: Apply a function to the model's tensors.
        add_callback: Add a callback function for an event.
        clear_callback: Clear all callbacks for an event.
        reset_callbacks: Reset all callbacks to their default functions.

    Examples:
        >>> from ultralytics import YOLO
        >>> model = YOLO("yolo11n.pt")
        >>> results = model.predict("image.jpg")
        >>> model.train(data="coco8.yaml", epochs=3)
        >>> metrics = model.val()
        >>> model.export(format="onnx")
    """

    def __init__(
        self,
        model: str | Path | Model = "yolo11n.pt",
        task: str | None = None,
        verbose: bool = False,
    ) -> None:
        """Initialize a new instance of the YOLO model class.

        This constructor sets up the model based on the provided model path or name. It handles various types of model
        sources, including local files, Ultralytics HUB models, and Triton Server models. The method initializes several
        important attributes of the model and prepares it for operations like training, prediction, or export.

        Args:
            model (str | Path | Model): Path or name of the model to load or create. Can be a local file path, a model
                name from Ultralytics HUB, a Triton Server model, or an already initialized Model instance.
            task (str, optional): The specific task for the model. If None, it will be inferred from the config.
            verbose (bool): If True, enables verbose output during the model's initialization and subsequent operations.

        Raises:
            FileNotFoundError: If the specified model file does not exist or is inaccessible.
            ValueError: If the model file or configuration is invalid or unsupported.
            ImportError: If required dependencies for specific model types (like HUB SDK) are not installed.
        """
        if isinstance(model, Model):
            self.__dict__ = model.__dict__  # accepts an already initialized Model
            return
        super().__init__()
        self.callbacks = callbacks.get_default_callbacks()
        self.predictor = None  # reuse predictor
        self.model = None  # model object
        self.trainer = None  # trainer object
        self.ckpt = {}  # if loaded from *.pt
        self.cfg = None  # if loaded from *.yaml
        self.ckpt_path = None
        self.overrides = {}  # overrides for trainer object
        self.metrics = None  # validation/training metrics
        self.session = None  # HUB session
        self.task = task  # task type
        self.model_name = None  # model name
        model = str(model).strip()

        # Check if Ultralytics HUB model from https://hub.ultralytics.com
        if self.is_hub_model(model):
            from ultralytics.hub import HUBTrainingSession

            # Fetch model from HUB
            checks.check_requirements("hub-sdk>=0.0.12")
            session = HUBTrainingSession.create_session(model)
            model = session.model_file
            if session.train_args:  # training sent from HUB
                self.session = session

        # Check if Triton Server model
        elif self.is_triton_model(model):
            self.model_name = self.model = model
            self.overrides["task"] = task or "detect"  # set `task=detect` if not explicitly set
            return

        # Load or create new YOLO model
        __import__("os").environ["CUBLAS_WORKSPACE_CONFIG"] = ":4096:8"  # to avoid deterministic warnings
        if str(model).endswith((".yaml", ".yml")):
            self._new(model, task=task, verbose=verbose)
        else:
            self._load(model, task=task)

        # Delete super().training for accessing self.model.training
        del self.training
```

Example 4 (python):
```python
def names(self) -> dict[int, str]
```

---

## Reference for ultralytics/nn/modules/head.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/nn/modules/head/

**Contents:**
- Reference for ultralytics/nn/modules/head.py
- class ultralytics.nn.modules.head.Detect
  - method ultralytics.nn.modules.head.Detect._inference
  - method ultralytics.nn.modules.head.Detect.bias_init
  - method ultralytics.nn.modules.head.Detect.decode_bboxes
  - method ultralytics.nn.modules.head.Detect.forward
  - method ultralytics.nn.modules.head.Detect.forward_end2end
  - method ultralytics.nn.modules.head.Detect.postprocess
- class ultralytics.nn.modules.head.Segment
  - method ultralytics.nn.modules.head.Segment.forward

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/nn/modules/head.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

YOLO Detect head for object detection models.

This class implements the detection head used in YOLO models for predicting bounding boxes and class probabilities. It supports both training and inference modes, with optional end-to-end detection capabilities.

Decode predicted bounding boxes and class probabilities based on multiple-level feature maps.

Initialize Detect() biases, WARNING: requires stride availability.

Decode bounding boxes from predictions.

Concatenate and return predicted bounding boxes and class probabilities.

Perform forward pass of the v10Detect module.

Post-process YOLO model predictions.

YOLO Segment head for segmentation models.

This class extends the Detect head to include mask prediction capabilities for instance segmentation tasks.

Return model outputs and mask coefficients if training, otherwise return outputs and mask coefficients.

YOLO OBB detection head for detection with rotation models.

This class extends the Detect head to include oriented bounding box prediction with rotation angles.

Decode rotated bounding boxes.

Concatenate and return predicted bounding boxes and class probabilities.

YOLO Pose head for keypoints models.

This class extends the Detect head to include keypoint prediction capabilities for pose estimation tasks.

Perform forward pass through YOLO model and return predictions.

Decode keypoints from predictions.

YOLO classification head, i.e. x(b,c1,20,20) to x(b,c2).

This class implements a classification head that transforms feature maps into class predictions.

Perform forward pass of the YOLO model on input image data.

Head for integrating YOLO detection models with semantic understanding from text embeddings.

This class extends the standard Detect head to incorporate text embeddings for enhanced semantic understanding in object detection tasks.

Initialize Detect() biases, WARNING: requires stride availability.

Concatenate and return predicted bounding boxes and class probabilities.

Lightweight Region Proposal and Classification Head for efficient object detection.

This head combines region proposal filtering with classification to enable efficient detection with dynamic vocabulary support.

Convert a 1x1 convolutional layer to a linear layer.

Process classification and localization features to generate detection proposals.

Head for integrating YOLO detection models with semantic understanding from text embeddings.

This class extends the standard Detect head to support text-guided detection with enhanced semantic understanding through text embeddings and visual prompt embeddings.

Initialize biases for detection heads.

Process features with class prompt embeddings to generate detections.

Process features with fused text embeddings to generate detections for prompt-free model.

Fuse text features with model weights for efficient inference.

Get text prompt embeddings with normalization.

Get visual prompt embeddings with spatial awareness.

YOLO segmentation head with text embedding capabilities.

This class extends YOLOEDetect to include mask prediction capabilities for instance segmentation tasks with text-guided semantic understanding.

Return model outputs and mask coefficients if training, otherwise return outputs and mask coefficients.

Real-Time Deformable Transformer Decoder (RTDETRDecoder) module for object detection.

This decoder module utilizes Transformer architecture along with deformable convolutions to predict bounding boxes and class labels for objects in an image. It integrates features from multiple layers and runs through a series of Transformer decoder layers to output the final predictions.

Generate anchor bounding boxes for given shapes with specific grid size and validate them.

Generate and prepare the input required for the decoder from the provided features and shapes.

Process and return encoder inputs by getting projection features from input and concatenating them.

Initialize or reset the parameters of the model's various components with predefined weights and biases.

Run the forward pass of the module, returning bounding box and classification scores for the input.

v10 Detection head from https://arxiv.org/pdf/2405.14458.

This class implements the YOLOv10 detection head with dual-assignment training and consistent dual predictions for improved efficiency and performance.

Remove the one2many head for inference optimization.

**Examples:**

Example 1 (typescript):
```typescript
Detect(self, nc: int = 80, ch: tuple = ())
```

Example 2 (unknown):
```unknown
Create a detection head for 80 classes
>>> detect = Detect(nc=80, ch=(256, 512, 1024))
>>> x = [torch.randn(1, 256, 80, 80), torch.randn(1, 512, 40, 40), torch.randn(1, 1024, 20, 20)]
>>> outputs = detect(x)
```

Example 3 (python):
```python
class Detect(nn.Module):
    """YOLO Detect head for object detection models.

    This class implements the detection head used in YOLO models for predicting bounding boxes and class probabilities.
    It supports both training and inference modes, with optional end-to-end detection capabilities.

    Attributes:
        dynamic (bool): Force grid reconstruction.
        export (bool): Export mode flag.
        format (str): Export format.
        end2end (bool): End-to-end detection mode.
        max_det (int): Maximum detections per image.
        shape (tuple): Input shape.
        anchors (torch.Tensor): Anchor points.
        strides (torch.Tensor): Feature map strides.
        legacy (bool): Backward compatibility for v3/v5/v8/v9 models.
        xyxy (bool): Output format, xyxy or xywh.
        nc (int): Number of classes.
        nl (int): Number of detection layers.
        reg_max (int): DFL channels.
        no (int): Number of outputs per anchor.
        stride (torch.Tensor): Strides computed during build.
        cv2 (nn.ModuleList): Convolution layers for box regression.
        cv3 (nn.ModuleList): Convolution layers for classification.
        dfl (nn.Module): Distribution Focal Loss layer.
        one2one_cv2 (nn.ModuleList): One-to-one convolution layers for box regression.
        one2one_cv3 (nn.ModuleList): One-to-one convolution layers for classification.

    Methods:
        forward: Perform forward pass and return predictions.
        forward_end2end: Perform forward pass for end-to-end detection.
        bias_init: Initialize detection head biases.
        decode_bboxes: Decode bounding boxes from predictions.
        postprocess: Post-process model predictions.

    Examples:
        Create a detection head for 80 classes
        >>> detect = Detect(nc=80, ch=(256, 512, 1024))
        >>> x = [torch.randn(1, 256, 80, 80), torch.randn(1, 512, 40, 40), torch.randn(1, 1024, 20, 20)]
        >>> outputs = detect(x)
    """

    dynamic = False  # force grid reconstruction
    export = False  # export mode
    format = None  # export format
    end2end = False  # end2end
    max_det = 300  # max_det
    shape = None
    anchors = torch.empty(0)  # init
    strides = torch.empty(0)  # init
    legacy = False  # backward compatibility for v3/v5/v8/v9 models
    xyxy = False  # xyxy or xywh output

    def __init__(self, nc: int = 80, ch: tuple = ()):
        """Initialize the YOLO detection layer with specified number of classes and channels.

        Args:
            nc (int): Number of classes.
            ch (tuple): Tuple of channel sizes from backbone feature maps.
        """
        super().__init__()
        self.nc = nc  # number of classes
        self.nl = len(ch)  # number of detection layers
        self.reg_max = 16  # DFL channels (ch[0] // 16 to scale 4/8/12/16/20 for n/s/m/l/x)
        self.no = nc + self.reg_max * 4  # number of outputs per anchor
        self.stride = torch.zeros(self.nl)  # strides computed during build
        c2, c3 = max((16, ch[0] // 4, self.reg_max * 4)), max(ch[0], min(self.nc, 100))  # channels
        self.cv2 = nn.ModuleList(
            nn.Sequential(Conv(x, c2, 3), Conv(c2, c2, 3), nn.Conv2d(c2, 4 * self.reg_max, 1)) for x in ch
        )
        self.cv3 = (
            nn.ModuleList(nn.Sequential(Conv(x, c3, 3), Conv(c3, c3, 3), nn.Conv2d(c3, self.nc, 1)) for x in ch)
            if self.legacy
            else nn.ModuleList(
                nn.Sequential(
                    nn.Sequential(DWConv(x, x, 3), Conv(x, c3, 1)),
                    nn.Sequential(DWConv(c3, c3, 3), Conv(c3, c3, 1)),
                    nn.Conv2d(c3, self.nc, 1),
                )
                for x in ch
            )
        )
        self.dfl = DFL(self.reg_max) if self.reg_max > 1 else nn.Identity()

        if self.end2end:
            self.one2one_cv2 = copy.deepcopy(self.cv2)
            self.one2one_cv3 = copy.deepcopy(self.cv3)
```

Example 4 (python):
```python
def _inference(self, x: list[torch.Tensor]) -> torch.Tensor
```

---

## 使用 Ultralytics YOLO 进行模型预测 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/modes/predict/

**Contents:**
- 使用 Ultralytics YOLO 进行模型预测
- 简介
- 实际应用
- 为什么使用 Ultralytics YOLO 进行推理？
  - 预测模式的主要特性
- 推理来源
- 推理参数
- 图像和视频格式
  - 图像
  - 视频

在机器学习和计算机视觉领域，对视觉数据进行理解的过程通常被称为推理或预测。Ultralytics YOLO11提供了一个强大的功能，即预测模式，专为各种数据源上的高性能实时推理而设计。

观看： 如何从 Ultralytics YOLO11 任务中提取结果用于自定义项目 🚀

以下是您应考虑使用 YOLO11 预测模式来满足各种推理需求的原因：

YOLO11 的预测模式设计强大且用途广泛，具有以下特点：

Ultralytics YOLO 模型返回一个 Python 对象列表 Results 对象或内存高效的生成器 Results 当 stream=True 在推理期间传递给模型时：

如下表所示，YOLO11 可以处理不同类型的输入源以进行推理。这些源包括静态图像、视频流和各种数据格式。该表还指示了每个源是否可以在流模式下与参数一起使用 stream=True ✅。流模式有利于处理视频或直播流，因为它会创建一个结果生成器，而不是将所有帧加载到内存中。

使用 stream=True 用于处理长视频或大型数据集，以有效管理内存。当 stream=False时，所有帧或数据点的结果都存储在内存中，这会迅速累积，并导致大型输入出现内存不足错误。相反， stream=True 采用生成器，仅将当前帧或数据点的结果保存在内存中，从而显著降低内存消耗并防止内存溢出问题。

通过 URL 对远程托管的图像或视频运行推理。

对使用 python 图像库 (PIL) 打开的图像运行推理。

对表示为 numpy 数组的图像运行推理。

对表示为 PyTorch tensor 的图像运行推理。

对CSV文件中列出的图像、URL、视频和目录集合运行推理。

对视频文件运行推理。通过使用 stream=True，您可以创建一个 Results 对象生成器来减少内存使用。

对目录中的所有图像和视频运行推理。要包含子目录中的资产，请使用全局模式，例如 path/to/dir/**/*.

对所有与 glob 表达式匹配的图像和视频运行推理 * 字符。

在 YouTube 视频上运行推理。通过使用 stream=True，您可以创建一个 Results 对象生成器，以减少长视频的内存使用量。

使用流模式，通过 RTSP、RTMP、TCP 或 IP 地址协议在实时视频流上运行推理。如果提供单个流，模型将以 批次大小 为 1 的 batch-size 运行推理。对于多个流，可以使用 .streams 文本文件来执行批量推理，其中批量大小由提供的流的数量决定（例如，8 个流的批量大小为 8）。

对于单流使用，批量大小默认设置为 1，从而可以高效地实时处理视频流。

要同时处理多个视频流，请使用包含流媒体源的 .streams 文本文件，每行包含一个源。模型将运行批处理推理，其中批处理大小等于流的数量。这种设置能够高效地并发处理多个数据流。

文件中的每一行代表一个流媒体源，允许您同时监控多个视频流并对其执行推理。

您可以通过将特定摄像头的索引传递给 source.

model.predict() 来在连接的摄像头设备上运行推理。

Ultralytics 默认在推理过程中使用最小填充（rect=True）。在此模式下，每张图像的较短边仅填充到足以被模型最大步幅整除的程度，而不是一直填充到完整的 imgsz。当在一批图像上运行推理时，最小填充仅在所有图像具有相同大小时才有效。否则，图像将被统一填充为正方形，且两边都等于 imgsz.

YOLO11 支持各种图像和视频格式，如 ultralytics/data/utils.py 中所指定。请参见下表，了解有效的后缀和示例预测命令。

下表包含有效的 Ultralytics 图像格式。

下表包含有效的 Ultralytics 视频格式。

所有 Ultralytics predict() 调用将返回一个 Results 对象列表：

有关更多详细信息，请参见 Results 类文档.

Boxes 对象可用于索引、操作和将边界框转换为不同的格式。

以下是 Boxes 类的方法和属性表，包括它们的名称、类型和描述：

有关更多详细信息，请参见 Boxes 类文档.

Masks 对象可用于索引、操作和将掩码转换为分割。

以下是 Masks 类的方法和属性表，包括它们的名称、类型和描述：

有关更多详细信息，请参见 Masks 类文档.

Keypoints 对象可用于索引、操作和归一化坐标。

以下是 Keypoints 类的方法和属性表，包括它们的名称、类型和描述：

有关更多详细信息，请参见 Keypoints 类文档.

Probs 对象可用于索引、获取 top1 和 top5 分类的索引和分数。

下表总结了以下方法的属性： Probs 函数：

有关更多详细信息，请参见 Probs 类文档.

OBB 对象可用于索引、操作和转换有向边界框为不同的格式。

以下是 OBB 类的方法和属性表，包括它们的名称、类型和描述：

有关更多详细信息，请参见 OBB 类文档.

字段 plot() 方法在 Results 对象有助于通过将检测到的对象（例如边界框、掩码、关键点和概率）叠加到原始图像上来可视化预测。此方法将带注释的图像作为 NumPy 数组返回，便于显示或保存。

字段 plot() 方法支持各种参数来自定义输出：

当您在不同线程中并行运行多个 YOLO 模型时，确保推理过程中的线程安全至关重要。线程安全推理保证每个线程的预测都是隔离的，不会相互干扰，从而避免竞争条件，并确保输出的一致性和可靠性。

在多线程应用程序中使用 YOLO 模型时，为每个线程实例化单独的模型对象或采用线程局部存储以防止冲突非常重要：

在每个线程内实例化一个模型，以实现线程安全推理：

要深入了解 YOLO 模型的线程安全推理以及分步说明，请参阅我们的 YOLO 线程安全推理指南。本指南将为您提供避免常见陷阱并确保多线程推理顺利运行所需的所有信息。

这是一个使用OpenCV的Python脚本（cv2) 和 YOLO 运行视频帧推理的 python 脚本。此脚本假定您已安装必要的软件包 (opencv-python 和 ultralytics）。

此脚本将对视频的每一帧运行预测，可视化结果，并在窗口中显示它们。可以通过按“q”退出循环。

Ultralytics YOLO 是一种最先进的实时目标检测、分割和分类模型。它的 predict 模式 允许用户对各种数据源（如图像、视频和直播流）执行高速推理。它专为性能和多功能性而设计，还提供批量处理和流式传输模式。有关其功能的更多详细信息，请查看Ultralytics YOLO predict 模式。

Ultralytics YOLO 可以处理各种数据源，包括单个图像、视频、目录、URL 和流。您可以在以下位置指定数据源 model.predict() 调用。例如，使用 'image.jpg' 用于本地图像或 'https://ultralytics.com/images/bus.jpg' 用于 URL。查看各种 推理来源 的详细示例，请参阅文档。

要优化推理速度并有效地管理内存，您可以通过设置 stream=True 在 predictor 的调用方法中使用流式传输模式。流式传输模式生成一个内存高效的 Results 对象生成器，而不是将所有帧加载到内存中。对于处理长视频或大型数据集，流式传输模式特别有用。了解更多关于 流式传输模式.

字段 model.predict() 方法在 YOLO 中支持各种参数，例如 conf, iou, imgsz, device等等。这些参数允许您自定义推理过程，设置置信度阈值、图像大小和用于计算的设备等参数。有关这些参数的详细说明，请参阅 推理参数 部分。

在使用 YOLO 运行推理后， Results 对象包含用于显示和保存带注释图像的方法。您可以使用诸如 result.show() 和 result.save(filename="result.jpg") 等方法来可视化和保存结果。有关这些方法的完整列表，请参阅 处理结果 部分。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Load a model
model = YOLO("yolo11n.pt")  # pretrained YOLO11n model

# Run batched inference on a list of images
results = model(["image1.jpg", "image2.jpg"])  # return a list of Results objects

# Process results list
for result in results:
    boxes = result.boxes  # Boxes object for bounding box outputs
    masks = result.masks  # Masks object for segmentation masks outputs
    keypoints = result.keypoints  # Keypoints object for pose outputs
    probs = result.probs  # Probs object for classification outputs
    obb = result.obb  # Oriented boxes object for OBB outputs
    result.show()  # display to screen
    result.save(filename="result.jpg")  # save to disk
```

Example 2 (python):
```python
from ultralytics import YOLO

# Load a model
model = YOLO("yolo11n.pt")  # pretrained YOLO11n model

# Run batched inference on a list of images
results = model(["image1.jpg", "image2.jpg"], stream=True)  # return a generator of Results objects

# Process results generator
for result in results:
    boxes = result.boxes  # Boxes object for bounding box outputs
    masks = result.masks  # Masks object for segmentation masks outputs
    keypoints = result.keypoints  # Keypoints object for pose outputs
    probs = result.probs  # Probs object for classification outputs
    obb = result.obb  # Oriented boxes object for OBB outputs
    result.show()  # display to screen
    result.save(filename="result.jpg")  # save to disk
```

Example 3 (python):
```python
from ultralytics import YOLO

# Load a pretrained YOLO11n model
model = YOLO("yolo11n.pt")

# Define path to the image file
source = "path/to/image.jpg"

# Run inference on the source
results = model(source)  # list of Results objects
```

Example 4 (python):
```python
from ultralytics import YOLO

# Load a pretrained YOLO11n model
model = YOLO("yolo11n.pt")

# Define current screenshot as source
source = "screen"

# Run inference on the source
results = model(source)  # list of Results objects
```

---

---

## 🚀 YOLO11 推理高级优化指南

### 目录
- [YOLO11 推理核心优化](#yolo11-推理核心优化)
- [性能调优参数详解](#性能调优参数详解)
- [实际应用场景](#实际应用场景)
- [多线程与批量推理](#多线程与批量推理)
- [结果处理最佳实践](#结果处理最佳实践)

### YOLO11 推理核心优化

YOLO11 在推理性能方面相比 YOLOv8 有显著提升，主要体现在以下几个方面：

1. **更高效的架构设计**：优化的 C3k2 模块和 SPPF 模块
2. **改进的 NMS 策略**：更快的后处理速度
3. **更好的内存管理**：支持更大的批次大小
4. **增强的量化支持**：FP16/INT8 推理优化

#### YOLO11 推理完整示例

```python
from ultralytics import YOLO
import cv2
import numpy as np
from pathlib import Path

class YOLO11InferenceEngine:
    """YOLO11 推理引擎 - 封装推理逻辑，提供易用接口"""
    
    def __init__(self, model_path="yolo11n.pt", device="cuda", conf=0.25, iou=0.45):
        """
        初始化 YOLO11 推理引擎
        
        Args:
            model_path (str): 模型路径，支持 .pt, .onnx, .engine 等格式
            device (str): 推理设备 ('cuda', 'cpu', 'mps')
            conf (float): 置信度阈值，过滤低置信度检测
            iou (float): NMS IOU 阈值，控制重叠框抑制
        """
        self.model = YOLO(model_path)
        self.model.to(device)
        self.conf = conf
        self.iou = iou
        self.device = device
        
        # 预热模型 - 首次推理较慢，预热后速度稳定
        dummy_input = np.zeros((640, 640, 3), dtype=np.uint8)
        _ = self.model.predict(dummy_input, verbose=False)
    
    def predict_single(self, image_path, **kwargs):
        """
        单张图像推理
        
        Args:
            image_path (str): 图像路径
            **kwargs: 额外的预测参数
            
        Returns:
            Results: YOLO11 推理结果对象
        """
        # 设置默认参数
        kwargs.setdefault('conf', self.conf)
        kwargs.setdefault('iou', self.iou)
        kwargs.setdefault('verbose', False)
        
        results = self.model.predict(image_path, **kwargs)
        return results[0] if results else None
    
    def predict_batch(self, image_paths, batch_size=8, **kwargs):
        """
        批量图像推理 - 优化内存使用
        
        Args:
            image_paths (list): 图像路径列表
            batch_size (int): 批次大小，根据显存调整
            **kwargs: 额外的预测参数
            
        Yields:
            Results: 每张图像的推理结果
        """
        kwargs.setdefault('conf', self.conf)
        kwargs.setdefault('iou', self.iou)
        kwargs.setdefault('stream', True)  # 使用流式处理节省内存
        kwargs.setdefault('verbose', False)
        
        results = self.model.predict(image_paths, **kwargs)
        for result in results:
            yield result
    
    def predict_video(self, video_path, output_path=None, show=False, **kwargs):
        """
        视频流推理 - 支持实时处理
        
        Args:
            video_path (str): 视频文件路径或摄像头索引 (0, 1, ...)
            output_path (str, optional): 输出视频路径
            show (bool): 是否显示实时画面
            **kwargs: 额外的预测参数
            
        Yields:
            tuple: (frame, results) 每帧及其检测结果
        """
        cap = cv2.VideoCapture(video_path)
        
        # 视频写入器
        video_writer = None
        if output_path:
            fps = int(cap.get(cv2.CAP_PROP_FPS))
            width = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
            height = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))
            fourcc = cv2.VideoWriter_fourcc(*'mp4v')
            video_writer = cv2.VideoWriter(output_path, fourcc, fps, (width, height))
        
        kwargs.setdefault('conf', self.conf)
        kwargs.setdefault('iou', self.iou)
        kwargs.setdefault('verbose', False)
        kwargs.setdefault('stream', True)
        
        try:
            while cap.isOpened():
                success, frame = cap.read()
                if not success:
                    break
                
                # 推理
                results = self.model.predict(frame, **kwargs)
                
                # 可视化
                annotated_frame = results[0].plot() if results else frame
                
                if show:
                    cv2.imshow('YOLO11 Inference', annotated_frame)
                    if cv2.waitKey(1) & 0xFF == ord('q'):
                        break
                
                if video_writer:
                    video_writer.write(annotated_frame)
                
                yield frame, results[0] if results else None
                
        finally:
            cap.release()
            if video_writer:
                video_writer.release()
            cv2.destroyAllWindows()

# 使用示例
if __name__ == "__main__":
    # 初始化推理引擎
    engine = YOLO11InferenceEngine(
        model_path="yolo11n.pt",
        device="cuda",
        conf=0.35,
        iou=0.5
    )
    
    # 1. 单张图像推理
    result = engine.predict_single("test.jpg")
    if result:
        print(f"检测到 {len(result.boxes)} 个对象")
        result.save("output.jpg")
    
    # 2. 批量推理
    image_list = ["img1.jpg", "img2.jpg", "img3.jpg"]
    for result in engine.predict_batch(image_list, batch_size=4):
        print(f"处理完成: {result.path}")
    
    # 3. 视频推理
    for frame, result in engine.predict_video("video.mp4", output_path="output.mp4"):
        if result and len(result.boxes) > 0:
            print(f"帧检测到 {len(result.boxes)} 个对象")
```

### 性能调优参数详解

YOLO11 提供丰富的推理参数，合理配置可显著提升性能：

#### 核心推理参数表

| 参数 | 类型 | 默认值 | 说明 | 推荐值 |
|------|------|--------|------|--------|
| **conf** | float | 0.25 | 置信度阈值，过滤低置信度检测 | 0.25-0.50 |
| **iou** | float | 0.45 | NMS IOU 阈值，控制重叠框抑制 | 0.40-0.60 |
| **imgsz** | int | 640 | 输入图像尺寸 | 640/1280/按需调整 |
| **device** | str | None | 推理设备 (cuda/cpu/mps) | cuda (如有GPU) |
| **half** | bool | False | 是否使用 FP16 半精度推理 | True (加速推理) |
| **max_det** | int | 300 | 每张图像最大检测数 | 100-500 |
| **vid_stride** | int | 1 | 视频帧采样步长 | 1-4 (加速视频处理) |
| **stream** | bool | False | 是否使用流式处理 | True (大视频/批量) |
| **visualize** | bool | False | 是否可视化特征图 | False (调试时设为True) |
| **augment** | bool | False | 推理时是否使用数据增强 | False (精度优先) |
| **agnostic_nms** | bool | False | 类别无关的 NMS | False (多类场景设为True) |
| **classes** | list | None | 只检测指定类别 | [0, 1, 2] 等 |
| **retina_masks** | bool | False | 高分辨率分割掩码 | True (分割任务) |

#### 性能优化代码示例

```python
from ultralytics import YOLO

# 高速推理模式（牺牲少量精度换取速度）
model = YOLO("yolo11n.pt")

# 场景1: 实时应用（监控、直播）
results = model.predict(
    source="rtsp://stream_url",
    conf=0.3,          # 降低阈值提高召回率
    imgsz=640,         # 使用较小尺寸
    half=True,         # FP16 加速
    device="cuda",     # GPU 加速
    stream=True,       # 流式处理
    vid_stride=2       # 跳帧处理，提升2倍速度
)

# 场景2: 高精度检测（医学图像、质检）
results = model.predict(
    source="high_res_image.jpg",
    conf=0.5,          # 提高阈值减少误检
    imgsz=1280,        # 使用大尺寸
    augment=True,      # 启用增强提升精度
    iou=0.5,           # 更严格的 NMS
    max_det=100        # 限制检测数量
)

# 场景3: 特定类别检测（交通场景只检测车辆）
results = model.predict(
    source="traffic_video.mp4",
    classes=[2, 3, 5, 7],  # COCO数据集中的车辆类别
    conf=0.4,
    agnostic_nms=True     # 类别无关NMS，避免车辆间相互抑制
)

# 场景4: 边缘设备优化（Jetson、树莓派）
results = model.predict(
    source="edge_camera",
    imgsz=416,          # 降低输入尺寸
    half=True,          # FP16 推理
    device="cpu",       # 根据设备选择
    max_det=50          # 减少输出数量
)
```

### 实际应用场景

#### 场景1: 智能安防系统

```python
import cv2
from datetime import datetime
from ultralytics import YOLO

class SecurityMonitor:
    """智能安防监控系统 - 支持人员入侵检测"""
    
    def __init__(self, source, output_dir="alerts"):
        self.model = YOLO("yolo11n.pt")
        self.source = source
        self.output_dir = Path(output_dir)
        self.output_dir.mkdir(exist_ok=True)
        
        # COCO 数据集中 person 类别索引为 0
        self.target_classes = [0]  # 只检测人
        self.alert_threshold = 3   # 检测到3人以上触发警报
        
    def monitor(self, show=False):
        """持续监控并保存异常画面"""
        cap = cv2.VideoCapture(self.source)
        
        while cap.isOpened():
            success, frame = cap.read()
            if not success:
                break
            
            # 推理 - 只检测人
            results = self.model.predict(
                frame,
                classes=self.target_classes,
                conf=0.4,
                verbose=False
            )[0]
            
            # 检测人数
            num_people = len(results.boxes) if results.boxes else 0
            
            # 绘制结果
            annotated = results.plot()
            
            # 添加人数统计
            cv2.putText(
                annotated,
                f"People: {num_people}",
                (10, 50),
                cv2.FONT_HERSHEY_SIMPLEX,
                1.5,
                (0, 255, 0) if num_people < self.alert_threshold else (0, 0, 255),
                3
            )
            
            # 异常警报
            if num_people >= self.alert_threshold:
                timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
                alert_path = self.output_dir / f"alert_{timestamp}.jpg"
                cv2.imwrite(str(alert_path), annotated)
                print(f"⚠️  警报: 检测到 {num_people} 人，已保存至 {alert_path}")
            
            if show:
                cv2.imshow("Security Monitor", annotated)
                if cv2.waitKey(1) & 0xFF == ord('q'):
                    break
        
        cap.release()
        cv2.destroyAllWindows()

# 使用示例
monitor = SecurityMonitor(source=0)  # 使用摄像头
monitor.monitor(show=True)
```

#### 场景2: 工业质检系统

```python
import cv2
import numpy as np
from ultralytics import YOLO

class QualityInspection:
    """工业质检系统 - 检测产品缺陷"""
    
    def __init__(self, model_path, defect_classes):
        """
        Args:
            defect_classes (dict): 缺陷类别映射 {类别名: COCO索引}
                例如: {"scratch": 0, "dent": 1, "crack": 2}
        """
        self.model = YOLO(model_path)
        self.defect_classes = defect_classes
        
    def inspect_product(self, image_path, quality_threshold=0.9):
        """
        检测产品缺陷
        
        Args:
            image_path: 产品图像路径
            quality_threshold: 质量阈值，低于此值判定为不合格
            
        Returns:
            dict: 检测结果 {
                "pass": bool,           # 是否合格
                "defects": list,        # 缺陷列表
                "quality_score": float  # 质量分数
            }
        """
        results = self.model.predict(
            image_path,
            conf=0.25,
            iou=0.5,
            verbose=False
        )[0]
        
        defects = []
        total_confidence = 0
        
        if results.boxes is not None:
            for box in results.boxes:
                cls_id = int(box.cls[0])
                conf = float(box.conf[0])
                
                # 获取缺陷名称
                defect_name = self._get_class_name(cls_id)
                
                # 获取边界框坐标
                xyxy = box.xyxy[0].cpu().numpy()
                
                defects.append({
                    "type": defect_name,
                    "confidence": conf,
                    "bbox": xyxy.tolist()
                })
                
                total_confidence += conf
        
        # 计算质量分数（无缺陷则满分，有缺陷则按置信度降低）
        num_defects = len(defects)
        if num_defects == 0:
            quality_score = 1.0
        else:
            quality_score = max(0, 1.0 - (total_confidence / num_defects))
        
        return {
            "pass": quality_score >= quality_threshold,
            "defects": defects,
            "quality_score": quality_score,
            "defect_count": num_defects
        }
    
    def _get_class_name(self, cls_id):
        """获取类别名称"""
        for name, idx in self.defect_classes.items():
            if idx == cls_id:
                return name
        return f"class_{cls_id}"

# 使用示例
inspector = QualityInspection(
    model_path="yolo11n.pt",
    defect_classes={
        "scratch": 0,   # 划痕
        "dent": 1,      # 凹陷
        "crack": 2      # 裂纹
    }
)

result = inspector.inspect_product("product_001.jpg")
if result["pass"]:
    print(f"✅ 产品合格 (质量分数: {result['quality_score']:.2f})")
else:
    print(f"❌ 产品不合格")
    print(f"   缺陷数量: {result['defect_count']}")
    for defect in result["defects"]:
        print(f"   - {defect['type']}: {defect['confidence']:.2f}")
```

#### 场景3: 实时交通流量统计

```python
import cv2
from collections import defaultdict
from ultralytics import YOLO

class TrafficCounter:
    """交通流量统计 - 统计车辆和行人数量"""
    
    # COCO 数据集类别
    # 2: car, 3: motorcycle, 5: bus, 7: truck, 0: person
    VEHICLE_CLASSES = {
        "car": 2,
        "motorcycle": 3,
        "bus": 5,
        "truck": 7,
        "person": 0
    }
    
    def __init__(self, source, roi_points=None):
        """
        Args:
            source: 视频源（文件路径或摄像头索引）
            roi_points: 感兴趣区域点列表 [(x1,y1), (x2,y2), ...]
        """
        self.model = YOLO("yolo11n.pt")
        self.source = source
        self.roi_points = roi_points
        
        # 统计数据
        self.counts = defaultdict(int)
        
    def count_traffic(self, duration_seconds=60):
        """统计指定时长的交通流量"""
        cap = cv2.VideoCapture(self.source)
        fps = int(cap.get(cv2.CAP_PROP_FPS))
        total_frames = fps * duration_seconds
        
        frame_count = 0
        
        while cap.isOpened() and frame_count < total_frames:
            success, frame = cap.read()
            if not success:
                break
            
            # 推理
            results = self.model.predict(
                frame,
                classes=list(self.VEHICLE_CLASSES.values()),
                conf=0.4,
                verbose=False
            )[0]
            
            # 统计
            if results.boxes is not None:
                for box in results.boxes:
                    cls_id = int(box.cls[0])
                    class_name = self._get_class_name(cls_id)
                    self.counts[class_name] += 1
            
            frame_count += 1
            
            # 可视化
            annotated = results.plot()
            self._draw_stats(annotated)
            cv2.imshow("Traffic Counter", annotated)
            
            if cv2.waitKey(1) & 0xFF == ord('q'):
                break
        
        cap.release()
        cv2.destroyAllWindows()
        
        return dict(self.counts)
    
    def _get_class_name(self, cls_id):
        """获取类别名称"""
        for name, idx in self.VEHICLE_CLASSES.items():
            if idx == cls_id:
                return name
        return "unknown"
    
    def _draw_stats(self, frame):
        """在画面上绘制统计信息"""
        y_offset = 50
        for vehicle_type, count in self.counts.items():
            text = f"{vehicle_type}: {count}"
            cv2.putText(
                frame,
                text,
                (10, y_offset),
                cv2.FONT_HERSHEY_SIMPLEX,
                1,
                (0, 255, 0),
                2
            )
            y_offset += 30

# 使用示例
counter = TrafficCounter(source="traffic_video.mp4")
counts = counter.count_traffic(duration_seconds=30)
print("\n📊 交通流量统计:")
for vehicle_type, count in counts.items():
    print(f"  {vehicle_type}: {count}")
```

### 多线程与批量推理

#### 多线程推理实现

```python
import threading
import queue
from ultralytics import YOLO
from pathlib import Path

class MultiThreadInference:
    """多线程 YOLO11 推理 - 适用于多摄像头场景"""
    
    def __init__(self, model_path, num_workers=4):
        self.model_path = model_path
        self.num_workers = num_workers
        self.task_queue = queue.Queue()
        self.result_queue = queue.Queue()
        self.workers = []
        
    def _worker(self):
        """工作线程 - 每个线程加载独立模型"""
        # 每个线程创建独立的模型实例（避免线程冲突）
        model = YOLO(self.model_path)
        
        while True:
            task = self.task_queue.get()
            if task is None:  # 毒丸，退出信号
                break
            
            image_path, task_id = task
            results = model.predict(image_path, verbose=False)
            self.result_queue.put((task_id, results))
            
    def start_workers(self):
        """启动工作线程"""
        for _ in range(self.num_workers):
            worker = threading.Thread(target=self._worker)
            worker.start()
            self.workers.append(worker)
    
    def stop_workers(self):
        """停止工作线程"""
        for _ in range(self.num_workers):
            self.task_queue.put(None)
        for worker in self.workers:
            worker.join()
    
    def process_images(self, image_paths):
        """批量处理图像"""
        # 启动工作线程
        self.start_workers()
        
        # 提交任务
        for idx, img_path in enumerate(image_paths):
            self.task_queue.put((img_path, idx))
        
        # 收集结果
        results = [None] * len(image_paths)
        for _ in range(len(image_paths)):
            task_id, result = self.result_queue.get()
            results[task_id] = result
        
        # 停止工作线程
        self.stop_workers()
        
        return results

# 使用示例
inference = MultiThreadInference("yolo11n.pt", num_workers=4)

# 准备图像列表
image_paths = list(Path("images").glob("*.jpg"))

# 批量处理
results = inference.process_images(image_paths)

for img_path, result in zip(image_paths, results):
    print(f"{img_path.name}: 检测到 {len(result[0].boxes)} 个对象")
```

### 结果处理最佳实践

```python
from ultralytics import YOLO
import pandas as pd
import json

class ResultProcessor:
    """YOLO11 结果处理器 - 提供多种输出格式"""
    
    def __init__(self, model_path):
        self.model = YOLO(model_path)
    
    def predict_and_export(self, source, output_dir="outputs"):
        """推理并导出多种格式结果"""
        output_path = Path(output_dir)
        output_path.mkdir(exist_ok=True)
        
        results = self.model.predict(source, verbose=False)
        
        for idx, result in enumerate(results):
            base_name = Path(result.path).stem
            
            # 1. 保存可视化图像
            result.save(str(output_path / f"{base_name}_annotated.jpg"))
            
            # 2. 导出为 JSON
            json_data = self._to_json(result)
            with open(output_path / f"{base_name}_result.json", "w") as f:
                json.dump(json_data, f, indent=2)
            
            # 3. 导出为 CSV
            if result.boxes is not None:
                df = self._to_dataframe(result)
                df.to_csv(output_path / f"{base_name}_result.csv", index=False)
    
    def _to_json(self, result):
        """转换为 JSON 格式"""
        data = {
            "image": result.path,
            "image_shape": result.orig_shape,
            "detections": []
        }
        
        if result.boxes is not None:
            for box in result.boxes:
                detection = {
                    "class_id": int(box.cls[0]),
                    "class_name": result.names[int(box.cls[0])],
                    "confidence": float(box.conf[0]),
                    "bbox_xyxy": box.xyxy[0].tolist(),
                    "bbox_xywh": box.xywh[0].tolist()
                }
                data["detections"].append(detection)
        
        return data
    
    def _to_dataframe(self, result):
        """转换为 Pandas DataFrame"""
        detections = []
        
        if result.boxes is not None:
            for box in result.boxes:
                detections.append({
                    "class_id": int(box.cls[0]),
                    "class_name": result.names[int(box.cls[0])],
                    "confidence": float(box.conf[0]),
                    "x1": float(box.xyxy[0][0]),
                    "y1": float(box.xyxy[0][1]),
                    "x2": float(box.xyxy[0][2]),
                    "y2": float(box.xyxy[0][3])
                })
        
        return pd.DataFrame(detections)

# 使用示例
processor = ResultProcessor("yolo11n.pt")
processor.predict_and_export("test_images/")
```

---

## 📚 YOLO11 推理常见问题解答

### Q1: YOLO11 与 YOLOv8 在推理上有何区别？

**答:** YOLO11 相比 YOLOv8 在推理方面有以下改进：

1. **更快的推理速度**：优化了模型架构，推理速度提升约 20-30%
2. **更低的内存占用**：改进了内存管理，支持更大批次
3. **更好的精度**：在相同速度下，mAP 提升 2-5%
4. **增强的量化支持**：FP16/INT8 推理性能更优

### Q2: 如何选择合适的模型大小？

**答:** 根据应用场景选择：

| 模型 | 参数量 | 适用场景 |
|------|--------|----------|
| yolo11n | 2.6M | 边缘设备、移动端、实时应用 |
| yolo11s | 9.4M | 一般服务器、桌面应用 |
| yolo11m | 20.1M | 高性能服务器、精度要求高 |
| yolo11l | 25.3M | 精度优先、服务器部署 |
| yolo11x | 56.9M | 最高精度、云端部署 |

### Q3: 如何优化视频推理速度？

**答:** 多种优化策略：

1. **使用流式处理**：`stream=True` 避免内存堆积
2. **调整采样率**：`vid_stride=2` 跳帧处理
3. **降低分辨率**：`imgsz=416` 减少计算量
4. **使用半精度**：`half=True` FP16 加速
5. **GPU 加速**：`device="cuda"` 利用 GPU

### Q4: 推理时显存不足怎么办？

**答:** 解决方案：

1. **减小批次大小**：`batch=1` 或更小
2. **降低输入尺寸**：`imgsz=416` 或更小
3. **使用半精度**：`half=True` 减少显存占用
4. **使用流式处理**：`stream=True` 逐帧处理
5. **使用更小模型**：yolo11n 代替 yolo11x

### Q5: 如何提升检测精度？

**答:** 提升精度的方法：

1. **提高置信度阈值**：`conf=0.5` 减少误检
2. **增大输入尺寸**：`imgsz=1280` 捕获更多细节
3. **启用数据增强**：`augment=True` 提升鲁棒性
4. **调整 IOU 阈值**：`iou=0.6` 更严格的 NMS
5. **使用更大模型**：yolo11x 精度最高

---

## 🎯 YOLO11 推理最佳实践总结

1. **模型选择**：根据硬件和精度要求选择合适的模型大小
2. **参数调优**：合理设置 conf、iou、imgsz 等关键参数
3. **内存管理**：大视频或批量图像使用 stream=True
4. **硬件加速**：优先使用 GPU，启用 half=True
5. **结果处理**：使用 Results 对象的方法提取所需信息
6. **性能监控**：使用 benchmark() 对比不同配置性能

---

*本优化指南基于 YOLO11 最新版本编写，涵盖实际项目中最常见的推理场景和优化策略。*


## Reference for ultralytics/utils/loss.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/loss/

**Contents:**
- Reference for ultralytics/utils/loss.py
- class ultralytics.utils.loss.VarifocalLoss
  - method ultralytics.utils.loss.VarifocalLoss.forward
- class ultralytics.utils.loss.FocalLoss
  - method ultralytics.utils.loss.FocalLoss.forward
- class ultralytics.utils.loss.DFLoss
  - method ultralytics.utils.loss.DFLoss.__call__
- class ultralytics.utils.loss.BboxLoss
  - method ultralytics.utils.loss.BboxLoss.forward
- class ultralytics.utils.loss.RotatedBboxLoss

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/loss.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Varifocal loss by Zhang et al.

Implements the Varifocal Loss function for addressing class imbalance in object detection by focusing on hard-to-classify examples and balancing positive/negative samples.

Compute varifocal loss between predictions and ground truth.

Wraps focal loss around existing loss_fcn(), i.e. criteria = FocalLoss(nn.BCEWithLogitsLoss(), gamma=1.5).

Implements the Focal Loss function for addressing class imbalance by down-weighting easy examples and focusing on hard negatives during training.

Calculate focal loss with modulating factors for class imbalance.

Criterion class for computing Distribution Focal Loss (DFL).

Return sum of left and right DFL losses from https://ieeexplore.ieee.org/document/9792391.

Criterion class for computing training losses for bounding boxes.

Compute IoU and DFL losses for bounding boxes.

Criterion class for computing training losses for rotated bounding boxes.

Compute IoU and DFL losses for rotated bounding boxes.

Criterion class for computing keypoint losses.

Calculate keypoint loss factor and Euclidean distance loss for keypoints.

Criterion class for computing training losses for YOLOv8 object detection.

Calculate the sum of the loss for box, cls and dfl multiplied by batch size.

Decode predicted object bounding box coordinates from anchor points and distribution.

Preprocess targets by converting to tensor format and scaling coordinates.

Bases: v8DetectionLoss

Criterion class for computing training losses for YOLOv8 segmentation.

Calculate and return the combined loss for detection and segmentation.

Calculate the loss for instance segmentation.

The batch loss can be computed for improved speed at higher memory usage. For example, pred_mask can be computed as follows: pred_mask = torch.einsum('in,nhw->ihw', pred, proto) # (i, 32) @ (32, 160, 160) -> (i, 160, 160)

Compute the instance segmentation loss for a single image.

The function uses the equation pred_mask = torch.einsum('in,nhw->ihw', pred, proto) to produce the predicted masks from the prototype masks and predicted mask coefficients.

Bases: v8DetectionLoss

Criterion class for computing training losses for YOLOv8 pose estimation.

Calculate the total loss and detach it for pose estimation.

Calculate the keypoints loss for the model.

This function calculates the keypoints loss and keypoints object loss for a given batch. The keypoints loss is based on the difference between the predicted keypoints and ground truth keypoints. The keypoints object loss is a binary classification loss that classifies whether a keypoint is present or not.

Decode predicted keypoints to image coordinates.

Criterion class for computing training losses for classification.

Compute the classification loss between predictions and true labels.

Bases: v8DetectionLoss

Calculates losses for object detection, classification, and box distribution in rotated YOLO models.

Calculate and return the loss for oriented bounding box detection.

Decode predicted object bounding box coordinates from anchor points and distribution.

Preprocess targets for oriented bounding box detection.

Criterion class for computing training losses for end-to-end detection.

Calculate the sum of the loss for box, cls and dfl multiplied by batch size.

Criterion class for computing training losses for text-visual prompt detection.

Calculate the loss for text-visual prompt detection.

Extract visual-prompt features from the model output.

Criterion class for computing training losses for text-visual prompt segmentation.

Calculate the loss for text-visual prompt segmentation.

**Examples:**

Example 1 (typescript):
```typescript
VarifocalLoss(self, gamma: float = 2.0, alpha: float = 0.75)
```

Example 2 (python):
```python
class VarifocalLoss(nn.Module):
    """Varifocal loss by Zhang et al.

    Implements the Varifocal Loss function for addressing class imbalance in object detection by focusing on
    hard-to-classify examples and balancing positive/negative samples.

    Attributes:
        gamma (float): The focusing parameter that controls how much the loss focuses on hard-to-classify examples.
        alpha (float): The balancing factor used to address class imbalance.

    References:
        https://arxiv.org/abs/2008.13367
    """

    def __init__(self, gamma: float = 2.0, alpha: float = 0.75):
        """Initialize the VarifocalLoss class with focusing and balancing parameters."""
        super().__init__()
        self.gamma = gamma
        self.alpha = alpha
```

Example 3 (python):
```python
def forward(self, pred_score: torch.Tensor, gt_score: torch.Tensor, label: torch.Tensor) -> torch.Tensor
```

Example 4 (python):
```python
def forward(self, pred_score: torch.Tensor, gt_score: torch.Tensor, label: torch.Tensor) -> torch.Tensor:
    """Compute varifocal loss between predictions and ground truth."""
    weight = self.alpha * pred_score.sigmoid().pow(self.gamma) * (1 - label) + gt_score * label
    with autocast(enabled=False):
        loss = (
            (F.binary_cross_entropy_with_logits(pred_score.float(), gt_score.float(), reduction="none") * weight)
            .mean(1)
            .sum()
        )
    return loss
```

---

## Reference for ultralytics/models/sam/sam3/sam3_image.py

**URL:** https://docs.ultralytics.com/zh/reference/models/sam/sam3/sam3_image/

**Contents:**
- Reference for ultralytics/models/sam/sam3/sam3_image.py
- class ultralytics.models.sam.sam3.sam3_image.SAM3SemanticModel
  - method ultralytics.models.sam.sam3.sam3_image.SAM3SemanticModel._encode_prompt
  - method ultralytics.models.sam.sam3.sam3_image.SAM3SemanticModel._run_decoder
  - method ultralytics.models.sam.sam3.sam3_image.SAM3SemanticModel._run_encoder
  - method ultralytics.models.sam.sam3.sam3_image.SAM3SemanticModel._run_segmentation_heads
  - method ultralytics.models.sam.sam3.sam3_image.SAM3SemanticModel._update_scores_and_boxes
  - method ultralytics.models.sam.sam3.sam3_image.SAM3SemanticModel.forward_grounding
  - method ultralytics.models.sam.sam3.sam3_image.SAM3SemanticModel.set_classes
  - method ultralytics.models.sam.sam3.sam3_image.SAM3SemanticModel.set_imgsz

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/models/sam/sam3/sam3_image.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Bases: torch.nn.Module

SAM3 model for semantic segmentation with vision-language backbone.

Encode the geometric and visual prompts.

Run the transformer decoder.

Run the transformer encoder.

Run segmentation heads and get masks.

Update output dict with class scores and box predictions.

Forward pass for grounding (detection + segmentation) given input images and text.

Set the text embeddings for the given class names.

Set the image size for the model.

Helper function to update output dictionary with main and auxiliary outputs.

**Examples:**

Example 1 (python):
```python
def __init__(
    self,
    backbone: SAM3VLBackbone,
    transformer,
    input_geometry_encoder,
    segmentation_head=None,
    num_feature_levels=1,
    o2m_mask_predict=True,
    dot_prod_scoring=None,
    use_instance_query: bool = True,
    multimask_output: bool = True,
    use_act_checkpoint_seg_head: bool = True,
    matcher=None,
    use_dot_prod_scoring=True,
    supervise_joint_box_scores: bool = False,  # only relevant if using presence token/score
    detach_presence_in_joint_score: bool = False,  # only relevant if using presence token/score
    separate_scorer_for_instance: bool = False,
    num_interactive_steps_val: int = 0,
)
```

Example 2 (python):
```python
class SAM3SemanticModel(torch.nn.Module):
    """SAM3 model for semantic segmentation with vision-language backbone."""

    def __init__(
        self,
        backbone: SAM3VLBackbone,
        transformer,
        input_geometry_encoder,
        segmentation_head=None,
        num_feature_levels=1,
        o2m_mask_predict=True,
        dot_prod_scoring=None,
        use_instance_query: bool = True,
        multimask_output: bool = True,
        use_act_checkpoint_seg_head: bool = True,
        matcher=None,
        use_dot_prod_scoring=True,
        supervise_joint_box_scores: bool = False,  # only relevant if using presence token/score
        detach_presence_in_joint_score: bool = False,  # only relevant if using presence token/score
        separate_scorer_for_instance: bool = False,
        num_interactive_steps_val: int = 0,
    ):
        """Initialize the SAM3SemanticModel."""
        super().__init__()
        self.backbone = backbone
        self.geometry_encoder = input_geometry_encoder
        self.transformer = transformer
        self.hidden_dim = transformer.d_model
        self.num_feature_levels = num_feature_levels
        self.segmentation_head = segmentation_head

        self.o2m_mask_predict = o2m_mask_predict

        self.dot_prod_scoring = dot_prod_scoring
        self.use_act_checkpoint_seg_head = use_act_checkpoint_seg_head
        self.matcher = matcher

        self.num_interactive_steps_val = num_interactive_steps_val
        self.use_dot_prod_scoring = use_dot_prod_scoring

        if self.use_dot_prod_scoring:
            assert dot_prod_scoring is not None
            self.dot_prod_scoring = dot_prod_scoring
            self.instance_dot_prod_scoring = None
            if separate_scorer_for_instance:
                self.instance_dot_prod_scoring = deepcopy(dot_prod_scoring)
        else:
            self.class_embed = torch.nn.Linear(self.hidden_dim, 1)
            self.instance_class_embed = None
            if separate_scorer_for_instance:
                self.instance_class_embed = deepcopy(self.class_embed)

        self.supervise_joint_box_scores = supervise_joint_box_scores
        self.detach_presence_in_joint_score = detach_presence_in_joint_score

        # verify the number of queries for O2O and O2M
        num_o2o_static = self.transformer.decoder.num_queries
        num_o2m_static = self.transformer.decoder.num_o2m_queries
        assert num_o2m_static == (num_o2o_static if self.transformer.decoder.dac else 0)
        self.dac = self.transformer.decoder.dac

        self.use_instance_query = use_instance_query
        self.multimask_output = multimask_output

        self.text_embeddings = {}
        self.names = []
```

Example 3 (python):
```python
def _encode_prompt(
    self,
    img_feats,
    img_pos_embeds,
    vis_feat_sizes,
    geometric_prompt,
    visual_prompt_embed=None,
    visual_prompt_mask=None,
    prev_mask_pred=None,
)
```

Example 4 (python):
```python
def _encode_prompt(
    self,
    img_feats,
    img_pos_embeds,
    vis_feat_sizes,
    geometric_prompt,
    visual_prompt_embed=None,
    visual_prompt_mask=None,
    prev_mask_pred=None,
):
    """Encode the geometric and visual prompts."""
    if prev_mask_pred is not None:
        img_feats = [img_feats[-1] + prev_mask_pred]
    # Encode geometry
    geo_feats, geo_masks = self.geometry_encoder(
        geo_prompt=geometric_prompt,
        img_feats=img_feats,
        img_sizes=vis_feat_sizes,
        img_pos_embeds=img_pos_embeds,
    )
    if visual_prompt_embed is None:
        visual_prompt_embed = torch.zeros((0, *geo_feats.shape[1:]), device=geo_feats.device)
        visual_prompt_mask = torch.zeros(
            (*geo_masks.shape[:-1], 0),
            device=geo_masks.device,
            dtype=geo_masks.dtype,
        )
    prompt = torch.cat([geo_feats, visual_prompt_embed], dim=0)
    prompt_mask = torch.cat([geo_masks, visual_prompt_mask], dim=1)
    return prompt, prompt_mask
```

---

## Reference for ultralytics/trackers/bot_sort.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/trackers/bot_sort/

**Contents:**
- Reference for ultralytics/trackers/bot_sort.py
- class ultralytics.trackers.bot_sort.BOTrack
  - property ultralytics.trackers.bot_sort.BOTrack.tlwh
  - method ultralytics.trackers.bot_sort.BOTrack.convert_coords
  - method ultralytics.trackers.bot_sort.BOTrack.multi_predict
  - method ultralytics.trackers.bot_sort.BOTrack.predict
  - method ultralytics.trackers.bot_sort.BOTrack.re_activate
  - method ultralytics.trackers.bot_sort.BOTrack.tlwh_to_xywh
  - method ultralytics.trackers.bot_sort.BOTrack.update
  - method ultralytics.trackers.bot_sort.BOTrack.update_features

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/trackers/bot_sort.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

An extended version of the STrack class for YOLO, adding object tracking features.

This class extends the STrack class to include additional functionalities for object tracking, such as feature smoothing, Kalman filter prediction, and reactivation of tracks.

Return the current bounding box position in (top left x, top left y, width, height) format.

Convert tlwh bounding box coordinates to xywh format.

Predict the mean and covariance for multiple object tracks using a shared Kalman filter.

Predict the object's future state using the Kalman filter to update its mean and covariance.

Reactivate a track with updated features and optionally assign a new ID.

Convert bounding box from tlwh (top-left-width-height) to xywh (center-x-center-y-width-height) format.

Update the track with new detection information and the current frame ID.

Update the feature vector and apply exponential moving average smoothing.

An extended version of the BYTETracker class for YOLO, designed for object tracking with ReID and GMC algorithm.

The class is designed to work with a YOLO object detection model and supports ReID only if enabled via args.

Calculate distances between tracks and detections using IoU and optionally ReID embeddings.

Return an instance of KalmanFilterXYWH for predicting and updating object states in the tracking process.

Initialize object tracks using detection bounding boxes, scores, class labels, and optional ReID features.

Predict the mean and covariance of multiple object tracks using a shared Kalman filter.

Reset the BOTSORT tracker to its initial state, clearing all tracked objects and internal states.

YOLO model as encoder for re-identification.

Extract embeddings for detected objects.

**Examples:**

Example 1 (typescript):
```typescript
BOTrack(self, xywh: np.ndarray, score: float, cls: int, feat: np.ndarray | None = None, feat_history: int = 50)
```

Example 2 (sql):
```sql
Create a BOTrack instance and update its features
>>> bo_track = BOTrack(xywh=np.array([100, 50, 80, 40, 0]), score=0.9, cls=1, feat=np.random.rand(128))
>>> bo_track.predict()
>>> new_track = BOTrack(xywh=np.array([110, 60, 80, 40, 0]), score=0.85, cls=1, feat=np.random.rand(128))
>>> bo_track.update(new_track, frame_id=2)
```

Example 3 (python):
```python
class BOTrack(STrack):
    """An extended version of the STrack class for YOLO, adding object tracking features.

    This class extends the STrack class to include additional functionalities for object tracking, such as feature
    smoothing, Kalman filter prediction, and reactivation of tracks.

    Attributes:
        shared_kalman (KalmanFilterXYWH): A shared Kalman filter for all instances of BOTrack.
        smooth_feat (np.ndarray): Smoothed feature vector.
        curr_feat (np.ndarray): Current feature vector.
        features (deque): A deque to store feature vectors with a maximum length defined by `feat_history`.
        alpha (float): Smoothing factor for the exponential moving average of features.
        mean (np.ndarray): The mean state of the Kalman filter.
        covariance (np.ndarray): The covariance matrix of the Kalman filter.

    Methods:
        update_features: Update features vector and smooth it using exponential moving average.
        predict: Predict the mean and covariance using Kalman filter.
        re_activate: Reactivate a track with updated features and optionally new ID.
        update: Update the track with new detection and frame ID.
        tlwh: Property that gets the current position in tlwh format `(top left x, top left y, width, height)`.
        multi_predict: Predict the mean and covariance of multiple object tracks using shared Kalman filter.
        convert_coords: Convert tlwh bounding box coordinates to xywh format.
        tlwh_to_xywh: Convert bounding box to xywh format `(center x, center y, width, height)`.

    Examples:
        Create a BOTrack instance and update its features
        >>> bo_track = BOTrack(xywh=np.array([100, 50, 80, 40, 0]), score=0.9, cls=1, feat=np.random.rand(128))
        >>> bo_track.predict()
        >>> new_track = BOTrack(xywh=np.array([110, 60, 80, 40, 0]), score=0.85, cls=1, feat=np.random.rand(128))
        >>> bo_track.update(new_track, frame_id=2)
    """

    shared_kalman = KalmanFilterXYWH()

    def __init__(
        self, xywh: np.ndarray, score: float, cls: int, feat: np.ndarray | None = None, feat_history: int = 50
    ):
        """Initialize a BOTrack object with temporal parameters, such as feature history, alpha, and current features.

        Args:
            xywh (np.ndarray): Bounding box in `(x, y, w, h, idx)` or `(x, y, w, h, angle, idx)` format, where (x, y) is
                the center, (w, h) are width and height, and `idx` is the detection index.
            score (float): Confidence score of the detection.
            cls (int): Class ID of the detected object.
            feat (np.ndarray, optional): Feature vector associated with the detection.
            feat_history (int): Maximum length of the feature history deque.
        """
        super().__init__(xywh, score, cls)

        self.smooth_feat = None
        self.curr_feat = None
        if feat is not None:
            self.update_features(feat)
        self.features = deque([], maxlen=feat_history)
        self.alpha = 0.9
```

Example 4 (python):
```python
def tlwh(self) -> np.ndarray
```

---

## Reference for ultralytics/models/yolo/pose/predict.py

**URL:** https://docs.ultralytics.com/zh/reference/models/yolo/pose/predict/

**Contents:**
- Reference for ultralytics/models/yolo/pose/predict.py
- class ultralytics.models.yolo.pose.predict.PosePredictor
  - method ultralytics.models.yolo.pose.predict.PosePredictor.construct_result

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/models/yolo/pose/predict.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Bases: DetectionPredictor

A class extending the DetectionPredictor class for prediction based on a pose model.

This class specializes in pose estimation, handling keypoints detection alongside standard object detection capabilities inherited from DetectionPredictor.

Sets up a PosePredictor instance, configuring it for pose detection tasks and handling device-specific warnings for Apple MPS.

Construct the result object from the prediction, including keypoints.

Extends the parent class implementation by extracting keypoint data from predictions and adding them to the result object.

**Examples:**

Example 1 (rust):
```rust
PosePredictor(self, cfg = DEFAULT_CFG, overrides = None, _callbacks = None)
```

Example 2 (sql):
```sql
>>> from ultralytics.utils import ASSETS
>>> from ultralytics.models.yolo.pose import PosePredictor
>>> args = dict(model="yolo11n-pose.pt", source=ASSETS)
>>> predictor = PosePredictor(overrides=args)
>>> predictor.predict_cli()
```

Example 3 (python):
```python
class PosePredictor(DetectionPredictor):
    """A class extending the DetectionPredictor class for prediction based on a pose model.

    This class specializes in pose estimation, handling keypoints detection alongside standard object detection
    capabilities inherited from DetectionPredictor.

    Attributes:
        args (namespace): Configuration arguments for the predictor.
        model (torch.nn.Module): The loaded YOLO pose model with keypoint detection capabilities.

    Methods:
        construct_result: Construct the result object from the prediction, including keypoints.

    Examples:
        >>> from ultralytics.utils import ASSETS
        >>> from ultralytics.models.yolo.pose import PosePredictor
        >>> args = dict(model="yolo11n-pose.pt", source=ASSETS)
        >>> predictor = PosePredictor(overrides=args)
        >>> predictor.predict_cli()
    """

    def __init__(self, cfg=DEFAULT_CFG, overrides=None, _callbacks=None):
        """Initialize PosePredictor for pose estimation tasks.

        Sets up a PosePredictor instance, configuring it for pose detection tasks and handling device-specific warnings
        for Apple MPS.

        Args:
            cfg (Any): Configuration for the predictor.
            overrides (dict, optional): Configuration overrides that take precedence over cfg.
            _callbacks (list, optional): List of callback functions to be invoked during prediction.
        """
        super().__init__(cfg, overrides, _callbacks)
        self.args.task = "pose"
```

Example 4 (python):
```python
def construct_result(self, pred, img, orig_img, img_path)
```

---

## 带有 Ultralytics YOLO11 的 Triton 推理服务器

**URL:** https://docs.ultralytics.com/zh/guides/triton-inference-server/

**Contents:**
- 带有 Ultralytics YOLO11 的 Triton 推理服务器
- 什么是 Triton Inference Server？
- Triton 推理服务器的主要优势
- 准备工作
- 将 YOLO11 导出为 ONNX 格式
- 设置 Triton 模型仓库
- 运行 Triton 推理服务器
- TensorRT 优化（可选）
- 常见问题
  - 如何设置 Ultralytics YOLO11 与 NVIDIA Triton 推理服务器？

Triton 推理服务器（前身为 TensorRT 推理服务器）是由 NVIDIA 开发的开源软件解决方案。它提供了一个针对 NVIDIA GPU 优化的云推理解决方案。Triton 简化了在生产环境中大规模部署 AI 模型的过程。将 Ultralytics YOLO11 与 Triton 推理服务器集成，您可以部署可扩展的、高性能的深度学习推理工作负载。本指南提供了设置和测试集成的步骤。

观看： NVIDIA Triton 推理服务器入门。

Triton 推理服务器旨在部署各种 AI 模型到生产环境。它支持各种深度学习和机器学习框架，包括 TensorFlow、PyTorch、ONNX Runtime 等。其主要用例包括：

将 Triton 推理服务器与 Ultralytics YOLO11 结合使用具有以下几个优势：

在 Triton 上部署模型之前，必须将其导出为 ONNX 格式。ONNX（开放神经网络交换）是一种允许在不同深度学习框架之间传输模型的格式。使用 export 中的 YOLO 函数：

Triton 模型仓库是 Triton 可以访问和加载模型的存储位置。

将导出的 ONNX 模型移动到 Triton 仓库：

使用 Docker 运行 Triton 推理服务器：

然后使用 Triton Server 模型运行推理：

为了获得更高的性能，您可以将 TensorRT 与 Triton Inference Server 结合使用。TensorRT 是专为 NVIDIA GPU 构建的高性能深度学习优化器，可以显著提高推理速度。

将 TensorRT 与 Triton 结合使用的主要优势包括：

要直接使用 TensorRT，您可以将您的 YOLO11 模型导出为 TensorRT 格式：

有关 TensorRT 优化的更多信息，请参阅TensorRT 集成指南。

通过遵循上述步骤，您可以高效地在 Triton Inference Server 上部署和运行 Ultralytics YOLO11 模型，从而为深度学习推理任务提供可扩展且高性能的解决方案。如果您遇到任何问题或有其他疑问，请参阅 Triton 官方文档 或联系 Ultralytics 社区以获得支持。

使用 NVIDIA Triton Inference Server 设置 Ultralytics YOLO11 涉及几个关键步骤：

将 YOLO11 导出为 ONNX 格式：

此设置可以帮助您在 Triton Inference Server 上高效地大规模部署 YOLO11 模型，以实现高性能的 AI 模型推理。

将 Ultralytics YOLO11 与 NVIDIA Triton Inference Server 集成提供了以下几个优势：

有关使用 Triton 设置和运行 YOLO11 的详细说明，您可以参考设置指南。

在将 Ultralytics YOLO11 模型部署到 NVIDIA Triton Inference Server 之前，使用 ONNX（开放神经网络交换）格式可以带来以下几个主要优势：

您可以按照 ONNX 集成指南 中的步骤完成该过程。

是的，您可以在 NVIDIA Triton Inference Server 上使用 Ultralytics YOLO11 模型运行推理。一旦您的模型在 Triton Model Repository 中设置好并且服务器正在运行，您可以按如下方式加载并运行您的模型推理：

这种方法允许您利用 Triton 的优化，同时使用熟悉的 Ultralytics YOLO 接口。有关使用 YOLO11 设置和运行 Triton Server 的深入指南，请参阅运行 triton 推理服务器部分。

与用于部署的 TensorFlow 和 PyTorch 模型相比，Ultralytics YOLO11 具有以下几个独特的优势：

有关更多详细信息，请比较模型导出指南中的部署选项。

**Examples:**

Example 1 (unknown):
```unknown
pip install tritonclient[all]
```

Example 2 (python):
```python
from ultralytics import YOLO

# Load a model
model = YOLO("yolo11n.pt")  # load an official model

# Retrieve metadata during export. Metadata needs to be added to config.pbtxt. See next section.
metadata = []


def export_cb(exporter):
    metadata.append(exporter.metadata)


model.add_callback("on_export_end", export_cb)

# Export the model
onnx_file = model.export(format="onnx", dynamic=True)
```

Example 3 (python):
```python
from pathlib import Path

# Define paths
model_name = "yolo"
triton_repo_path = Path("tmp") / "triton_repo"
triton_model_path = triton_repo_path / model_name

# Create directories
(triton_model_path / "1").mkdir(parents=True, exist_ok=True)
```

Example 4 (python):
```python
from pathlib import Path

# Move ONNX model to Triton Model path
Path(onnx_file).rename(triton_model_path / "1" / "model.onnx")

# Create config file
(triton_model_path / "config.pbtxt").touch()

data = """
# Add metadata
parameters {
  key: "metadata"
  value {
    string_value: "%s"
  }
}

# (Optional) Enable TensorRT for GPU inference
# First run will be slow due to TensorRT engine conversion
optimization {
  execution_accelerators {
    gpu_execution_accelerator {
      name: "tensorrt"
      parameters {
        key: "precision_mode"
        value: "FP16"
      }
      parameters {
        key: "max_workspace_size_bytes"
        value: "3221225472"
      }
      parameters {
        key: "trt_engine_cache_enable"
        value: "1"
      }
      parameters {
        key: "trt_engine_cache_path"
        value: "/models/yolo/1"
      }
    }
  }
}
""" % metadata[0]  # noqa

with open(triton_model_path / "config.pbtxt", "w") as f:
    f.write(data)
```

---

## Reference for ultralytics/models/sam/predict.py

**URL:** https://docs.ultralytics.com/zh/reference/models/sam/predict/

**Contents:**
- Reference for ultralytics/models/sam/predict.py
- class ultralytics.models.sam.predict.Predictor
  - method ultralytics.models.sam.predict.Predictor._inference_features
  - method ultralytics.models.sam.predict.Predictor._prepare_prompts
  - method ultralytics.models.sam.predict.Predictor.generate
  - method ultralytics.models.sam.predict.Predictor.get_im_features
  - method ultralytics.models.sam.predict.Predictor.get_model
  - method ultralytics.models.sam.predict.Predictor.inference
  - method ultralytics.models.sam.predict.Predictor.inference_features
  - method ultralytics.models.sam.predict.Predictor.postprocess

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/models/sam/predict.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Predictor class for SAM, enabling real-time image segmentation with promptable capabilities.

This class extends BasePredictor and implements the Segment Anything Model (SAM) for advanced image segmentation tasks. It supports various input prompts like points, bounding boxes, and masks for fine-grained control over segmentation results.

Sets up the Predictor object for SAM (Segment Anything Model) and applies any configuration overrides or callbacks provided. Initializes task-specific settings for SAM, such as retina_masks being set to True for optimal results.

Perform inference on image features using the SAM model.

Prepare and transform the input prompts for processing based on the destination shape.

Perform image segmentation using the Segment Anything Model (SAM).

This method segments an entire image into constituent parts by leveraging SAM's advanced architecture and real-time performance capabilities. It can optionally work on image crops for finer segmentation.

Extract image features using the SAM model's image encoder for subsequent mask prediction.

Retrieve or build the Segment Anything Model (SAM) for image segmentation tasks.

Perform image segmentation inference based on the given input cues, using the currently loaded image.

This method leverages SAM's (Segment Anything Model) architecture consisting of image encoder, prompt encoder, and mask decoder for real-time and promptable segmentation tasks.

Perform prompts preprocessing and inference on provided image features using the SAM model.

Post-process SAM's inference outputs to generate object detection masks and bounding boxes.

This method scales masks and boxes to the original image size and applies a threshold to the mask predictions. It leverages SAM's advanced architecture for real-time, promptable segmentation tasks.

Perform initial transformations on the input image for preprocessing.

This method applies transformations such as resizing to prepare the image for further preprocessing. Currently, batched inference is not supported; hence the list length should be 1.

Preprocess the input image for model inference.

This method prepares the input image by applying transformations and normalization. It supports both torch.Tensor and list of np.ndarray as input formats. For OpenCV-loaded images, the input is typically BGR and is converted to RGB during preprocessing.

Perform image segmentation inference based on input cues using SAM's specialized architecture.

This internal function leverages the Segment Anything Model (SAM) for prompt-based, real-time segmentation. It processes various input prompts such as bounding boxes, points, and masks to generate segmentation masks.

Remove small disconnected regions and holes from segmentation masks.

This function performs post-processing on segmentation masks generated by the Segment Anything Model (SAM). It removes small disconnected regions and holes from the input masks, and then performs Non-Maximum Suppression (NMS) to eliminate any newly created duplicate boxes.

Reset the current image and its features, clearing them for subsequent inference.

Preprocess and set a single image for inference.

This method prepares the model for inference on a single image by setting up the model if not already initialized, configuring the data source, and preprocessing the image for feature extraction. It ensures that only one image is set at a time and extracts image features for subsequent use.

Set prompts for subsequent inference operations.

Initialize the Segment Anything Model (SAM) for inference.

This method sets up the SAM model by allocating it to the appropriate device and initializing the necessary parameters for image normalization and other Ultralytics compatibility settings.

Set up the data source for SAM inference.

SAM2Predictor class for advanced image segmentation using Segment Anything Model 2 architecture.

This class extends the base Predictor class to implement SAM2-specific functionality for image segmentation tasks. It provides methods for model initialization, feature extraction, and prompt-based inference.

Perform inference on image features using the SAM2 model.

Prepare and transform the input prompts for processing based on the destination shape.

Extract image features from the SAM image encoder for subsequent processing.

Retrieve and initialize the Segment Anything Model 2 (SAM2) for image segmentation tasks.

Set up the data source and image size for SAM2 inference.

SAM2VideoPredictor to handle user interactions with videos and manage inference states.

This class extends the functionality of SAM2Predictor to support video processing and maintains the state of inference operations. It includes configurations for managing non-overlapping masks, clearing memory for non-conditional inputs, and setting up callbacks for prediction events.

This constructor initializes the SAM2VideoPredictor with a given configuration, applies any specified overrides, and sets up the inference state along with certain flags that control the behavior of the predictor.

The fill_hole_area attribute is defined but not used in the current implementation.

Split a multi-object output into per-object output slices and add them into Output_Dict_Per_Obj.

The resulting slices share the same tensor storage.

Remove the non-conditioning memory around the input frame.

When users provide correction clicks, the surrounding frames' non-conditioning memories can still contain outdated object appearance information and could confuse the model. This method clears those non-conditioning memories surrounding the interacted frame to avoid giving the model both old and new information about the object.

Consolidate per-object temporary outputs into a single output for all objects.

This method combines the temporary outputs for each object on a given frame into a unified output. It fills in any missing objects either from the main output dictionary or leaves placeholders if they do not exist in the main output. Optionally, it can re-run the memory encoder after applying non-overlapping constraints to the object scores.

Get a dummy object pointer based on an empty mask on the current frame.

Cache and manage the positional encoding for mask memory across frames and objects.

This method optimizes storage by caching the positional encoding (maskmem_pos_enc) for mask memory, which is constant across frames and objects, thus reducing the amount of redundant information stored during an inference session. It checks if the positional encoding has already been cached; if not, it caches a slice of the provided encoding. If the batch size is greater than one, it expands the cached positional encoding to match the current batch size.

Initialize an inference state.

This function sets up the initial state required for performing inference on video data. It includes initializing various dictionaries and ordered dictionaries that will store inputs, outputs, and other metadata relevant to the tracking process.

Map client-side object id to model-side object index.

Prune old non-conditioning frames to bound memory usage.

Reset all tracking inputs and results across the videos.

Run the memory encoder on masks.

This is usually after applying non-overlapping constraints to object scores. Since their scores changed, their memory also needs to be computed again with the memory encoder.

Run tracking on a single frame based on current inputs and previous memory.

Add new points or masks to a specific frame for a given object ID.

This method updates the inference state with new prompts (points or masks) for a specified object and frame index. It ensures that the prompts are either points or masks, but not both, and updates the internal state accordingly. It also handles the generation of new segmentations based on the provided prompts and the existing state.

Remove all input points or mask in a specific frame for a given object.

Remove all input points or mask in all frames throughout the video.

Extract and process image features using SAM2's image encoder for subsequent segmentation tasks.

Retrieve and configure the model with binarization enabled.

This method overrides the base class implementation to set the binarize flag to True.

Perform image segmentation inference based on the given input cues, using the currently loaded image. This

method leverages SAM's (Segment Anything Model) architecture consisting of image encoder, prompt encoder, and mask decoder for real-time and promptable segmentation tasks.

Initialize an inference state for the predictor.

This function sets up the initial state required for performing inference on video data. It includes initializing various dictionaries and ordered dictionaries that will store inputs, outputs, and other metadata relevant to the tracking process.

Post-process the predictions to apply non-overlapping constraints if required.

This method extends the post-processing functionality by applying non-overlapping constraints to the predicted masks if the non_overlap_masks flag is set to True. This ensures that the masks do not overlap, which can be useful for certain applications.

If non_overlap_masks is True, the method applies constraints to ensure non-overlapping masks.

Prepare inference_state and consolidate temporary outputs before tracking.

This method marks the start of tracking, disallowing the addition of new objects until the session is reset. It consolidates temporary outputs from temp_output_dict_per_obj and merges them into output_dict. Additionally, it clears non-conditioning memory around input frames and ensures that the state is consistent with the provided inputs.

Remove an object id from the tracking state. If strict is True, we check whether the object id actually

exists and raise an error if it doesn't exist.

SAM2DynamicInteractivePredictor extends SAM2Predictor to support dynamic interactions with video frames or a

This constructor initializes the SAM2DynamicInteractivePredictor with a given configuration, applies any specified overrides

Map client-side object id to model-side object index.

Prepare memory-conditioned features for the current image state.

If obj_idx is provided, features are prepared for a specific prompted object in the image. If obj_idx is None, features are prepared for all objects. If no memory is available, a no-memory embedding is added to the current vision features. Otherwise, memory from previous frames is used to condition the current vision features via a transformer attention mechanism.

Initialize the image state by processing the input image and extracting features.

Get memory and positional encoding from memory, which is used to condition the current image features.

Perform inference on a single image with optional bounding boxes, masks, points and object IDs. It has two

modes: one is to run inference on a single image without updating the memory, and the other is to update the memory with the provided prompts and object IDs. When update_memory is True, it will update the memory with the provided prompts and obj_ids. When update_memory is False, it will only run inference on the provided image without updating the memory.

Tracking step for the current image state to predict masks.

This method processes the image features and runs the SAM heads to predict masks. If obj_idx is provided, it processes the features for a specific prompted object in the image. If obj_idx is None, it processes the features for all objects in the image. The method supports both mask-based output without SAM and full SAM processing with memory-conditioned features.

Append the imgState to the memory_bank and update the memory for the model.

Segment Anything Model 3 (SAM3) Interactive Predictor for image segmentation tasks.

Retrieve and initialize the Segment Anything Model 3 (SAM3) for image segmentation tasks.

Setup the SAM3 model with appropriate mean and standard deviation for preprocessing.

Segment Anything Model 3 (SAM3) Predictor for image segmentation tasks.

Get a dummy geometric prompt with zero boxes.

Run inference on the extracted features with optional bounding boxes and labels.

Prepare prompts by normalizing bounding boxes and points to the destination shape.

Extract image features using the model's backbone.

Retrieve and initialize the Segment Anything Model 3 (SAM3) for image segmentation tasks.

Perform inference on a single image with optional prompts.

Perform prompts preprocessing and inference on provided image features using the SAM model.

Post-process the predictions to apply non-overlapping constraints if required.

Perform initial transformations on the input image for preprocessing.

This method applies transformations such as resizing to prepare the image for further preprocessing. Currently, batched inference is not supported; hence the list length should be 1.

Reset the prompts for the predictor.

Bases: SAM2VideoPredictor, SAM3Predictor

Segment Anything Model 3 (SAM3) Video Predictor for video segmentation tasks.

Perform image segmentation inference based on the given input cues, using the currently loaded image. This

method leverages SAM's (Segment Anything Model) architecture consisting of image encoder, prompt encoder, and mask decoder for real-time and promptable segmentation tasks.

Bases: SAM3SemanticPredictor

Segment Anything Model 3 (SAM3) Video Semantic Predictor.

Applies non-overlapping constraints object wise (i.e. only one object can claim the overlapping region).

Match detections on the current frame with the existing masklets.

Build and cache SAM2 backbone features.

This function handles one-step inference for the DenseTracking model in an SPMD manner. At a high-level, all

GPUs execute the same function calls as if it's done on a single GPU, while under the hood, some function calls involve distributed computation based on sharded SAM2 states.

Drop a few new detections based on the maximum number of objects. We drop new objects based on their

detection scores, keeping the high-scoring ones and dropping the low-scoring ones.

Extract and filter detection outputs.

Initialize metadata for the masklets.

Handle hotstart heuristics to remove unmatched or duplicated objects.

Inference_states: list of inference states, each state corresponds to a different set of objects.

Recondition masklets based on new high-confidence detections.

Perform inference on a single frame and get its inference results.

Suppress detections too close to image edges (for normalized boxes).

boxes: (N, 4) in xyxy format, normalized [0,1] margin: fraction of image

Suppress overlapping masks based on the most recent occlusion information. If an object is removed by

hotstart, we always suppress it if it overlaps with any other object.

Add a new object to SAM2 inference states.

Remove an object from SAM2 inference states. This would remove the object from all frames in the video.

Run Sam2 memory encoder, enforcing non-overlapping constraints globally.

Add text, point or box prompts on a single frame. This method returns the inference outputs only on the

Note that text prompts are NOT associated with a particular frame (i.e. they apply to all frames). However, we only run inference on the frame specified in frame_idx.

Build the output masks for the current frame.

Perform inference on a video sequence with optional prompts.

Initialize an inference state for the predictor.

This function sets up the initial state required for performing inference on video data. It includes initializing various dictionaries and ordered dictionaries that will store inputs, outputs, and other metadata relevant to the tracking process.

Post-process the predictions to apply non-overlapping constraints if required.

Run backbone and detection for a single frame.

Run the tracker propagation phase for a single frame in an SPMD manner.

Execute the tracker update plan for a single frame in an SPMD manner.

Run the tracker update planning phase for a single frame in an SPMD manner.

Setup the SAM3VideoSemanticPredictor model.

Setup the source for the SAM3VideoSemanticPredictor model.

Update the confirmation status of masklets based on the current frame's detection results.

**Examples:**

Example 1 (rust):
```rust
Predictor(self, cfg = DEFAULT_CFG, overrides = None, _callbacks = None)
```

Example 2 (unknown):
```unknown
>>> predictor = Predictor()
>>> predictor.setup_model(model_path="sam_model.pt")
>>> predictor.set_image("image.jpg")
>>> bboxes = [[100, 100, 200, 200]]
>>> results = predictor(bboxes=bboxes)
```

Example 3 (python):
```python
class Predictor(BasePredictor):
    """Predictor class for SAM, enabling real-time image segmentation with promptable capabilities.

    This class extends BasePredictor and implements the Segment Anything Model (SAM) for advanced image segmentation
    tasks. It supports various input prompts like points, bounding boxes, and masks for fine-grained control over
    segmentation results.

    Attributes:
        args (SimpleNamespace): Configuration arguments for the predictor.
        model (torch.nn.Module): The loaded SAM model.
        device (torch.device): The device (CPU or GPU) on which the model is loaded.
        im (torch.Tensor): The preprocessed input image.
        features (torch.Tensor): Extracted image features.
        prompts (dict[str, Any]): Dictionary to store various types of prompts (e.g., bboxes, points, masks).
        segment_all (bool): Flag to indicate if full image segmentation should be performed.
        mean (torch.Tensor): Mean values for image normalization.
        std (torch.Tensor): Standard deviation values for image normalization.

    Methods:
        preprocess: Prepare input images for model inference.
        pre_transform: Perform initial transformations on the input image.
        inference: Perform segmentation inference based on input prompts.
        prompt_inference: Internal function for prompt-based segmentation inference.
        generate: Generate segmentation masks for an entire image.
        setup_model: Initialize the SAM model for inference.
        get_model: Build and return a SAM model.
        postprocess: Post-process model outputs to generate final results.
        setup_source: Set up the data source for inference.
        set_image: Set and preprocess a single image for inference.
        get_im_features: Extract image features using the SAM image encoder.
        set_prompts: Set prompts for subsequent inference.
        reset_image: Reset the current image and its features.
        remove_small_regions: Remove small disconnected regions and holes from masks.

    Examples:
        >>> predictor = Predictor()
        >>> predictor.setup_model(model_path="sam_model.pt")
        >>> predictor.set_image("image.jpg")
        >>> bboxes = [[100, 100, 200, 200]]
        >>> results = predictor(bboxes=bboxes)
    """

    stride = 16

    def __init__(self, cfg=DEFAULT_CFG, overrides=None, _callbacks=None):
        """Initialize the Predictor with configuration, overrides, and callbacks.

        Sets up the Predictor object for SAM (Segment Anything Model) and applies any configuration overrides or
        callbacks provided. Initializes task-specific settings for SAM, such as retina_masks being set to True for
        optimal results.

        Args:
            cfg (dict): Configuration dictionary containing default settings.
            overrides (dict | None): Dictionary of values to override default configuration.
            _callbacks (dict | None): Dictionary of callback functions to customize behavior.
        """
        if overrides is None:
            overrides = {}
        overrides.update(dict(task="segment", mode="predict", batch=1))
        super().__init__(cfg, overrides, _callbacks)
        self.args.retina_masks = True
        self.im = None
        self.features = None
        self.prompts = {}
        self.segment_all = False
```

Example 4 (python):
```python
def _inference_features(
    self,
    features,
    bboxes=None,
    points=None,
    labels=None,
    masks=None,
    multimask_output=False,
)
```

---

## Reference for ultralytics/models/nas/predict.py

**URL:** https://docs.ultralytics.com/zh/reference/models/nas/predict/

**Contents:**
- Reference for ultralytics/models/nas/predict.py
- class ultralytics.models.nas.predict.NASPredictor
  - method ultralytics.models.nas.predict.NASPredictor.postprocess

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/models/nas/predict.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Bases: DetectionPredictor

Ultralytics YOLO NAS Predictor for object detection.

This class extends the DetectionPredictor from Ultralytics engine and is responsible for post-processing the raw predictions generated by the YOLO NAS models. It applies operations like non-maximum suppression and scaling the bounding boxes to fit the original image dimensions.

Typically, this class is not instantiated directly. It is used internally within the NAS class.

Postprocess NAS model predictions to generate final detection results.

This method takes raw predictions from a YOLO NAS model, converts bounding box formats, and applies post-processing operations to generate the final detection results compatible with Ultralytics result visualization and analysis tools.

**Examples:**

Example 1 (unknown):
```unknown
NASPredictor()
```

Example 2 (python):
```python
>>> from ultralytics import NAS
>>> model = NAS("yolo_nas_s")
>>> predictor = model.predictor

Assume that raw_preds, img, orig_imgs are available
>>> results = predictor.postprocess(raw_preds, img, orig_imgs)
```

Example 3 (php):
```php
class NASPredictor(DetectionPredictor):
```

Example 4 (python):
```python
def postprocess(self, preds_in, img, orig_imgs)
```

---

## Reference for ultralytics/models/yolo/detect/predict.py

**URL:** https://docs.ultralytics.com/zh/reference/models/yolo/detect/predict/

**Contents:**
- Reference for ultralytics/models/yolo/detect/predict.py
- class ultralytics.models.yolo.detect.predict.DetectionPredictor
  - method ultralytics.models.yolo.detect.predict.DetectionPredictor.construct_result
  - method ultralytics.models.yolo.detect.predict.DetectionPredictor.construct_results
  - method ultralytics.models.yolo.detect.predict.DetectionPredictor.get_obj_feats
  - method ultralytics.models.yolo.detect.predict.DetectionPredictor.postprocess

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/models/yolo/detect/predict.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class extending the BasePredictor class for prediction based on a detection model.

This predictor specializes in object detection tasks, processing model outputs into meaningful detection results with bounding boxes and class predictions.

Construct a single Results object from one image prediction.

Construct a list of Results objects from model predictions.

Extract object features from the feature maps.

Post-process predictions and return a list of Results objects.

This method applies non-maximum suppression to raw model predictions and prepares them for visualization and further analysis.

**Examples:**

Example 1 (unknown):
```unknown
DetectionPredictor()
```

Example 2 (sql):
```sql
>>> from ultralytics.utils import ASSETS
>>> from ultralytics.models.yolo.detect import DetectionPredictor
>>> args = dict(model="yolo11n.pt", source=ASSETS)
>>> predictor = DetectionPredictor(overrides=args)
>>> predictor.predict_cli()
```

Example 3 (php):
```php
class DetectionPredictor(BasePredictor):
```

Example 4 (python):
```python
def construct_result(self, pred, img, orig_img, img_path)
```

---

## Reference for ultralytics/utils/__init__.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/__init__/

**Contents:**
- Reference for ultralytics/utils/__init__.py
- class ultralytics.utils.DataExportMixin
  - method ultralytics.utils.DataExportMixin.to_csv
  - method ultralytics.utils.DataExportMixin.to_df
  - method ultralytics.utils.DataExportMixin.to_json
- class ultralytics.utils.SimpleClass
  - method ultralytics.utils.SimpleClass.__getattr__
  - method ultralytics.utils.SimpleClass.__repr__
  - method ultralytics.utils.SimpleClass.__str__
- class ultralytics.utils.IterableSimpleNamespace

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/__init__.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Mixin class for exporting validation metrics or prediction results in various formats.

This class provides utilities to export performance metrics (e.g., mAP, precision, recall) or prediction results from classification, object detection, segmentation, or pose estimation tasks into various formats: Polars DataFrame, CSV, and JSON.

Export results or metrics to CSV string format.

Create a Polars DataFrame from the prediction results summary or validation metrics.

Export results to JSON format.

A simple base class for creating objects with string representations of their attributes.

This class provides a foundation for creating objects that can be easily printed or represented as strings, showing all their non-callable attributes. It's useful for debugging and introspection of object states.

Provide a custom attribute access error message with helpful information.

Return a machine-readable string representation of the object.

Return a human-readable string representation of the object.

Bases: SimpleNamespace

An iterable SimpleNamespace class that provides enhanced functionality for attribute access and iteration.

This class extends the SimpleNamespace class with additional methods for iteration, string representation, and attribute access. It is designed to be used as a convenient container for storing and accessing configuration parameters.

This class is particularly useful for storing configuration parameters in a more accessible and iterable format compared to a standard dictionary.

Provide a custom attribute access error message with helpful information.

Return an iterator of key-value pairs from the namespace's attributes.

Return a human-readable string representation of the object.

Return the value of the specified key if it exists; otherwise, return the default value.

A decorator class for ensuring thread-safe execution of a function or method.

This class can be used as a decorator to make sure that if the decorated function is called from multiple threads, only one thread at a time will be able to execute the function.

Run thread-safe execution of function or method.

YAML utility class for efficient file operations with automatic C-implementation detection.

This class provides optimized YAML loading and saving operations using PyYAML's fastest available implementation (C-based when possible). It implements a singleton pattern with lazy initialization, allowing direct class method usage without explicit instantiation. The class handles file path creation, validation, and character encoding issues automatically.

The implementation prioritizes performance through: - Automatic C-based loader/dumper selection when available - Singleton pattern to reuse the same instance - Lazy initialization to defer import costs until needed - Fallback mechanisms for handling problematic YAML content

Initialize singleton instance on first use.

Load YAML file to Python object with robust error handling.

Pretty print YAML file or object to console.

Save Python object as YAML file.

Bases: contextlib.ContextDecorator

Ultralytics TryExcept class for handling exceptions gracefully.

This class can be used as a decorator or context manager to catch exceptions and optionally print warning messages. It allows code to continue execution even when exceptions occur, which is useful for non-critical operations.

Execute when entering TryExcept context, initialize instance.

Define behavior when exiting a 'with' block, print error message if necessary.

Bases: contextlib.ContextDecorator

Retry class for function execution with exponential backoff.

This decorator can be used to retry a function on exceptions, up to a specified number of times with an exponentially increasing delay between retries. It's useful for handling transient failures in network operations or other unreliable processes.

Decorator implementation for Retry with exponential backoff.

A dictionary-like class that provides JSON persistence for its contents.

This class extends the built-in dictionary to automatically save its contents to a JSON file whenever they are modified. It ensures thread-safe operations using a lock and handles JSON serialization of Path objects.

Remove an item and update the persistent storage.

Store a key-value pair and persist to disk.

Return a pretty-printed JSON string representation of the dictionary.

Handle JSON serialization of Path objects.

Load the data from the JSON file into the dictionary.

Save the current state of the dictionary to the JSON file.

Clear all entries and update the persistent storage.

Update the dictionary and persist changes.

SettingsManager class for managing and persisting Ultralytics settings.

This class extends JSONDict to provide JSON persistence for settings, ensuring thread-safe operations and default values. It validates settings on initialization and provides methods to update or reset settings. The settings include directories for datasets, weights, and runs, as well as various integration flags.

Update one key: value pair.

Validate the current settings and reset if necessary.

Reset the settings to default and save them.

Update settings, validating keys and types.

Decorator to temporarily set rc parameters and the backend for a plotting function.

Set up logging with UTF-8 encoding and configurable verbosity.

This function configures logging for the Ultralytics library, setting the appropriate logging level and formatter based on the verbosity flag and the current process rank. It handles special cases for Windows environments where UTF-8 encoding might not be the default.

Return platform-dependent emoji-safe version of string.

Read the device model information from the system and cache it for quick access.

Check if the OS is Ubuntu.

Check if the OS is Debian.

Check if the current script is running inside a Google Colab notebook.

Check if the current script is running inside a Kaggle kernel.

Check if the current script is running inside a Jupyter Notebook.

Check if the current script is running inside a RunPod container.

Determine if the script is running inside a Docker container.

Determine if the Python environment is running on a Raspberry Pi.

Determine if the Python environment is running on an NVIDIA Jetson device.

Fast online check using DNS (v4/v6) resolution (Cloudflare + Google).

Determine if the file at the given filepath is part of a pip package.

Check if a directory is writable.

Determine whether pytest is currently running or not.

Determine if the current environment is a GitHub Actions runner.

Return a dictionary of default arguments for a function.

Retrieve the Ubuntu version if the OS is Ubuntu.

Return a writable config dir, preferring YOLO_CONFIG_DIR and being OS-aware.

Color a string based on the provided color and style arguments using ANSI escape codes.

This function can be called in two ways: - colorstr('color', 'style', 'your string') - colorstr('your string')

In the second form, 'blue' and 'bold' will be applied by default.

Supported Colors and Styles: - Basic Colors: 'black', 'red', 'green', 'yellow', 'blue', 'magenta', 'cyan', 'white' - Bright Colors: 'bright_black', 'bright_red', 'bright_green', 'bright_yellow', 'bright_blue', 'bright_magenta', 'bright_cyan', 'bright_white' - Misc: 'end', 'bold', 'underline'

References: https://en.wikipedia.org/wiki/ANSI_escape_code

Remove ANSI escape codes from a string, effectively un-coloring it.

Multi-thread a target function by default and return the thread or function result.

This decorator provides flexible execution of the target function, either in a separate thread or synchronously. By default, the function runs in a thread, but this can be controlled via the 'threaded=False' keyword argument which is removed from kwargs before calling the function.

Initialize the Sentry SDK for error tracking and reporting.

Only used if sentry_sdk package is installed and sync=True in settings. Run 'yolo settings' to see and update settings.

Conditions required to send errors (ALL conditions must be met or no errors will be reported): - sentry_sdk package is installed - sync=True in YOLO settings - pytest is not running - running in a pip package installation - running in a non-git directory - running with rank -1 or 0 - online environment - CLI used to run package (checked with 'yolo' as the name of the main CLI command)

Issue a deprecation warning when a deprecated argument is used, suggesting an updated argument.

Strip auth from URL, i.e. https://url.com/file.txt?auth -> https://url.com/file.txt.

Convert URL to filename, i.e. https://url.com/file.txt?auth -> file.txt.

Display a message to install Ultralytics-Snippets for VS Code if not already installed.

**Examples:**

Example 1 (unknown):
```unknown
DataExportMixin()
```

Example 2 (unknown):
```unknown
>>> model = YOLO("yolo11n.pt")
>>> results = model("image.jpg")
>>> df = results.to_df()
>>> print(df)
>>> csv_data = results.to_csv()
```

Example 3 (python):
```python
class DataExportMixin:
```

Example 4 (python):
```python
def to_csv(self, normalize = False, decimals = 5)
```

---

## Reference for ultralytics/models/utils/ops.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/models/utils/ops/

**Contents:**
- Reference for ultralytics/models/utils/ops.py
- class ultralytics.models.utils.ops.HungarianMatcher
  - method ultralytics.models.utils.ops.HungarianMatcher.forward
- function ultralytics.models.utils.ops.get_cdn_group

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/models/utils/ops.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A module implementing the HungarianMatcher for optimal assignment between predictions and ground truth.

HungarianMatcher performs optimal bipartite assignment over predicted and ground truth bounding boxes using a cost function that considers classification scores, bounding box coordinates, and optionally mask predictions. This is used in end-to-end object detection models like DETR.

Compute optimal assignment between predictions and ground truth using Hungarian algorithm.

This method calculates matching costs based on classification scores, bounding box coordinates, and optionally mask predictions, then finds the optimal bipartite assignment between predictions and ground truth.

Generate contrastive denoising training group with positive and negative samples from ground truths.

This function creates denoising queries for contrastive denoising training by adding noise to ground truth bounding boxes and class labels. It generates both positive and negative samples to improve model robustness.

**Examples:**

Example 1 (python):
```python
def __init__(
    self,
    cost_gain: dict[str, float] | None = None,
    use_fl: bool = True,
    with_mask: bool = False,
    num_sample_points: int = 12544,
    alpha: float = 0.25,
    gamma: float = 2.0,
)
```

Example 2 (json):
```json
Initialize a HungarianMatcher with custom cost gains
>>> matcher = HungarianMatcher(cost_gain={"class": 2, "bbox": 5, "giou": 2})

Perform matching between predictions and ground truth
>>> pred_boxes = torch.rand(2, 100, 4)  # batch_size=2, num_queries=100
>>> pred_scores = torch.rand(2, 100, 80)  # 80 classes
>>> gt_boxes = torch.rand(10, 4)  # 10 ground truth boxes
>>> gt_classes = torch.randint(0, 80, (10,))
>>> gt_groups = [5, 5]  # 5 GT boxes per image
>>> indices = matcher(pred_boxes, pred_scores, gt_boxes, gt_classes, gt_groups)
```

Example 3 (python):
```python
class HungarianMatcher(nn.Module):
    """A module implementing the HungarianMatcher for optimal assignment between predictions and ground truth.

    HungarianMatcher performs optimal bipartite assignment over predicted and ground truth bounding boxes using a cost
    function that considers classification scores, bounding box coordinates, and optionally mask predictions. This is
    used in end-to-end object detection models like DETR.

    Attributes:
        cost_gain (dict[str, float]): Dictionary of cost coefficients for 'class', 'bbox', 'giou', 'mask', and 'dice'
            components.
        use_fl (bool): Whether to use Focal Loss for classification cost calculation.
        with_mask (bool): Whether the model makes mask predictions.
        num_sample_points (int): Number of sample points used in mask cost calculation.
        alpha (float): Alpha factor in Focal Loss calculation.
        gamma (float): Gamma factor in Focal Loss calculation.

    Methods:
        forward: Compute optimal assignment between predictions and ground truths for a batch.
        _cost_mask: Compute mask cost and dice cost if masks are predicted.

    Examples:
        Initialize a HungarianMatcher with custom cost gains
        >>> matcher = HungarianMatcher(cost_gain={"class": 2, "bbox": 5, "giou": 2})

        Perform matching between predictions and ground truth
        >>> pred_boxes = torch.rand(2, 100, 4)  # batch_size=2, num_queries=100
        >>> pred_scores = torch.rand(2, 100, 80)  # 80 classes
        >>> gt_boxes = torch.rand(10, 4)  # 10 ground truth boxes
        >>> gt_classes = torch.randint(0, 80, (10,))
        >>> gt_groups = [5, 5]  # 5 GT boxes per image
        >>> indices = matcher(pred_boxes, pred_scores, gt_boxes, gt_classes, gt_groups)
    """

    def __init__(
        self,
        cost_gain: dict[str, float] | None = None,
        use_fl: bool = True,
        with_mask: bool = False,
        num_sample_points: int = 12544,
        alpha: float = 0.25,
        gamma: float = 2.0,
    ):
        """Initialize HungarianMatcher for optimal assignment of predicted and ground truth bounding boxes.

        Args:
            cost_gain (dict[str, float], optional): Dictionary of cost coefficients for different matching cost
                components. Should contain keys 'class', 'bbox', 'giou', 'mask', and 'dice'.
            use_fl (bool): Whether to use Focal Loss for classification cost calculation.
            with_mask (bool): Whether the model makes mask predictions.
            num_sample_points (int): Number of sample points used in mask cost calculation.
            alpha (float): Alpha factor in Focal Loss calculation.
            gamma (float): Gamma factor in Focal Loss calculation.
        """
        super().__init__()
        if cost_gain is None:
            cost_gain = {"class": 1, "bbox": 5, "giou": 2, "mask": 1, "dice": 1}
        self.cost_gain = cost_gain
        self.use_fl = use_fl
        self.with_mask = with_mask
        self.num_sample_points = num_sample_points
        self.alpha = alpha
        self.gamma = gamma
```

Example 4 (python):
```python
def forward(
    self,
    pred_bboxes: torch.Tensor,
    pred_scores: torch.Tensor,
    gt_bboxes: torch.Tensor,
    gt_cls: torch.Tensor,
    gt_groups: list[int],
    masks: torch.Tensor | None = None,
    gt_mask: list[torch.Tensor] | None = None,
) -> list[tuple[torch.Tensor, torch.Tensor]]
```

---

## 使用 Ultralytics YOLO11 的 Streamlit 应用程序进行实时推理

**URL:** https://docs.ultralytics.com/zh/guides/streamlit-live-inference/

**Contents:**
- 使用 Ultralytics YOLO11 的 Streamlit 应用程序进行实时推理
- 简介
- 实时推理的优势
- Streamlit 应用代码
- 工作原理
- 结论
- 与社区分享您的想法
  - 在哪里可以找到帮助和支持
  - 官方文档
- 常见问题

Streamlit 使构建和部署交互式 Web 应用程序变得简单。将其与 Ultralytics YOLO11 结合使用，可以直接在您的浏览器中进行实时对象检测和分析。YOLO11 的高精度和速度确保了实时视频流的无缝性能，使其成为安全、零售等领域应用的理想选择。

观看： 如何将 Streamlit 与 Ultralytics 结合使用以实现实时性 计算机视觉 在您的浏览器中

在开始构建应用程序之前，请确保您已安装 Ultralytics python 包。

使用 Streamlit 和 Ultralytics YOLO 进行推理

这些命令启动随 Ultralytics 提供的默认 Streamlit 界面。使用 yolo solutions inference --help 以查看其他标志，例如 source, conf或 persist 如果您想在不编辑Python代码的情况下自定义体验。

这将在您的默认网络浏览器中启动Streamlit应用程序。您将看到主标题、副标题以及带有配置选项的侧边栏。选择您想要的YOLO11模型，设置置信度和NMS阈值，然后点击“开始”按钮以开始实时object detection。

在底层，Streamlit 应用程序使用 Ultralytics 解决方案模块 来创建一个交互式界面。当您开始推理时，该应用程序：

该应用程序提供了一个干净、用户友好的界面，其中包含用于调整模型参数以及随时启动/停止推理的控件。

通过遵循本指南，您已经成功创建了一个使用 Streamlit 和 Ultralytics YOLO11 的实时对象检测应用程序。此应用程序允许您体验 YOLO11 在通过网络摄像头检测对象方面的强大功能，它具有用户友好的界面，并且可以随时停止视频流。

为了进一步增强功能，您可以探索添加更多功能，例如录制视频流、保存带注释的帧或与其他计算机视觉库集成。

与社区互动以了解更多信息、解决问题并分享您的项目：

使用 Streamlit 和 Ultralytics YOLO11 设置实时对象检测应用程序非常简单。首先，请确保已使用以下命令安装 Ultralytics python 包：

然后，您可以创建一个基本的 Streamlit 应用程序来运行实时推理：

有关实际设置的更多详细信息，请参阅文档的Streamlit 应用程序代码部分。

将 Ultralytics YOLO11 与 Streamlit 结合使用进行实时对象检测具有以下几个优势：

在实时推理的优势部分中了解更多关于这些优势的信息。

在完成整合 Ultralytics YOLO11 的 Streamlit 应用程序编码后，您可以通过运行以下命令来部署它：

此命令将在您的默认网络浏览器中启动应用程序，使您能够选择 YOLO11 模型、设置置信度和 NMS 阈值，并通过简单的点击开始实时目标检测。有关详细指南，请参阅Streamlit 应用程序代码部分。

使用 Streamlit 和 Ultralytics YOLO11 的实时目标检测可以应用于各个领域：

要了解更多深入的使用案例和示例，请浏览 Ultralytics 解决方案。

与 YOLOv5 和 RCNN 等早期模型相比，Ultralytics YOLO11 提供了以下几项增强功能：

有关全面的比较，请查看Ultralytics YOLO11 文档和讨论模型性能的相关博客文章。

**Examples:**

Example 1 (unknown):
```unknown
pip install ultralytics
```

Example 2 (unknown):
```unknown
yolo solutions inference

yolo solutions inference model="path/to/model.pt"
```

Example 3 (python):
```python
from ultralytics import solutions

inf = solutions.Inference(
    model="yolo11n.pt",  # you can use any model that Ultralytics supports, e.g., YOLO11, or a custom-trained model
)

inf.inference()

# Make sure to run the file using command `streamlit run path/to/file.py`
```

Example 4 (unknown):
```unknown
pip install ultralytics
```

---

## Reference for ultralytics/models/rtdetr/predict.py

**URL:** https://docs.ultralytics.com/zh/reference/models/rtdetr/predict/

**Contents:**
- Reference for ultralytics/models/rtdetr/predict.py
- class ultralytics.models.rtdetr.predict.RTDETRPredictor
  - method ultralytics.models.rtdetr.predict.RTDETRPredictor.postprocess
  - method ultralytics.models.rtdetr.predict.RTDETRPredictor.pre_transform

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/models/rtdetr/predict.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

RT-DETR (Real-Time Detection Transformer) Predictor extending the BasePredictor class for making predictions.

This class leverages Vision Transformers to provide real-time object detection while maintaining high accuracy. It supports key features like efficient hybrid encoding and IoU-aware query selection.

Postprocess the raw predictions from the model to generate bounding boxes and confidence scores.

The method filters detections based on confidence and class if specified in self.args. It converts model predictions to Results objects containing properly scaled bounding boxes.

Pre-transform input images before feeding them into the model for inference.

The input images are letterboxed to ensure a square aspect ratio and scale-filled.

**Examples:**

Example 1 (unknown):
```unknown
RTDETRPredictor()
```

Example 2 (sql):
```sql
>>> from ultralytics.utils import ASSETS
>>> from ultralytics.models.rtdetr import RTDETRPredictor
>>> args = dict(model="rtdetr-l.pt", source=ASSETS)
>>> predictor = RTDETRPredictor(overrides=args)
>>> predictor.predict_cli()
```

Example 3 (php):
```php
class RTDETRPredictor(BasePredictor):
```

Example 4 (python):
```python
def postprocess(self, preds, img, orig_imgs)
```

---

## Reference for ultralytics/engine/predictor.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/engine/predictor/

**Contents:**
- Reference for ultralytics/engine/predictor.py
- class ultralytics.engine.predictor.BasePredictor
  - method ultralytics.engine.predictor.BasePredictor.__call__
  - method ultralytics.engine.predictor.BasePredictor.add_callback
  - method ultralytics.engine.predictor.BasePredictor.inference
  - method ultralytics.engine.predictor.BasePredictor.postprocess
  - method ultralytics.engine.predictor.BasePredictor.pre_transform
  - method ultralytics.engine.predictor.BasePredictor.predict_cli
  - method ultralytics.engine.predictor.BasePredictor.preprocess
  - method ultralytics.engine.predictor.BasePredictor.run_callbacks

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/engine/predictor.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A base class for creating predictors.

This class provides the foundation for prediction functionality, handling model setup, inference, and result processing across various input sources.

Perform inference on an image or stream.

Add a callback function for a specific event.

Run inference on a given image using the specified model and arguments.

Post-process predictions for an image and return them.

Pre-transform input image before inference.

Method used for Command Line Interface (CLI) prediction.

This function is designed to run predictions using the CLI. It sets up the source and model, then processes the inputs in a streaming manner. This method ensures that no outputs accumulate in memory by consuming the generator without storing results.

Do not modify this function or remove the generator. The generator ensures that no outputs are accumulated in memory, which is critical for preventing memory issues during long-running predictions.

Prepare input image before inference.

Run all registered callbacks for a specific event.

Save video predictions as mp4 or images as jpg at specified path.

Initialize YOLO model with given parameters and set it to evaluation mode.

Set up source and inference mode.

Display an image in a window.

Stream real-time inference on camera feed and save results to file.

Write inference results to a file or directory.

**Examples:**

Example 1 (python):
```python
def __init__(
    self,
    cfg=DEFAULT_CFG,
    overrides: dict[str, Any] | None = None,
    _callbacks: dict[str, list[callable]] | None = None,
)
```

Example 2 (python):
```python
class BasePredictor:
    """A base class for creating predictors.

    This class provides the foundation for prediction functionality, handling model setup, inference, and result
    processing across various input sources.

    Attributes:
        args (SimpleNamespace): Configuration for the predictor.
        save_dir (Path): Directory to save results.
        done_warmup (bool): Whether the predictor has finished setup.
        model (torch.nn.Module): Model used for prediction.
        data (dict): Data configuration.
        device (torch.device): Device used for prediction.
        dataset (Dataset): Dataset used for prediction.
        vid_writer (dict[str, cv2.VideoWriter]): Dictionary of {save_path: video_writer} for saving video output.
        plotted_img (np.ndarray): Last plotted image.
        source_type (SimpleNamespace): Type of input source.
        seen (int): Number of images processed.
        windows (list[str]): List of window names for visualization.
        batch (tuple): Current batch data.
        results (list[Any]): Current batch results.
        transforms (callable): Image transforms for classification.
        callbacks (dict[str, list[callable]]): Callback functions for different events.
        txt_path (Path): Path to save text results.
        _lock (threading.Lock): Lock for thread-safe inference.

    Methods:
        preprocess: Prepare input image before inference.
        inference: Run inference on a given image.
        postprocess: Process raw predictions into structured results.
        predict_cli: Run prediction for command line interface.
        setup_source: Set up input source and inference mode.
        stream_inference: Stream inference on input source.
        setup_model: Initialize and configure the model.
        write_results: Write inference results to files.
        save_predicted_images: Save prediction visualizations.
        show: Display results in a window.
        run_callbacks: Execute registered callbacks for an event.
        add_callback: Register a new callback function.
    """

    def __init__(
        self,
        cfg=DEFAULT_CFG,
        overrides: dict[str, Any] | None = None,
        _callbacks: dict[str, list[callable]] | None = None,
    ):
        """Initialize the BasePredictor class.

        Args:
            cfg (str | dict): Path to a configuration file or a configuration dictionary.
            overrides (dict, optional): Configuration overrides.
            _callbacks (dict, optional): Dictionary of callback functions.
        """
        self.args = get_cfg(cfg, overrides)
        self.save_dir = get_save_dir(self.args)
        if self.args.conf is None:
            self.args.conf = 0.25  # default conf=0.25
        self.done_warmup = False
        if self.args.show:
            self.args.show = check_imshow(warn=True)

        # Usable if setup is done
        self.model = None
        self.data = self.args.data  # data_dict
        self.imgsz = None
        self.device = None
        self.dataset = None
        self.vid_writer = {}  # dict of {save_path: video_writer, ...}
        self.plotted_img = None
        self.source_type = None
        self.seen = 0
        self.windows = []
        self.batch = None
        self.results = None
        self.transforms = None
        self.callbacks = _callbacks or callbacks.get_default_callbacks()
        self.txt_path = None
        self._lock = threading.Lock()  # for automatic thread-safe inference
        callbacks.add_integration_callbacks(self)
```

Example 3 (typescript):
```typescript
def __call__(self, source = None, model = None, stream: bool = False, *args, **kwargs)
```

Example 4 (python):
```python
def __call__(self, source=None, model=None, stream: bool = False, *args, **kwargs):
    """Perform inference on an image or stream.

    Args:
        source (str | Path | list[str] | list[Path] | list[np.ndarray] | np.ndarray | torch.Tensor, optional):
            Source for inference.
        model (str | Path | torch.nn.Module, optional): Model for inference.
        stream (bool): Whether to stream the inference results. If True, returns a generator.
        *args (Any): Additional arguments for the inference method.
        **kwargs (Any): Additional keyword arguments for the inference method.

    Returns:
        (list[ultralytics.engine.results.Results] | generator): Results objects or generator of Results objects.
    """
    self.stream = stream
    if stream:
        return self.stream_inference(source, model, *args, **kwargs)
    else:
        return list(self.stream_inference(source, model, *args, **kwargs))  # merge list of Results into one
```

---

## Reference for ultralytics/models/yolo/classify/predict.py

**URL:** https://docs.ultralytics.com/zh/reference/models/yolo/classify/predict/

**Contents:**
- Reference for ultralytics/models/yolo/classify/predict.py
- class ultralytics.models.yolo.classify.predict.ClassificationPredictor
  - method ultralytics.models.yolo.classify.predict.ClassificationPredictor.postprocess
  - method ultralytics.models.yolo.classify.predict.ClassificationPredictor.preprocess
  - method ultralytics.models.yolo.classify.predict.ClassificationPredictor.setup_source

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/models/yolo/classify/predict.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class extending the BasePredictor class for prediction based on a classification model.

This predictor handles the specific requirements of classification models, including preprocessing images and postprocessing predictions to generate classification results.

This constructor initializes a ClassificationPredictor instance, which extends BasePredictor for classification tasks. It ensures the task is set to 'classify' regardless of input configuration.

Process predictions to return Results objects with classification probabilities.

Convert input images to model-compatible tensor format with appropriate normalization.

Set up source and inference mode and classify transforms.

**Examples:**

Example 1 (rust):
```rust
ClassificationPredictor(self, cfg = DEFAULT_CFG, overrides = None, _callbacks = None)
```

Example 2 (sql):
```sql
>>> from ultralytics.utils import ASSETS
>>> from ultralytics.models.yolo.classify import ClassificationPredictor
>>> args = dict(model="yolo11n-cls.pt", source=ASSETS)
>>> predictor = ClassificationPredictor(overrides=args)
>>> predictor.predict_cli()
```

Example 3 (python):
```python
class ClassificationPredictor(BasePredictor):
    """A class extending the BasePredictor class for prediction based on a classification model.

    This predictor handles the specific requirements of classification models, including preprocessing images and
    postprocessing predictions to generate classification results.

    Attributes:
        args (dict): Configuration arguments for the predictor.

    Methods:
        preprocess: Convert input images to model-compatible format.
        postprocess: Process model predictions into Results objects.

    Examples:
        >>> from ultralytics.utils import ASSETS
        >>> from ultralytics.models.yolo.classify import ClassificationPredictor
        >>> args = dict(model="yolo11n-cls.pt", source=ASSETS)
        >>> predictor = ClassificationPredictor(overrides=args)
        >>> predictor.predict_cli()

    Notes:
        - Torchvision classification models can also be passed to the 'model' argument, i.e. model='resnet18'.
    """

    def __init__(self, cfg=DEFAULT_CFG, overrides=None, _callbacks=None):
        """Initialize the ClassificationPredictor with the specified configuration and set task to 'classify'.

        This constructor initializes a ClassificationPredictor instance, which extends BasePredictor for classification
        tasks. It ensures the task is set to 'classify' regardless of input configuration.

        Args:
            cfg (dict): Default configuration dictionary containing prediction settings.
            overrides (dict, optional): Configuration overrides that take precedence over cfg.
            _callbacks (list, optional): List of callback functions to be executed during prediction.
        """
        super().__init__(cfg, overrides, _callbacks)
        self.args.task = "classify"
```

Example 4 (python):
```python
def postprocess(self, preds, img, orig_imgs)
```

---

## 在 Vertex AI 上使用 Ultralytics 部署预训练的 YOLO 模型进行推理

**URL:** https://docs.ultralytics.com/zh/guides/vertex-ai-deployment-with-docker/

**Contents:**
- 在 Vertex AI 上使用 Ultralytics 部署预训练的 YOLO 模型进行推理
- 您将学到什么
- 准备工作
- 1. 使用FastAPI创建一个推理后端
  - Vertex AI 合规性基础
  - 项目文件夹结构
  - 使用依赖项创建 pyproject.toml
  - 使用 Ultralytics YOLO11 创建推理逻辑
  - 使用 FastAPI 创建 HTTP 推理服务器
- 2. 使用您的应用程序扩展Ultralytics Docker镜像

本指南将向您展示如何使用 Ultralytics 对预训练的 YOLO11 模型进行容器化，为其构建 FastAPI 推理服务器，并在 Google Cloud Vertex AI 上部署带有推理服务器的模型。示例实现将涵盖 YOLO11 的对象 detect 用例，但相同的原理也适用于使用其他 YOLO 模式。

在开始之前，您需要创建一个 Google Cloud Platform (GCP) 项目。作为新用户，您将获得 300 美元的 GCP 抵用金以供免费使用，此金额足以测试一个正在运行的设置，您可以稍后将其扩展到任何其他 YOLO11 用例，包括训练或批量和流式推理。

首先，您需要创建一个 FastAPI 应用程序，该应用程序将服务于 YOLO11 模型推理请求。此应用程序将处理模型加载、图像预处理和推理（预测）逻辑。

Vertex AI 要求您的容器实现两个特定端点：

预测 端点 (/predict）：接受带有以下内容的结构化预测请求： base64 编码 图像和可选参数。 有效负载大小限制 根据端点类型应用。

的请求有效负载 /predict 端点应遵循以下 JSON 结构：

我们的大部分构建工作将在 Docker 容器内部进行，并且 Ultralytics 也将加载一个预训练的 YOLO11 模型，因此您可以保持本地文件夹结构简单：

Ultralytics YOLO11模型和框架在AGPL-3.0下获得许可，该许可具有重要的合规性要求。请务必阅读Ultralytics文档，了解如何遵守许可条款。

要方便地管理您的项目，请创建一个 pyproject.toml 文件，包含以下依赖项：

现在您已经设置了项目结构和依赖项，您可以实现核心 YOLO11 推理逻辑。创建一个 src/app.py 文件，它将使用 Ultralytics Python API 处理模型加载、图像处理和预测。

这将在容器启动时加载一次模型，并且该模型将在所有请求之间共享。如果您的模型将处理繁重的推理负载，建议在稍后的步骤中在 Vertex AI 中导入模型时，选择具有更多内存的机器类型。

接下来，创建两个实用函数，用于输入和输出图像处理，使用 pillow。YOLO11原生支持PIL图像。

最后，实现 run_inference 该函数将处理对象检测。 在此示例中，我们将从模型预测中提取边界框、类名称和置信度分数。 该函数将返回一个包含检测结果和原始结果的字典，以供进一步处理或注释。

（可选）您可以添加一个函数，使用 Ultralytics 内置的绘图方法来注释带有边界框和标签的图像。如果您想在预测响应中返回带注释的图像，这将非常有用。

现在您已经有了核心 YOLO11 推理逻辑，您可以创建一个 FastAPI 应用程序来提供它。这将包括 Vertex AI 所需的健康检查和预测端点。

首先，添加导入并配置 Vertex AI 的日志记录。由于 Vertex AI 将 stderr 视为错误输出，因此将日志通过管道传输到 stdout 是有意义的。

为了完全符合 Vertex AI 的合规性，请在环境变量中定义所需的端点，并设置请求的大小限制。建议对生产部署使用私有 Vertex AI 端点。这样，您将获得更高的请求负载限制（公共端点为 1.5 MB，而私有端点为 10 MB），以及强大的安全性和访问控制。

添加两个 Pydantic 模型以验证您的请求和响应：

添加运行状况检查端点以验证您的模型准备情况。 这对 Vertex AI 非常重要，因为如果没有专门的健康检查，其编排器将 ping 随机套接字，并且无法确定模型是否已准备好进行推理。您的检查必须返回 200 OK 为了成功和 503 Service Unavailable 失败时：

现在，您拥有了实现预测端点所需的一切，该端点将处理推理请求。它将接受图像文件，运行推理并返回结果。请注意，图像必须经过 base64 编码，这还会使有效负载的大小增加多达 33%。

最后，添加应用程序入口点以运行 FastAPI 服务器。

现在，您有了一个完整的 FastAPI 应用程序，可以提供 YOLO11 推理请求。您可以通过安装依赖项并运行服务器在本地对其进行测试，例如使用 uv。

要测试服务器，你可以查询 /health 和 /predict 使用 cURL 的端点。将测试图像放入 tests 文件夹。然后在您的终端中，运行以下命令：

您应该收到一个包含检测到的对象的JSON响应。在您第一次请求时，预计会有短暂的延迟，因为Ultralytics需要拉取并加载 YOLO11 模型。

Ultralytics 提供了多个 Docker 镜像，您可以将它们用作应用程序镜像的基础。Docker 将安装 Ultralytics 和必要的 GPU 驱动程序。

要使用 Ultralytics YOLO 模型的全部功能，你应该选择 CUDA 优化的图像以进行 GPU 推理。但是，如果 CPU 推理足以满足你的任务，你也可以通过选择仅使用 CPU 的图像来节省计算资源：

创建一个 Dockerfile 在项目的根目录中，包含以下内容：

在此示例中，使用了 Ultralytics 官方 Docker 镜像 ultralytics:latest 用作基础。它已经包含 YOLO11 模型和所有必要的依赖项。服务器的入口点与我们用于在本地测试 FastAPI 应用程序的入口点相同。

现在您可以使用以下命令构建 Docker 镜像：

替换 IMAGE_NAME 和 IMAGE_VERSION 使用您期望的值，例如， yolo11-fastapi:0.1。请注意，您必须为 linux/amd64 如果您在 Vertex AI 上部署，请应用架构。 --platform 如果您在 Apple Silicon Mac 或任何其他非 x86 架构上构建镜像，则需要显式设置该参数。

一旦镜像构建完成，您可以在本地测试 Docker 镜像：

您的 Docker 容器现在正在端口上运行 FastAPI 服务器 8080，已准备好接受推理请求。您可以测试 /health 和 /predict 端点，使用与之前相同的 cURL 命令：

要在 Vertex AI 中导入容器化模型，您需要将 Docker 镜像上传到 Google Cloud Artifact Registry。如果您还没有 Artifact Registry 存储库，则需要先创建一个。

在 Google Cloud Console 中打开 Artifact Registry 页面。如果您是首次使用 Artifact Registry，系统可能会提示您先启用 Artifact Registry API。

区域选择可能会影响机器的可用性以及非企业用户的某些计算限制。您可以在 Vertex AI 官方文档中找到更多信息：Vertex AI 配额和限制

将您的 Docker 客户端验证到您刚刚创建的 Artifact Registry 仓库。在您的终端中运行以下命令：

标记 Docker 镜像并将其推送到 Google Artifact Registry。

建议每次更新镜像时都使用唯一的标签。大多数 GCP 服务（包括 Vertex AI）都依赖于镜像标签来实现自动版本控制和扩展，因此使用语义版本控制或基于日期的标签是一个好习惯。

使用 Artifact Registry 存储库 URL 标记您的镜像。将占位符替换为您之前保存的值。

将已标记的图像推送到 Artifact Registry 存储库。

等待流程完成。现在您应该可以在您的 Artifact Registry 仓库中看到该图像。

有关如何在 Artifact Registry 中使用图像的更多具体说明，请参阅 Artifact Registry 文档：推送和拉取镜像。

使用您刚刚推送的 Docker 镜像，您现在可以在 Vertex AI 中导入模型。

在 Google Cloud 导航菜单中，转到 Vertex AI > 模型注册表。或者，在 Google Cloud Console 顶部的搜索栏中搜索“Vertex AI”。

在 Container image 字段中，浏览您之前创建的 Artifact Registry 仓库，然后选择您刚刚推送的镜像。

向下滚动到“环境变量”部分，然后输入 predict 和 health 端点，以及您在 FastAPI 应用程序中定义的端口。

点击导入。Vertex AI 将需要几分钟来注册模型并准备部署。导入完成后，您将收到一封电子邮件通知。

在 Vertex AI 术语中，端点指的是已部署的模型，因为它们代表您发送推理请求的 HTTP 端点，而模型是存储在模型注册表中的已训练 ML 项目。

要部署模型，您需要在 Vertex AI 中创建一个 Endpoint。

在加速器类型中，选择您要用于推理的 GPU 类型。如果您不确定选择哪个 GPU，可以从 NVIDIA T4 开始，它支持 CUDA。

请记住，某些地区的计算配额非常有限，因此您可能无法在您所在的地区选择某些机器类型或 GPU。如果这一点至关重要，请将您的部署区域更改为配额更大的区域。有关更多信息，请参阅 Vertex AI 官方文档：Vertex AI 配额和限制。

选择机器类型后，您可以点击继续。此时，您可以选择在Vertex AI中启用模型监控——这是一项额外服务，将跟踪您的模型性能并提供对其行为的洞察。此功能是可选的，并会产生额外费用，因此请根据您的需求进行选择。点击创建。

Vertex AI 将花费几分钟（在某些区域最多 30 分钟）来部署模型。部署完成后，您将收到一封电子邮件通知。

部署完成后，Vertex AI 将为您提供一个示例 API 接口来测试您的模型。

要测试远程推理，你可以使用提供的 cURL 命令或创建另一个 python 客户端库，该库将向已部署的模型发送请求。请记住，你需要先将图像编码为 base64，然后再将其发送到 /predict 端点。

与本地测试类似，首次请求时预计会有短暂延迟，因为 Ultralytics 需要在运行的容器中拉取并加载 YOLO11 模型。

您已成功使用Ultralytics在Google Cloud Vertex AI上部署了预训练的YOLO11模型。

是的；但是，您首先需要将模型导出为与 Vertex AI 兼容的格式，例如 TensorFlow、Scikit-learn 或 XGBoost。Google Cloud 提供了一份关于运行 .pt 模型在 Vertex 上的部署，其中包含转换过程的完整概述： 在 Vertex AI 上运行 PyTorch 模型.

请注意，最终的设置将仅依赖于 Vertex AI 标准服务层，并且不支持高级的 Ultralytics 框架功能。由于 Vertex AI 完全支持容器化模型，并能根据您的部署配置自动扩展，因此它允许您充分利用 Ultralytics YOLO 模型的全部功能，而无需将它们转换为不同的格式。

FastAPI 为推理工作负载提供高吞吐量。异步支持允许处理多个并发请求，而不会阻塞主线程，这在服务计算机视觉模型时非常重要。

使用 FastAPI 进行自动请求/响应验证，可减少生产推理服务中的运行时错误。这对于输入格式一致性至关重要的对象检测 API 尤其有价值。

FastAPI 为您的推理管道增加了最少的计算开销，从而为模型执行和图像处理任务留出更多可用资源。

FastAPI 还支持 SSE（服务器发送事件），这对于流式推理场景非常有用。

这实际上是 Google Cloud Platform 的一项多功能特性，您需要为使用的每项服务选择一个区域。对于在 Vertex AI 上部署容器化模型的任务，您最重要的区域选择是模型注册表的区域。它将决定模型部署的机器类型和配额的可用性。

此外，如果您将扩展设置并将预测数据或结果存储在 Cloud Storage 或 BigQuery 中，您将需要使用与模型注册表相同的区域，以最大程度地减少延迟并确保数据访问的高吞吐量。

**Examples:**

Example 1 (json):
```json
{
    "instances": [{ "image": "base64_encoded_image" }],
    "parameters": { "confidence": 0.5 }
}
```

Example 2 (unknown):
```unknown
YOUR_PROJECT/
├── src/
│   ├── __init__.py
│   ├── app.py              # Core YOLO11 inference logic
│   └── main.py             # FastAPI inference server
├── tests/
├── .env                    # Environment variables for local development
├── Dockerfile              # Container configuration
├── LICENSE                 # AGPL-3.0 License
└── pyproject.toml          # Python dependencies and project config
```

Example 3 (json):
```json
[project]
name = "YOUR_PROJECT_NAME"
version = "0.0.1"
description = "YOUR_PROJECT_DESCRIPTION"
requires-python = ">=3.10,<3.13"
dependencies = [
   "ultralytics>=8.3.0",
   "fastapi[all]>=0.89.1",
   "uvicorn[standard]>=0.20.0",
   "pillow>=9.0.0",
]

[build-system]
requires = ["setuptools>=61.0"]
build-backend = "setuptools.build_meta"
```

Example 4 (python):
```python
# src/app.py

from ultralytics import YOLO

# Model initialization and readiness state
model_yolo = None
_model_ready = False


def _initialize_model():
    """Initialize the YOLO model."""
    global model_yolo, _model_ready

    try:
        # Use pretrained YOLO11n model from Ultralytics base image
        model_yolo = YOLO("yolo11n.pt")
        _model_ready = True

    except Exception as e:
        print(f"Error initializing YOLO model: {e}")
        _model_ready = False
        model_yolo = None


# Initialize model on module import
_initialize_model()


def is_model_ready() -> bool:
    """Check if the model is ready for inference."""
    return _model_ready and model_yolo is not None
```

---

## Reference for ultralytics/trackers/track.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/trackers/track/

**Contents:**
- Reference for ultralytics/trackers/track.py
- function ultralytics.trackers.track.on_predict_start
- function ultralytics.trackers.track.on_predict_postprocess_end
- function ultralytics.trackers.track.register_tracker

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/trackers/track.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Initialize trackers for object tracking during prediction.

Postprocess detected boxes and update with object tracking.

Register tracking callbacks to the model for object tracking during prediction.

**Examples:**

Example 1 (typescript):
```typescript
def on_predict_start(predictor: object, persist: bool = False) -> None
```

Example 2 (unknown):
```unknown
Initialize trackers for a predictor object
>>> predictor = SomePredictorClass()
>>> on_predict_start(predictor, persist=True)
```

Example 3 (python):
```python
def on_predict_start(predictor: object, persist: bool = False) -> None:
    """Initialize trackers for object tracking during prediction.

    Args:
        predictor (ultralytics.engine.predictor.BasePredictor): The predictor object to initialize trackers for.
        persist (bool, optional): Whether to persist the trackers if they already exist.

    Examples:
        Initialize trackers for a predictor object
        >>> predictor = SomePredictorClass()
        >>> on_predict_start(predictor, persist=True)
    """
    if predictor.args.task == "classify":
        raise ValueError("❌ Classification doesn't support 'mode=track'")

    if hasattr(predictor, "trackers") and persist:
        return

    tracker = check_yaml(predictor.args.tracker)
    cfg = IterableSimpleNamespace(**YAML.load(tracker))

    if cfg.tracker_type not in {"bytetrack", "botsort"}:
        raise AssertionError(f"Only 'bytetrack' and 'botsort' are supported for now, but got '{cfg.tracker_type}'")

    predictor._feats = None  # reset in case used earlier
    if hasattr(predictor, "_hook"):
        predictor._hook.remove()
    if cfg.tracker_type == "botsort" and cfg.with_reid and cfg.model == "auto":
        from ultralytics.nn.modules.head import Detect

        if not (
            isinstance(predictor.model.model, torch.nn.Module)
            and isinstance(predictor.model.model.model[-1], Detect)
            and not predictor.model.model.model[-1].end2end
        ):
            cfg.model = "yolo11n-cls.pt"
        else:
            # Register hook to extract input of Detect layer
            def pre_hook(module, input):
                predictor._feats = list(input[0])  # unroll to new list to avoid mutation in forward

            predictor._hook = predictor.model.model.model[-1].register_forward_pre_hook(pre_hook)

    trackers = []
    for _ in range(predictor.dataset.bs):
        tracker = TRACKER_MAP[cfg.tracker_type](args=cfg, frame_rate=30)
        trackers.append(tracker)
        if predictor.dataset.mode != "stream":  # only need one tracker for other modes
            break
    predictor.trackers = trackers
    predictor.vid_path = [None] * predictor.dataset.bs  # for determining when to reset tracker on new video
```

Example 4 (typescript):
```typescript
def on_predict_postprocess_end(predictor: object, persist: bool = False) -> None
```

---

## 回调 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/usage/callbacks/

**Contents:**
- 回调函数
- 示例
  - 返回带有预测的附加信息
  - 使用以下方式访问模型指标 on_model_save 回调函数
- 所有回调
  - 训练器回调
  - 验证器回调
  - 预测器回调
  - 导出器回调
- 常见问题

Ultralytics 框架支持回调函数，这些回调函数在训练过程的关键阶段作为入口点。 train, val, export和 predict 模式。每个回调都接受一个 Trainer, Validator或 Predictor 对象，具体取决于操作类型。这些对象的所有属性都在 参考章节 文档的。

观看： 如何使用 Ultralytics Callbacks | 预测、训练、验证和导出 Callbacks | Ultralytics YOLO🚀

在此示例中，我们将演示如何返回原始帧以及每个结果对象：

此示例展示了如何在保存检查点后检索训练详细信息，例如 best_fitness 分数、total_loss 和其他指标，使用 on_model_save 回调函数。

以下是所有支持的回调。有关更多详细信息，请参阅回调源代码。

Ultralytics 回调是专门的切入点，在模型运行的关键阶段（如训练、验证、导出和预测）期间触发。这些回调在过程的特定点启用自定义功能，允许对 Trainer, Validator或 Predictor 对象，具体取决于操作类型。有关这些对象的详细属性，请参阅 参考章节.

要使用回调，请定义一个函数并使用以下代码将其添加到模型中 model.add_callback() 方法。以下是一个在预测期间返回附加信息的示例：

通过在训练过程的特定阶段注入逻辑来自定义您的 Ultralytics 训练程序。Ultralytics YOLO 提供了各种训练回调，例如 on_train_start, on_train_end和 on_train_batch_end，允许您添加自定义指标、处理或日志记录。

以下是如何在使用回调冻结层时冻结 BatchNorm 统计信息：

有关有效使用训练回调的更多详细信息，请参见训练指南。

在 Ultralytics YOLO 验证期间使用回调可以通过启用自定义处理、日志记录或指标计算来增强模型评估。诸如的回调 on_val_start, on_val_batch_end和 on_val_end 之类的回调提供了注入自定义逻辑的入口点，从而确保详细而全面的验证过程。

例如，要绘制所有验证批次，而不仅仅是前三个：

有关将回调合并到验证过程中的更多见解，请参阅验证指南。

要在 Ultralytics YOLO 的预测模式中附加自定义回调，请定义一个回调函数并将其注册到预测过程中。常见的预测回调包括 on_predict_start, on_predict_batch_end和 on_predict_end。这些允许修改预测输出，并集成附加功能，如数据记录或结果转换。

以下示例展示了如何根据特定类别的对象是否存在，使用自定义回调来保存预测结果：

如需更全面的使用方法，请参阅预测指南，其中包含详细的说明和其他自定义选项。

Ultralytics YOLO 支持各种回调的实际应用，以增强和自定义不同的阶段，如训练、验证和预测。一些实际的例子包括：

示例：在使用期间，将帧与预测结果相结合 on_predict_batch_end:

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO


def on_predict_batch_end(predictor):
    """Combine prediction results with corresponding frames."""
    _, image, _, _ = predictor.batch

    # Ensure that image is a list
    image = image if isinstance(image, list) else [image]

    # Combine the prediction results with the corresponding frames
    predictor.results = zip(predictor.results, image)


# Create a YOLO model instance
model = YOLO("yolo11n.pt")

# Add the custom callback to the model
model.add_callback("on_predict_batch_end", on_predict_batch_end)

# Iterate through the results and frames
for result, frame in model.predict():  # or model.track()
    pass
```

Example 2 (python):
```python
from ultralytics import YOLO

# Load a YOLO model
model = YOLO("yolo11n.pt")


def print_checkpoint_metrics(trainer):
    """Print trainer metrics and loss details after each checkpoint is saved."""
    print(
        f"Model details\n"
        f"Best fitness: {trainer.best_fitness}, "
        f"Loss names: {trainer.loss_names}, "  # List of loss names
        f"Metrics: {trainer.metrics}, "
        f"Total loss: {trainer.tloss}"  # Total loss value
    )


if __name__ == "__main__":
    # Add on_model_save callback.
    model.add_callback("on_model_save", print_checkpoint_metrics)

    # Run model training on custom dataset.
    results = model.train(data="coco8.yaml", epochs=3)
```

Example 3 (python):
```python
from ultralytics import YOLO


def on_predict_batch_end(predictor):
    """Handle prediction batch end by combining results with corresponding frames; modifies predictor results."""
    _, image, _, _ = predictor.batch
    image = image if isinstance(image, list) else [image]
    predictor.results = zip(predictor.results, image)


model = YOLO("yolo11n.pt")
model.add_callback("on_predict_batch_end", on_predict_batch_end)
for result, frame in model.predict():
    pass
```

Example 4 (python):
```python
from ultralytics import YOLO


# Add a callback to put the frozen layers in eval mode to prevent BN values from changing
def put_in_eval_mode(trainer):
    n_layers = trainer.args.freeze
    if not isinstance(n_layers, int):
        return

    for i, (name, module) in enumerate(trainer.model.named_modules()):
        if name.endswith("bn") and int(name.split(".")[1]) < n_layers:
            module.eval()
            module.track_running_stats = False


model = YOLO("yolo11n.pt")
model.add_callback("on_train_epoch_start", put_in_eval_mode)
model.train(data="coco.yaml", epochs=10)
```

---

## Reference for ultralytics/models/sam/modules/decoders.py

**URL:** https://docs.ultralytics.com/zh/reference/models/sam/modules/decoders/

**Contents:**
- Reference for ultralytics/models/sam/modules/decoders.py
- class ultralytics.models.sam.modules.decoders.MaskDecoder
  - method ultralytics.models.sam.modules.decoders.MaskDecoder.forward
  - method ultralytics.models.sam.modules.decoders.MaskDecoder.predict_masks
- class ultralytics.models.sam.modules.decoders.SAM2MaskDecoder
  - method ultralytics.models.sam.modules.decoders.SAM2MaskDecoder._dynamic_multimask_via_stability
  - method ultralytics.models.sam.modules.decoders.SAM2MaskDecoder._get_stability_scores
  - method ultralytics.models.sam.modules.decoders.SAM2MaskDecoder.forward
  - method ultralytics.models.sam.modules.decoders.SAM2MaskDecoder.predict_masks

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/models/sam/modules/decoders.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Decoder module for generating masks and their associated quality scores using a transformer architecture.

This class predicts masks given image and prompt embeddings, utilizing a transformer to process the inputs and generate mask predictions along with their quality scores.

Predict masks given image and prompt embeddings.

Predict masks and quality scores using image and prompt embeddings via transformer architecture.

Transformer-based decoder for predicting instance segmentation masks from image and prompt embeddings.

This class extends the functionality of the MaskDecoder, incorporating additional features such as high-resolution feature processing, dynamic multimask output, and object score prediction.

This decoder extends the functionality of MaskDecoder, incorporating additional features such as high-resolution feature processing, dynamic multimask output, and object score prediction.

Dynamically select the most stable mask output based on stability scores and IoU predictions.

This method is used when outputting a single mask. If the stability score from the current single-mask output (based on output token 0) falls below a threshold, it instead selects from multi-mask outputs (based on output tokens 1-3) the mask with the highest predicted IoU score. This ensures a valid mask for both clicking and tracking scenarios.

Compute mask stability scores based on IoU between upper and lower thresholds.

Predict masks given image and prompt embeddings.

Predict instance segmentation masks from image and prompt embeddings using a transformer.

**Examples:**

Example 1 (python):
```python
def __init__(
    self,
    transformer_dim: int,
    transformer: nn.Module,
    num_multimask_outputs: int = 3,
    activation: type[nn.Module] = nn.GELU,
    iou_head_depth: int = 3,
    iou_head_hidden_dim: int = 256,
) -> None
```

Example 2 (json):
```json
>>> decoder = MaskDecoder(transformer_dim=256, transformer=transformer_module)
>>> masks, iou_pred = decoder(
...     image_embeddings, image_pe, sparse_prompt_embeddings, dense_prompt_embeddings, multimask_output=True
... )
>>> print(f"Predicted masks shape: {masks.shape}, IoU predictions shape: {iou_pred.shape}")
```

Example 3 (python):
```python
class MaskDecoder(nn.Module):
    """Decoder module for generating masks and their associated quality scores using a transformer architecture.

    This class predicts masks given image and prompt embeddings, utilizing a transformer to process the inputs and
    generate mask predictions along with their quality scores.

    Attributes:
        transformer_dim (int): Channel dimension for the transformer module.
        transformer (nn.Module): Transformer module used for mask prediction.
        num_multimask_outputs (int): Number of masks to predict for disambiguating masks.
        iou_token (nn.Embedding): Embedding for the IoU token.
        num_mask_tokens (int): Number of mask tokens.
        mask_tokens (nn.Embedding): Embedding for the mask tokens.
        output_upscaling (nn.Sequential): Neural network sequence for upscaling the output.
        output_hypernetworks_mlps (nn.ModuleList): Hypernetwork MLPs for generating masks.
        iou_prediction_head (nn.Module): MLP for predicting mask quality.

    Methods:
        forward: Predict masks given image and prompt embeddings.
        predict_masks: Internal method for mask prediction.

    Examples:
        >>> decoder = MaskDecoder(transformer_dim=256, transformer=transformer_module)
        >>> masks, iou_pred = decoder(
        ...     image_embeddings, image_pe, sparse_prompt_embeddings, dense_prompt_embeddings, multimask_output=True
        ... )
        >>> print(f"Predicted masks shape: {masks.shape}, IoU predictions shape: {iou_pred.shape}")
    """

    def __init__(
        self,
        transformer_dim: int,
        transformer: nn.Module,
        num_multimask_outputs: int = 3,
        activation: type[nn.Module] = nn.GELU,
        iou_head_depth: int = 3,
        iou_head_hidden_dim: int = 256,
    ) -> None:
        """Initialize the MaskDecoder module for generating masks and their associated quality scores.

        Args:
            transformer_dim (int): Channel dimension for the transformer module.
            transformer (nn.Module): Transformer module used for mask prediction.
            num_multimask_outputs (int): Number of masks to predict for disambiguating masks.
            activation (Type[nn.Module]): Type of activation to use when upscaling masks.
            iou_head_depth (int): Depth of the MLP used to predict mask quality.
            iou_head_hidden_dim (int): Hidden dimension of the MLP used to predict mask quality.
        """
        super().__init__()
        self.transformer_dim = transformer_dim
        self.transformer = transformer

        self.num_multimask_outputs = num_multimask_outputs

        self.iou_token = nn.Embedding(1, transformer_dim)
        self.num_mask_tokens = num_multimask_outputs + 1
        self.mask_tokens = nn.Embedding(self.num_mask_tokens, transformer_dim)

        self.output_upscaling = nn.Sequential(
            nn.ConvTranspose2d(transformer_dim, transformer_dim // 4, kernel_size=2, stride=2),
            LayerNorm2d(transformer_dim // 4),
            activation(),
            nn.ConvTranspose2d(transformer_dim // 4, transformer_dim // 8, kernel_size=2, stride=2),
            activation(),
        )
        self.output_hypernetworks_mlps = nn.ModuleList(
            [MLP(transformer_dim, transformer_dim, transformer_dim // 8, 3) for _ in range(self.num_mask_tokens)]
        )

        self.iou_prediction_head = MLP(transformer_dim, iou_head_hidden_dim, self.num_mask_tokens, iou_head_depth)
```

Example 4 (python):
```python
def forward(
    self,
    image_embeddings: torch.Tensor,
    image_pe: torch.Tensor,
    sparse_prompt_embeddings: torch.Tensor,
    dense_prompt_embeddings: torch.Tensor,
    multimask_output: bool,
) -> tuple[torch.Tensor, torch.Tensor]
```

---

## 命令行界面 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/usage/cli/

**Contents:**
- 命令行界面
- 训练
- 验证
- 预测
- 导出
- 覆盖默认参数
- 覆盖默认配置文件
- 解决方案命令
- 常见问题
  - 如何使用 Ultralytics YOLO 命令行界面 (CLI) 进行模型训练？

Ultralytics 命令行界面 (CLI) 提供了一种直接使用 Ultralytics YOLO 模型的方法，而无需 Python 环境。CLI 支持使用以下命令直接从终端运行各种任务 yolo 命令，无需定制或 python 代码。

观看： 精通 Ultralytics YOLO：CLI

Ultralytics yolo 命令使用以下语法：

哪里： - TASK （可选）是 [detect, segment, classify, 姿势估计, obb] 之一 - MODE （必需）是 [train, val, predict, export, track, benchmark] 之一 - ARGS （可选）是任意数量的自定义 arg=value 键值对，例如 imgsz=320 用于覆盖默认值。

在完整版中查看所有 ARGS 配置指南 或使用 yolo cfg.

训练一个检测模型 10 个 epochs，初始学习率为 0.01：

使用预训练的分割模型在YouTube视频上以320的图像尺寸进行预测：

使用 1 的 batch size 和 640 的图像大小验证预训练的检测模型：

将 YOLO 分类模型导出为 ONNX 格式，图像大小为 224x128（无需 TASK）：

运行特殊命令以查看版本、设置、运行检查等：

参数必须以 arg=val 对，用等号分隔 = 签名，并用空格分隔对。不要使用 -- 参数前缀或逗号 , 在参数之间。

在 COCO8 数据集上以图像尺寸 640 进行 100 个 epoch 的 YOLO 训练。有关可用参数的完整列表，请参阅配置页面。

开始在 COCO8 上训练 YOLO11n 100个 epoch，图像尺寸为 640：

验证 准确性 训练模型在 COCO8 数据集上的表现。无需任何参数，因为 model 保留其训练 data 和参数作为模型属性，因此无需任何参数。

将模型导出为其他格式，如 ONNX 或 CoreML。

将官方 YOLO11n 模型导出为 ONNX 格式：

将自定义训练的模型导出为 ONNX 格式：

下表列出了可用的 Ultralytics 导出格式。您可以使用以下命令导出为任何格式： format 参数，即 format='onnx' 或 format='engine'.

查看完整 export 有关的详细信息 导出 页面。

通过在 CLI 中传递参数来覆盖默认参数，例如 arg=value 键值对的形式传递参数来覆盖默认参数。

训练一个检测模型 10 个 epochs，学习率为 0.01：

使用预训练的分割模型在YouTube视频上以320的图像尺寸进行预测：

使用 1 的批量大小和 640 的图像大小验证预训练的检测模型：

覆盖 default.yaml 通过传递一个新文件来完全替换配置文件。 cfg 参数，例如 cfg=custom.yaml.

为此，首先创建一份副本 default.yaml 在您当前的工作目录中使用 yolo copy-cfg 命令，它会创建一个 default_copy.yaml 文件。

然后，您可以将此文件作为以下内容传递： cfg=default_copy.yaml 以及任何其他参数，例如 imgsz=320 在此示例中：

Ultralytics 通过 CLI 为常见的计算机视觉应用提供开箱即用的解决方案。这些解决方案简化了物体计数、运动监测和队列管理等复杂任务的实现。

使用 Streamlit 在 Web 浏览器中执行对象检测、实例分割或姿势估计：

有关 Ultralytics 解决方案的更多信息，请访问解决方案页面。

要使用 CLI 训练模型，请在终端中执行单行命令。例如，要训练一个检测模型 10 个 epoch，学习率为 0.01，请运行：

此命令使用 train 具有特定参数的模式。有关可用参数的完整列表，请参阅 配置指南.

Ultralytics YOLO CLI 支持各种任务，包括检测、分割、分类、姿势估计和旋转框检测。您还可以执行以下操作：

使用各种参数自定义每个任务。有关详细的语法和示例，请参阅相应的章节，如 Train、Predict 和 Export。

要验证模型的 准确性，使用 val 模式。例如，要验证具有批处理大小为 1 和图像大小为 640 的预训练检测模型，请运行： 批次大小 为 1，图像大小为 640，运行：

此命令会在指定数据集上评估模型，并提供性能指标，例如mAP、精确率和召回率。欲了解更多详情，请参阅验证部分。

您可以将 YOLO 模型导出为各种格式，包括 ONNX、TensorRT、CoreML、TensorFlow 等。例如，要将模型导出为 ONNX 格式，请运行：

export 命令支持许多选项，可以针对特定的部署环境优化您的模型。有关所有可用导出格式及其特定参数的完整详细信息，请访问导出页面。

Ultralytics 通过以下方式提供即用型解决方案 solutions 命令。例如，要计算视频中的对象：

这些解决方案只需要最少的配置，并为常见的计算机视觉任务提供即时功能。要查看所有可用的解决方案，请运行 yolo solutions help。每个解决方案都有特定的参数，可以自定义以满足您的需求。

**Examples:**

Example 1 (unknown):
```unknown
yolo TASK MODE ARGS
```

Example 2 (unknown):
```unknown
yolo train data=coco8.yaml model=yolo11n.pt epochs=10 lr0=0.01
```

Example 3 (unknown):
```unknown
yolo predict model=yolo11n-seg.pt source='https://youtu.be/LNwODJXcvt4' imgsz=320
```

Example 4 (unknown):
```unknown
yolo val model=yolo11n.pt data=coco8.yaml batch=1 imgsz=640
```

---

## 使用YOLO模型进行线程安全推理 - Ultralytics YOLO文档

**URL:** https://docs.ultralytics.com/zh/guides/yolo-thread-safe-inference/

**Contents:**
- 使用 YOLO 模型进行线程安全推理
- 理解 Python 线程
- 共享模型实例的风险
  - 非线程安全示例：单个模型实例
  - 非线程安全示例：多个模型实例
- 线程安全推理
  - 线程安全示例
- 使用 ThreadingLocked 装饰器
- 结论
- 常见问题

在多线程环境中运行 YOLO 模型需要仔细考虑，以确保线程安全。Python 的 threading 模块允许您同时运行多个线程，但当涉及到在这些线程中使用 YOLO 模型时，有一些重要的安全问题需要注意。本页将指导您创建线程安全的 YOLO 模型推理。

观看： 如何在 python 中使用 Ultralytics YOLO 模型执行线程安全推理 | 多线程 🚀

Python 线程是一种并行形式，允许您的程序一次运行多个操作。但是，Python 的全局解释器锁 (GIL) 意味着一次只能执行一个 Python 字节码。

虽然这听起来像是一个限制，但线程仍然可以提供并发性，特别是对于 I/O 密集型操作，或者当使用释放 GIL 的操作时，例如 YOLO 的底层 C 库执行的操作。

在线程外部实例化 YOLO 模型并在多个线程之间共享此实例可能会导致竞争条件，其中模型的内部状态由于并发访问而被不一致地修改。当模型或其组件保持并非设计为线程安全的状态时，这尤其成问题。

在 python 中使用线程时，重要的是要识别可能导致并发问题的模式。以下是您应该避免的情况：在多个线程之间共享单个 YOLO 模型实例。

在上面的例子中， shared_model 被多个线程使用，这可能会导致不可预测的结果，因为 predict 可以由多个线程同时执行。

类似地，这是一个包含多个 YOLO 模型实例的不安全模式：

即使存在两个独立的模型实例，仍然存在并发问题的风险。如果 YOLO 不是线程安全的，使用单独的实例可能无法防止竞争条件，特别是如果这些实例共享任何非线程本地的底层资源或状态。

要执行线程安全推理，您应该在每个线程中实例化一个单独的 YOLO 模型。这确保了每个线程都有其自己隔离的模型实例，从而消除了出现竞争条件的风险。

以下是如何在每个线程中实例化 YOLO 模型以实现安全的并行推理：

在此示例中，每个线程都创建了自己的 YOLO 实例。这可以防止任何线程干扰另一个线程的模型状态，从而确保每个线程安全地执行推理，并且不会与其他线程发生意外交互。

Ultralytics 提供了一个 ThreadingLocked 装饰器，可用于确保函数的线程安全执行。此装饰器使用锁来确保一次只有一个线程可以执行被装饰的函数。

字段 ThreadingLocked 装饰器在需要在线程之间共享模型实例，但又希望确保一次只有一个线程可以访问它时特别有用。与为每个线程创建一个新的模型实例相比，这种方法可以节省内存，但可能会降低并发性，因为线程需要等待锁被释放。

当将 YOLO 模型与 python 的 threading，始终在使用模型的线程中实例化模型，以确保线程安全。这种做法可以避免竞争条件，并确保推理任务可靠运行。

对于更高级的场景，并进一步优化您的多线程推理性能，请考虑使用基于进程的并行处理（使用 multiprocessing）或利用具有专用工作进程的任务队列。

为了防止在多线程 python 环境中使用 Ultralytics YOLO 模型时出现竞争条件，请在每个线程中实例化一个单独的 YOLO 模型。这确保了每个线程都有其自己隔离的模型实例，从而避免了模型状态的并发修改。

有关确保线程安全的更多信息，请访问使用 YOLO 模型进行线程安全推理。

为了在 Python 中安全地运行多线程 YOLO 模型推理，请遵循以下最佳实践：

有关其他上下文，请参阅关于线程安全推理的部分。

每个线程应有自己的 YOLO 模型实例，以防止出现竞争条件。当多个线程共享一个模型实例时，并发访问可能导致不可预测的行为和模型内部状态的修改。通过使用单独的实例，您可以确保线程隔离，从而使您的多线程任务可靠且安全。

有关详细指导，请查看非线程安全示例：单个模型实例和线程安全示例部分。

Python 的全局解释器锁 (GIL) 允许一次只执行一个 Python 字节码，这会限制 CPU 密集型多线程任务的性能。但是，对于 I/O 密集型操作或使用释放 GIL 的库（如 YOLO 的底层 C 库）的进程，您仍然可以实现并发。为了提高性能，请考虑使用 Python 的 multiprocessing 模块。

有关 python 中线程的更多信息，请参阅理解 Python 线程部分。

是的，使用 python 的 multiprocessing 模块对于并行运行 YOLO 模型推理更安全，通常也更有效。基于进程的并行处理创建单独的内存空间，避免了全局解释器锁 (GIL)，并降低了并发问题的风险。每个进程将使用其自己的 YOLO 模型实例独立运行。

有关使用YOLO模型进行基于进程的并行处理的更多详细信息，请参阅关于线程安全推理的页面。

**Examples:**

Example 1 (python):
```python
# Unsafe: Sharing a single model instance across threads
from threading import Thread

from ultralytics import YOLO

# Instantiate the model outside the thread
shared_model = YOLO("yolo11n.pt")


def predict(image_path):
    """Predicts objects in an image using a preloaded YOLO model, take path string to image as argument."""
    results = shared_model.predict(image_path)
    # Process results


# Starting threads that share the same model instance
Thread(target=predict, args=("image1.jpg",)).start()
Thread(target=predict, args=("image2.jpg",)).start()
```

Example 2 (python):
```python
# Unsafe: Sharing multiple model instances across threads can still lead to issues
from threading import Thread

from ultralytics import YOLO

# Instantiate multiple models outside the thread
shared_model_1 = YOLO("yolo11n_1.pt")
shared_model_2 = YOLO("yolo11n_2.pt")


def predict(model, image_path):
    """Runs prediction on an image using a specified YOLO model, returning the results."""
    results = model.predict(image_path)
    # Process results


# Starting threads with individual model instances
Thread(target=predict, args=(shared_model_1, "image1.jpg")).start()
Thread(target=predict, args=(shared_model_2, "image2.jpg")).start()
```

Example 3 (python):
```python
# Safe: Instantiating a single model inside each thread
from threading import Thread

from ultralytics import YOLO


def thread_safe_predict(image_path):
    """Predict on an image using a new YOLO model instance in a thread-safe manner; takes image path as input."""
    local_model = YOLO("yolo11n.pt")
    results = local_model.predict(image_path)
    # Process results


# Starting threads that each have their own model instance
Thread(target=thread_safe_predict, args=("image1.jpg",)).start()
Thread(target=thread_safe_predict, args=("image2.jpg",)).start()
```

Example 4 (python):
```python
from ultralytics import YOLO
from ultralytics.utils import ThreadingLocked

# Create a model instance
model = YOLO("yolo11n.pt")


# Decorate the predict method to make it thread-safe
@ThreadingLocked()
def thread_safe_predict(image_path):
    """Thread-safe prediction using a shared model instance."""
    results = model.predict(image_path)
    return results


# Now you can safely call this function from multiple threads
```

---

## Reference for ultralytics/data/loaders.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/data/loaders/

**Contents:**
- Reference for ultralytics/data/loaders.py
- class ultralytics.data.loaders.SourceTypes
- class ultralytics.data.loaders.LoadStreams
  - method ultralytics.data.loaders.LoadStreams.__iter__
  - method ultralytics.data.loaders.LoadStreams.__len__
  - method ultralytics.data.loaders.LoadStreams.__next__
  - method ultralytics.data.loaders.LoadStreams.close
  - method ultralytics.data.loaders.LoadStreams.update
- class ultralytics.data.loaders.LoadScreenshots
  - method ultralytics.data.loaders.LoadScreenshots.__iter__

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/data/loaders.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Class to represent various types of input sources for predictions.

This class uses dataclass to define boolean flags for different types of input sources that can be used for making predictions with YOLO models.

Stream Loader for various types of video streams.

Supports RTSP, RTMP, HTTP, and TCP streams. This class handles the loading and processing of multiple video streams simultaneously, making it suitable for real-time video analysis tasks.

Iterate through YOLO image feed and re-open unresponsive streams.

Return the number of video streams in the LoadStreams object.

Return the next batch of frames from multiple video streams for processing.

Terminate stream loader, stop threads, and release video capture resources.

Read stream frames in daemon thread and update image buffer.

Ultralytics screenshot dataloader for capturing and processing screen images.

This class manages the loading of screenshot images for processing with YOLO. It is suitable for use with yolo predict source=screen.

Yield the next screenshot image from the specified screen or region for processing.

Capture and return the next screenshot as a numpy array using the mss library.

A class for loading and processing images and videos for YOLO object detection.

This class manages the loading and pre-processing of image and video data from various sources, including single image files, video files, and lists of image and video paths.

Iterate through image/video files, yielding source paths, images, and metadata.

Return the number of files (images and videos) in the dataset.

Return the next batch of images or video frames with their paths and metadata.

Create a new video capture object for the given path and initialize video-related attributes.

Load images from PIL and Numpy arrays for batch processing.

This class manages loading and pre-processing of image data from both PIL and Numpy formats. It performs basic validation and format conversion to ensure that the images are in the required format for downstream processing.

Iterate through PIL/numpy images, yielding paths, raw images, and metadata for processing.

Return the length of the 'im0' attribute, representing the number of loaded images.

Return the next batch of images, paths, and metadata for processing.

Validate and format an image to a NumPy array.

A class for loading and processing tensor data for object detection tasks.

This class handles the loading and pre-processing of image data from PyTorch tensors, preparing them for further processing in object detection pipelines.

Yield an iterator object for iterating through tensor image data.

Return the batch size of the tensor input.

Yield the next batch of tensor images and metadata for processing.

Validate and format a single image tensor, ensuring correct shape and normalization.

Merge a list of sources into a list of numpy arrays or PIL images for Ultralytics prediction.

Retrieve the URL of the best quality MP4 video stream from a given YouTube video.

**Examples:**

Example 1 (unknown):
```unknown
SourceTypes()
```

Example 2 (unknown):
```unknown
>>> source_types = SourceTypes(stream=True, screenshot=False, from_img=False)
>>> print(source_types.stream)
True
>>> print(source_types.from_img)
False
```

Example 3 (python):
```python
@dataclass
class SourceTypes:
```

Example 4 (typescript):
```typescript
LoadStreams(self, sources: str = "file.streams", vid_stride: int = 1, buffer: bool = False, channels: int = 3)
```

---

## Ultralytics 文档：将 YOLO11 与 SAHI 结合使用于切片推理

**URL:** https://docs.ultralytics.com/zh/guides/sahi-tiled-inference/

**Contents:**
- Ultralytics 文档：将 YOLO11 与 SAHI 结合使用于切片推理
- SAHI 简介
  - SAHI 的主要特性
- 什么是分片推理 (Sliced Inference)？
  - 分片推理的优势
- 安装与准备
  - 安装
  - 导入模块和下载资源
- 使用 YOLO11 进行标准推理
  - 实例化模型

欢迎阅读 Ultralytics 关于如何将 YOLO11 与 SAHI (Slicing Aided Hyper Inference) 结合使用的文档。本综合指南旨在为您提供将 SAHI 与 YOLO11 一起实施所需的所有基本知识。我们将深入探讨 SAHI 是什么，为什么切片推理对于大规模应用至关重要，以及如何将这些功能与 YOLO11 集成以增强 目标检测 性能。

SAHI (Slicing Aided Hyper Inference) 是一个创新的库，旨在优化用于大规模和高分辨率图像的对象检测算法。它的核心功能在于将图像划分为可管理的切片，在每个切片上运行对象检测，然后将结果缝合在一起。SAHI 与包括 YOLO 系列在内的一系列对象检测模型兼容，从而在确保优化利用计算资源的同时提供灵活性。

观看： 使用 Ultralytics YOLO11 的 SAHI（Slicing Aided Hyper Inference，切片辅助超推理）进行推理

分片推理是指将大型或高分辨率图像细分为更小的片段（切片），在这些切片上进行对象检测，然后重新编译这些切片以重建原始图像上的对象位置。当计算资源有限或处理可能导致内存问题的极高分辨率图像时，此技术非常宝贵。

降低计算负担：较小的图像切片处理速度更快，并且消耗更少的内存，从而可以在低端硬件上更流畅地运行。

保持检测质量: 由于每个切片都是独立处理的，因此不会降低对象检测的质量，前提是切片足够大，可以捕获感兴趣的对象。

增强的可扩展性: 该技术允许更容易地跨不同大小和分辨率的图像缩放目标检测，使其成为从卫星图像到医疗诊断等各种应用的理想选择。

要开始使用，请安装最新版本的 SAHI 和 Ultralytics：

以下是如何导入必要的模块并下载 YOLO11 模型和一些测试图像：

您可以像这样实例化一个用于对象检测的 YOLO11 模型：

使用图像路径或 numpy 图像执行标准推理。

通过指定切片尺寸和重叠比率来执行切片推理：

SAHI 提供了一个 PredictionResult 目标，可以转换为各种标注格式：

您现在已准备好将YOLO11与SAHI结合使用，进行标准推理和切片推理。

如果您在研究或开发工作中使用 SAHI，请引用 SAHI 原始论文并感谢作者：

我们感谢 SAHI 研究小组创建和维护这一宝贵资源，该资源面向 计算机视觉 社区。 有关 SAHI 及其创建者的更多信息，请访问 SAHI GitHub 存储库。

将 Ultralytics YOLO11 与 SAHI（Slicing Aided Hyper Inference，切片辅助超推理）集成以进行切片推理，通过将高分辨率图像分割成可管理的切片来优化您的对象检测任务。这种方法提高了内存使用率并确保了高检测精度。首先，您需要安装 ultralytics 和 sahi 库：

然后，下载 YOLO11 模型和测试图像：

有关更详细的说明，请参阅我们的分片推理指南。

将 SAHI 与 Ultralytics YOLO11 结合使用，以便在大图像上进行目标检测，具有以下几个优点：

在我们的文档中了解更多关于分片推理的优势。

是的，将 YOLO11 与 SAHI 结合使用时，您可以可视化预测结果。以下是如何导出和可视化结果的方法：

此命令会将可视化的预测保存到指定的目录，然后您可以加载图像以在笔记本或应用程序中查看它。 有关详细指南，请查看标准推理部分。

SAHI（Slicing Aided Hyper Inference）提供了一些补充 Ultralytics YOLO11 进行目标检测的功能：

要深入了解，请阅读 SAHI 的主要特性。

要使用 YOLO11 和 SAHI 处理大规模推理项目，请遵循以下最佳实践：

有关更详细的步骤，请访问我们的批量预测部分。

**Examples:**

Example 1 (unknown):
```unknown
pip install -U ultralytics sahi
```

Example 2 (sql):
```sql
from sahi.utils.file import download_from_url
from sahi.utils.ultralytics import download_yolo11n_model

# Download YOLO11 model
model_path = "models/yolo11n.pt"
download_yolo11n_model(model_path)

# Download test images
download_from_url(
    "https://raw.githubusercontent.com/obss/sahi/main/demo/demo_data/small-vehicles1.jpeg",
    "demo_data/small-vehicles1.jpeg",
)
download_from_url(
    "https://raw.githubusercontent.com/obss/sahi/main/demo/demo_data/terrain2.png",
    "demo_data/terrain2.png",
)
```

Example 3 (python):
```python
from sahi import AutoDetectionModel

detection_model = AutoDetectionModel.from_pretrained(
    model_type="ultralytics",
    model_path=model_path,
    confidence_threshold=0.3,
    device="cpu",  # or 'cuda:0'
)
```

Example 4 (sql):
```sql
from sahi.predict import get_prediction
from sahi.utils.cv import read_image

# With an image path
result = get_prediction("demo_data/small-vehicles1.jpeg", detection_model)

# With a numpy image
result_with_np_image = get_prediction(read_image("demo_data/small-vehicles1.jpeg"), detection_model)
```

---

## Reference for ultralytics/models/yolo/yoloe/predict.py

**URL:** https://docs.ultralytics.com/zh/reference/models/yolo/yoloe/predict/

**Contents:**
- Reference for ultralytics/models/yolo/yoloe/predict.py
- class ultralytics.models.yolo.yoloe.predict.YOLOEVPDetectPredictor
  - method ultralytics.models.yolo.yoloe.predict.YOLOEVPDetectPredictor._process_single_image
  - method ultralytics.models.yolo.yoloe.predict.YOLOEVPDetectPredictor.get_vpe
  - method ultralytics.models.yolo.yoloe.predict.YOLOEVPDetectPredictor.inference
  - method ultralytics.models.yolo.yoloe.predict.YOLOEVPDetectPredictor.pre_transform
  - method ultralytics.models.yolo.yoloe.predict.YOLOEVPDetectPredictor.set_prompts
  - method ultralytics.models.yolo.yoloe.predict.YOLOEVPDetectPredictor.setup_model
- class ultralytics.models.yolo.yoloe.predict.YOLOEVPSegPredictor

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/models/yolo/yoloe/predict.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Bases: DetectionPredictor

A mixin class for YOLO-EVP (Enhanced Visual Prompting) predictors.

This mixin provides common functionality for YOLO models that use visual prompting, including model setup, prompt handling, and preprocessing transformations.

Process a single image by resizing bounding boxes or masks and generating visuals.

Process the source to get the visual prompt embeddings (VPE).

Run inference with visual prompts.

Preprocess images and prompts before inference.

This method applies letterboxing to the input image and transforms the visual prompts (bounding boxes or masks) accordingly.

Set the visual prompts for the model.

Set up the model for prediction.

Bases: YOLOEVPDetectPredictor, SegmentationPredictor

Predictor for YOLO-EVP segmentation tasks combining detection and segmentation capabilities.

**Examples:**

Example 1 (unknown):
```unknown
YOLOEVPDetectPredictor()
```

Example 2 (php):
```php
class YOLOEVPDetectPredictor(DetectionPredictor):
```

Example 3 (python):
```python
def _process_single_image(self, dst_shape, src_shape, category, bboxes = None, masks = None)
```

Example 4 (julia):
```julia
def _process_single_image(self, dst_shape, src_shape, category, bboxes=None, masks=None):
    """Process a single image by resizing bounding boxes or masks and generating visuals.

    Args:
        dst_shape (tuple): The target shape (height, width) of the image.
        src_shape (tuple): The original shape (height, width) of the image.
        category (str): The category of the image for visual prompts.
        bboxes (list | np.ndarray, optional): A list of bounding boxes in the format [x1, y1, x2, y2].
        masks (np.ndarray, optional): A list of masks corresponding to the image.

    Returns:
        (torch.Tensor): The processed visuals for the image.

    Raises:
        ValueError: If neither `bboxes` nor `masks` are provided.
    """
    if bboxes is not None and len(bboxes):
        bboxes = np.array(bboxes, dtype=np.float32)
        if bboxes.ndim == 1:
            bboxes = bboxes[None, :]
        # Calculate scaling factor and adjust bounding boxes
        gain = min(dst_shape[0] / src_shape[0], dst_shape[1] / src_shape[1])  # gain = old / new
        bboxes *= gain
        bboxes[..., 0::2] += round((dst_shape[1] - src_shape[1] * gain) / 2 - 0.1)
        bboxes[..., 1::2] += round((dst_shape[0] - src_shape[0] * gain) / 2 - 0.1)
    elif masks is not None:
        # Resize and process masks
        resized_masks = super().pre_transform(masks)
        masks = np.stack(resized_masks)  # (N, H, W)
        masks[masks == 114] = 0  # Reset padding values to 0
    else:
        raise ValueError("Please provide valid bboxes or masks")

    # Generate visuals using the visual prompt loader
    return LoadVisualPrompt().get_visuals(category, dst_shape, bboxes, masks)
```

---

## Reference for ultralytics/trackers/byte_tracker.py

**URL:** https://docs.ultralytics.com/zh/reference/trackers/byte_tracker/

**Contents:**
- Reference for ultralytics/trackers/byte_tracker.py
- class ultralytics.trackers.byte_tracker.STrack
  - property ultralytics.trackers.byte_tracker.STrack.tlwh
  - property ultralytics.trackers.byte_tracker.STrack.xyxy
  - property ultralytics.trackers.byte_tracker.STrack.xywh
  - property ultralytics.trackers.byte_tracker.STrack.xywha
  - property ultralytics.trackers.byte_tracker.STrack.result
  - method ultralytics.trackers.byte_tracker.STrack.__repr__
  - method ultralytics.trackers.byte_tracker.STrack.activate
  - method ultralytics.trackers.byte_tracker.STrack.convert_coords

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/trackers/byte_tracker.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Single object tracking representation that uses Kalman filtering for state estimation.

This class is responsible for storing all the information regarding individual tracklets and performs state updates and predictions based on Kalman filter.

Get the bounding box in top-left-width-height format from the current state estimate.

Convert bounding box from (top left x, top left y, width, height) to (min x, min y, max x, max y) format.

Get the current position of the bounding box in (center x, center y, width, height) format.

Get position in (center x, center y, width, height, angle) format, warning if angle is missing.

Get the current tracking results in the appropriate bounding box format.

Return a string representation of the STrack object including start frame, end frame, and track ID.

Activate a new tracklet using the provided Kalman filter and initialize its state and covariance.

Convert a bounding box's top-left-width-height format to its x-y-aspect-height equivalent.

Update state tracks positions and covariances using a homography matrix for multiple tracks.

Perform multi-object predictive tracking using Kalman filter for the provided list of STrack instances.

Predict the next state (mean and covariance) of the object using the Kalman filter.

Reactivate a previously lost track using new detection data and update its state and attributes.

Convert bounding box from tlwh format to center-x-center-y-aspect-height (xyah) format.

Update the state of a matched track.

BYTETracker: A tracking algorithm built on top of YOLOv8 for object detection and tracking.

This class encapsulates the functionality for initializing, updating, and managing the tracks for detected objects in a video sequence. It maintains the state of tracked, lost, and removed tracks over frames, utilizes Kalman filtering for predicting the new object locations, and performs data association.

Calculate the distance between tracks and detections using IoU and optionally fuse scores.

Return a Kalman filter object for tracking bounding boxes using KalmanFilterXYAH.

Initialize object tracking with given detections, scores, and class labels using the STrack algorithm.

Combine two lists of STrack objects into a single list, ensuring no duplicates based on track IDs.

Predict the next states for multiple tracks using Kalman filter.

Remove duplicate stracks from two lists based on Intersection over Union (IoU) distance.

Reset the tracker by clearing all tracked, lost, and removed tracks and reinitializing the Kalman filter.

Reset the ID counter for STrack instances to ensure unique track IDs across tracking sessions.

Filter out the stracks present in the second list from the first list.

Update the tracker with new detections and return the current list of tracked objects.

**Examples:**

Example 1 (unknown):
```unknown
STrack(self, xywh: list[float], score: float, cls: Any)
```

Example 2 (unknown):
```unknown
Initialize and activate a new track
>>> track = STrack(xywh=[100, 200, 50, 80, 0], score=0.9, cls="person")
>>> track.activate(kalman_filter=KalmanFilterXYAH(), frame_id=1)
```

Example 3 (python):
```python
class STrack(BaseTrack):
    """Single object tracking representation that uses Kalman filtering for state estimation.

    This class is responsible for storing all the information regarding individual tracklets and performs state updates
    and predictions based on Kalman filter.

    Attributes:
        shared_kalman (KalmanFilterXYAH): Shared Kalman filter used across all STrack instances for prediction.
        _tlwh (np.ndarray): Private attribute to store top-left corner coordinates and width and height of bounding box.
        kalman_filter (KalmanFilterXYAH): Instance of Kalman filter used for this particular object track.
        mean (np.ndarray): Mean state estimate vector.
        covariance (np.ndarray): Covariance of state estimate.
        is_activated (bool): Boolean flag indicating if the track has been activated.
        score (float): Confidence score of the track.
        tracklet_len (int): Length of the tracklet.
        cls (Any): Class label for the object.
        idx (int): Index or identifier for the object.
        frame_id (int): Current frame ID.
        start_frame (int): Frame where the object was first detected.
        angle (float | None): Optional angle information for oriented bounding boxes.

    Methods:
        predict: Predict the next state of the object using Kalman filter.
        multi_predict: Predict the next states for multiple tracks.
        multi_gmc: Update multiple track states using a homography matrix.
        activate: Activate a new tracklet.
        re_activate: Reactivate a previously lost tracklet.
        update: Update the state of a matched track.
        convert_coords: Convert bounding box to x-y-aspect-height format.
        tlwh_to_xyah: Convert tlwh bounding box to xyah format.

    Examples:
        Initialize and activate a new track
        >>> track = STrack(xywh=[100, 200, 50, 80, 0], score=0.9, cls="person")
        >>> track.activate(kalman_filter=KalmanFilterXYAH(), frame_id=1)
    """

    shared_kalman = KalmanFilterXYAH()

    def __init__(self, xywh: list[float], score: float, cls: Any):
        """Initialize a new STrack instance.

        Args:
            xywh (list[float]): Bounding box in `(x, y, w, h, idx)` or `(x, y, w, h, angle, idx)` format, where (x, y)
                is the center, (w, h) are width and height, and `idx` is the detection index.
            score (float): Confidence score of the detection.
            cls (Any): Class label for the detected object.
        """
        super().__init__()
        # xywh+idx or xywha+idx
        assert len(xywh) in {5, 6}, f"expected 5 or 6 values but got {len(xywh)}"
        self._tlwh = np.asarray(xywh2ltwh(xywh[:4]), dtype=np.float32)
        self.kalman_filter = None
        self.mean, self.covariance = None, None
        self.is_activated = False

        self.score = score
        self.tracklet_len = 0
        self.cls = cls
        self.idx = xywh[-1]
        self.angle = xywh[4] if len(xywh) == 6 else None
```

Example 4 (python):
```python
def tlwh(self) -> np.ndarray
```

---

## Python 用法 - Ultralytics YOLO 文档

**URL:** https://docs.ultralytics.com/zh/usage/python/

**Contents:**
- Python 用法
- 训练
- 验证
- 预测
- 导出
- 追踪
- 基准测试
- 使用训练器
- 常见问题
  - 如何将 YOLO 集成到我的 python 项目中以进行目标检测？

欢迎使用 Ultralytics YOLO Python 用法文档！本指南旨在帮助您将 Ultralytics YOLO 无缝集成到您的 Python 项目中，以实现对象检测、分割和分类。在这里，您将学习如何加载和使用预训练模型、训练新模型以及对图像执行预测。易于使用的 Python 界面对于任何希望将 YOLO 纳入其 Python 项目的人来说都是宝贵的资源，使您能够快速实现高级对象检测功能。让我们开始吧！

观看： 掌握 Ultralytics YOLO: python

例如，用户只需几行代码即可加载模型、训练模型、评估其在验证集上的性能，甚至可以将其导出为 ONNX 格式。

训练模式用于在自定义数据集上训练 YOLO 模型。在此模式下，模型使用指定的数据集和超参数进行训练。训练过程包括优化模型的参数，使其能够准确预测图像中物体的类别和位置。

Val 模式用于在 YOLO 模型训练完成后对其进行验证。在此模式下，模型会在验证集上进行评估，以衡量其准确性和泛化性能。此模式可用于调整模型的超参数，以提高其性能。

Predict mode 用于使用经过训练的 YOLO 模型对新图像或视频进行预测。在此模式下，模型从检查点文件加载，用户可以提供图像或视频来执行推理。该模型预测输入图像或视频中对象的类别和位置。

导出模式用于将 YOLO 模型导出为可用于部署的格式。在此模式下，模型将转换为可供其他软件应用程序或硬件设备使用的格式。在将模型部署到生产环境时，此模式非常有用。

将官方 YOLO 模型导出为具有动态批大小和图像大小的 ONNX。

将官方 YOLO 模型导出到 TensorRT 在 device=0 以在 CUDA 设备上加速。

追踪模式用于使用 YOLO 模型实时追踪物体。在此模式下，模型从检查点文件加载，用户可以提供实时视频流以执行实时物体追踪。此模式适用于监控系统或自动驾驶汽车等应用。

基准测试模式 用于分析 YOLO 各种导出格式的速度和准确性。这些基准测试提供了有关导出格式大小的信息，其 mAP50-95 指标（用于目标检测和分割）或 accuracy_top5 指标（用于分类）以及在各种导出格式（如 ONNX、TF）中每张图像的推理时间（以毫秒为单位）。 OpenVINO， TensorRT 等。这些信息可以帮助用户根据他们对速度和准确性的要求，为其特定用例选择最佳导出格式。

对官方 YOLO 模型在所有导出格式上进行基准测试。

字段 YOLO model 类是 Trainer 类的高级封装器。每个 YOLO 任务都有自己的训练器，它继承自 BaseTrainer。这种架构允许在您的机器学习工作流程中实现更大的灵活性和定制化。 机器学习工作流程.

您可以轻松地自定义 Trainer 以支持自定义任务或探索研发想法。Ultralytics YOLO 的模块化设计使您可以根据自己的特定需求调整框架，无论您是从事新的计算机视觉任务还是微调现有模型以获得更好的性能。

将 Ultralytics YOLO 集成到您的 python 项目中非常简单。您可以加载预训练模型或从头开始训练新模型。以下是如何开始：

请参阅我们的预测模式部分，获取更详细的示例。

Ultralytics YOLO 提供了各种模式来满足不同的机器学习工作流程。这些包括：

每种模式都旨在为模型开发和部署的不同阶段提供全面的功能。

要训练自定义 YOLO 模型，您需要指定数据集和其他超参数。这是一个快速示例：

有关训练的更多详细信息以及指向示例用法的超链接，请访问我们的Train 模式页面。

使用以下命令以适合部署的格式导出 YOLO 模型非常简单 export 函数。例如，您可以将模型导出为 ONNX 格式：

是的，可以在不同的数据集上验证 YOLO 模型。训练后，您可以使用验证模式来评估性能：

查看Val 模式页面，获取详细示例和用法。

**Examples:**

Example 1 (python):
```python
from ultralytics import YOLO

# Create a new YOLO model from scratch
model = YOLO("yolo11n.yaml")

# Load a pretrained YOLO model (recommended for training)
model = YOLO("yolo11n.pt")

# Train the model using the 'coco8.yaml' dataset for 3 epochs
results = model.train(data="coco8.yaml", epochs=3)

# Evaluate the model's performance on the validation set
results = model.val()

# Perform object detection on an image using the model
results = model("https://ultralytics.com/images/bus.jpg")

# Export the model to ONNX format
success = model.export(format="onnx")
```

Example 2 (typescript):
```typescript
from ultralytics import YOLO

model = YOLO("yolo11n.pt")  # pass any model type
results = model.train(epochs=5)
```

Example 3 (python):
```python
from ultralytics import YOLO

model = YOLO("yolo11n.yaml")
results = model.train(data="coco8.yaml", epochs=5)
```

Example 4 (unknown):
```unknown
model = YOLO("last.pt")
results = model.train(resume=True)
```

---

## Reference for ultralytics/models/yolo/obb/predict.py

**URL:** https://docs.ultralytics.com/zh/reference/models/yolo/obb/predict/

**Contents:**
- Reference for ultralytics/models/yolo/obb/predict.py
- class ultralytics.models.yolo.obb.predict.OBBPredictor
  - method ultralytics.models.yolo.obb.predict.OBBPredictor.construct_result

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/models/yolo/obb/predict.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Bases: DetectionPredictor

A class extending the DetectionPredictor class for prediction based on an Oriented Bounding Box (OBB) model.

This predictor handles oriented bounding box detection tasks, processing images and returning results with rotated bounding boxes.

Construct the result object from the prediction.

**Examples:**

Example 1 (rust):
```rust
OBBPredictor(self, cfg = DEFAULT_CFG, overrides = None, _callbacks = None)
```

Example 2 (sql):
```sql
>>> from ultralytics.utils import ASSETS
>>> from ultralytics.models.yolo.obb import OBBPredictor
>>> args = dict(model="yolo11n-obb.pt", source=ASSETS)
>>> predictor = OBBPredictor(overrides=args)
>>> predictor.predict_cli()
```

Example 3 (python):
```python
class OBBPredictor(DetectionPredictor):
    """A class extending the DetectionPredictor class for prediction based on an Oriented Bounding Box (OBB) model.

    This predictor handles oriented bounding box detection tasks, processing images and returning results with rotated
    bounding boxes.

    Attributes:
        args (namespace): Configuration arguments for the predictor.
        model (torch.nn.Module): The loaded YOLO OBB model.

    Examples:
        >>> from ultralytics.utils import ASSETS
        >>> from ultralytics.models.yolo.obb import OBBPredictor
        >>> args = dict(model="yolo11n-obb.pt", source=ASSETS)
        >>> predictor = OBBPredictor(overrides=args)
        >>> predictor.predict_cli()
    """

    def __init__(self, cfg=DEFAULT_CFG, overrides=None, _callbacks=None):
        """Initialize OBBPredictor with optional model and data configuration overrides.

        Args:
            cfg (dict, optional): Default configuration for the predictor.
            overrides (dict, optional): Configuration overrides that take precedence over the default config.
            _callbacks (list, optional): List of callback functions to be invoked during prediction.
        """
        super().__init__(cfg, overrides, _callbacks)
        self.args.task = "obb"
```

Example 4 (python):
```python
def construct_result(self, pred, img, orig_img, img_path)
```

---

## Reference for ultralytics/solutions/streamlit_inference.py

**URL:** https://docs.ultralytics.com/zh/reference/solutions/streamlit_inference/

**Contents:**
- Reference for ultralytics/solutions/streamlit_inference.py
- class ultralytics.solutions.streamlit_inference.Inference
  - method ultralytics.solutions.streamlit_inference.Inference.configure
  - method ultralytics.solutions.streamlit_inference.Inference.image_inference
  - method ultralytics.solutions.streamlit_inference.Inference.inference
  - method ultralytics.solutions.streamlit_inference.Inference.sidebar
  - method ultralytics.solutions.streamlit_inference.Inference.source_upload
  - method ultralytics.solutions.streamlit_inference.Inference.web_ui

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/solutions/streamlit_inference.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class to perform object detection, image classification, image segmentation and pose estimation inference.

This class provides functionalities for loading models, configuring settings, uploading video files, and performing real-time inference using Streamlit and Ultralytics YOLO models.

Configure the model and load selected classes for inference.

Perform inference on uploaded images.

Perform real-time object detection inference on video or webcam feed.

Configure the Streamlit sidebar for model and inference settings.

Handle video file uploads through the Streamlit interface.

Set up the Streamlit web interface with custom HTML elements.

**Examples:**

Example 1 (rust):
```rust
Inference(self, **kwargs: Any) -> None
```

Example 2 (unknown):
```unknown
Create an Inference instance with a custom model
>>> inf = Inference(model="path/to/model.pt")
>>> inf.inference()

Create an Inference instance with default settings
>>> inf = Inference()
>>> inf.inference()
```

Example 3 (python):
```python
class Inference:
    """A class to perform object detection, image classification, image segmentation and pose estimation inference.

    This class provides functionalities for loading models, configuring settings, uploading video files, and performing
    real-time inference using Streamlit and Ultralytics YOLO models.

    Attributes:
        st (module): Streamlit module for UI creation.
        temp_dict (dict): Temporary dictionary to store the model path and other configuration.
        model_path (str): Path to the loaded model.
        model (YOLO): The YOLO model instance.
        source (str): Selected video source (webcam or video file).
        enable_trk (bool): Enable tracking option.
        conf (float): Confidence threshold for detection.
        iou (float): IoU threshold for non-maximum suppression.
        org_frame (Any): Container for the original frame to be displayed.
        ann_frame (Any): Container for the annotated frame to be displayed.
        vid_file_name (str | int): Name of the uploaded video file or webcam index.
        selected_ind (list[int]): List of selected class indices for detection.

    Methods:
        web_ui: Set up the Streamlit web interface with custom HTML elements.
        sidebar: Configure the Streamlit sidebar for model and inference settings.
        source_upload: Handle video file uploads through the Streamlit interface.
        configure: Configure the model and load selected classes for inference.
        inference: Perform real-time object detection inference.

    Examples:
        Create an Inference instance with a custom model
        >>> inf = Inference(model="path/to/model.pt")
        >>> inf.inference()

        Create an Inference instance with default settings
        >>> inf = Inference()
        >>> inf.inference()
    """

    def __init__(self, **kwargs: Any) -> None:
        """Initialize the Inference class, checking Streamlit requirements and setting up the model path.

        Args:
            **kwargs (Any): Additional keyword arguments for model configuration.
        """
        check_requirements("streamlit>=1.29.0")  # scope imports for faster ultralytics package load speeds
        import streamlit as st

        self.st = st  # Reference to the Streamlit module
        self.source = None  # Video source selection (webcam or video file)
        self.img_file_names = []  # List of image file names
        self.enable_trk = False  # Flag to toggle object tracking
        self.conf = 0.25  # Confidence threshold for detection
        self.iou = 0.45  # Intersection-over-Union (IoU) threshold for non-maximum suppression
        self.org_frame = None  # Container for the original frame display
        self.ann_frame = None  # Container for the annotated frame display
        self.vid_file_name = None  # Video file name or webcam index
        self.selected_ind: list[int] = []  # List of selected class indices for detection
        self.model = None  # YOLO model instance

        self.temp_dict = {"model": None, **kwargs}
        self.model_path = None  # Model file path
        if self.temp_dict["model"] is not None:
            self.model_path = self.temp_dict["model"]

        LOGGER.info(f"Ultralytics Solutions: ✅ {self.temp_dict}")
```

Example 4 (python):
```python
def configure(self) -> None
```

---
