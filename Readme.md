# Wound-Lens: multi-class wound classification (research prototype)

Code and data for the manuscript *"Wound-Lens: An Automated Computer Vision System for Diabetic Wound Screening and Classification using ResNet50"* (PeerJ Computer Science, submission 133600).

> Wound-Lens is a research prototype. It is **not** a medical device and has no external or prospective clinical validation.

## What this repository contains

| Path | Content |
|---|---|
| `WoundLens.ipynb` | Full pipeline: data loading, stratified 5-fold cross-validation, ResNet50 and MobileNetV2 training, statistics, LIME |
| `data/Wound_Dataset/` | The 2,940 images used in the study, one folder per class (10 classes) |
| `requirements.txt` | Python packages |

Model weights (`*.h5`) are not included because of file size. They are reproduced by running the notebook.

## Data

- **Source:** *Collected and Categorized Wound Images Dataset* on Kaggle, compiled by Ibrahim Fateen (2023): https://www.kaggle.com/datasets/ibrahimfateen/wound-classification
- **Classes (10):** abrasions, bruises, burns, cuts, diabetic wounds, lacerations, normal skin, pressure wounds, surgical wounds, venous wounds.
- **Origin and labels:** The Kaggle page does not state the original sources or how the images were labelled. Part of the images in the dataset come from the AZH wound dataset (Anisuzzaman et al., 2022), whose images were labelled by a wound specialist. The AZH collection covers only some of our ten classes, so it accounts for only part of the dataset. The remaining images appear to come from other public collections (for example WoundDB 2021 and Medetec), and their labelling is not documented. We used the labels as provided and did not re-label the images.
- **Licence and reuse:** The Kaggle page lists the licence as "Unknown", and the images come from third-party collections. We include the images only so that the results can be reproduced. Please check the terms of the original collections before any other use.
- **Duplicates:** We found 21 images that are identical copies of other images. In every fold, an image and its identical copies are kept in the same set. The dataset may also contain other near-duplicate (for example mirrored) images; the notebook assigns each image to a group automatically.

## How to run

1. Install the packages: `pip install -r requirements.txt` (the study used TensorFlow 2.21.0).
2. Open `WoundLens.ipynb`. In the first cell, set `BASE = "data/Wound_Dataset"` and `OUT = "results"` (the notebook creates this folder when it runs; remove the Google Drive block if you do not use Colab).
3. Run all cells. The cross-validation step saves a checkpoint (`results/cv_results.pkl`) after every fold and resumes from it if interrupted.

A GPU is recommended: the 10 training runs (ResNet50 and MobileNetV2, 5 folds each) took about 1 hour on an A100 GPU.

## Method in brief

- Images are resized to 224 × 224 pixels (aspect ratio preserved, zero padding).
- Stratified, duplicate-grouped 5-fold cross-validation (seed 42). In each fold, the test fold is never used for training, early stopping, or model selection.
- Augmentation (training only): rotation ±25°, shift ±18%, shear ±15°, zoom 0.75–1.25, brightness 0.75–1.25, horizontal flip.
- ResNet50 (last 12 layers fine-tuned) with a head of global average pooling, batch normalization, dense 512, dropout 0.5, and softmax; MobileNetV2 baseline with the same head.
- Adam (learning rate 1e-4), batch size 32, up to 30 epochs, class-weighted loss, early stopping, and learning-rate scheduling.
- Mean ± SD and 95% confidence intervals (Nadeau–Bengio correction) across folds; paired t-test for the backbone comparison.
- LIME for qualitative inspection only.

The results are reported in the manuscript; the notebook regenerates the corresponding tables and figures in `results/` when it is run.

## References

- Fateen I. 2023. Collected and Categorized Wound Images Dataset. Kaggle.
- Anisuzzaman DM, Patel Y, Rostami B, Niezgoda J, Gopalakrishnan S, Yu Z. 2022. Multi-modal wound classification using wound image and location by deep neural network. Scientific Reports 12:20057.

## Contact

Corresponding author: David Agustriawan (david.agustriawan@umn.ac.id), Universitas Multimedia Nusantara, Indonesia.