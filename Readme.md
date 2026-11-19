# Wound-Lens: Automated Multi-Class Wound Classification

## Description
Wound-Lens is a deep learning pipeline for multi-class wound image classification, including diabetic wounds. It fine-tunes a ResNet50 backbone to classify images into ten wound and skin categories, and compares it with a MobileNetV2 baseline under the same five-fold cross-validation. Local Interpretable Model-agnostic Explanations (LIME) are used to inspect individual predictions qualitatively. This repository accompanies the manuscript submitted to PeerJ Computer Science.

## Dataset Information
This study uses one dataset:

- **Wound Classification Dataset (10 classes):** the [Kaggle Wound Classification Dataset](https://www.kaggle.com/datasets/ibrahimfateen/wound-classification/data) compiled by Ibrahim Fateen (2023). It contains 2,940 images in ten classes: abrasions, bruises, burns, cuts, diabetic wounds, lacerations, normal skin, pressure wounds, surgical wounds, and venous wounds. The images are provided in `data/Wound_Dataset/`, one folder per class.
- **Origin and labels:** The Kaggle page does not state the original sources or how the images were labelled. Part of the images come from the AZH wound dataset (Anisuzzaman et al., 2022), whose images were labelled by a wound specialist. AZH covers only some of the ten classes, so it accounts for only part of the dataset. The remaining images appear to come from other public collections (for example WoundDB 2021 and Medetec), and their labelling is not documented. We used the labels as provided and did not re-label the images.
- **Licence and reuse:** The Kaggle page lists the licence as "Unknown", and the images come from third-party collections. We include the images only so that the results can be reproduced. Please check the terms of the original sources before reusing them.
- **Duplicates:** We found 21 images that are identical copies of other images. In every fold, an image and its identical copies are kept in the same set. The dataset may also contain other near-duplicate images (for example mirrored copies) that this check does not detect.

## Code Information
`WoundLens.ipynb` contains the full pipeline: data loading and duplicate grouping, stratified five-fold cross-validation, training of ResNet50 and MobileNetV2, performance metrics, statistical comparison, and LIME explanations.

- **Model:** ImageNet-pretrained ResNet50 (last 12 layers fine-tuned) with a compact head: global average pooling, batch normalisation, dense layer (512 units), dropout (0.5), and a softmax layer (10 classes). MobileNetV2 uses the same head.
- **Interpretability:** LIME highlights are shown as qualitative illustrations. They are local and approximate, and they were not evaluated by clinicians.

## Usage Instructions
1. **Dataset preparation:** The images are in `data/Wound_Dataset/` with one subfolder per class. No further preparation is needed.
2. **Environment setup:** Install the packages listed under Requirements.
3. **Configuration:** In the first cell of the notebook, set `BASE = "data/Wound_Dataset"` and `OUT = "results"`. Remove the Google Drive mounting block if you do not use Colab. The notebook creates the `results/` folder when it runs.
4. **Training and evaluation:** Run all cells. Both models are trained in five folds (seed 42). A checkpoint is saved after each fold, so an interrupted run can resume.
5. **Explanations:** The last cells generate LIME explanations for representative test images from the first fold.

## Requirements
```
pip install -r requirements.txt
```
The study used TensorFlow 2.21.0. The other packages are numpy, pandas, scipy, scikit-learn, opencv-python, matplotlib, seaborn, lime, and scikit-image (see `requirements.txt`). Model weights (`*.h5`) are not included because of file size; running the notebook recreates them.

## Methodology (Data Processing & Modeling)
### Preprocessing
- **Resizing:** Images are resized to 224 × 224 pixels with the aspect ratio preserved (letterbox padding).
- **Augmentation (training images only):** rotation up to ±25°, shift up to ±18%, shear up to ±15°, zoom 0.75–1.25, brightness 0.75–1.25, and horizontal flip.
- **Class imbalance:** Class weights are used in the loss (largest class about six times the smallest).

### Cross-validation
Stratified five-fold cross-validation with duplicate grouping (one repeat). In each fold, 10% of the training portion is held out as a validation set for early stopping and learning-rate scheduling. The test fold is never used for training or model selection.

### Training
Adam optimizer (learning rate 1e-4), batch size 32, up to 30 epochs, early stopping (patience 10, best weights restored), and learning-rate reduction on plateau (factor 0.2, patience 3).

### Backbone comparison
ResNet50 and MobileNetV2 are trained and tested on exactly the same five folds. The paired difference is tested with a paired t-test, with and without the Nadeau–Bengio variance correction.

## Materials & Methods
### Computing Infrastructure
- **Platform:** Google Colab with a GPU (NVIDIA A100)
- **Framework:** TensorFlow 2.21.0 (Keras)

### Evaluation Method
Performance is measured on the held-out test folds as accuracy, balanced accuracy, macro-F1, per-class precision, recall, and F1, and a pooled confusion matrix. Results are reported as mean ± standard deviation across the five folds, with 95% confidence intervals.

| Metric (mean ± SD, five test folds) | MobileNetV2 | ResNet50 (Wound-Lens) |
|---|---|---|
| Accuracy | 0.864 ± 0.023 | 0.934 ± 0.017 |
| Balanced accuracy | 0.887 ± 0.021 | 0.934 ± 0.016 |
| Macro-F1 | 0.876 ± 0.021 | 0.932 ± 0.018 |

The difference between the two models is statistically supported by paired t-tests (accuracy: p = 0.0024; p = 0.0102 with variance correction). With only five folds, these results should be read with caution. The dataset has no patient identifiers, so separation is at the file level, not the patient level. See the manuscript for the full results and limitations.

## Citations
- Fateen I. 2023. Collected and Categorized Wound Images Dataset (Wound Classification). Kaggle. https://www.kaggle.com/datasets/ibrahimfateen/wound-classification
- Anisuzzaman DM, Patel Y, Rostami B, Niezgoda J, Gopalakrishnan S, Yu Z. 2022. Multi-modal wound classification using wound image and location by deep neural network. Scientific Reports 12:20057.

If you use this code, please cite the DOI below and the accompanying manuscript.

## Acknowledgements
The authors acknowledge the Department of Informatics at Universitas Multimedia Nusantara and supervisor David Agustriawan for their guidance and support during this research.

## License & Contribution Guidelines
No licence has been set for the code yet. The images are third-party material; see the licence note under Dataset Information. For questions or corrections, please open an issue in this repository.

## DOI
https://doi.org/10.5281/zenodo.24369281