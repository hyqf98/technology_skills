# Yolo - Deployment

**Pages:** 3

---

## Reference for ultralytics/engine/exporter.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/engine/exporter/

**Contents:**
- Reference for ultralytics/engine/exporter.py
- class ultralytics.engine.exporter.Exporter
  - method ultralytics.engine.exporter.Exporter.__call__
  - method ultralytics.engine.exporter.Exporter._add_tflite_metadata
  - method ultralytics.engine.exporter.Exporter._pipeline_coreml
  - method ultralytics.engine.exporter.Exporter._transform_fn
  - method ultralytics.engine.exporter.Exporter.add_callback
  - method ultralytics.engine.exporter.Exporter.export_axelera
  - method ultralytics.engine.exporter.Exporter.export_coreml
  - method ultralytics.engine.exporter.Exporter.export_edgetpu

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/engine/exporter.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class for exporting YOLO models to various formats.

This class provides functionality to export YOLO models to different formats including ONNX, TensorRT, CoreML, TensorFlow, and others. It handles format validation, device selection, model preparation, and the actual export process for each supported format.

Export a model and return the final exported path as a string.

Add metadata to *.tflite models per https://ai.google.dev/edge/litert/models/metadata.

Create CoreML pipeline with NMS for YOLO detection models.

The transformation function for Axelera/OpenVINO quantization preprocessing.

Append the given callback to the specified event.

Export YOLO model to CoreML format.

Export YOLO model to Edge TPU format https://coral.ai/docs/edgetpu/models-intro/.

Export YOLO model to TensorRT format https://developer.nvidia.com/tensorrt.

Exports a model to ExecuTorch (.pte) format into a dedicated directory and saves the required metadata,

following Ultralytics conventions.

Export YOLO model to IMX format.

Export YOLO model to MNN format using MNN https://github.com/alibaba/MNN.

Export YOLO model to NCNN format using PNNX https://github.com/pnnx/pnnx.

Export YOLO model to ONNX format.

Export YOLO model to OpenVINO format.

Export YOLO model to PaddlePaddle format.

Export YOLO model to TensorFlow GraphDef *.pb format https://github.com/leimao/Frozen-Graph-TensorFlow.

Export YOLO model to RKNN format.

Export YOLO model to TensorFlow SavedModel format.

Export YOLO model to TensorFlow.js format.

Export YOLO model to TensorFlow Lite format.

Export YOLO model to TorchScript format.

Build and return a dataloader for calibration of INT8 models.

Execute all callbacks for a given event.

Bases: torch.nn.Module

Wrap an Ultralytics YOLO model for Apple iOS CoreML export.

Normalize predictions of object detection model with input size-dependent factors.

Bases: torch.nn.Module

Model wrapper with embedded NMS for Detect, Segment, Pose and OBB.

Perform inference with NMS post-processing. Supports Detect, Segment, OBB and Pose.

Return a dictionary of Ultralytics YOLO export formats.

Return max ONNX opset for this torch version with ONNX fallback.

Validate arguments based on the export format.

YOLO export decorator, i.e. @try_export.

**Examples:**

Example 1 (rust):
```rust
Exporter(self, cfg = DEFAULT_CFG, overrides = None, _callbacks = None)
```

Example 2 (json):
```json
Export a YOLOv8 model to ONNX format
>>> from ultralytics.engine.exporter import Exporter
>>> exporter = Exporter()
>>> exporter(model="yolov8n.pt")  # exports to yolov8n.onnx

Export with specific arguments
>>> args = {"format": "onnx", "dynamic": True, "half": True}
>>> exporter = Exporter(overrides=args)
>>> exporter(model="yolov8n.pt")
```

