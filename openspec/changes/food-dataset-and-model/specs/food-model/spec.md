# Spec Delta

## Purpose

Defines the model foundation for FoodVision: MobileNetV2 transfer learning (frozen base plus new head, optional fine-tuning), documented training strategy (optimizer, learning rate, batch size, epochs, early stopping, random seed), evaluation metrics (accuracy, precision, recall, F1-score with macro and weighted averages, saved confusion matrix) without test-set hyperparameter tuning or invented results, model export in `.keras` format with class names in JSON preserving class-to-index mapping, inference preprocessing consistent with training, and loading the trained model by the application without retraining.

## ADDED Requirements

### Requirement: MobileNetV2 transfer learning used
The system SHALL train a food image classifier using MobileNetV2 transfer learning with a frozen base and a new classification head.

#### Scenario: Transfer learning base
- **WHEN** training starts
- **THEN** MobileNetV2 is used as the frozen feature extractor with a new classification head

### Requirement: Fine-tuning optional
The system SHALL make fine-tuning of the MobileNetV2 base optional, depending on available time and validation performance.

#### Scenario: Fine-tuning applied conditionally
- **WHEN** time permits and validation performance warrants it
- **THEN** fine-tuning of the MobileNetV2 base may be performed; otherwise the base remains frozen

### Requirement: Training strategy documented
The system SHALL document the training strategy: optimizer, learning rate, batch size, epochs, early stopping, and random seed.

#### Scenario: Training strategy applied
- **WHEN** the model is trained
- **THEN** training uses the documented optimizer, learning rate, batch size, epoch cap, early stopping, and random seed

### Requirement: Evaluation metrics computed
The system SHALL compute accuracy, precision, recall, and F1-score on the test split, including macro and weighted averages where appropriate.

#### Scenario: Metrics produced
- **WHEN** evaluation runs on the test split
- **THEN** accuracy, precision, recall, and F1-score are computed with macro and weighted averages where appropriate

### Requirement: Confusion matrix generated and saved
The system SHALL generate and save the confusion matrix from test-split evaluation.

#### Scenario: Confusion matrix saved
- **WHEN** evaluation completes
- **THEN** a confusion matrix is generated and saved

### Requirement: Test set not used for hyperparameter tuning
The system SHALL NOT use the test set to tune hyperparameters.

#### Scenario: Test set untouched during tuning
- **WHEN** hyperparameters are tuned
- **THEN** only training and validation data are used; the test set is held out until final evaluation

### Requirement: No invented results
The system SHALL report only measured training and evaluation results; no results may be invented.

#### Scenario: Results are measured not invented
- **WHEN** results are reported
- **THEN** all metrics reflect actual measured values from training and evaluation

### Requirement: Model exported in .keras format
The system SHALL export the trained model in `.keras` format.

#### Scenario: Model file produced
- **WHEN** export runs
- **THEN** a `.keras` model file is written

### Requirement: Class names exported in JSON with class-to-index mapping
The system SHALL export class names in JSON alongside the model, preserving the exact class-to-index mapping.

#### Scenario: Class names file produced with mapping
- **WHEN** export runs
- **THEN** a JSON file mapping class indices to class names is written, preserving the exact class-to-index mapping

### Requirement: Model loadable without retraining
The system SHALL allow the application to load the exported `.keras` model and class names without retraining.

#### Scenario: Application loads model
- **WHEN** the Streamlit app starts
- **THEN** it loads the `.keras` model and class names JSON and classifies images without retraining
