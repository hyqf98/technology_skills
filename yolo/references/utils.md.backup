# Yolo - Utils

**Pages:** 20

---

## Reference for hub_sdk/helpers/exceptions.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/hub/sdk/reference/helpers/exceptions/

**Contents:**
- Reference for hub_sdk/helpers/exceptions.py
- function hub_sdk.helpers.exceptions.suppress_exceptions

This page is sourced from https://github.com/ultralytics/hub-sdk/blob/main/hub_sdk/helpers/exceptions.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Suppress exceptions locally based on the global HUB_EXCEPTIONS flag.

If the HUB_EXCEPTIONS flag is set to False, this function raises the caught exception, allowing it to propagate and be handled elsewhere. If the flag is set to True, the function suppresses the exception, effectively handling it locally.

This function is designed to be used in conjunction with the global HUB_EXCEPTIONS constant to control exception handling behavior across multiple parts of the codebase.

**Examples:**

Example 1 (python):
```python
def suppress_exceptions() -> None
```

Example 2 (markdown):
```markdown
# Set the HUB_EXCEPTIONS constant to control exception handling globally
>>> HUB_EXCEPTIONS = False

>>> try:
...     # Your code that may raise an exception
...     pass
... except ValueError as e:
...     # The exception will be suppressed if HUB_EXCEPTIONS is True
...     suppress_exceptions()
...     # Exception handling continues here if HUB_EXCEPTIONS is False
```

Example 3 (python):
```python
def suppress_exceptions() -> None:
    """Suppress exceptions locally based on the global HUB_EXCEPTIONS flag.

    If the HUB_EXCEPTIONS flag is set to False, this function raises the caught exception, allowing it to propagate and
    be handled elsewhere. If the flag is set to True, the function suppresses the exception, effectively handling it
    locally.

    Examples:
        # Set the HUB_EXCEPTIONS constant to control exception handling globally
        >>> HUB_EXCEPTIONS = False

        >>> try:
        ...     # Your code that may raise an exception
        ...     pass
        ... except ValueError as e:
        ...     # The exception will be suppressed if HUB_EXCEPTIONS is True
        ...     suppress_exceptions()
        ...     # Exception handling continues here if HUB_EXCEPTIONS is False

    Notes:
        This function is designed to be used in conjunction with the global HUB_EXCEPTIONS constant
        to control exception handling behavior across multiple parts of the codebase.
    """
    if not HUB_EXCEPTIONS:
        raise
```

---

## Reference for ultralytics/utils/tuner.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/tuner/

**Contents:**
- Reference for ultralytics/utils/tuner.py
- function ultralytics.utils.tuner.run_ray_tune

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/tuner.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Run hyperparameter tuning using Ray Tune.

**Examples:**

Example 1 (typescript):
```typescript
def run_ray_tune(
    model,
    space: dict | None = None,
    grace_period: int = 10,
    gpu_per_trial: int | None = None,
    max_samples: int = 10,
    **train_args,
)
```

Example 2 (python):
```python
>>> from ultralytics import YOLO
>>> model = YOLO("yolo11n.pt")  # Load a YOLO11n model

Start tuning hyperparameters for YOLO11n training on the COCO8 dataset
>>> result_grid = model.tune(data="coco8.yaml", use_ray=True)
```

Example 3 (python):
```python
def run_ray_tune(
    model,
    space: dict | None = None,
    grace_period: int = 10,
    gpu_per_trial: int | None = None,
    max_samples: int = 10,
    **train_args,
):
    """Run hyperparameter tuning using Ray Tune.

    Args:
        model (YOLO): Model to run the tuner on.
        space (dict, optional): The hyperparameter search space. If not provided, uses default space.
        grace_period (int, optional): The grace period in epochs of the ASHA scheduler.
        gpu_per_trial (int, optional): The number of GPUs to allocate per trial.
        max_samples (int, optional): The maximum number of trials to run.
        **train_args (Any): Additional arguments to pass to the `train()` method.

    Returns:
        (ray.tune.ResultGrid): A ResultGrid containing the results of the hyperparameter search.

    Examples:
        >>> from ultralytics import YOLO
        >>> model = YOLO("yolo11n.pt")  # Load a YOLO11n model

        Start tuning hyperparameters for YOLO11n training on the COCO8 dataset
        >>> result_grid = model.tune(data="coco8.yaml", use_ray=True)
    """
    LOGGER.info("💡 Learn about RayTune at https://docs.ultralytics.com/integrations/ray-tune")
    try:
        checks.check_requirements("ray[tune]")

        import ray
        from ray import tune
        from ray.air import RunConfig
        from ray.air.integrations.wandb import WandbLoggerCallback
        from ray.tune.schedulers import ASHAScheduler
    except ImportError:
        raise ModuleNotFoundError('Ray Tune required but not found. To install run: pip install "ray[tune]"')

    try:
        import wandb

        assert hasattr(wandb, "__version__")
    except (ImportError, AssertionError):
        wandb = False

    checks.check_version(ray.__version__, ">=2.0.0", "ray")
    default_space = {
        # 'optimizer': tune.choice(['SGD', 'Adam', 'AdamW', 'NAdam', 'RAdam', 'RMSProp']),
        "lr0": tune.uniform(1e-5, 1e-1),
        "lrf": tune.uniform(0.01, 1.0),  # final OneCycleLR learning rate (lr0 * lrf)
        "momentum": tune.uniform(0.6, 0.98),  # SGD momentum/Adam beta1
        "weight_decay": tune.uniform(0.0, 0.001),  # optimizer weight decay
        "warmup_epochs": tune.uniform(0.0, 5.0),  # warmup epochs (fractions ok)
        "warmup_momentum": tune.uniform(0.0, 0.95),  # warmup initial momentum
        "box": tune.uniform(0.02, 0.2),  # box loss gain
        "cls": tune.uniform(0.2, 4.0),  # cls loss gain (scale with pixels)
        "hsv_h": tune.uniform(0.0, 0.1),  # image HSV-Hue augmentation (fraction)
        "hsv_s": tune.uniform(0.0, 0.9),  # image HSV-Saturation augmentation (fraction)
        "hsv_v": tune.uniform(0.0, 0.9),  # image HSV-Value augmentation (fraction)
        "degrees": tune.uniform(0.0, 45.0),  # image rotation (+/- deg)
        "translate": tune.uniform(0.0, 0.9),  # image translation (+/- fraction)
        "scale": tune.uniform(0.0, 0.9),  # image scale (+/- gain)
        "shear": tune.uniform(0.0, 10.0),  # image shear (+/- deg)
        "perspective": tune.uniform(0.0, 0.001),  # image perspective (+/- fraction), range 0-0.001
        "flipud": tune.uniform(0.0, 1.0),  # image flip up-down (probability)
        "fliplr": tune.uniform(0.0, 1.0),  # image flip left-right (probability)
        "bgr": tune.uniform(0.0, 1.0),  # swap RGB↔BGR channels (probability)
        "mosaic": tune.uniform(0.0, 1.0),  # image mosaic (probability)
        "mixup": tune.uniform(0.0, 1.0),  # image mixup (probability)
        "cutmix": tune.uniform(0.0, 1.0),  # image cutmix (probability)
        "copy_paste": tune.uniform(0.0, 1.0),  # segment copy-paste (probability)
    }

    # Put the model in ray store
    task = model.task
    model_in_store = ray.put(model)
    base_name = train_args.get("name", "tune")

    def _tune(config):
        """Train the YOLO model with the specified hyperparameters and return results."""
        model_to_train = ray.get(model_in_store)  # get the model from ray store for tuning
        model_to_train.reset_callbacks()
        config.update(train_args)

        # Set trial-specific name for W&B logging
        try:
            trial_id = tune.get_trial_id()  # Get current trial ID (e.g., "2c2fc_00000")
            trial_suffix = trial_id.split("_")[-1] if "_" in trial_id else trial_id
            config["name"] = f"{base_name}_{trial_suffix}"
        except Exception:
            # Not in Ray Tune context or error getting trial ID, use base name
            config["name"] = base_name

        results = model_to_train.train(**config)
        return results.results_dict

    # Get search space
    if not space and not train_args.get("resume"):
        space = default_space
        LOGGER.warning("Search space not provided, using default search space.")

    # Get dataset
    data = train_args.get("data", TASK2DATA[task])
    space["data"] = data
    if "data" not in train_args:
        LOGGER.warning(f'Data not provided, using default "data={data}".')

    # Define the trainable function with allocated resources
    trainable_with_resources = tune.with_resources(_tune, {"cpu": NUM_THREADS, "gpu": gpu_per_trial or 0})

    # Define the ASHA scheduler for hyperparameter search
    asha_scheduler = ASHAScheduler(
        time_attr="epoch",
        metric=TASK2METRIC[task],
        mode="max",
        max_t=train_args.get("epochs") or DEFAULT_CFG_DICT["epochs"] or 100,
        grace_period=grace_period,
        reduction_factor=3,
    )

    # Define the callbacks for the hyperparameter search
    tuner_callbacks = [WandbLoggerCallback(project="YOLOv8-tune")] if wandb else []

    # Create the Ray Tune hyperparameter search tuner
    tune_dir = get_save_dir(
        get_cfg(
            DEFAULT_CFG,
            {**train_args, **{"exist_ok": train_args.pop("resume", False)}},  # resume w/ same tune_dir
        ),
        name=train_args.pop("name", "tune"),  # runs/{task}/{tune_dir}
    )  # must be absolute dir
    tune_dir.mkdir(parents=True, exist_ok=True)
    if tune.Tuner.can_restore(tune_dir):
        LOGGER.info(f"{colorstr('Tuner: ')} Resuming tuning run {tune_dir}...")
        tuner = tune.Tuner.restore(str(tune_dir), trainable=trainable_with_resources, resume_errored=True)
    else:
        tuner = tune.Tuner(
            trainable_with_resources,
            param_space=space,
            tune_config=tune.TuneConfig(
                scheduler=asha_scheduler,
                num_samples=max_samples,
                trial_name_creator=lambda trial: f"{trial.trainable_name}_{trial.trial_id}",
                trial_dirname_creator=lambda trial: f"{trial.trainable_name}_{trial.trial_id}",
            ),
            run_config=RunConfig(callbacks=tuner_callbacks, storage_path=tune_dir.parent, name=tune_dir.name),
        )

    # Run the hyperparameter search
    tuner.fit()

    # Get the results of the hyperparameter search
    results = tuner.get_results()

    # Shut down Ray to clean up workers
    ray.shutdown()

    return results
```

