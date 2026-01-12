# Yolo - Classification

**Pages:** 2

---

## Reference for ultralytics/data/split.py - Ultralytics YOLO Docs

**URL:** https://docs.ultralytics.com/zh/reference/data/split/

**Contents:**
- Reference for ultralytics/data/split.py
- function ultralytics.data.split.split_classify_dataset
- function ultralytics.data.split.autosplit

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/data/split.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

Split classification dataset into train and val directories in a new directory.

Creates a new directory '{source_dir}_split' with train/val subdirectories, preserving the original class structure with an 80/20 split by default.

Directory structure: Before: caltech/ ├── class1/ │ ├── img1.jpg │ ├── img2.jpg │ └── ... ├── class2/ │ ├── img1.jpg │ └── ... └── ...

Automatically split a dataset into train/val/test splits and save the resulting splits into autosplit_*.txt

**Examples:**

Example 1 (typescript):
```typescript
def split_classify_dataset(source_dir: str | Path, train_ratio: float = 0.8) -> Path
```

Example 2 (yaml):
```yaml
After:
    caltech_split/
    ├── train/
    │   ├── class1/
    │   │   ├── img1.jpg
    │   │   └── ...
    │   ├── class2/
    │   │   ├── img1.jpg
    │   │   └── ...
    │   └── ...
    └── val/
        ├── class1/
        │   ├── img2.jpg
        │   └── ...
        ├── class2/
        │   └── ...
        └── ...
```

Example 3 (unknown):
```unknown
Split dataset with default 80/20 ratio
>>> split_classify_dataset("path/to/caltech")

Split with custom ratio
>>> split_classify_dataset("path/to/caltech", 0.75)
```

Example 4 (php):
```php
def split_classify_dataset(source_dir: str | Path, train_ratio: float = 0.8) -> Path:
    """Split classification dataset into train and val directories in a new directory.

    Creates a new directory '{source_dir}_split' with train/val subdirectories, preserving the original class structure
    with an 80/20 split by default.

    Directory structure:
        Before:
            caltech/
            ├── class1/
            │   ├── img1.jpg
            │   ├── img2.jpg
            │   └── ...
            ├── class2/
            │   ├── img1.jpg
            │   └── ...
            └── ...

        After:
            caltech_split/
            ├── train/
            │   ├── class1/
            │   │   ├── img1.jpg
            │   │   └── ...
            │   ├── class2/
            │   │   ├── img1.jpg
            │   │   └── ...
            │   └── ...
            └── val/
                ├── class1/
                │   ├── img2.jpg
                │   └── ...
                ├── class2/
                │   └── ...
                └── ...

    Args:
        source_dir (str | Path): Path to classification dataset root directory.
        train_ratio (float): Ratio for train split, between 0 and 1.

    Returns:
        (Path): Path to the created split directory.

    Examples:
        Split dataset with default 80/20 ratio
        >>> split_classify_dataset("path/to/caltech")

        Split with custom ratio
        >>> split_classify_dataset("path/to/caltech", 0.75)
    """
    source_path = Path(source_dir)
    split_path = Path(f"{source_path}_split")
    train_path, val_path = split_path / "train", split_path / "val"

    # Create directory structure
    split_path.mkdir(exist_ok=True)
    train_path.mkdir(exist_ok=True)
    val_path.mkdir(exist_ok=True)

    # Process class directories
    class_dirs = [d for d in source_path.iterdir() if d.is_dir()]
    total_images = sum(len(list(d.glob("*.*"))) for d in class_dirs)
    stats = f"{len(class_dirs)} classes, {total_images} images"
    LOGGER.info(f"Splitting {source_path} ({stats}) into {train_ratio:.0%} train, {1 - train_ratio:.0%} val...")

    for class_dir in class_dirs:
        # Create class directories
        (train_path / class_dir.name).mkdir(exist_ok=True)
        (val_path / class_dir.name).mkdir(exist_ok=True)

        # Split and copy files
        image_files = list(class_dir.glob("*.*"))
        random.shuffle(image_files)
        split_idx = int(len(image_files) * train_ratio)

        for img in image_files[:split_idx]:
            shutil.copy2(img, train_path / class_dir.name / img.name)

        for img in image_files[split_idx:]:
            shutil.copy2(img, val_path / class_dir.name / img.name)

    LOGGER.info(f"Split complete in {split_path} ✅")
    return split_path
```

---

## Reference for ultralytics/hub/google/__init__.py

**URL:** https://docs.ultralytics.com/zh/reference/hub/google/__init__/

**Contents:**
- Reference for ultralytics/hub/google/__init__.py
- class ultralytics.hub.google.GCPRegions
  - method ultralytics.hub.google.GCPRegions._ping_region
  - method ultralytics.hub.google.GCPRegions.lowest_latency
  - method ultralytics.hub.google.GCPRegions.tier1
  - method ultralytics.hub.google.GCPRegions.tier2