Example 3 (python):
```python
class Exporter:
    """A class for exporting YOLO models to various formats.

    This class provides functionality to export YOLO models to different formats including ONNX, TensorRT, CoreML,
    TensorFlow, and others. It handles format validation, device selection, model preparation, and the actual export
    process for each supported format.

    Attributes:
        args (SimpleNamespace): Configuration arguments for the exporter.
        callbacks (dict): Dictionary of callback functions for different export events.
        im (torch.Tensor): Input tensor for model inference during export.
        model (torch.nn.Module): The YOLO model to be exported.
        file (Path): Path to the model file being exported.
        output_shape (tuple): Shape of the model output tensor(s).
        pretty_name (str): Formatted model name for display purposes.
        metadata (dict): Model metadata including description, author, version, etc.
        device (torch.device): Device on which the model is loaded.
        imgsz (tuple): Input image size for the model.

    Methods:
        __call__: Main export method that handles the export process.
        get_int8_calibration_dataloader: Build dataloader for INT8 calibration.
        export_torchscript: Export model to TorchScript format.
        export_onnx: Export model to ONNX format.
        export_openvino: Export model to OpenVINO format.
        export_paddle: Export model to PaddlePaddle format.
        export_mnn: Export model to MNN format.
        export_ncnn: Export model to NCNN format.
        export_coreml: Export model to CoreML format.
        export_engine: Export model to TensorRT format.
        export_saved_model: Export model to TensorFlow SavedModel format.
        export_pb: Export model to TensorFlow GraphDef format.
        export_tflite: Export model to TensorFlow Lite format.
        export_edgetpu: Export model to Edge TPU format.
        export_tfjs: Export model to TensorFlow.js format.
        export_rknn: Export model to RKNN format.
        export_imx: Export model to IMX format.

    Examples:
        Export a YOLOv8 model to ONNX format
        >>> from ultralytics.engine.exporter import Exporter
        >>> exporter = Exporter()
        >>> exporter(model="yolov8n.pt")  # exports to yolov8n.onnx

        Export with specific arguments
        >>> args = {"format": "onnx", "dynamic": True, "half": True}
        >>> exporter = Exporter(overrides=args)
        >>> exporter(model="yolov8n.pt")
    """

    def __init__(self, cfg=DEFAULT_CFG, overrides=None, _callbacks=None):
        """Initialize the Exporter class.

        Args:
            cfg (str, optional): Path to a configuration file.
            overrides (dict, optional): Configuration overrides.
            _callbacks (dict, optional): Dictionary of callback functions.
        """
        self.args = get_cfg(cfg, overrides)
        self.callbacks = _callbacks or callbacks.get_default_callbacks()
        callbacks.add_integration_callbacks(self)
```

Example 4 (python):
```python
def __call__(self, model = None) -> str
```

---

## Reference for ultralytics/utils/export/imx.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/export/imx/

**Contents:**
- Reference for ultralytics/utils/export/imx.py
- class ultralytics.utils.export.imx.FXModel
  - method ultralytics.utils.export.imx.FXModel.forward
- class ultralytics.utils.export.imx.NMSWrapper
  - method ultralytics.utils.export.imx.NMSWrapper.forward
- function ultralytics.utils.export.imx._inference
- function ultralytics.utils.export.imx.pose_forward
- function ultralytics.utils.export.imx.segment_forward
- function ultralytics.utils.export.imx.torch2imx

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/export/imx.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Bases: torch.nn.Module

A custom model class for torch.fx compatibility.

This class extends torch.nn.Module and is designed to ensure compatibility with torch.fx for tracing and graph manipulation. It copies attributes from an existing model and explicitly sets the model attribute to ensure proper copying.

Forward pass through the model.

This method performs the forward pass through the model, handling the dependencies between layers and saving intermediate outputs.

Bases: torch.nn.Module

Wrap PyTorch Module with multiclass_nms layer from edge-mdt-cl.

Forward pass with model inference and NMS post-processing.

Decode boxes and cls scores for imx object detection.

Forward pass for imx pose estimation, including keypoint decoding.

Forward pass for imx segmentation.

Export YOLO model to IMX format for deployment on Sony IMX500 devices.

This function quantizes a YOLO model using Model Compression Toolkit (MCT) and exports it to IMX format compatible with Sony IMX500 edge devices. It supports both YOLOv8n and YOLO11n models for detection and pose estimation tasks.

**Examples:**

Example 1 (unknown):
```unknown
FXModel(self, model, imgsz = (640, 640))
```