---

## Reference for ultralytics/utils/files.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/files/

**Contents:**
- Reference for ultralytics/utils/files.py
- class ultralytics.utils.files.WorkingDirectory
  - method ultralytics.utils.files.WorkingDirectory.__enter__
  - method ultralytics.utils.files.WorkingDirectory.__exit__
- function ultralytics.utils.files.spaces_in_path
- function ultralytics.utils.files.increment_path
- function ultralytics.utils.files.file_age
- function ultralytics.utils.files.file_date
- function ultralytics.utils.files.file_size
- function ultralytics.utils.files.get_latest_run

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/files.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Bases: contextlib.ContextDecorator

A context manager and decorator for temporarily changing the working directory.

This class allows for the temporary change of the working directory using a context manager or decorator. It ensures that the original working directory is restored after the context or decorated function completes.

Change the current working directory to the specified directory upon entering the context.

Restore the original working directory when exiting the context.

Context manager to handle paths with spaces in their names.

If a path contains spaces, it replaces them with underscores, copies the file/directory to the new path, executes the context code block, then copies the file/directory back to its original location.

Increment a file or directory path, i.e., runs/exp --> runs/exp{sep}2, runs/exp{sep}3, ... etc.

If the path exists and exist_ok is not True, the path will be incremented by appending a number and sep to the end of the path. If the path is a file, the file extension will be preserved. If the path is a directory, the number will be appended directly to the end of the path.

Return days since the last modification of the specified file.

Return the file modification date in 'YYYY-M-D' format.

Return the size of a file or directory in megabytes (MB).

Return the path to the most recent 'last.pt' file in the specified directory for resuming training.

Update and re-save specified YOLO models in an 'updated_models' subdirectory.

**Examples:**

Example 1 (unknown):
```unknown
WorkingDirectory(self, new_dir: str | Path)
```

Example 2 (python):
```python
Using as a context manager:
>>> with WorkingDirectory("/path/to/new/dir"):
...     # Perform operations in the new directory
...     pass

Using as a decorator:
>>> @WorkingDirectory("/path/to/new/dir")
... def some_function():
...     # Perform operations in the new directory
...     pass
```

Example 3 (python):
```python
class WorkingDirectory(contextlib.ContextDecorator):
    """A context manager and decorator for temporarily changing the working directory.

    This class allows for the temporary change of the working directory using a context manager or decorator. It ensures
    that the original working directory is restored after the context or decorated function completes.

    Attributes:
        dir (Path | str): The new directory to switch to.
        cwd (Path): The original current working directory before the switch.

    Methods:
        __enter__: Changes the current directory to the specified directory.
        __exit__: Restores the original working directory on context exit.

    Examples:
        Using as a context manager:
        >>> with WorkingDirectory("/path/to/new/dir"):
        ...     # Perform operations in the new directory
        ...     pass

        Using as a decorator:
        >>> @WorkingDirectory("/path/to/new/dir")
        ... def some_function():
        ...     # Perform operations in the new directory
        ...     pass
    """

    def __init__(self, new_dir: str | Path):
        """Initialize the WorkingDirectory context manager with the target directory."""
        self.dir = new_dir  # new dir
        self.cwd = Path.cwd().resolve()  # current dir
```

Example 4 (python):
```python
def __enter__(self)
```

---

## Reference for ultralytics/utils/tqdm.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/tqdm/

**Contents:**
- Reference for ultralytics/utils/tqdm.py
- class ultralytics.utils.tqdm.TQDM
  - method ultralytics.utils.tqdm.TQDM.__del__
  - method ultralytics.utils.tqdm.TQDM.__enter__
  - method ultralytics.utils.tqdm.TQDM.__exit__
  - method ultralytics.utils.tqdm.TQDM.__iter__
  - method ultralytics.utils.tqdm.TQDM._display
  - method ultralytics.utils.tqdm.TQDM._format_num
  - method ultralytics.utils.tqdm.TQDM._format_rate
  - method ultralytics.utils.tqdm.TQDM._format_time

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/tqdm.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Lightweight zero-dependency progress bar for Ultralytics.

Provides clean, rich-style progress bars suitable for various environments including Weights & Biases, console outputs, and other logging systems. Features zero external dependencies, clean single-line output, rich-style progress bars with Unicode block characters, context manager support, iterator protocol support, and dynamic description updates.

Destructor to ensure cleanup.

Enter context manager.

Exit context manager and close progress bar.

Iterate over the wrapped iterable with progress updates.

Display progress bar.

Format number with optional unit scaling.

Format rate with units, switching between it/s and s/it for readability.

Format time duration.

Generate progress bar.

Check if display should update.

Set postfix (appends to description).

Update progress by n steps.

Static method to write without breaking progress bar.

Check for known non-interactive console environments.

**Examples:**

Example 1 (python):
```python
def __init__(
    self,
    iterable: Any = None,
    desc: str | None = None,
    total: int | None = None,
    leave: bool = True,
    file: IO[str] | None = None,
    mininterval: float = 0.1,
    disable: bool | None = None,
    unit: str = "it",
    unit_scale: bool = True,
    unit_divisor: int = 1000,
    bar_format: str | None = None,  # kept for API compatibility; not used for formatting
    initial: int = 0,
    **kwargs,
) -> None
```

Example 2 (typescript):
```typescript
Basic usage with iterator:
>>> for i in TQDM(range(100)):
...     time.sleep(0.01)

With custom description:
>>> pbar = TQDM(range(100), desc="Processing")
>>> for i in pbar:
...     pbar.set_description(f"Processing item {i}")

Context manager usage:
>>> with TQDM(total=100, unit="B", unit_scale=True) as pbar:
...     for i in range(100):
...         pbar.update(1)

Manual updates:
>>> pbar = TQDM(total=100, desc="Training")
>>> for epoch in range(100):
...     # Do work
...     pbar.update(1)
>>> pbar.close()
```

Example 3 (python):
```python
class TQDM:
    """Lightweight zero-dependency progress bar for Ultralytics.

    Provides clean, rich-style progress bars suitable for various environments including Weights & Biases, console
    outputs, and other logging systems. Features zero external dependencies, clean single-line output, rich-style
    progress bars with Unicode block characters, context manager support, iterator protocol support, and dynamic
    description updates.

    Attributes:
        iterable (object): Iterable to wrap with progress bar.
        desc (str): Prefix description for the progress bar.
        total (int): Expected number of iterations.
        disable (bool): Whether to disable the progress bar.
        unit (str): String for units of iteration.
        unit_scale (bool): Auto-scale units flag.
        unit_divisor (int): Divisor for unit scaling.
        leave (bool): Whether to leave the progress bar after completion.
        mininterval (float): Minimum time interval between updates.
        initial (int): Initial counter value.
        n (int): Current iteration count.
        closed (bool): Whether the progress bar is closed.
        bar_format (str): Custom bar format string.
        file (object): Output file stream.

    Methods:
        update: Update progress by n steps.
        set_description: Set or update the description.
        set_postfix: Set postfix for the progress bar.
        close: Close the progress bar and clean up.
        refresh: Refresh the progress bar display.
        clear: Clear the progress bar from display.
        write: Write a message without breaking the progress bar.

    Examples:
        Basic usage with iterator:
        >>> for i in TQDM(range(100)):
        ...     time.sleep(0.01)

        With custom description:
        >>> pbar = TQDM(range(100), desc="Processing")
        >>> for i in pbar:
        ...     pbar.set_description(f"Processing item {i}")

        Context manager usage:
        >>> with TQDM(total=100, unit="B", unit_scale=True) as pbar:
        ...     for i in range(100):
        ...         pbar.update(1)

        Manual updates:
        >>> pbar = TQDM(total=100, desc="Training")
        >>> for epoch in range(100):
        ...     # Do work
        ...     pbar.update(1)
        >>> pbar.close()
    """

    # Constants
    MIN_RATE_CALC_INTERVAL = 0.01  # Minimum time interval for rate calculation
    RATE_SMOOTHING_FACTOR = 0.3  # Factor for exponential smoothing of rates
    MAX_SMOOTHED_RATE = 1000000  # Maximum rate to apply smoothing to
    NONINTERACTIVE_MIN_INTERVAL = 60.0  # Minimum interval for non-interactive environments

    def __init__(
        self,
        iterable: Any = None,
        desc: str | None = None,
        total: int | None = None,
        leave: bool = True,
        file: IO[str] | None = None,
        mininterval: float = 0.1,
        disable: bool | None = None,
        unit: str = "it",
        unit_scale: bool = True,
        unit_divisor: int = 1000,
        bar_format: str | None = None,  # kept for API compatibility; not used for formatting
        initial: int = 0,
        **kwargs,
    ) -> None:
        """Initialize the TQDM progress bar with specified configuration options.

        Args:
            iterable (object, optional): Iterable to wrap with progress bar.
            desc (str, optional): Prefix description for the progress bar.
            total (int, optional): Expected number of iterations.
            leave (bool, optional): Whether to leave the progress bar after completion.
            file (object, optional): Output file stream for progress display.
            mininterval (float, optional): Minimum time interval between updates (default 0.1s, 60s in GitHub Actions).
            disable (bool, optional): Whether to disable the progress bar. Auto-detected if None.
            unit (str, optional): String for units of iteration (default "it" for items).
            unit_scale (bool, optional): Auto-scale units for bytes/data units.
            unit_divisor (int, optional): Divisor for unit scaling (default 1000).
            bar_format (str, optional): Custom bar format string.
            initial (int, optional): Initial counter value.
            **kwargs (Any): Additional keyword arguments for compatibility (ignored).
        """
        # Disable if not verbose
        if disable is None:
            try:
                from ultralytics.utils import LOGGER, VERBOSE

                disable = not VERBOSE or LOGGER.getEffectiveLevel() > 20
            except ImportError:
                disable = False

        self.iterable = iterable
        self.desc = desc or ""
        self.total = total or (len(iterable) if hasattr(iterable, "__len__") else None) or None  # prevent total=0
        self.disable = disable
        self.unit = unit
        self.unit_scale = unit_scale
        self.unit_divisor = unit_divisor
        self.leave = leave
        self.noninteractive = is_noninteractive_console()
        self.mininterval = max(mininterval, self.NONINTERACTIVE_MIN_INTERVAL) if self.noninteractive else mininterval
        self.initial = initial

        # Kept for API compatibility (unused for f-string formatting)
        self.bar_format = bar_format

        self.file = file or sys.stdout

        # Internal state
        self.n = self.initial
        self.last_print_n = self.initial
        self.last_print_t = time.time()
        self.start_t = time.time()
        self.last_rate = 0.0
        self.closed = False
        self.is_bytes = unit_scale and unit in {"B", "bytes"}
        self.scales = (
            [(1073741824, "GB/s"), (1048576, "MB/s"), (1024, "KB/s")]
            if self.is_bytes
            else [(1e9, f"G{self.unit}/s"), (1e6, f"M{self.unit}/s"), (1e3, f"K{self.unit}/s")]
        )

        if not self.disable and self.total and not self.noninteractive:
            self._display()
```

