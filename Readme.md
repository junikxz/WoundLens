# Wound-Lens: Automated Multi-Class Wound Classification

## Description
Wound-Lens is an automated computer vision system developed for diabetic wound screening and multi-class wound classification. Utilizing the ResNet50 architecture, the system provides high-fidelity identification across ten clinically relevant wound categories, offering transparent decision-making through Local Interpretable Model-agnostic Explanations (LIME).

## Dataset Information
This research involved two primary datasets:
* **Final Dataset (10-Class):** The [Kaggle Wound Classification Dataset](https://www.kaggle.com/datasets/ibrahimfateen/wound-classification/data) by Ibrahim Fateen, featuring 10 categories including Diabetic, Venous, Surgical, Burn, Pressure, Laceration, Bruise, and Normal Tissue.
* **Preliminary Dataset (6-Class):** An [open-access repository](https://github.com/uwm-bigdata/wound-classification-using-images-and-locations) by the University of Wisconsin–Milwaukee Big Data Group, focused on six primary wound and tissue classes.

## Code Information
The implementation utilizes a pre-trained ResNet50 backbone with a compact classification head for optimal computational efficiency. Transparency is ensured via LIME heatmaps overlaid on original images to highlight clinically meaningful regions such as necrotic tissue and ulcer beds.

## Usage Instructions
1. **Dataset Preparation:** Download the Kaggle 10-class dataset and organize images into category-specific subfolders.
2. **Environment Setup:** Ensure Python 3.12 is installed and all dependencies from the Requirements section are met.
3. **Training:** Execute the provided training script to initialize the ResNet50 architecture with ImageNet weights.
4. **Validation & XAI:** Generate LIME heatmaps for test images to verify that the model's decision logic aligns with clinical signatures.

## Requirements
To set up the environment on macOS with Python 3.12, install the following dependencies:
```bash
pip install tensorflow keras numpy matplotlib scikit-learn lime
```

# Methodology (Data Processing & Modeling)

## Preprocessing
To address data limitations and ensure model robustness, the following techniques were implemented:
* **Data Augmentation**: Applied aggressive transformations, including rotations and flips, to increase dataset variance.
* **Regularization**: Tuned regularization parameters specifically to mitigate the impact of class imbalance and prevent overfitting.

## Backbone Selection
The architecture was transitioned from MobileNetV2 to ResNet50. This shift was conducted to capture richer hierarchical features and deeper spatial representations necessary for complex medical imaging tasks.

## Interpretability
The LIME framework was integrated to ensure clinical reliability. This validation step confirms that the model focuses on pathological features rather than non-clinical artifacts such as bed linens or measurement rulers.

---

# Materials & Methods

## Computing Infrastructure
* **Operating System**: macOS
* **Python Version**: 3.12
* **Hardware**: Apple Silicon (M-series) or mid-range GPUs

## Evaluation Method
The system was evaluated through the following rigorous metrics:
1. **Training and Validation Behavior**: Initial experiments on the 6-class dataset revealed a gap between training (~93%) and validation (76%) accuracy due to data volume and class imbalance.
2. **Classification Performance on Test Set**: The final Wound-Lens model achieved 96% overall accuracy on a held-out test set of 882 images.

### Comparison Between Previous Model
| Metric | Preliminary Model (MobileNetV2) | Final Model (ResNet50 - Wound-Lens) |
| :--- | :--- | :--- |
| **Dataset Size** | 6-Class (Limited) | 10-Class (Comprehensive) |
| **Training Accuracy** | ~93% | High/Stable |
| **Validation Accuracy** | 76% | 96% |
| **Generalization** | Limited (Overfitting) | Robust |

---

# Citations
Not Applicable

---

# Acknowledgements
The authors acknowledge the Department of Informatics at Universitas Multimedia Nusantara and supervisor David Agustriawan for their guidance and support during this research.

---

# License & Contribution Guidelines
Not Applicable

# DOI
DOI: 10.5281/zenodo.18885215
