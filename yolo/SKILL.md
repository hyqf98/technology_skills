---
name: yolo
description: Use this skill when working with Ultralytics YOLO (You Only Look Once) - the latest real-time object detection and image segmentation models. Includes YOLO11, prediction, training, detection, segmentation, classification, pose estimation, object tracking, and SAM (Segment Anything Model).
---

# Yolo Skill

Comprehensive assistance with yolo development, generated from official documentation.

## When to Use This Skill

This skill should be triggered when:
- Working with yolo
- Asking about yolo features or APIs
- Implementing yolo solutions
- Debugging yolo code
- Learning yolo best practices

## Quick Reference

### Common Patterns

**Pattern 1:** # Example: Train YOLOv5s on a custom dataset for 100 epochs python train.py --img 640 --batch 16 --epochs 100 --data custom_dataset.yaml --weights yolov5s.pt

```
# Example: Train YOLOv5s on a custom dataset for 100 epochs
python train.py --img 640 --batch 16 --epochs 100 --data custom_dataset.yaml --weights yolov5s.pt
```

**Pattern 2:** import torch # Example: Loading YOLOv5s from PyTorch Hub for inference model = torch.hub.load("ultralytics/yolov5", "yolov5s", pretrained=True) # Inference on an image results = model("https://ultralytics.com/images/zidane.jpg") # Print results results.print()

```
import torch

# Example: Loading YOLOv5s from PyTorch Hub for inference
model = torch.hub.load("ultralytics/yolov5", "yolov5s", pretrained=True)

# Inference on an image
results = model("https://ultralytics.com/images/zidane.jpg")

# Print results
results.print()
```

**Pattern 3:** # Example: Train YOLOv5s on the COCO128 dataset for 3 epochs python train.py --img 640 --batch 16 --epochs 3 --data coco128.yaml --weights yolov5s.pt

```
# Example: Train YOLOv5s on the COCO128 dataset for 3 epochs
python train.py --img 640 --batch 16 --epochs 3 --data coco128.yaml --weights yolov5s.pt
```

**Pattern 4:** def __init__( self, im, line_width: int | None = None, font_size: int | None = None, font: str = "Arial.ttf", pil: bool = False, example: str = "abc", )

```
def __init__(
    self,
    im,
    line_width: int | None = None,
    font_size: int | None = None,
    font: str = "Arial.ttf",
    pil: bool = False,
    example: str = "abc",
)
```

**Pattern 5:** def __init__( self, im: np.ndarray, line_width: int | None = None, font_size: int | None = None, font: str = "Arial.ttf", pil: bool = False, example: str = "abc", )

```
def __init__(
    self,
    im: np.ndarray,
    line_width: int | None = None,
    font_size: int | None = None,
    font: str = "Arial.ttf",
    pil: bool = False,
    example: str = "abc",
)
```

**Pattern 6:** %%bash source activate yolov5env # Activate environment within the cell # Example: Run validation using the activated environment python val.py --weights yolov5s.pt --data coco128.yaml --img 640

```
%%bash
source activate yolov5env # Activate environment within the cell

# Example: Run validation using the activated environment
python val.py --weights yolov5s.pt --data coco128.yaml --img 640
```

### Example Code Patterns

**Example 1** (sql):
```sql
sudo apt-get update
```

**Example 2** (markdown):
```markdown
# Clone the YOLOv5 repository
git clone https://github.com/ultralytics/yolov5
cd yolov5

# Install dependencies
pip install -r requirements.txt
```

**Example 3** (rust):
```rust
Events(self) -> None
```

**Example 4** (rust):
```rust
FastSAMPredictor(self, cfg = DEFAULT_CFG, overrides = None, _callbacks = None)
```

**Example 5** (rust):
```rust
SegmentationValidator(self, dataloader = None, save_dir = None, args = None, _callbacks = None) -> None
```

## Reference Files

This skill includes comprehensive documentation in `references/`:

- **advanced.md** - Advanced documentation
- **classification.md** - Classification documentation
- **configuration.md** - Configuration documentation
- **datasets.md** - Datasets documentation
- **deployment.md** - Deployment documentation
- **detection.md** - Detection documentation
- **getting_started.md** - Getting Started documentation
- **guides.md** - Guides documentation
- **models.md** - Models documentation
- **other.md** - Other documentation
- **prediction.md** - Prediction documentation
- **segmentation.md** - Segmentation documentation
- **tracking.md** - Tracking documentation
- **training.md** - Training documentation
- **utils.md** - Utils documentation

Use `view` to read specific reference files when detailed information is needed.

## Working with This Skill

### For Beginners
Start with the getting_started or tutorials reference files for foundational concepts.

### For Specific Features
Use the appropriate category reference file (api, guides, etc.) for detailed information.

### For Code Examples
The quick reference section above contains common patterns extracted from the official docs.

## Resources

### references/
Organized documentation extracted from official sources. These files contain:
- Detailed explanations
- Code examples with language annotations
- Links to original documentation
- Table of contents for quick navigation

### scripts/
Add helper scripts here for common automation tasks.

### assets/
Add templates, boilerplate, or example projects here.

## Notes

- This skill was automatically generated from official documentation
- Reference files preserve the structure and examples from source docs
- Code examples include language detection for better syntax highlighting
- Quick reference patterns are extracted from common usage examples in the docs

## Updating

To refresh this skill with updated documentation:
1. Re-run the scraper with the same configuration
2. The skill will be rebuilt with the latest information