Example 4 (python):
```python
def __del__(self) -> None
```

---

## Reference for ultralytics/hub/utils.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/hub/utils/

**Contents:**
- Reference for ultralytics/hub/utils.py
- function ultralytics.hub.utils.request_with_credentials
- function ultralytics.hub.utils.requests_with_progress
- function ultralytics.hub.utils.smart_request

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/hub/utils.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Make an AJAX request with cookies attached in a Google Colab environment.

Make an HTTP request using the specified method and URL, with an optional progress bar.

Make an HTTP request using the 'requests' library, with exponential backoff retries up to a specified timeout.

**Examples:**

Example 1 (python):
```python
def request_with_credentials(url: str) -> Any
```

Example 2 (javascript):
```javascript
def request_with_credentials(url: str) -> Any:
    """Make an AJAX request with cookies attached in a Google Colab environment.

    Args:
        url (str): The URL to make the request to.

    Returns:
        (Any): The response data from the AJAX request.

    Raises:
        OSError: If the function is not run in a Google Colab environment.
    """
    if not IS_COLAB:
        raise OSError("request_with_credentials() must run in a Colab environment")
    from google.colab import output
    from IPython import display

    display.display(
        display.Javascript(
            f"""
            window._hub_tmp = new Promise((resolve, reject) => {{
                const timeout = setTimeout(() => reject("Failed authenticating existing browser session"), 5000)
                fetch("{url}", {{
                    method: 'POST',
                    credentials: 'include'
                }})
                    .then((response) => resolve(response.json()))
                    .then((json) => {{
                    clearTimeout(timeout);
                    }}).catch((err) => {{
                    clearTimeout(timeout);
                    reject(err);
                }});
            }});
            """
        )
    )
    return output.eval_js("_hub_tmp")
```

Example 3 (python):
```python
def requests_with_progress(method: str, url: str, **kwargs)
```

Example 4 (python):
```python
def requests_with_progress(method: str, url: str, **kwargs):
    """Make an HTTP request using the specified method and URL, with an optional progress bar.

    Args:
        method (str): The HTTP method to use (e.g. 'GET', 'POST').
        url (str): The URL to send the request to.
        **kwargs (Any): Additional keyword arguments to pass to the underlying `requests.request` function.

    Returns:
        (requests.Response): The response object from the HTTP request.

    Notes:
        - If 'progress' is set to True, the progress bar will display the download progress for responses with a known
          content length.
        - If 'progress' is a number then progress bar will display assuming content length = progress.
    """
    import requests  # scoped as slow import

    progress = kwargs.pop("progress", False)
    if not progress:
        return requests.request(method, url, **kwargs)
    response = requests.request(method, url, stream=True, **kwargs)
    total = int(response.headers.get("content-length", 0) if isinstance(progress, bool) else progress)  # total size
    try:
        pbar = TQDM(total=total, unit="B", unit_scale=True, unit_divisor=1024)
        for data in response.iter_content(chunk_size=1024):
            pbar.update(len(data))
        pbar.close()
    except requests.exceptions.ChunkedEncodingError:  # avoid 'Connection broken: IncompleteRead' warnings
        response.close()
    return response
```

---

## Reference for ultralytics/utils/instance.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/instance/

**Contents:**
- Reference for ultralytics/utils/instance.py
- class ultralytics.utils.instance.Bboxes
  - method ultralytics.utils.instance.Bboxes.__getitem__
  - method ultralytics.utils.instance.Bboxes.__len__
  - method ultralytics.utils.instance.Bboxes.add
  - method ultralytics.utils.instance.Bboxes.areas
  - method ultralytics.utils.instance.Bboxes.concatenate
  - method ultralytics.utils.instance.Bboxes.convert
  - method ultralytics.utils.instance.Bboxes.mul
- class ultralytics.utils.instance.Instances

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/instance.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class for handling bounding boxes in multiple formats.

The class supports various bounding box formats like 'xyxy', 'xywh', and 'ltwh' and provides methods for format conversion, scaling, and area calculation. Bounding box data should be provided as numpy arrays.

This class does not handle normalization or denormalization of bounding boxes.

Retrieve a specific bounding box or a set of bounding boxes using indexing.

When using boolean indexing, make sure to provide a boolean array with the same length as the number of bounding boxes.

Return the number of bounding boxes.

Add offset to bounding box coordinates.

Calculate the area of bounding boxes.

Concatenate a list of Bboxes objects into a single Bboxes object.

The input should be a list or tuple of Bboxes objects.

Convert bounding box format from one type to another.

Multiply bounding box coordinates by scale factor(s).

Container for bounding boxes, segments, and keypoints of detected objects in an image.

This class provides a unified interface for handling different types of object annotations including bounding boxes, segmentation masks, and keypoints. It supports various operations like scaling, normalization, clipping, and format conversion.

Calculate the area of bounding boxes.

Return bounding boxes.

Retrieve a specific instance or a set of instances using indexing.

When using boolean indexing, make sure to provide a boolean array with the same length as the number of instances.

Return the number of instances.

Add padding to coordinates.

Clip coordinates to stay within image boundaries.

Concatenate a list of Instances objects into a single Instances object.

The Instances objects in the list should have the same properties, such as the format of the bounding boxes, whether keypoints are present, and if the coordinates are normalized.

Convert bounding box format.

Convert normalized coordinates to absolute coordinates.

Flip coordinates horizontally.

Flip coordinates vertically.

Convert absolute coordinates to normalized coordinates.

Remove zero-area boxes, i.e. after clipping some boxes may have zero width or height.

Scale coordinates by given factors.

Update instance variables.

Create a function that converts input to n-tuple by repeating singleton values.

**Examples:**

Example 1 (typescript):
```typescript
Bboxes(self, bboxes: np.ndarray, format: str = "xyxy") -> None
```

Example 2 (unknown):
```unknown
Create bounding boxes in YOLO format
>>> bboxes = Bboxes(np.array([[100, 50, 150, 100]]), format="xywh")
>>> bboxes.convert("xyxy")
>>> print(bboxes.areas())
```

Example 3 (python):
```python
class Bboxes:
    """A class for handling bounding boxes in multiple formats.

    The class supports various bounding box formats like 'xyxy', 'xywh', and 'ltwh' and provides methods for format
    conversion, scaling, and area calculation. Bounding box data should be provided as numpy arrays.

    Attributes:
        bboxes (np.ndarray): The bounding boxes stored in a 2D numpy array with shape (N, 4).
        format (str): The format of the bounding boxes ('xyxy', 'xywh', or 'ltwh').

    Methods:
        convert: Convert bounding box format from one type to another.
        areas: Calculate the area of bounding boxes.
        mul: Multiply bounding box coordinates by scale factor(s).
        add: Add offset to bounding box coordinates.
        concatenate: Concatenate multiple Bboxes objects.

    Examples:
        Create bounding boxes in YOLO format
        >>> bboxes = Bboxes(np.array([[100, 50, 150, 100]]), format="xywh")
        >>> bboxes.convert("xyxy")
        >>> print(bboxes.areas())

    Notes:
        This class does not handle normalization or denormalization of bounding boxes.
    """

    def __init__(self, bboxes: np.ndarray, format: str = "xyxy") -> None:
        """Initialize the Bboxes class with bounding box data in a specified format.

        Args:
            bboxes (np.ndarray): Array of bounding boxes with shape (N, 4) or (4,).
            format (str): Format of the bounding boxes, one of 'xyxy', 'xywh', or 'ltwh'.
        """
        assert format in _formats, f"Invalid bounding box format: {format}, format must be one of {_formats}"
        bboxes = bboxes[None, :] if bboxes.ndim == 1 else bboxes
        assert bboxes.ndim == 2
        assert bboxes.shape[1] == 4
        self.bboxes = bboxes
        self.format = format
```

Example 4 (python):
```python
def __getitem__(self, index: int | np.ndarray | slice) -> Bboxes
```

---

## Reference for ultralytics/utils/nms.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/nms/

