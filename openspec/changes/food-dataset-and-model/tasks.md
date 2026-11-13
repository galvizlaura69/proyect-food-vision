# Tasks

## 1. Dataset and classes

- [ ] 1.1 Select a public food image dataset and verify its license permits academic use; verify the license is documented.
- [ ] 1.2 Download the dataset locally and verify all files are present and loadable.
- [ ] 1.3 Verify image integrity and identify/exclude corrupt or unreadable images.
- [ ] 1.4 Verify class availability and enumerate the available food classes.
- [ ] 1.5 Select the initial food class list (approximately 10–15 classes based on actual availability); verify the class list is documented.

## 2. Data exploration, preprocessing, augmentation

- [ ] 2.1 Run data exploration and class distribution analysis; verify per-class image counts are reported.
- [ ] 2.2 Implement image preprocessing as RGB resized to 224x224 using TensorFlow/Keras MobileNetV2 `preprocess_input`; verify output is RGB, 224x224, and normalized.
- [ ] 2.3 Verify preprocessing is identical between training and inference.
- [ ] 2.4 Implement image augmentation on training data only; verify augmentation applies to the training split and not to validation or test splits.

## 3. Train/validation/test splits

- [ ] 3.1 Create reproducible train/validation/test splits using a fixed random seed; verify running twice with the same seed yields identical splits.
- [ ] 3.2 Use stratification to preserve class proportions across all splits; verify class proportions are preserved.
- [ ] 3.3 Detect exact or near-duplicate images before splitting; verify duplicates are identified and handled before splits are created.
- [ ] 3.4 Verify no sample appears in more than one split (no leakage).
- [ ] 3.5 Keep test data isolated from training and model selection; verify the test set is not used for training or hyperparameter tuning.

## 4. MobileNetV2 transfer-learning training

- [ ] 4.1 Implement MobileNetV2 transfer learning with a frozen base and new classification head; verify the model compiles and trains.
- [ ] 4.2 Document the training strategy (optimizer, learning rate, batch size, epochs, early stopping, random seed); verify training uses the documented values.
- [ ] 4.3 Make fine-tuning optional depending on time and validation performance; verify fine-tuning is applied only when conditions warrant.

## 5. Evaluation

- [ ] 5.1 Compute accuracy, precision, recall, and F1-score on the test split with macro and weighted averages where appropriate; verify all metrics are reported.
- [ ] 5.2 Generate and save the confusion matrix; verify the confusion matrix file is written.
- [ ] 5.3 Verify the test set is not used to tune hyperparameters.
- [ ] 5.4 Verify all reported results are measured, not invented.

## 6. Model export and application loading

- [ ] 6.1 Export the trained model in `.keras` format and class names in JSON preserving the exact class-to-index mapping; verify both files are written and loadable.
- [ ] 6.2 Verify inference uses the same image preprocessing as training.
- [ ] 6.3 Update the Streamlit app to load the `.keras` model and class names JSON without retraining; verify the app classifies an image using the loaded model.