Example 2 (python):
```python
class FXModel(torch.nn.Module):
    """A custom model class for torch.fx compatibility.

    This class extends `torch.nn.Module` and is designed to ensure compatibility with torch.fx for tracing and graph
    manipulation. It copies attributes from an existing model and explicitly sets the model attribute to ensure proper
    copying.

    Attributes:
        model (nn.Module): The original model's layers.
    """

    def __init__(self, model, imgsz=(640, 640)):
        """Initialize the FXModel.

        Args:
            model (nn.Module): The original model to wrap for torch.fx compatibility.
            imgsz (tuple[int, int]): The input image size (height, width). Default is (640, 640).
        """
        super().__init__()
        copy_attr(self, model)
        # Explicitly set `model` since `copy_attr` somehow does not copy it.
        self.model = model.model
        self.imgsz = imgsz
```

Example 3 (python):
```python
def forward(self, x)
```

Example 4 (python):
```python
def forward(self, x):
    """Forward pass through the model.

    This method performs the forward pass through the model, handling the dependencies between layers and saving
    intermediate outputs.

    Args:
        x (torch.Tensor): The input tensor to the model.

    Returns:
        (torch.Tensor): The output tensor from the model.
    """
    y = []  # outputs
    for m in self.model:
        if m.f != -1:  # if not from previous layer
            # from earlier layers
            x = y[m.f] if isinstance(m.f, int) else [x if j == -1 else y[j] for j in m.f]
        if isinstance(m, Detect):
            m._inference = types.MethodType(_inference, m)  # bind method to Detect
            m.anchors, m.strides = (
                x.transpose(0, 1)
                for x in make_anchors(
                    torch.cat([s / m.stride.unsqueeze(-1) for s in self.imgsz], dim=1), m.stride, 0.5
                )
            )
        if type(m) is Pose:
            m.forward = types.MethodType(pose_forward, m)  # bind method to Detect
        if type(m) is Segment:
            m.forward = types.MethodType(segment_forward, m)  # bind method to Detect
        x = m(x)  # run
        y.append(x)  # save output
    return x
```

---

## Reference for ultralytics/utils/export/engine.py

**URL:** https://docs.ultralytics.com/zh/reference/utils/export/engine/

**Contents:**
- Reference for ultralytics/utils/export/engine.py
- function ultralytics.utils.export.engine.torch2onnx
- function ultralytics.utils.export.engine.onnx2engine

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/export/engine.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Export a PyTorch model to ONNX format.

Setting do_constant_folding=True may cause issues with DNN inference for torch>=1.12.

Export a YOLO model to TensorRT engine format.

TensorRT version compatibility is handled for workspace size and engine building. INT8 calibration requires a dataset and generates a calibration cache. Metadata is serialized and written to the engine file if provided.

**Examples:**

Example 1 (typescript):
```typescript
def torch2onnx(
    torch_model: torch.nn.Module,
    im: torch.Tensor,
    onnx_file: str,
    opset: int = 14,
    input_names: list[str] = ["images"],
    output_names: list[str] = ["output0"],
    dynamic: bool | dict = False,
) -> None
```

Example 2 (json):
```json
def torch2onnx(
    torch_model: torch.nn.Module,
    im: torch.Tensor,
    onnx_file: str,
    opset: int = 14,
    input_names: list[str] = ["images"],
    output_names: list[str] = ["output0"],
    dynamic: bool | dict = False,
) -> None:
    """Export a PyTorch model to ONNX format.

    Args:
        torch_model (torch.nn.Module): The PyTorch model to export.
        im (torch.Tensor): Example input tensor for the model.
        onnx_file (str): Path to save the exported ONNX file.
        opset (int): ONNX opset version to use for export.
        input_names (list[str]): List of input tensor names.
        output_names (list[str]): List of output tensor names.
        dynamic (bool | dict, optional): Whether to enable dynamic axes.

    Notes:
        Setting `do_constant_folding=True` may cause issues with DNN inference for torch>=1.12.
    """
    kwargs = {"dynamo": False} if TORCH_2_4 else {}
    torch.onnx.export(
        torch_model,
        im,
        onnx_file,
        verbose=False,
        opset_version=opset,
        do_constant_folding=True,  # WARNING: DNN inference with torch>=1.12 may require do_constant_folding=False
        input_names=input_names,
        output_names=output_names,
        dynamic_axes=dynamic or None,
        **kwargs,
    )
```