**Contents:**
- Reference for ultralytics/utils/nms.py
- class ultralytics.utils.nms.TorchNMS
  - method ultralytics.utils.nms.TorchNMS.batched_nms
  - method ultralytics.utils.nms.TorchNMS.fast_nms
  - method ultralytics.utils.nms.TorchNMS.nms
- function ultralytics.utils.nms.non_max_suppression

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/nms.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Ultralytics custom NMS implementation optimized for YOLO.

This class provides static methods for performing non-maximum suppression (NMS) operations on bounding boxes, including both standard NMS and batched NMS for multi-class scenarios.

Batched NMS for class-aware suppression.

Fast-NMS implementation from https://arxiv.org/pdf/1904.02689 using upper triangular matrix operations.

Optimized NMS with early termination that matches torchvision behavior exactly.

Perform non-maximum suppression (NMS) on prediction results.

Applies NMS to filter overlapping bounding boxes based on confidence and IoU thresholds. Supports multiple detection formats including standard boxes, rotated boxes, and masks.

**Examples:**

Example 1 (unknown):
```unknown
Perform standard NMS on boxes and scores
>>> boxes = torch.tensor([[0, 0, 10, 10], [5, 5, 15, 15]])
>>> scores = torch.tensor([0.9, 0.8])
>>> keep = TorchNMS.nms(boxes, scores, 0.5)
```

Example 2 (python):
```python
class TorchNMS:
```

Example 3 (typescript):
```typescript
def batched_nms(
    boxes: torch.Tensor,
    scores: torch.Tensor,
    idxs: torch.Tensor,
    iou_threshold: float,
    use_fast_nms: bool = False,
) -> torch.Tensor
```

Example 4 (unknown):
```unknown
Apply batched NMS across multiple classes
>>> boxes = torch.tensor([[0, 0, 10, 10], [5, 5, 15, 15]])
>>> scores = torch.tensor([0.9, 0.8])
>>> idxs = torch.tensor([0, 1])
>>> keep = TorchNMS.batched_nms(boxes, scores, idxs, 0.5)
```

---

## Reference for ultralytics/utils/torch_utils.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/torch_utils/

**Contents:**
- Reference for ultralytics/utils/torch_utils.py
- class ultralytics.utils.torch_utils.ModelEMA
  - method ultralytics.utils.torch_utils.ModelEMA.update
  - method ultralytics.utils.torch_utils.ModelEMA.update_attr
- class ultralytics.utils.torch_utils.EarlyStopping
  - method ultralytics.utils.torch_utils.EarlyStopping.__call__
- function ultralytics.utils.torch_utils.torch_distributed_zero_first
- function ultralytics.utils.torch_utils.smart_inference_mode
- function ultralytics.utils.torch_utils.autocast
- function ultralytics.utils.torch_utils.get_cpu_info

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/torch_utils.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Updated Exponential Moving Average (EMA) implementation.

Keeps a moving average of everything in the model state_dict (parameters and buffers). For EMA details see References.

To disable EMA set the enabled attribute to False.

Update EMA parameters.

Update attributes and save stripped model with optimizer removed.

Early stopping class that stops training when a specified number of epochs have passed without improvement.

Check whether to stop training.

Ensure all processes in distributed training wait for the local master (rank 0) to complete a task first.

Apply torch.inference_mode() decorator if torch>=1.9.0 else torch.no_grad() decorator.

Get the appropriate autocast context manager based on PyTorch version and AMP setting.

This function returns a context manager for automatic mixed precision (AMP) training that is compatible with both older and newer versions of PyTorch. It handles the differences in the autocast API between PyTorch versions.

Return a string with system CPU information, i.e. 'Apple M2'.

Return a string with system GPU information, i.e. 'Tesla T4, 15102MiB'.

Select the appropriate PyTorch device based on the provided arguments.

The function takes a string specifying the device or a torch.device object and returns a torch.device object representing the selected device. The function also validates the number of available devices and raises an exception if the requested device(s) are not available.

Sets the 'CUDA_VISIBLE_DEVICES' environment variable for specifying which GPUs to use.

Return PyTorch-accurate time.

Fuse Conv2d and BatchNorm2d layers for inference optimization.

Fuse ConvTranspose2d and BatchNorm2d layers for inference optimization.

Print and return detailed model information layer by layer.

Return the total number of parameters in a YOLO model.

Return the total number of parameters with gradients in a YOLO model.

Return model info dict with useful model information.

Calculate FLOPs (floating point operations) for a model in billions.

Attempts two calculation methods: first with a stride-based tensor for efficiency, then falls back to full image size if needed (e.g., for RTDETR models). Returns 0.0 if thop library is unavailable or calculation fails.

Compute model FLOPs using torch profiler (alternative to thop package, but 2-10x slower).

Initialize model weights to random values.

Scale and pad an image tensor, optionally maintaining aspect ratio and padding to gs multiple.

Copy attributes from object 'b' to object 'a', with options to include/exclude certain attributes.

Return a dictionary of intersecting keys with matching shapes, excluding 'exclude' keys, using da values.

Return True if model is of type DP or DDP.

Unwrap compiled and parallel models to get the base model.

Return a lambda function for sinusoidal ramp from y1 to y2 https://arxiv.org/pdf/1812.01187.pdf.

Initialize random number generator (RNG) seeds https://pytorch.org/docs/stable/notes/randomness.html.

Unset all the configurations applied for deterministic training.

Strip optimizer from 'f' to finalize training, optionally save as 's'.

Convert the state_dict of a given optimizer to FP16, focusing on the 'state' key for tensor conversions.

Monitor and manage CUDA memory usage.

This function checks if CUDA is available and, if so, empties the CUDA cache to free up unused memory. It then yields a dictionary containing memory usage information, which can be updated by the caller. Finally, it updates the dictionary with the amount of memory reserved by CUDA on the specified device.

Ultralytics speed, memory and FLOPs profiler.

Compile a model with torch.compile and optionally warm up the graph to reduce first-iteration latency.

This utility attempts to compile the provided model using the inductor backend with dynamic shapes enabled and an autotuning mode. If compilation is unavailable or fails, the original model is returned unchanged. An optional warmup performs a single forward pass on a dummy input to prime the compiled graph and measure compile/warmup time.

**Examples:**

Example 1 (unknown):
```unknown
ModelEMA(self, model, decay = 0.9999, tau = 2000, updates = 0)
```

Example 2 (python):
```python
class ModelEMA:
    """Updated Exponential Moving Average (EMA) implementation.

    Keeps a moving average of everything in the model state_dict (parameters and buffers). For EMA details see
    References.

    To disable EMA set the `enabled` attribute to `False`.

    Attributes:
        ema (nn.Module): Copy of the model in evaluation mode.
        updates (int): Number of EMA updates.
        decay (function): Decay function that determines the EMA weight.
        enabled (bool): Whether EMA is enabled.

    References:
        - https://github.com/rwightman/pytorch-image-models
        - https://www.tensorflow.org/api_docs/python/tf/train/ExponentialMovingAverage
    """

    def __init__(self, model, decay=0.9999, tau=2000, updates=0):
        """Initialize EMA for 'model' with given arguments.

        Args:
            model (nn.Module): Model to create EMA for.
            decay (float, optional): Maximum EMA decay rate.
            tau (int, optional): EMA decay time constant.
            updates (int, optional): Initial number of updates.
        """
        self.ema = deepcopy(unwrap_model(model)).eval()  # FP32 EMA
        self.updates = updates  # number of EMA updates
        self.decay = lambda x: decay * (1 - math.exp(-x / tau))  # decay exponential ramp (to help early epochs)
        for p in self.ema.parameters():
            p.requires_grad_(False)
        self.enabled = True
```

Example 3 (python):
```python
def update(self, model)
```

Example 4 (python):
```python
def update(self, model):
    """Update EMA parameters.

    Args:
        model (nn.Module): Model to update EMA from.
    """
    if self.enabled:
        self.updates += 1
        d = self.decay(self.updates)

        msd = unwrap_model(model).state_dict()  # model state_dict
        for k, v in self.ema.state_dict().items():
            if v.dtype.is_floating_point:  # true for FP16 and FP32
                v *= d
                v += (1 - d) * msd[k].detach()
```

---

## Reference for hub_sdk/helpers/utils.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/hub/sdk/reference/helpers/utils/

**Contents:**
- Reference for hub_sdk/helpers/utils.py
- function hub_sdk.helpers.utils.threaded

This page is sourced from https://github.com/ultralytics/hub-sdk/blob/main/hub_sdk/helpers/utils.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Multi-threads a target function by default and returns the thread or function result.

This decorator provides flexible execution of the target function, either in a separate thread or synchronously. By default, the function runs in a thread, but this can be controlled via the 'threaded=False' keyword argument which is removed from kwargs before calling the function.

**Examples:**

Example 1 (python):
```python
def threaded(func)
```

Example 2 (python):
```python
>>> @threaded
... def process_data(data):
...     return data
>>>
>>> thread = process_data(my_data)  # Runs in background thread
>>> result = process_data(my_data, threaded=False)  # Runs synchronously, returns function result
```

Example 3 (python):
```python
def threaded(func):
    """Multi-threads a target function by default and returns the thread or function result.

    This decorator provides flexible execution of the target function, either in a separate thread or synchronously. By
    default, the function runs in a thread, but this can be controlled via the 'threaded=False' keyword argument which
    is removed from kwargs before calling the function.

    Args:
        func (callable): The function to be potentially executed in a separate thread.

    Returns:
        (callable): A wrapper function that either returns a daemon thread or the direct function result.

    Examples:
        >>> @threaded
        ... def process_data(data):
        ...     return data
        >>>
        >>> thread = process_data(my_data)  # Runs in background thread
        >>> result = process_data(my_data, threaded=False)  # Runs synchronously, returns function result
    """

    def wrapper(*args, **kwargs):
        """Multi-threads a given function based on 'threaded' kwarg and returns the thread or function result.

        Args:
            *args: Variable length argument list to pass to the target function.
            **kwargs: Arbitrary keyword arguments to pass to the target function.
        Keyword Args:
            threaded (bool, optional): Whether to run in a thread. Defaults to True.

        Returns:
            Union[threading.Thread, Any]: Either a started daemon thread or the direct result of the function call,
                depending on the value of the 'threaded' parameter.
        """
        if kwargs.pop("threaded", True):  # run in thread
            thread = threading.Thread(target=func, args=args, kwargs=kwargs, daemon=True)
            thread.start()
            return thread
        else:
            return func(*args, **kwargs)

    return wrapper
```