This page is sourced from https://github.com/ultralytics/ultralytics/blob/main/ultralytics/hub/google/__init__.py. Have an improvement or example to add? Open a Pull Request — thank you! 🙏

A class for managing and analyzing Google Cloud Platform (GCP) regions.

This class provides functionality to initialize, categorize, and analyze GCP regions based on their geographical location, tier classification, and network latency.

Ping a specified GCP region and measure network latency statistics.

Determine the GCP regions with the lowest latency based on ping tests.

Return a list of GCP regions classified as tier 1 based on predefined criteria.

Return a list of GCP regions classified as tier 2 based on predefined criteria.

**Examples:**

Example 1 (unknown):
```unknown
GCPRegions(self)
```

Example 2 (python):
```python
>>> from ultralytics.hub.google import GCPRegions
>>> regions = GCPRegions()
>>> lowest_latency_region = regions.lowest_latency(verbose=True, attempts=3)
>>> print(f"Lowest latency region: {lowest_latency_region[0][0]}")
```

Example 3 (python):
```python
class GCPRegions:
    """A class for managing and analyzing Google Cloud Platform (GCP) regions.

    This class provides functionality to initialize, categorize, and analyze GCP regions based on their geographical
    location, tier classification, and network latency.

    Attributes:
        regions (dict[str, tuple[int, str, str]]): A dictionary of GCP regions with their tier, city, and country.

    Methods:
        tier1: Returns a list of tier 1 GCP regions.
        tier2: Returns a list of tier 2 GCP regions.
        lowest_latency: Determines the GCP region(s) with the lowest network latency.

    Examples:
        >>> from ultralytics.hub.google import GCPRegions
        >>> regions = GCPRegions()
        >>> lowest_latency_region = regions.lowest_latency(verbose=True, attempts=3)
        >>> print(f"Lowest latency region: {lowest_latency_region[0][0]}")
    """

    def __init__(self):
        """Initialize the GCPRegions class with predefined Google Cloud Platform regions and their details."""
        self.regions = {
            "asia-east1": (1, "Taiwan", "China"),
            "asia-east2": (2, "Hong Kong", "China"),
            "asia-northeast1": (1, "Tokyo", "Japan"),
            "asia-northeast2": (1, "Osaka", "Japan"),
            "asia-northeast3": (2, "Seoul", "South Korea"),
            "asia-south1": (2, "Mumbai", "India"),
            "asia-south2": (2, "Delhi", "India"),
            "asia-southeast1": (2, "Jurong West", "Singapore"),
            "asia-southeast2": (2, "Jakarta", "Indonesia"),
            "australia-southeast1": (2, "Sydney", "Australia"),
            "australia-southeast2": (2, "Melbourne", "Australia"),
            "europe-central2": (2, "Warsaw", "Poland"),
            "europe-north1": (1, "Hamina", "Finland"),
            "europe-southwest1": (1, "Madrid", "Spain"),
            "europe-west1": (1, "St. Ghislain", "Belgium"),
            "europe-west10": (2, "Berlin", "Germany"),
            "europe-west12": (2, "Turin", "Italy"),
            "europe-west2": (2, "London", "United Kingdom"),
            "europe-west3": (2, "Frankfurt", "Germany"),
            "europe-west4": (1, "Eemshaven", "Netherlands"),
            "europe-west6": (2, "Zurich", "Switzerland"),
            "europe-west8": (1, "Milan", "Italy"),
            "europe-west9": (1, "Paris", "France"),
            "me-central1": (2, "Doha", "Qatar"),
            "me-west1": (1, "Tel Aviv", "Israel"),
            "northamerica-northeast1": (2, "Montreal", "Canada"),
            "northamerica-northeast2": (2, "Toronto", "Canada"),
            "southamerica-east1": (2, "São Paulo", "Brazil"),
            "southamerica-west1": (2, "Santiago", "Chile"),
            "us-central1": (1, "Iowa", "United States"),
            "us-east1": (1, "South Carolina", "United States"),
            "us-east4": (1, "Northern Virginia", "United States"),
            "us-east5": (1, "Columbus", "United States"),
            "us-south1": (1, "Dallas", "United States"),
            "us-west1": (1, "Oregon", "United States"),
            "us-west2": (2, "Los Angeles", "United States"),
            "us-west3": (2, "Salt Lake City", "United States"),
            "us-west4": (2, "Las Vegas", "United States"),
        }
```

Example 4 (typescript):
```typescript
def _ping_region(region: str, attempts: int = 1) -> tuple[str, float, float, float, float]
```

---
