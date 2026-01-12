# Yolo - Configuration

**Pages:** 3

---

## Reference for hub_sdk/helpers/logger.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/hub/sdk/reference/helpers/logger/

**Contents:**
- Reference for hub_sdk/helpers/logger.py
- class hub_sdk.helpers.logger.Logger
  - method hub_sdk.helpers.logger.Logger._configure_logger
  - method hub_sdk.helpers.logger.Logger.get_logger

This page is sourced from https://github.com/ultralytics/hub-sdk/blob/main/hub_sdk/helpers/logger.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Represents a logger configuration for handling log messages.

Configure the logger with the provided settings.

Get the configured logger instance.

**Examples:**

Example 1 (rust):
```rust
Logger(self, logger_name = None, log_format = None, log_level = None)
```

Example 2 (python):
```python
class Logger:
    """Represents a logger configuration for handling log messages.

    Attributes:
        logger_name (str): Name of the logger. Defaults to the name of the calling module.
        log_format (str): Format for log messages. Defaults to the value of 'LOGGER_FORMAT' environment variable or
            '%(asctime)s - %(name)s - %(levelname)s - %(message)s'.
        log_level (str): Log level for the logger. Defaults to the value of 'LOGGER_LEVEL' environment variable or
            'INFO'.
        logger (logging.Logger): The configured logger instance.
    """

    def __init__(self, logger_name=None, log_format=None, log_level=None):
        """Initialize a Logger instance.

        Args:
            logger_name (str, optional): Name of the logger. If not provided, defaults to the root logger.
            log_format (str, optional): Format for log messages. Defaults to the value of 'LOGGER_FORMAT' environment
                variable or '%(asctime)s - %(name)s - %(levelname)s - %(message)s'.
            log_level (str, optional): Log level for the logger. Defaults to the value of 'LOGGER_LEVEL' environment
                variable or 'INFO'.
        """
        self.log_format = log_format or os.environ.get(
            "LOGGER_FORMAT", "%(asctime)s - %(name)s - %(levelname)s - %(message)s"
        )
        self.log_level = log_level or os.environ.get("LOGGER_LEVEL", "INFO")
        self.logger_name = logger_name or __name__

        self.logger = self._configure_logger()
```

Example 3 (python):
```python
def _configure_logger(self) -> logging.Logger
```

Example 4 (python):
```python
def _configure_logger(self) -> logging.Logger:
    """Configure the logger with the provided settings.

    Returns:
        (logging.Logger): A configured logger instance.
    """
    logger = logging.getLogger(self.logger_name)
    logger.setLevel(self.log_level)

    formatter = logging.Formatter(self.log_format)

    handler = logging.StreamHandler()
    handler.setFormatter(formatter)

    logger.addHandler(handler)
    return logger
```

---

## Reference for ultralytics/solutions/config.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/solutions/config/

**Contents:**
- Reference for ultralytics/solutions/config.py
- class ultralytics.solutions.config.SolutionConfig
  - method ultralytics.solutions.config.SolutionConfig.update

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/solutions/config.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Manages configuration parameters for Ultralytics Vision AI solutions.

The SolutionConfig class serves as a centralized configuration container for all the Ultralytics solution modules: https://docs.ultralytics.com/solutions/#solutions. It leverages Python dataclass for clear, type-safe, and maintainable parameter definitions.

Update configuration parameters with new values provided as keyword arguments.

**Examples:**

Example 1 (unknown):
```unknown
SolutionConfig()
```

Example 2 (python):
```python
>>> from ultralytics.solutions.config import SolutionConfig
>>> cfg = SolutionConfig(model="yolo11n.pt", region=[(0, 0), (100, 0), (100, 100), (0, 100)])
>>> cfg.update(show=False, conf=0.3)
>>> print(cfg.model)
```

Example 3 (python):
```python
@dataclass
class SolutionConfig:
```

Example 4 (python):
```python
def update(self, **kwargs: Any)
```

---

## Reference for ultralytics/cfg/__init__.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/cfg/__init__/

**Contents:**
- Reference for ultralytics/cfg/__init__.py
- function ultralytics.cfg.cfg2dict
- function ultralytics.cfg.get_cfg
- function ultralytics.cfg.check_cfg
- function ultralytics.cfg.get_save_dir
- function ultralytics.cfg._handle_deprecation
- function ultralytics.cfg.check_dict_alignment
- function ultralytics.cfg.merge_equals_args
- function ultralytics.cfg.handle_yolo_hub
- function ultralytics.cfg.handle_yolo_settings

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/cfg/__init__.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Convert a configuration object to a dictionary.