---

## Reference for ultralytics/utils/errors.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/errors/

**Contents:**
- Reference for ultralytics/utils/errors.py
- class ultralytics.utils.errors.HUBModelError

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/errors.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Exception raised when a model cannot be found or retrieved from Ultralytics HUB.

This custom exception is used specifically for handling errors related to model fetching in Ultralytics YOLO. The error message is processed to include emojis for better user experience.

This exception is raised when a requested model is not found or cannot be retrieved from Ultralytics HUB. The message is processed to include emojis for better user experience.

**Examples:**

Example 1 (typescript):
```typescript
HUBModelError(self, message: str = "Model not found. Please check model URL and try again.")
```

Example 2 (typescript):
```typescript
>>> try:
...     # Code that might fail to find a model
...     raise HUBModelError("Custom model not found message")
... except HUBModelError as e:
...     print(e)  # Displays the emoji-enhanced error message
```

Example 3 (python):
```python
class HUBModelError(Exception):
    """Exception raised when a model cannot be found or retrieved from Ultralytics HUB.

    This custom exception is used specifically for handling errors related to model fetching in Ultralytics YOLO. The
    error message is processed to include emojis for better user experience.

    Attributes:
        message (str): The error message displayed when the exception is raised.

    Methods:
        __init__: Initialize the HUBModelError with a custom message.

    Examples:
        >>> try:
        ...     # Code that might fail to find a model
        ...     raise HUBModelError("Custom model not found message")
        ... except HUBModelError as e:
        ...     print(e)  # Displays the emoji-enhanced error message
    """

    def __init__(self, message: str = "Model not found. Please check model URL and try again."):
        """Initialize a HUBModelError exception.

        This exception is raised when a requested model is not found or cannot be retrieved from Ultralytics HUB. The
        message is processed to include emojis for better user experience.

        Args:
            message (str, optional): The error message to display when the exception is raised.
        """
        super().__init__(emojis(message))
```

---

## Reference for ultralytics/utils/downloads.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/downloads/

**Contents:**
- Reference for ultralytics/utils/downloads.py
- function ultralytics.utils.downloads.is_url
- function ultralytics.utils.downloads.delete_dsstore
- function ultralytics.utils.downloads.zip_directory
- function ultralytics.utils.downloads.unzip_file
- function ultralytics.utils.downloads.check_disk_space
- function ultralytics.utils.downloads.get_google_drive_file_info
- function ultralytics.utils.downloads.safe_download
- function ultralytics.utils.downloads.get_github_assets
- function ultralytics.utils.downloads.attempt_download_asset

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/downloads.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Validate if the given string is a URL and optionally check if the URL exists online.

Delete all specified system files in a directory.

".DS_Store" files are created by the Apple operating system and contain metadata about folders and files. They are hidden system files and can cause issues when transferring files between different operating systems.

Zip the contents of a directory, excluding specified files.

The resulting zip file is named after the directory and placed alongside it.

Unzip a *.zip file to the specified path, excluding specified files.

If the zipfile does not contain a single top-level directory, the function will create a new directory with the same name as the zipfile (without the extension) to extract its contents. If a path is not provided, the function will use the parent directory of the zipfile as the default path.

Check if there is sufficient disk space to download and store a file.

Retrieve the direct download link and filename for a shareable Google Drive file link.

Download files from a URL with options for retrying, unzipping, and deleting the downloaded file. Enhanced with

robust partial download detection using Content-Length validation.

Retrieve the specified version's tag and assets from a GitHub repository.

If the version is not specified, the function fetches the latest release assets.

Attempt to download a file from GitHub release assets if it is not found locally.

Download files from specified URLs to a given directory.

Supports concurrent downloads if multiple threads are specified.

**Examples:**

Example 1 (typescript):
```typescript
def is_url(url: str | Path, check: bool = False) -> bool
```

Example 2 (unknown):
```unknown
>>> valid = is_url("https://www.example.com")
>>> valid_and_exists = is_url("https://www.example.com", check=True)
```

Example 3 (typescript):
```typescript
def is_url(url: str | Path, check: bool = False) -> bool:
    """Validate if the given string is a URL and optionally check if the URL exists online.

    Args:
        url (str): The string to be validated as a URL.
        check (bool, optional): If True, performs an additional check to see if the URL exists online.

    Returns:
        (bool): True for a valid URL. If 'check' is True, also returns True if the URL exists online.

    Examples:
        >>> valid = is_url("https://www.example.com")
        >>> valid_and_exists = is_url("https://www.example.com", check=True)
    """
    try:
        url = str(url)
        result = parse.urlparse(url)
        if not (result.scheme and result.netloc):
            return False
        if check:
            r = request.urlopen(request.Request(url, method="HEAD"), timeout=3)
            return 200 <= r.getcode() < 400
        return True
    except Exception:
        return False
```

Example 4 (python):
```python
def delete_dsstore(path: str | Path, files_to_delete: tuple[str, ...] = (".DS_Store", "__MACOSX")) -> None
```

---

## Reference for ultralytics/utils/autodevice.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/autodevice/

**Contents:**
- Reference for ultralytics/utils/autodevice.py
- class ultralytics.utils.autodevice.GPUInfo
  - method ultralytics.utils.autodevice.GPUInfo.__del__
  - method ultralytics.utils.autodevice.GPUInfo._get_device_stats
  - method ultralytics.utils.autodevice.GPUInfo.print_status
  - method ultralytics.utils.autodevice.GPUInfo.refresh_stats
  - method ultralytics.utils.autodevice.GPUInfo.select_idle_gpu
  - method ultralytics.utils.autodevice.GPUInfo.shutdown

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/autodevice.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Manages NVIDIA GPU information via pynvml with robust error handling.

Provides methods to query detailed GPU statistics (utilization, memory, temp, power) and select the most idle GPUs based on configurable criteria. It safely handles the absence or initialization failure of the pynvml library by logging warnings and disabling related features, preventing application crashes.

Includes fallback logic using torch.cuda for basic device counting if NVML is unavailable during GPU selection. Manages NVML initialization and shutdown internally.

Ensure NVML is shut down when the object is garbage collected.

Get stats for a single GPU device.

Print GPU status in a compact table format using current stats.

Refresh the internal gpu_stats list by querying NVML.

Select the most idle GPUs based on utilization and free memory.

Returns fewer than 'count' if not enough qualify or exist. Returns basic CUDA indices if NVML fails. Empty list if no GPUs found.

Shut down NVML if it was initialized.

**Examples:**

Example 1 (unknown):
```unknown
GPUInfo(self)
```

Example 2 (sql):
```sql
Initialize GPUInfo and print status
>>> gpu_info = GPUInfo()
>>> gpu_info.print_status()

Select idle GPUs with minimum memory requirements
>>> selected = gpu_info.select_idle_gpu(count=2, min_memory_fraction=0.2)
>>> print(f"Selected GPU indices: {selected}")
```

Example 3 (python):
```python
class GPUInfo:
    """Manages NVIDIA GPU information via pynvml with robust error handling.

    Provides methods to query detailed GPU statistics (utilization, memory, temp, power) and select the most idle GPUs
    based on configurable criteria. It safely handles the absence or initialization failure of the pynvml library by
    logging warnings and disabling related features, preventing application crashes.

    Includes fallback logic using `torch.cuda` for basic device counting if NVML is unavailable during GPU
    selection. Manages NVML initialization and shutdown internally.

    Attributes:
        pynvml (module | None): The `pynvml` module if successfully imported and initialized, otherwise `None`.
        nvml_available (bool): Indicates if `pynvml` is ready for use. True if import and `nvmlInit()` succeeded, False
            otherwise.
        gpu_stats (list[dict[str, Any]]): A list of dictionaries, each holding stats for one GPU, populated on
        initialization and by `refresh_stats()`. Keys include: 'index', 'name', 'utilization' (%), 'memory_used' (MiB),
            'memory_total' (MiB), 'memory_free' (MiB), 'temperature' (C), 'power_draw' (W), 'power_limit' (W or 'N/A').
            Empty if NVML is unavailable or queries fail.

    Methods:
        refresh_stats: Refresh the internal gpu_stats list by querying NVML.
        print_status: Print GPU status in a compact table format using current stats.
        select_idle_gpu: Select the most idle GPUs based on utilization and free memory.
        shutdown: Shut down NVML if it was initialized.

    Examples:
        Initialize GPUInfo and print status
        >>> gpu_info = GPUInfo()
        >>> gpu_info.print_status()

        Select idle GPUs with minimum memory requirements
        >>> selected = gpu_info.select_idle_gpu(count=2, min_memory_fraction=0.2)
        >>> print(f"Selected GPU indices: {selected}")
    """

    def __init__(self):
        """Initialize GPUInfo, attempting to import and initialize pynvml."""
        self.pynvml: Any | None = None
        self.nvml_available: bool = False
        self.gpu_stats: list[dict[str, Any]] = []

        try:
            check_requirements("nvidia-ml-py>=12.0.0")
            self.pynvml = __import__("pynvml")
            self.pynvml.nvmlInit()
            self.nvml_available = True
            self.refresh_stats()
        except Exception as e:
            LOGGER.warning(f"Failed to initialize pynvml, GPU stats disabled: {e}")
```

Example 4 (python):
```python
def __del__(self)
```

