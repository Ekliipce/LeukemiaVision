# LeukoCare AI 🔬

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-red.svg)](https://pytorch.org/)

> **Early leukemia detection through explainable computer vision**

An AI-powered system that detects leukemia cells from microscopic blood images and provides interpretable explanations for medical professionals. This project combines state-of-the-art deep learning with clinical explainability to support early diagnosis.

## 🎯 Project Goal

Early detection of leukemia is critical for successful treatment. This project aims to:

1. **Detect** leukemia cells from microscopic images with high accuracy
2. **Explain** which visual features the AI model uses for classification (Grad-CAM heatmaps)
3. **Communicate** findings in natural language through LLM-generated medical explanations

The system is designed to be a **decision support tool** for hematologists, not a replacement for medical expertise.

---

## ✨ Key Features

- 🧠 **Deep Learning Classification**: ResNet/EfficientNet/ViT architectures for cell classification
- 🔍 **Visual Explainability**: Grad-CAM heatmaps highlighting relevant cellular features
- 💬 **Natural Language Explanations**: LLM-powered descriptions of diagnostic reasoning
- 📊 **Medical-Grade Metrics**: Precision, Recall, F1-Score, AUC-ROC optimized for clinical use
- 🎨 **Interactive Demo**: Streamlit/Gradio interface for easy testing

---



## 📁 Project Structure
```
leukocare-ai/
├── data/                      # Dataset storage
├── notebooks/                 # Jupyter notebooks for exploration
│   ├── 01_eda.ipynb          # Data exploration
│   ├── 02_preprocessing.ipynb
│   ├── 03_modeling.ipynb
│   └── 04_explainability.ipynb
├── src/                       # Source code (production-ready)
│   ├── data/                 # Data loading and augmentation
│   ├── models/               # Model architectures
│   ├── training/             # Training loops and metrics
│   ├── explainability/       # Grad-CAM, SHAP, visualization
│   └── llm/                  # LLM explanation generation
├── scripts/                   # Executable scripts
│   ├── train.py              # Training script
│   ├── evaluate.py           # Model evaluation
│   └── inference.py          # Single image inference
├── configs/                   # Configuration files
├── tests/                     # Unit tests
└── outputs/                   # Model checkpoints and results
```



. **Vision Transformer (ViT)**: State-of-the-art attention mechanism

### Training Strategy

- **Transfer Learning**: Pre-trained on ImageNet
- **Fine-tuning**: All layers unfrozen after initial training
- **Loss Function**: Focal Loss (handles class imbalance)
- **Optimizer**: Adam with cosine annealing
- **Augmentation**: Medical-specific (rotations, color jitter, no vertical flips)

---

## 🔍 Explainability

### Visual Explanations

**Grad-CAM (Gradient-weighted Class Activation Mapping)**

Generates heatmaps showing which regions of the cell image influenced the model's decision.
```python
from src.explainability.gradcam import GradCAM

# Generate explanation
cam = GradCAM(model, target_layer='layer4')
heatmap = cam.generate_cam(image, target_class=1)
```

**Key Features Analyzed**:
- Nucleus morphology (size, shape, chromatin pattern)
- Cytoplasm characteristics
- Nuclear-cytoplasmic ratio
- Presence of granulations

### Natural Language Explanations

LLM-powered explanations translate visual features into clinical language:
```
"The model classifies this cell as LEUKEMIC (confidence: 94.2%) 
based on the following observations:

1. Enlarged nucleus with irregular chromatin pattern (highlighted 
   in red on the heatmap)
2. High nuclear-cytoplasmic ratio characteristic of blast cells
3. Absence of normal granulations in the cytoplasm

These features are consistent with acute lymphoblastic leukemia (ALL) 
morphology. Clinical correlation and additional testing recommended."
```

## 🗺️ Roadmap

### Phase 1: MVP (Current)
- [] Binary classification (healthy vs leukemic)
- [] Grad-CAM visualization
- [] Basic LLM explanations

---

## 🤝 Contributing

Contributions are welcome! Please read our [Contributing Guidelines](CONTRIBUTING.md).

### Development Setup
```bash
# Install dev dependencies
pip install -r requirements-dev.txt

# Run tests
pytest tests/

# Code formatting
black src/
flake8 src/
```

---

## ⚠️ Medical Disclaimer

**THIS SOFTWARE IS FOR RESEARCH AND EDUCATIONAL PURPOSES ONLY.**

- ❌ **NOT** approved as a medical device
- ❌ **NOT** validated for clinical diagnosis
- ❌ **NOT** a replacement for professional medical judgment
- ❌ **NOT** suitable for patient care without proper validation

**Always consult qualified healthcare professionals for medical decisions.**

---

## 📚 References

### Datasets
- Labati et al. (2011). "ALL-IDB: Acute Lymphoblastic Leukemia Image Database"
- Gupta et al. (2019). "C-NMC Challenge Dataset"

### Methods
- Selvaraju et al. (2017). "Grad-CAM: Visual Explanations from Deep Networks"
- Lin et al. (2017). "Focal Loss for Dense Object Detection"
- Dosovitskiy et al. (2020). "An Image is Worth 16x16 Words: Transformers for Image Recognition"

### Medical Context
- Terwilliger & Abdul-Hay (2017). "Acute lymphoblastic leukemia: a comprehensive review"
- WHO Classification of Tumours of Haematopoietic and Lymphoid Tissues (2017)

---

## 👨‍💻 Author

**Charlie** - ML Engineer & Computer Vision Specialist
- GitHub: [@Ekliipce](https://github.com/Ekliipce)
- LinkedIn: [Charles-André Arsenec](https://linkedin.com/in/yourprofile)
- Project: [WearIT Paris](https://wearit-paris.com) - AI-powered virtual try-on

---

## 📧 Contact

For questions, suggestions, or collaboration opportunities:
- Email: charsenec@gmail.com
- Open an issue on GitHub

---

<div align="center">
**⭐ Star this repo if you find it useful!**
</div>