Load and merge configuration data from a file or dictionary, with optional overrides.

Check configuration argument types and values for the Ultralytics library.

This function validates the types and values of configuration arguments, ensuring correctness and converting them if necessary. It checks for specific key types defined in global variables such as CFG_FLOAT_KEYS, CFG_FRACTION_KEYS, CFG_INT_KEYS, and CFG_BOOL_KEYS.

Return the directory path for saving outputs, derived from arguments or default settings.

Handle deprecated configuration keys by mapping them to current equivalents with deprecation warnings.

This function modifies the input dictionary in-place, replacing deprecated keys with their current equivalents. It also handles value conversions where necessary, such as inverting boolean values for 'hide_labels' and 'hide_conf'.

Check alignment between custom and base configuration dictionaries, handling deprecated keys and providing error

messages for mismatched keys.

Merge arguments around isolated '=' in a list of strings and join fragments with brackets.

This function handles the following cases: 1. ['arg', '=', 'val'] becomes ['arg=val'] 2. ['arg=', 'val'] becomes ['arg=val'] 3. ['arg', '=val'] becomes ['arg=val'] 4. Joins fragments with brackets, e.g., ['imgsz=[3,', '640,', '640]'] becomes ['imgsz=[3,640,640]']

Handle Ultralytics HUB command-line interface (CLI) commands for authentication.

This function processes Ultralytics HUB CLI commands such as login and logout. It should be called when executing a script with arguments related to HUB authentication.

Handle YOLO settings command-line interface (CLI) commands.

This function processes YOLO settings CLI commands such as reset and updating individual settings. It should be called when executing a script with arguments related to YOLO settings management.

Process YOLO solutions arguments and run the specified computer vision solutions pipeline.

Parse a key-value pair string into separate key and value components.

Convert a string representation of a value to its appropriate Python type.

This function attempts to convert a given string into a Python object of the most appropriate type. It handles conversions to None, bool, int, float, and other types that can be evaluated safely.

Ultralytics entrypoint function for parsing and executing command-line arguments.

This function serves as the main entry point for the Ultralytics CLI, parsing command-line arguments and executing the corresponding tasks such as training, validation, prediction, exporting models, and more.

Copy the default configuration file and create a new one with '_copy' appended to its name.

This function duplicates the existing default configuration file (DEFAULT_CFG_PATH) and saves it with '_copy' appended to its name in the current working directory. It provides a convenient way to create a custom configuration file based on the default settings.

**Examples:**

Example 1 (python):
```python
def cfg2dict(cfg: str | Path | dict | SimpleNamespace) -> dict
```

Example 2 (python):
```python
Convert a YAML file path to a dictionary:
>>> config_dict = cfg2dict("config.yaml")

Convert a SimpleNamespace to a dictionary:
>>> from types import SimpleNamespace
>>> config_sn = SimpleNamespace(param1="value1", param2="value2")
>>> config_dict = cfg2dict(config_sn)

Pass through an already existing dictionary:
>>> config_dict = cfg2dict({"param1": "value1", "param2": "value2"})
```

Example 3 (python):
```python
def cfg2dict(cfg: str | Path | dict | SimpleNamespace) -> dict:
    """Convert a configuration object to a dictionary.

    Args:
        cfg (str | Path | dict | SimpleNamespace): Configuration object to be converted. Can be a file path, a string, a
            dictionary, or a SimpleNamespace object.

    Returns:
        (dict): Configuration object in dictionary format.

    Examples:
        Convert a YAML file path to a dictionary:
        >>> config_dict = cfg2dict("config.yaml")

        Convert a SimpleNamespace to a dictionary:
        >>> from types import SimpleNamespace
        >>> config_sn = SimpleNamespace(param1="value1", param2="value2")
        >>> config_dict = cfg2dict(config_sn)

        Pass through an already existing dictionary:
        >>> config_dict = cfg2dict({"param1": "value1", "param2": "value2"})

    Notes:
        - If cfg is a path or string, it's loaded as YAML and converted to a dictionary.
        - If cfg is a SimpleNamespace object, it's converted to a dictionary using vars().
        - If cfg is already a dictionary, it's returned unchanged.
    """
    if isinstance(cfg, STR_OR_PATH):
        cfg = YAML.load(cfg)  # load dict
    elif isinstance(cfg, SimpleNamespace):
        cfg = vars(cfg)  # convert to dict
    return cfg
```

Example 4 (python):
```python
def get_cfg(
    cfg: str | Path | dict | SimpleNamespace = DEFAULT_CFG_DICT, overrides: dict | None = None
) -> SimpleNamespace
```

---