---

## Reference for ultralytics/utils/patches.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/patches/

**Contents:**
- Reference for ultralytics/utils/patches.py
- function ultralytics.utils.patches.imread
- function ultralytics.utils.patches.imwrite
- function ultralytics.utils.patches.imshow
- function ultralytics.utils.patches.torch_load
- function ultralytics.utils.patches.torch_save
- function ultralytics.utils.patches.arange_patch
- function ultralytics.utils.patches.onnx_export_patch
- function ultralytics.utils.patches.override_configs

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/patches.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Read an image from a file with multilanguage filename support.

Write an image to a file with multilanguage filename support.

Display an image in the specified window with multilanguage window name support.

This function is a wrapper around OpenCV's imshow function that displays an image in a named window. It handles multilanguage window names by encoding them properly for OpenCV compatibility.

Load a PyTorch model with updated arguments to avoid warnings.

This function wraps torch.load and adds the 'weights_only' argument for PyTorch 1.13.0+ to prevent warnings.

For PyTorch versions 1.13 and above, this function automatically sets weights_only=False if the argument is not provided, to avoid deprecation warnings.

Save PyTorch objects with retry mechanism for robustness.

This function wraps torch.save with 3 retries and exponential backoff in case of save failures, which can occur due to device flushing delays or antivirus scanning.

Workaround for ONNX torch.arange incompatibility with FP16.

https://github.com/pytorch/pytorch/issues/148041.

Workaround for ONNX export issues in PyTorch 2.9+ with Dynamo enabled.

Context manager to temporarily override configurations in args.

**Examples:**

Example 1 (typescript):
```typescript
def imread(filename: str, flags: int = cv2.IMREAD_COLOR) -> np.ndarray | None
```

Example 2 (unknown):
```unknown
>>> img = imread("path/to/image.jpg")
>>> img = imread("path/to/image.jpg", cv2.IMREAD_GRAYSCALE)
```

Example 3 (python):
```python
def imread(filename: str, flags: int = cv2.IMREAD_COLOR) -> np.ndarray | None:
    """Read an image from a file with multilanguage filename support.

    Args:
        filename (str): Path to the file to read.
        flags (int, optional): Flag that can take values of cv2.IMREAD_*. Controls how the image is read.

    Returns:
        (np.ndarray | None): The read image array, or None if reading fails.

    Examples:
        >>> img = imread("path/to/image.jpg")
        >>> img = imread("path/to/image.jpg", cv2.IMREAD_GRAYSCALE)
    """
    file_bytes = np.fromfile(filename, np.uint8)
    if filename.endswith((".tiff", ".tif")):
        success, frames = cv2.imdecodemulti(file_bytes, cv2.IMREAD_UNCHANGED)
        if success:
            # Handle multi-frame TIFFs and color images
            return frames[0] if len(frames) == 1 and frames[0].ndim == 3 else np.stack(frames, axis=2)
        return None
    else:
        im = cv2.imdecode(file_bytes, flags)
        return im[..., None] if im is not None and im.ndim == 2 else im  # Always ensure 3 dimensions
```

Example 4 (python):
```python
def imwrite(filename: str, img: np.ndarray, params: list[int] | None = None) -> bool
```

---

## Reference for ultralytics/utils/triton.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/triton/

**Contents:**
- Reference for ultralytics/utils/triton.py
- class ultralytics.utils.triton.TritonRemoteModel
  - method ultralytics.utils.triton.TritonRemoteModel.__call__

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/triton.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Client for interacting with a remote Triton Inference Server model.

This class provides a convenient interface for sending inference requests to a Triton Inference Server and processing the responses. Supports both HTTP and gRPC communication protocols.

Arguments may be provided individually or parsed from a collective 'url' argument of the form :////

Call the model with the given inputs and return inference results.

**Examples:**

Example 1 (typescript):
```typescript
TritonRemoteModel(self, url: str, endpoint: str = "", scheme: str = "")
```

Example 2 (json):
```json
Initialize a Triton client with HTTP
>>> model = TritonRemoteModel(url="localhost:8000", endpoint="yolov8", scheme="http")

Make inference with numpy arrays
>>> outputs = model(np.random.rand(1, 3, 640, 640).astype(np.float32))
```

Example 3 (python):
```python
class TritonRemoteModel:
    """Client for interacting with a remote Triton Inference Server model.

    This class provides a convenient interface for sending inference requests to a Triton Inference Server and
    processing the responses. Supports both HTTP and gRPC communication protocols.

    Attributes:
        endpoint (str): The name of the model on the Triton server.
        url (str): The URL of the Triton server.
        triton_client: The Triton client (either HTTP or gRPC).
        InferInput: The input class for the Triton client.
        InferRequestedOutput: The output request class for the Triton client.
        input_formats (list[str]): The data types of the model inputs.
        np_input_formats (list[type]): The numpy data types of the model inputs.
        input_names (list[str]): The names of the model inputs.
        output_names (list[str]): The names of the model outputs.
        metadata: The metadata associated with the model.

    Methods:
        __call__: Call the model with the given inputs and return the outputs.

    Examples:
        Initialize a Triton client with HTTP
        >>> model = TritonRemoteModel(url="localhost:8000", endpoint="yolov8", scheme="http")

        Make inference with numpy arrays
        >>> outputs = model(np.random.rand(1, 3, 640, 640).astype(np.float32))
    """

    def __init__(self, url: str, endpoint: str = "", scheme: str = ""):
        """Initialize the TritonRemoteModel for interacting with a remote Triton Inference Server.

        Arguments may be provided individually or parsed from a collective 'url' argument of the form
        <scheme>://<netloc>/<endpoint>/<task_name>

        Args:
            url (str): The URL of the Triton server.
            endpoint (str, optional): The name of the model on the Triton server.
            scheme (str, optional): The communication scheme ('http' or 'grpc').
        """
        if not endpoint and not scheme:  # Parse all args from URL string
            splits = urlsplit(url)
            endpoint = splits.path.strip("/").split("/", 1)[0]
            scheme = splits.scheme
            url = splits.netloc

        self.endpoint = endpoint
        self.url = url

        # Choose the Triton client based on the communication scheme
        if scheme == "http":
            import tritonclient.http as client

            self.triton_client = client.InferenceServerClient(url=self.url, verbose=False, ssl=False)
            config = self.triton_client.get_model_config(endpoint)
        else:
            import tritonclient.grpc as client

            self.triton_client = client.InferenceServerClient(url=self.url, verbose=False, ssl=False)
            config = self.triton_client.get_model_config(endpoint, as_json=True)["config"]

        # Sort output names alphabetically, i.e. 'output0', 'output1', etc.
        config["output"] = sorted(config["output"], key=lambda x: x.get("name"))

        # Define model attributes
        type_map = {"TYPE_FP32": np.float32, "TYPE_FP16": np.float16, "TYPE_UINT8": np.uint8}
        self.InferRequestedOutput = client.InferRequestedOutput
        self.InferInput = client.InferInput
        self.input_formats = [x["data_type"] for x in config["input"]]
        self.np_input_formats = [type_map[x] for x in self.input_formats]
        self.input_names = [x["name"] for x in config["input"]]
        self.output_names = [x["name"] for x in config["output"]]
        self.metadata = ast.literal_eval(config.get("parameters", {}).get("metadata", {}).get("string_value", "None"))
```

Example 4 (python):
```python
def __call__(self, *inputs: np.ndarray) -> list[np.ndarray]
```

---

## Reference for ultralytics/utils/ops.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/ops/

**Contents:**
- Reference for ultralytics/utils/ops.py
- class ultralytics.utils.ops.Profile
  - method ultralytics.utils.ops.Profile.__enter__
  - method ultralytics.utils.ops.Profile.__exit__
  - method ultralytics.utils.ops.Profile.__str__
  - method ultralytics.utils.ops.Profile.time
- function ultralytics.utils.ops.segment2box
- function ultralytics.utils.ops.scale_boxes
- function ultralytics.utils.ops.make_divisible
- function ultralytics.utils.ops.clip_boxes

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/ops.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Bases: contextlib.ContextDecorator

Ultralytics Profile class for timing code execution.

Use as a decorator with @Profile() or as a context manager with 'with Profile():'. Provides accurate timing measurements with CUDA synchronization support for GPU operations.

Return a human-readable string representing the accumulated elapsed time.

Get current time with CUDA synchronization if applicable.

Convert segment coordinates to bounding box coordinates.

Converts a single segment label to a box label by finding the minimum and maximum x and y coordinates. Applies inside-image constraint and clips coordinates when necessary.

Rescale bounding boxes from one image shape to another.

Rescales bounding boxes from img1_shape to img0_shape, accounting for padding and aspect ratio changes. Supports both xyxy and xywh box formats.

Return the nearest number that is divisible by the given divisor.

Clip bounding boxes to image boundaries.

Clip line coordinates to image boundaries.

Convert bounding box coordinates from (x1, y1, x2, y2) format to (x, y, width, height) format where (x1, y1) is

the top-left corner and (x2, y2) is the bottom-right corner.

Convert bounding box coordinates from (x, y, width, height) format to (x1, y1, x2, y2) format where (x1, y1) is

the top-left corner and (x2, y2) is the bottom-right corner. Note: ops per 2 channels faster than per channel.

Convert normalized bounding box coordinates to pixel coordinates.

Convert bounding box coordinates from (x1, y1, x2, y2) format to (x, y, width, height, normalized) format. x, y,

width and height are normalized to image dimensions.

Convert bounding box format from [x, y, w, h] to [x1, y1, w, h] where x1, y1 are top-left coordinates.

Convert bounding boxes from [x1, y1, x2, y2] to [x1, y1, w, h] format.

Convert bounding boxes from [x1, y1, w, h] to [x, y, w, h] where xy1=top-left, xy=center.

Convert batched Oriented Bounding Boxes (OBB) from [xy1, xy2, xy3, xy4] to [xywh, rotation] format.

