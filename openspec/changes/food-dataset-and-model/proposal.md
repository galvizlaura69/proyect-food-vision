# Proposal

## Why

FoodVision is a Python web application that classifies food images, but it currently has no training pipeline, no dataset, and no trained model. The application cannot classify food without retraining from scratch on every deployment. This change establishes the data and model foundation: select a public food image dataset with verified academic-use license, define approximately 10–15 food classes based on actual availability, verify image integrity, build reproducible stratified train/validation/test splits with duplicate detection and test-set isolation, preprocess RGB images to 224x224 using MobileNetV2 `preprocess_input`, train a frozen MobileNetV2 transfer-learning model with optional fine-tuning, evaluate with accuracy/precision/recall/F1 (macro and weighted averages) and a saved confusion matrix without touching the test set for hyperparameter tuning, export the model as `.keras` with class names in JSON preserving the class-to-index mapping, and ensure the Streamlit app loads the exported artifacts without retraining. This is needed now to make the classifier functional for a university project developed by a team of three.

## What Changes

- Select a public food image dataset and verify its license permits academic use.
- Verify local download, image integrity, and class availability.
- Select approximately 10–15 food classes based on actual dataset availability.
- Define data exploration and class distribution analysis.
- Build reproducible train/validation/test splits with a fixed random seed, stratification, exact/near-duplicate detection, and test-set isolation from training and model selection.
- Preprocess RGB images resized to 224x224 pixels using TensorFlow/Keras MobileNetV2 `preprocess_input` consistently in training and inference.
- Apply augmentation only to training data.
- Train MobileNetV2 with a frozen base and new classification head; document optimizer, learning rate, batch size, epochs, early stopping, and random seed; make fine-tuning optional depending on time and validation performance.
- Evaluate with accuracy, precision, recall, and F1-score (macro and weighted averages), generate and save the confusion matrix; do not use the test set to tune hyperparameters; do not invent results.
- Export the trained model in `.keras` format and class names in JSON preserving the exact class-to-index mapping.
- Ensure inference uses the same image preprocessing as training.
- Ensure the Streamlit app loads the exported `.keras` model and class names JSON without retraining.

## Capabilities

### New Capabilities

- `food-dataset`: Covers dataset license verification for academic use, local download and image integrity verification, class availability and selection (~10–15 classes), data exploration and class distribution analysis, image preprocessing (RGB 224x224 with MobileNetV2 `preprocess_input`), augmentation on training data only, and reproducible stratified train/validation/test splits with duplicate detection and test-set isolation. Produces `specs/food-dataset/spec.md`.
- `food-model`: Covers MobileNetV2 transfer learning (frozen base plus new head, optional fine-tuning), documented training strategy (optimizer, learning rate, batch size, epochs, early stopping, random seed), evaluation metrics (accuracy, precision, recall, F1-score with macro and weighted averages, saved confusion matrix) without test-set hyperparameter tuning or invented results, model export in `.keras` format with class names in JSON preserving class-to-index mapping, inference preprocessing consistent with training, and loading the trained model by the application without retraining. Produces `specs/food-model/spec.md`.

### Modified Capabilities

- None. This is the first OpenSpec change for the FoodVision project; there are no existing spec-level capabilities to modify.

## Impact

- `notebooks/`: data exploration, preprocessing, training, and evaluation notebooks (Jupyter).
- `models/`: trained `.keras` model file and class names JSON.
- `src/` and `app.py`: Streamlit application loads the exported model without retraining.
- `requirements.txt`: TensorFlow/Keras and dataset-related dependencies.
- `tests/`: tests covering data splits, duplicate detection, preprocessing, model loading, and evaluation.
