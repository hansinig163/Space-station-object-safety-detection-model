# Space Station Safety Object Detection
## Duality AI Challenge #2 - Submission

---

## 📋 Table of Contents
1. [Project Overview](#project-overview)
2. [Installation & Setup](#installation--setup)
3. [Quick Start](#quick-start)
4. [File Structure](#file-structure)
5. [Model Information](#model-information)
6. [Running Training](#running-training)
7. [Running Inference](#running-inference)
8. [Reproducing Results](#reproducing-results)
9. [Performance Metrics](#performance-metrics)
10. [Troubleshooting](#troubleshooting)

---

## Project Overview

This project implements a **YOLOv8-based object detection system** designed to detect 7 critical safety objects in space station environments:

- **OxygenTank** (ID: 0)
- **NitrogenTank** (ID: 1)
- **FirstAidBox** (ID: 2)
- **FireAlarm** (ID: 3)
- **SafetySwitchPanel** (ID: 4)
- **EmergencyPhone** (ID: 5)
- **FireExtinguisher** (ID: 6)

### Key Features
✅ Real-time detection with Streamlit UI  
✅ CPU-optimized training (no GPU required)  
✅ Batch inference capability  
✅ Live training monitoring with logs  
✅ Comprehensive evaluation metrics  

---

## Installation & Setup

### Prerequisites
- **Python 3.9+** (tested on Python 3.13)
- **Windows/Linux/macOS**
- **4GB+ RAM** (8GB+ recommended)

### Step 1: Create Virtual Environment
```bash
cd submission
python -m venv yoloenv
```

### Step 2: Activate Virtual Environment

**Windows:**
```bash
yoloenv\Scripts\activate
```

**Linux/macOS:**
```bash
source yoloenv/bin/activate
```

### Step 3: Install Dependencies
```bash
pip install -r requirements.txt
```

**Note:** If using GPU (CUDA), replace `torch+cpu` with `torch+cu118` or appropriate CUDA version.

---

## Quick Start

### 1. Activate Virtual Environment

**Windows (PowerShell):**
```powershell
& ./yoloenv/Scripts/Activate.ps1
cd submission
```

**Linux/macOS:**
```bash
source yoloenv/bin/activate
cd submission
```

### 2. Launch Interactive Dashboard
```bash
python -m streamlit run streamlit_app.py
```

Then open: **http://localhost:8501**

### 3. Load Model & Run Detection
1. In sidebar, click **"Load Model"**
   - First time: Auto-downloads yolov8m.pt (~52MB)
   - Wait for "Loaded!" message
2. Choose a tab:
   - **Upload**: Upload a single image
   - **Test**: Run on validation dataset
   - **Batch**: Process multiple images
3. Adjust confidence threshold and view results

### 4. Start Training (Optional)
1. Click **"Start"** under Training in sidebar
2. Monitor logs in **"Logs"** tab
3. Trained model saves to: `runs/detect/train/weights/best.pt`

---

## File Structure

```
submission/
├── README.md                      # This file
├── QUICKSTART.md                  # Quick reference guide
├── requirements.txt               # Python dependencies
├── config.yaml                    # Training configuration
├── streamlit_app.py               # Interactive web dashboard
├── train_yolo.py                  # Training pipeline
│
├── scripts/
│   ├── train.py                   # Training module
│   ├── predict.py                 # Inference module
│   └── validate.py                # Validation module
│
├── train_2/
│   ├── train2/
│   │   ├── images/                # 1,767 training images
│   │   └── labels/                # Training annotations (YOLO format)
│   └── val2/
│       ├── images/                # 336 validation images
│       └── labels/                # Validation annotations
│
├── docs/
│   ├── METHODOLOGY.md             # Technical approach
│   ├── CHALLENGES_SOLUTIONS.md    # Issues & fixes
│   ├── PERFORMANCE_REPORT.pdf     # Results analysis
│   └── USE_CASE_APPLICATION.pdf   # Application details
│
└── runs/detect/train/             # Training outputs
    ├── weights/
    │   ├── best.pt                # Best model
    │   └── last.pt                # Last checkpoint
    ├── train.log                  # Training logs
    └── results.csv                # Metrics
```

---

## Model Information

### Architecture
- **Model Name:** YOLOv8m (Medium)
- **Pretrained:** COCO-pretrained weights (ImageNet backbone)
- **Classes:** 7 safety objects
- **Input Size:** 640×640 pixels
- **Framework:** PyTorch + Ultralytics

### Training Configuration
```yaml
Epochs: 100
Batch Size: 8
Device: CPU
Image Size: 640×640
Optimizer: SGD (lr=0.01, momentum=0.937)
Early Stopping: Patience=20 epochs
Augmentation: Mosaic, Flip, HSV, Rotation
Workers: 0 (Windows optimization)
```

### Dataset Split
- **Training:** 1,767 images (88%)
- **Validation:** 336 images (12%)
- **Total Classes:** 7

---

## Running Training

### Option 1: Via Streamlit Dashboard
```bash
streamlit run streamlit_app.py
```
Then click **"Start"** under Training section in sidebar.

### Option 2: Command Line
```bash
python train_yolo.py
```

### Training Parameters
- **Epochs**: 100
- **Batch Size**: 8 (CPU-optimized, reduce if OOM)
- **Device**: CPU (set to `0` for GPU if available)
- **Image Size**: 640×640
- **Optimizer**: SGD (learning rate: 0.01)

### Expected Output
- Trained model: `runs/detect/train/weights/best.pt`
- Training logs: `runs/detect/train/train.log`
- Metrics CSV: `runs/detect/train/results.csv`

### Training Time
- **CPU**: ~30-40 hours for 100 epochs
- **GPU (CUDA)**: ~2-4 hours

---

## Running Inference

### Via Streamlit Dashboard (Recommended)
1. Open: **http://localhost:8501**
2. Load model (sidebar → "Load Model")
3. Upload image or select test images
4. View detections with confidence scores

### Standalone Prediction Script
```bash
python scripts/predict.py --image path/to/image.jpg --model yolov8m.pt --conf 0.5
```

### Batch Inference
```bash
python scripts/predict.py --source path/to/images/ --model yolov8m.pt --batch 8
```

---

## Reproducing Results

### Full Training from Scratch
```bash
# 1. Ensure dataset is present at train_2/train2/
# 2. Run training
python scripts/train.py --epochs 100

# 3. Evaluate model
python scripts/evaluation_metrics.py --model model/best.pt

# 4. Generate report
python scripts/generate_report.py
```

### Expected Timeline
- **CPU Training:** ~30-40 hours for 100 epochs
- **GPU Training (CUDA):** ~2-4 hours for 100 epochs
- **Dataset Loading:** ~2-3 minutes
- **Single Image Inference:** ~2-3 seconds (CPU), ~0.2 seconds (GPU)

### Expected mAP Score (Approximate)
- **mAP@0.5:** 0.55-0.70 (depends on training time & augmentation)
- **mAP@0.5:0.95:** 0.30-0.45

---

## Performance Metrics

### Key Metrics
| Metric | Value |
|--------|-------|
| mAP@0.5 | See `PERFORMANCE_REPORT.pdf` |
| Precision | See `PERFORMANCE_REPORT.pdf` |
| Recall | See `PERFORMANCE_REPORT.pdf` |
| F1-Score | See `PERFORMANCE_REPORT.pdf` |
| Inference Speed (CPU) | ~2-3 sec/image |
| Model Size | ~49 MB |
| Parameters | ~25.8M |

### View Detailed Metrics
```bash
# Generate evaluation report
python scripts/evaluation_metrics.py --model model/best.pt

# View artifacts
artifacts/
├── training_curves.png      # Loss curves
├── confusion_matrix.png     # Per-class performance
└── failure_cases.png        # Challenging detections
```

---

## Troubleshooting

### Model Loading Fails
**Error**: "Not found: yolov8m.pt"  
**Solution**: Click "Load Model" - it will auto-download the first time (~52MB)

### Streamlit Shows Blank Page
**Solution**: Refresh browser (F5) and ensure model is loaded

### Streamlit Won't Start
```powershell
# Ensure venv is activated
& ./yoloenv/Scripts/Activate.ps1

# Clear cache
Remove-Item -Path .streamlit/cache -Recurse -Force -ErrorAction SilentlyContinue

# Restart app
python -m streamlit run streamlit_app.py
```

### Out of Memory During Training
**Solution**: Reduce batch size in `config.yaml`:
```yaml
batch: 4  # or 2 for very limited RAM
```

### Training Too Slow (CPU)
**Solution**: Use GPU if available. Install CUDA PyTorch:
```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

### Dataset Not Found
**Solution**: Ensure `train_2/train2/` and `train_2/val2/` exist with images and labels folders

### ImportError: No module named 'cv2'
**Solution**: Reinstall dependencies:
```powershell
pip install -r requirements.txt
```

---

## Additional Resources

- **Ultralytics Docs:** https://docs.ultralytics.com
- **YOLOv8 Guide:** https://github.com/ultralytics/ultralytics
- **YOLO Paper:** https://arxiv.org/abs/2312.10687

---

## Contact & Support

For issues or questions, refer to:
- `METHODOLOGY.md` - Technical approach
- `CHALLENGES_SOLUTIONS.md` - Common issues
- `PERFORMANCE_REPORT.pdf` - Results & analysis

---

**Last Updated:** November 26, 2025  
**Challenge:** Duality AI Challenge #2 - Space Station Safety Detection  
**Status:** ✅ Complete and ready for submission