Convert batched Oriented Bounding Boxes (OBB) from [xywh, rotation] to [xy1, xy2, xy3, xy4] format.

Convert bounding box from [x1, y1, w, h] to [x1, y1, x2, y2] where xy1=top-left, xy2=bottom-right.

Convert segment labels to box labels, i.e. (cls, xy1, xy2, ...) to (cls, xywh).

Resample segments to n points each using linear interpolation.

Crop masks to bounding box regions.

Apply masks to bounding boxes using mask head output.

Apply masks to bounding boxes using mask head output with native upsampling.

Rescale segment masks to target shape.

Rescale segment coordinates from img1_shape to img0_shape.

Regularize rotated bounding boxes to range [0, pi/2].

Convert masks to segments using contour detection.

Convert a batch of FP32 torch tensors to NumPy uint8 arrays, changing from BCHW to BHWC layout.

Clean a string by replacing special characters with '_' character.

Create empty torch.Tensor or np.ndarray with same shape as input and float32 dtype.

**Examples:**

Example 1 (typescript):
```typescript
Profile(self, t: float = 0.0, device: torch.device | None = None)
```

Example 2 (python):
```python
Use as a context manager to time code execution
>>> with Profile(device=device) as dt:
...     pass  # slow operation here
>>> print(dt)  # prints "Elapsed time is 9.5367431640625e-07 s"

Use as a decorator to time function execution
>>> @Profile()
... def slow_function():
...     time.sleep(0.1)
```

Example 3 (python):
```python
class Profile(contextlib.ContextDecorator):
    """Ultralytics Profile class for timing code execution.

    Use as a decorator with @Profile() or as a context manager with 'with Profile():'. Provides accurate timing
    measurements with CUDA synchronization support for GPU operations.

    Attributes:
        t (float): Accumulated time in seconds.
        device (torch.device): Device used for model inference.
        cuda (bool): Whether CUDA is being used for timing synchronization.

    Examples:
        Use as a context manager to time code execution
        >>> with Profile(device=device) as dt:
        ...     pass  # slow operation here
        >>> print(dt)  # prints "Elapsed time is 9.5367431640625e-07 s"

        Use as a decorator to time function execution
        >>> @Profile()
        ... def slow_function():
        ...     time.sleep(0.1)
    """

    def __init__(self, t: float = 0.0, device: torch.device | None = None):
        """Initialize the Profile class.

        Args:
            t (float): Initial accumulated time in seconds.
            device (torch.device, optional): Device used for model inference to enable CUDA synchronization.
        """
        self.t = t
        self.device = device
        self.cuda = bool(device and str(device).startswith("cuda"))
```

Example 4 (python):
```python
def __enter__(self)
```

---

## Reference for ultralytics/utils/checks.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/checks/

**Contents:**
- Reference for ultralytics/utils/checks.py
- function ultralytics.utils.checks.parse_requirements
- function ultralytics.utils.checks.parse_version
- function ultralytics.utils.checks.is_ascii
- function ultralytics.utils.checks.check_imgsz
- function ultralytics.utils.checks.check_uv
- function ultralytics.utils.checks.check_version
- function ultralytics.utils.checks.check_latest_pypi_version
- function ultralytics.utils.checks.check_pip_update_available
- function ultralytics.utils.checks.check_font

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/checks.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Parse a requirements.txt file, ignoring lines that start with '#' and any text after '#'.

Convert a version string to a tuple of integers, ignoring any extra non-numeric string attached to the version.

Check if a string is composed of only ASCII characters.

Verify image size is a multiple of the given stride in each dimension. If the image size is not a multiple of the

stride, update it to the nearest multiple of the stride that is greater than or equal to the given floor value.

Check if uv package manager is installed and can run successfully.

Check current version against the required version or range.

Return the latest version of a PyPI package without downloading or installing it.

Check if a new version of the ultralytics package is available on PyPI.

Find font locally or download to user's configuration directory if it does not already exist.

Check current python version against the required minimum version.

Check if apt packages are installed and install missing ones.

Check if installed dependencies meet Ultralytics YOLO models requirements and attempt to auto-update if needed.

Check the installed versions of PyTorch and Torchvision to ensure they're compatible.

This function checks the installed versions of PyTorch and Torchvision, and warns if they're incompatible according to the compatibility table based on: https://github.com/pytorch/vision#installation.

Check file(s) for acceptable suffix.

Replace legacy YOLOv5 filenames with updated YOLOv5u filenames.

Return a model filename from a valid model stem.

Search/download file (if necessary), check suffix (if provided), and return path.

Search/download YAML file (if necessary) and return path, checking suffix.

Check if the resolved path is under the intended directory to prevent path traversal.

Check if environment supports image displays.

Return a human-readable YOLO software and hardware summary.

Collect and print relevant system information including OS, Python, RAM, CPU, and CUDA.

Check the PyTorch Automatic Mixed Precision (AMP) functionality of a YOLO model.

If the checks fail, it means there are anomalies with AMP on the system that may cause NaN losses or zero-mAP results, so AMP will be disabled during training.

Check if there are multiple Ultralytics installations.

Print function arguments (optional args dict).

Get the number of NVIDIA GPUs available in the environment.

Check if CUDA is available in the environment.

Check if the current environment is running on a Rockchip SoC.

Check if the system has Intel hardware (CPU or GPU).

Check if the sudo command is available in the environment.

**Examples:**

Example 1 (python):
```python
def parse_requirements(file_path = ROOT.parent / "requirements.txt", package = "")
```

Example 2 (sql):
```sql
>>> from ultralytics.utils.checks import parse_requirements
>>> parse_requirements(package="ultralytics")
```

Example 3 (go):
```go
def parse_requirements(file_path=ROOT.parent / "requirements.txt", package=""):
    """Parse a requirements.txt file, ignoring lines that start with '#' and any text after '#'.

    Args:
        file_path (Path): Path to the requirements.txt file.
        package (str, optional): Python package to use instead of requirements.txt file.

    Returns:
        requirements (list[SimpleNamespace]): List of parsed requirements as SimpleNamespace objects with `name` and
            `specifier` attributes.

    Examples:
        >>> from ultralytics.utils.checks import parse_requirements
        >>> parse_requirements(package="ultralytics")
    """
    if package:
        requires = [x for x in metadata.distribution(package).requires if "extra == " not in x]
    else:
        requires = Path(file_path).read_text().splitlines()

    requirements = []
    for line in requires:
        line = line.strip()
        if line and not line.startswith("#"):
            line = line.partition("#")[0].strip()  # ignore inline comments
            if match := re.match(r"([a-zA-Z0-9-_]+)\s*([<>!=~]+.*)?", line):
                requirements.append(SimpleNamespace(name=match[1], specifier=match[2].strip() if match[2] else ""))

    return requirements
```

Example 4 (python):
```python
def parse_version(version = "0.0.0") -> tuple
```

---

## Reference for ultralytics/utils/git.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/git/

**Contents:**
- Reference for ultralytics/utils/git.py
- class ultralytics.utils.git.GitRepo
  - property ultralytics.utils.git.GitRepo.head
  - property ultralytics.utils.git.GitRepo.is_repo
  - property ultralytics.utils.git.GitRepo.branch
  - property ultralytics.utils.git.GitRepo.commit
  - property ultralytics.utils.git.GitRepo.origin
  - method ultralytics.utils.git.GitRepo._find_root
  - method ultralytics.utils.git.GitRepo._gitdir
  - method ultralytics.utils.git.GitRepo._read

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/git.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Represent a local Git repository and expose branch, commit, and remote metadata.

This class discovers the repository root by searching for a .git entry from the given path upward, resolves the actual .git directory (including worktrees), and reads Git metadata directly from on-disk files. It does not invoke the git binary and therefore works in restricted environments. All metadata properties are resolved lazily and cached; construct a new instance to refresh state.

True if inside a git repo.

Current branch or None.

Current commit SHA or None.

Return repo root or None.

Resolve actual .git directory (handles worktrees).

Read and strip file if exists.

Commit for ref (handles packed-refs).

**Examples:**

Example 1 (typescript):
```typescript
GitRepo(self, path: Path = Path(__file__).resolve())
```

Example 2 (python):
```python
Initialize from the current working directory and read metadata
>>> from pathlib import Path
>>> repo = GitRepo(Path.cwd())
>>> repo.is_repo
True
>>> repo.branch, repo.commit[:7], repo.origin
('main', '1a2b3c4', 'https://example.com/owner/repo.git')
```

Example 3 (python):
```python
class GitRepo:
    """Represent a local Git repository and expose branch, commit, and remote metadata.

    This class discovers the repository root by searching for a .git entry from the given path upward, resolves the
    actual .git directory (including worktrees), and reads Git metadata directly from on-disk files. It does not invoke
    the git binary and therefore works in restricted environments. All metadata properties are resolved lazily and
    cached; construct a new instance to refresh state.

    Attributes:
        root (Path | None): Repository root directory containing the .git entry; None if not in a repository.
        gitdir (Path | None): Resolved .git directory path; handles worktrees; None if unresolved.
        head (str | None): Raw contents of HEAD; a SHA for detached HEAD or "ref: <refname>" for branch heads.
        is_repo (bool): Whether the provided path resides inside a Git repository.
        branch (str | None): Current branch name when HEAD points to a branch; None for detached HEAD or non-repo.
        commit (str | None): Current commit SHA for HEAD; None if not determinable.
        origin (str | None): URL of the "origin" remote as read from gitdir/config; None if unset or unavailable.

    Examples:
        Initialize from the current working directory and read metadata
        >>> from pathlib import Path
        >>> repo = GitRepo(Path.cwd())
        >>> repo.is_repo
        True
        >>> repo.branch, repo.commit[:7], repo.origin
        ('main', '1a2b3c4', 'https://example.com/owner/repo.git')

    Notes:
        - Resolves metadata by reading files: HEAD, packed-refs, and config; no subprocess calls are used.
        - Caches properties on first access using cached_property; recreate the object to reflect repository changes.
    """

    def __init__(self, path: Path = Path(__file__).resolve()):
        """Initialize a Git repository context by discovering the repository root from a starting path.

        Args:
            path (Path, optional): File or directory path used as the starting point to locate the repository root.
        """
        self.root = self._find_root(path)
        self.gitdir = self._gitdir(self.root) if self.root else None
```

