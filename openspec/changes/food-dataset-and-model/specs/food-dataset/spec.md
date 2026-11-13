# Spec Delta

## Purpose

Defines the data foundation for FoodVision: public food dataset selection with license and academic-use verification, local download and image integrity verification, class availability and selection (~10–15 classes), data exploration and class distribution analysis, image preprocessing (RGB 224x224 with MobileNetV2 `preprocess_input`), augmentation on training data only, and reproducible stratified train/validation/test splits with duplicate detection and test-set isolation.

## ADDED Requirements

### Requirement: Dataset license permits academic use
The system SHALL select a public food image dataset whose license permits academic use.

#### Scenario: License verified for academic use
- **WHEN** the dataset license is reviewed
- **THEN** the license permits academic use and this is documented

### Requirement: Local download verified
The system SHALL verify the dataset downloads locally and all files are present.

#### Scenario: Dataset downloads and loads locally
- **WHEN** the dataset is downloaded
- **THEN** dataset files are present locally and loadable

### Requirement: Image integrity verified
The system SHALL verify the integrity of downloaded images.

#### Scenario: Images pass integrity check
- **WHEN** image integrity is verified
- **THEN** corrupt or unreadable images are identified and excluded

### Requirement: Class availability verified
The system SHALL verify which food classes are available in the dataset.

#### Scenario: Available classes enumerated
- **WHEN** class availability is checked
- **THEN** the set of available food classes is listed

### Requirement: Initial food classes defined
The system SHALL define an initial set of approximately 10 to 15 food classes based on actual dataset availability.

#### Scenario: Class count within range
- **WHEN** classes are enumerated
- **THEN** the number of classes is between 10 and 15 inclusive

### Requirement: Class distribution analyzed
The system SHALL analyze class distribution during data exploration.

#### Scenario: Distribution reported
- **WHEN** exploration runs
- **THEN** per-class image counts are reported

### Requirement: Images preprocessed to 224x224 RGB
The system SHALL preprocess images as RGB resized to 224x224 pixels using TensorFlow/Keras MobileNetV2 `preprocess_input`.

#### Scenario: Preprocessing output correct
- **WHEN** an image enters preprocessing
- **THEN** output is RGB, 224x224, and normalized via MobileNetV2 `preprocess_input`

### Requirement: Preprocessing consistent between training and inference
The system SHALL use the same MobileNetV2 `preprocess_input` preprocessing during training and inference.

#### Scenario: Preprocessing identical in training and inference
- **WHEN** preprocessing runs in training and in inference
- **THEN** both use identical RGB 224x224 MobileNetV2 `preprocess_input` steps

### Requirement: Augmentation applied to training data only
The system SHALL apply image augmentation only to training data.

#### Scenario: Augmentation scope
- **WHEN** augmentation is configured
- **THEN** it is applied to the training split and not to validation or test splits

### Requirement: Reproducible splits with fixed random seed
The system SHALL create train, validation, and test splits using a fixed random seed so splits are reproducible.

#### Scenario: Splits reproducible
- **WHEN** splits are generated twice with the same seed
- **THEN** the resulting splits are identical

### Requirement: Stratified splits
The system SHALL use stratification to preserve class proportions across train, validation, and test splits.

#### Scenario: Class proportions preserved
- **WHEN** stratified splits are generated
- **THEN** class proportions are preserved across all splits

### Requirement: Duplicate images detected before splitting
The system SHALL detect exact or near-duplicate images before creating splits to reduce data leakage.

#### Scenario: Duplicates detected
- **WHEN** duplicate detection runs before splitting
- **THEN** exact or near-duplicate images are identified and handled before splits are created

### Requirement: No image leakage across splits
The system SHALL ensure that no individual image or exact/near-duplicate image group appears in more than one of the train, validation, and test splits. All selected classes SHALL be represented in each split whenever the available sample counts allow stratification.

#### Scenario: No image leakage across splits
- **WHEN** train, validation, and test splits are generated
- **THEN** no image or duplicate group appears in more than one split
- **AND** all selected classes are represented in each split whenever feasible.

### Requirement: Test data isolated from training and model selection
The system SHALL keep test data isolated so it is not used during training or hyperparameter selection.

#### Scenario: Test set isolated
- **WHEN** training and model selection occur
- **THEN** the test set is not used for training or hyperparameter tuning