Example 3 (typescript):
```typescript
def onnx2engine(
    onnx_file: str,
    engine_file: str | None = None,
    workspace: int | None = None,
    half: bool = False,
    int8: bool = False,
    dynamic: bool = False,
    shape: tuple[int, int, int, int] = (1, 3, 640, 640),
    dla: int | None = None,
    dataset=None,
    metadata: dict | None = None,
    verbose: bool = False,
    prefix: str = "",
) -> None
```

Example 4 (python):
```python
def onnx2engine(
    onnx_file: str,
    engine_file: str | None = None,
    workspace: int | None = None,
    half: bool = False,
    int8: bool = False,
    dynamic: bool = False,
    shape: tuple[int, int, int, int] = (1, 3, 640, 640),
    dla: int | None = None,
    dataset=None,
    metadata: dict | None = None,
    verbose: bool = False,
    prefix: str = "",
) -> None:
    """Export a YOLO model to TensorRT engine format.

    Args:
        onnx_file (str): Path to the ONNX file to be converted.
        engine_file (str, optional): Path to save the generated TensorRT engine file.
        workspace (int, optional): Workspace size in GB for TensorRT.
        half (bool, optional): Enable FP16 precision.
        int8 (bool, optional): Enable INT8 precision.
        dynamic (bool, optional): Enable dynamic input shapes.
        shape (tuple[int, int, int, int], optional): Input shape (batch, channels, height, width).
        dla (int, optional): DLA core to use (Jetson devices only).
        dataset (ultralytics.data.build.InfiniteDataLoader, optional): Dataset for INT8 calibration.
        metadata (dict, optional): Metadata to include in the engine file.
        verbose (bool, optional): Enable verbose logging.
        prefix (str, optional): Prefix for log messages.

    Raises:
        ValueError: If DLA is enabled on non-Jetson devices or required precision is not set.
        RuntimeError: If the ONNX file cannot be parsed.

    Notes:
        TensorRT version compatibility is handled for workspace size and engine building.
        INT8 calibration requires a dataset and generates a calibration cache.
        Metadata is serialized and written to the engine file if provided.
    """
    import tensorrt as trt

    engine_file = engine_file or Path(onnx_file).with_suffix(".engine")

    logger = trt.Logger(trt.Logger.INFO)
    if verbose:
        logger.min_severity = trt.Logger.Severity.VERBOSE

    # Engine builder
    builder = trt.Builder(logger)
    config = builder.create_builder_config()
    workspace_bytes = int((workspace or 0) * (1 << 30))
    is_trt10 = int(trt.__version__.split(".", 1)[0]) >= 10  # is TensorRT >= 10
    if is_trt10 and workspace_bytes > 0:
        config.set_memory_pool_limit(trt.MemoryPoolType.WORKSPACE, workspace_bytes)
    elif workspace_bytes > 0:  # TensorRT versions 7, 8
        config.max_workspace_size = workspace_bytes
    flag = 1 << int(trt.NetworkDefinitionCreationFlag.EXPLICIT_BATCH)
    network = builder.create_network(flag)
    half = builder.platform_has_fast_fp16 and half
    int8 = builder.platform_has_fast_int8 and int8

    # Optionally switch to DLA if enabled
    if dla is not None:
        if not IS_JETSON:
            raise ValueError("DLA is only available on NVIDIA Jetson devices")
        LOGGER.info(f"{prefix} enabling DLA on core {dla}...")
        if not half and not int8:
            raise ValueError(
                "DLA requires either 'half=True' (FP16) or 'int8=True' (INT8) to be enabled. Please enable one of them and try again."
            )
        config.default_device_type = trt.DeviceType.DLA
        config.DLA_core = int(dla)
        config.set_flag(trt.BuilderFlag.GPU_FALLBACK)

    # Read ONNX file
    parser = trt.OnnxParser(network, logger)
    if not parser.parse_from_file(onnx_file):
        raise RuntimeError(f"failed to load ONNX file: {onnx_file}")

    # Network inputs
    inputs = [network.get_input(i) for i in range(network.num_inputs)]
    outputs = [network.get_output(i) for i in range(network.num_outputs)]
    for inp in inputs:
        LOGGER.info(f'{prefix} input "{inp.name}" with shape{inp.shape} {inp.dtype}')
    for out in outputs:
        LOGGER.info(f'{prefix} output "{out.name}" with shape{out.shape} {out.dtype}')

    if dynamic:
        profile = builder.create_optimization_profile()
        min_shape = (1, shape[1], 32, 32)  # minimum input shape
        max_shape = (*shape[:2], *(int(max(2, workspace or 2) * d) for d in shape[2:]))  # max input shape
        for inp in inputs:
            profile.set_shape(inp.name, min=min_shape, opt=shape, max=max_shape)
        config.add_optimization_profile(profile)
        if int8:
            config.set_calibration_profile(profile)

    LOGGER.info(f"{prefix} building {'INT8' if int8 else 'FP' + ('16' if half else '32')} engine as {engine_file}")
    if int8:
        config.set_flag(trt.BuilderFlag.INT8)
        config.profiling_verbosity = trt.ProfilingVerbosity.DETAILED

        class EngineCalibrator(trt.IInt8Calibrator):
            """Custom INT8 calibrator for TensorRT engine optimization.

            This calibrator provides the necessary interface for TensorRT to perform INT8 quantization calibration using
            a dataset. It handles batch generation, caching, and calibration algorithm selection.

            Attributes:
                dataset: Dataset for calibration.
                data_iter: Iterator over the calibration dataset.
                algo (trt.CalibrationAlgoType): Calibration algorithm type.
                batch (int): Batch size for calibration.
                cache (Path): Path to save the calibration cache.

            Methods:
                get_algorithm: Get the calibration algorithm to use.
                get_batch_size: Get the batch size to use for calibration.
                get_batch: Get the next batch to use for calibration.
                read_calibration_cache: Use existing cache instead of calibrating again.
                write_calibration_cache: Write calibration cache to disk.
            """

            def __init__(
                self,
                dataset,  # ultralytics.data.build.InfiniteDataLoader
                cache: str = "",
            ) -> None:
                """Initialize the INT8 calibrator with dataset and cache path."""
                trt.IInt8Calibrator.__init__(self)
                self.dataset = dataset
                self.data_iter = iter(dataset)
                self.algo = (
                    trt.CalibrationAlgoType.ENTROPY_CALIBRATION_2  # DLA quantization needs ENTROPY_CALIBRATION_2
                    if dla is not None
                    else trt.CalibrationAlgoType.MINMAX_CALIBRATION
                )
                self.batch = dataset.batch_size
                self.cache = Path(cache)

            def get_algorithm(self) -> trt.CalibrationAlgoType:
                """Get the calibration algorithm to use."""
                return self.algo

            def get_batch_size(self) -> int:
                """Get the batch size to use for calibration."""
                return self.batch or 1

            def get_batch(self, names) -> list[int] | None:
                """Get the next batch to use for calibration, as a list of device memory pointers."""
                try:
                    im0s = next(self.data_iter)["img"] / 255.0
                    im0s = im0s.to("cuda") if im0s.device.type == "cpu" else im0s
                    return [int(im0s.data_ptr())]
                except StopIteration:
                    # Return None to signal to TensorRT there is no calibration data remaining
                    return None

            def read_calibration_cache(self) -> bytes | None:
                """Use existing cache instead of calibrating again, otherwise, implicitly return None."""
                if self.cache.exists() and self.cache.suffix == ".cache":
                    return self.cache.read_bytes()

            def write_calibration_cache(self, cache: bytes) -> None:
                """Write calibration cache to disk."""
                _ = self.cache.write_bytes(cache)

        # Load dataset w/ builder (for batching) and calibrate
        config.int8_calibrator = EngineCalibrator(
            dataset=dataset,
            cache=str(Path(onnx_file).with_suffix(".cache")),
        )

    elif half:
        config.set_flag(trt.BuilderFlag.FP16)

    # Write file
    build = builder.build_serialized_network if is_trt10 else builder.build_engine
    with build(network, config) as engine, open(engine_file, "wb") as t:
        # Metadata
        if metadata is not None:
            meta = json.dumps(metadata)
            t.write(len(meta).to_bytes(4, byteorder="little", signed=True))
            t.write(meta.encode())
        # Model
        t.write(engine if is_trt10 else engine.serialize())
```

---