Example 4 (python):
```python
def head(self) -> str | None
```

---

## Reference for ultralytics/utils/logger.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/logger/

**Contents:**
- Reference for ultralytics/utils/logger.py
- class ultralytics.utils.logger.ConsoleLogger
  - method ultralytics.utils.logger.ConsoleLogger._flush_buffer
  - method ultralytics.utils.logger.ConsoleLogger._flush_worker
  - method ultralytics.utils.logger.ConsoleLogger._queue_log
  - method ultralytics.utils.logger.ConsoleLogger._write_destination
  - method ultralytics.utils.logger.ConsoleLogger.start_capture
  - method ultralytics.utils.logger.ConsoleLogger.stop_capture
- class ultralytics.utils.logger.SystemLogger
  - method ultralytics.utils.logger.SystemLogger._get_nvidia_metrics

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/logger.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Console output capture with batched streaming to file, API, or custom callback.

Captures stdout/stderr output and streams it with intelligent deduplication and configurable batching.

Flush buffered lines to destination and/or callback.

Background worker that flushes buffer periodically.

Queue console text with deduplication and timestamp processing.

Write content to file or API destination.

Start capturing console output and redirect stdout/stderr.

In DDP training, only activates on rank 0/-1 to prevent duplicate logging.

Stop capturing console output and flush remaining buffer.

Log dynamic system metrics for training monitoring.

Captures real-time system metrics including CPU, RAM, disk I/O, network I/O, and NVIDIA GPU statistics for training performance monitoring and analysis.

Get NVIDIA GPU metrics including utilization, memory, temperature, and power.

Initialize NVIDIA GPU monitoring with pynvml.

Get current system metrics including CPU, RAM, disk, network, and GPU usage.

Collects comprehensive system metrics including CPU usage, RAM usage, disk I/O statistics, network I/O statistics, and GPU metrics (if available).

Example output (rates=False, default):

Example output (rates=True):

**Examples:**

Example 1 (rust):
```rust
ConsoleLogger(self, destination = None, batch_size = 1, flush_interval = 5.0, on_flush = None)
```

Example 2 (python):
```python
File logging (immediate):
>>> logger = ConsoleLogger("training.log")
>>> logger.start_capture()
>>> print("This will be logged")
>>> logger.stop_capture()

API streaming with batching:
>>> logger = ConsoleLogger("https://api.example.com/logs", batch_size=10)
>>> logger.start_capture()

Custom callback with batching:
>>> def my_handler(content, line_count, chunk_id):
...     print(f"Received {line_count} lines")
>>> logger = ConsoleLogger(on_flush=my_handler, batch_size=5)
>>> logger.start_capture()
```

Example 3 (python):
```python
class ConsoleLogger:
    """Console output capture with batched streaming to file, API, or custom callback.

    Captures stdout/stderr output and streams it with intelligent deduplication and configurable batching.

    Attributes:
        destination (str | Path | None): Target destination for streaming (URL, Path, or None for callback-only).
        batch_size (int): Number of lines to batch before flushing (default: 1 for immediate).
        flush_interval (float): Seconds between automatic flushes (default: 5.0).
        on_flush (callable | None): Optional callback function called with batched content on flush.
        active (bool): Whether console capture is currently active.

    Examples:
        File logging (immediate):
        >>> logger = ConsoleLogger("training.log")
        >>> logger.start_capture()
        >>> print("This will be logged")
        >>> logger.stop_capture()

        API streaming with batching:
        >>> logger = ConsoleLogger("https://api.example.com/logs", batch_size=10)
        >>> logger.start_capture()

        Custom callback with batching:
        >>> def my_handler(content, line_count, chunk_id):
        ...     print(f"Received {line_count} lines")
        >>> logger = ConsoleLogger(on_flush=my_handler, batch_size=5)
        >>> logger.start_capture()
    """

    def __init__(self, destination=None, batch_size=1, flush_interval=5.0, on_flush=None):
        """Initialize console logger with optional batching.

        Args:
            destination (str | Path | None): API endpoint URL (http/https), local file path, or None.
            batch_size (int): Lines to accumulate before flush (1 = immediate, higher = batched).
            flush_interval (float): Max seconds between flushes when batching.
            on_flush (callable | None): Callback(content: str, line_count: int, chunk_id: int) for custom handling.
        """
        self.destination = destination
        self.is_api = isinstance(destination, str) and destination.startswith(("http://", "https://"))
        if destination is not None and not self.is_api:
            self.destination = Path(destination)

        # Batching configuration
        self.batch_size = max(1, batch_size)
        self.flush_interval = flush_interval
        self.on_flush = on_flush

        # Console capture state
        self.original_stdout = sys.stdout
        self.original_stderr = sys.stderr
        self.active = False
        self._log_handler = None  # Track handler for cleanup

        # Buffer for batching
        self.buffer = []
        self.buffer_lock = threading.Lock()
        self.flush_thread = None
        self.chunk_id = 0

        # Deduplication state
        self.last_line = ""
        self.last_time = 0.0
        self.last_progress_line = ""  # Track progress sequence key for deduplication
        self.last_was_progress = False  # Track if last line was a progress bar
```

Example 4 (python):
```python
def _flush_buffer(self)
```

---

## Reference for hub_sdk/helpers/error_handler.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/hub/sdk/reference/helpers/error_handler/

**Contents:**
- Reference for hub_sdk/helpers/error_handler.py
- class hub_sdk.helpers.error_handler.ErrorHandler
  - method hub_sdk.helpers.error_handler.ErrorHandler.get_default_message
  - method hub_sdk.helpers.error_handler.ErrorHandler.handle
  - method hub_sdk.helpers.error_handler.ErrorHandler.handle_internal_server_error
  - method hub_sdk.helpers.error_handler.ErrorHandler.handle_not_found
  - method hub_sdk.helpers.error_handler.ErrorHandler.handle_ratelimit_exceeded
  - method hub_sdk.helpers.error_handler.ErrorHandler.handle_unauthorized
  - method hub_sdk.helpers.error_handler.ErrorHandler.handle_unknown_error

This page is sourced from https://github.com/ultralytics/hub-sdk/blob/main/hub_sdk/helpers/error_handler.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Represents an error handler for managing HTTP status codes and error messages.

Get the default error message for a given HTTP status code.

Handle the error based on the provided status code.

Handle an internal server error (HTTP 500).

Handle a resource not found error (HTTP 404).

Handle rate limit exceeded error (HTTP 429).

Handle an unauthorized error (HTTP 401).

Handle an unknown error.

**Examples:**

Example 1 (rust):
```rust
ErrorHandler(self, status_code: int, message: str | None = None, headers: dict | None = None)
```

Example 2 (python):
```python
class ErrorHandler:
    """Represents an error handler for managing HTTP status codes and error messages.

    Attributes:
        status_code (int): The HTTP status code associated with the error.
        message (str | None): An optional error message providing additional details.
        headers (dict | None): An optional dictionary providing response headers details.

    Methods:
        handle: Handle the error based on the provided status code.
        handle_unauthorized: Handle an unauthorized error (HTTP 401).
        handle_ratelimit_exceeded: Handle rate limit exceeded error (HTTP 429).
        handle_not_found: Handle a resource not found error (HTTP 404).
        handle_internal_server_error: Handle an internal server error (HTTP 500).
        handle_unknown_error: Handle an unknown error.
        get_default_message: Get the default error message for a given HTTP status code.
    """

    def __init__(
        self,
        status_code: int,
        message: str | None = None,
        headers: dict | None = None,
    ):
        """Initialize the ErrorHandler object with a given status code.

        Args:
            status_code (int): The HTTP status code representing the error.
            message (str, optional): An optional error message providing additional details.
            headers (dict, optional): An optional dictionary providing response headers details.
        """
        self.status_code = status_code
        self.message = message
        self.headers = headers
```

Example 3 (python):
```python
def get_default_message(self) -> str
```

Example 4 (python):
```python
def get_default_message(self) -> str:
    """Get the default error message for a given HTTP status code.

    Returns:
        (str): The default error message associated with the provided status code or unknown error message if not
            found.
    """
    return http.client.responses.get(self.status_code, self.handle_unknown_error())
```

---

## Reference for ultralytics/utils/cpu.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/utils/cpu/

**Contents:**
- Reference for ultralytics/utils/cpu.py
- class ultralytics.utils.cpu.CPUInfo
  - method ultralytics.utils.cpu.CPUInfo.__str__
  - method ultralytics.utils.cpu.CPUInfo._clean
  - method ultralytics.utils.cpu.CPUInfo.name

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/cpu.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Provide cross-platform CPU brand and model information.

Query platform-specific sources to retrieve a human-readable CPU descriptor and normalize it for consistent presentation across macOS, Linux, and Windows. If platform-specific probing fails, generic platform identifiers are used to ensure a stable string is always returned.

Return the normalized CPU name.

Normalize and prettify a raw CPU descriptor string.

Return a normalized CPU model string from platform-specific sources.

**Examples:**

Example 1 (unknown):
```unknown
>>> CPUInfo.name()
'Apple M4 Pro'
>>> str(CPUInfo())
'Intel Core i7-9750H 2.60GHz'
```

Example 2 (python):
```python
class CPUInfo:
```

Example 3 (python):
```python
def __str__(self) -> str
```

Example 4 (python):
```python
def __str__(self) -> str:
    """Return the normalized CPU name."""
    return self.name()
```

---
