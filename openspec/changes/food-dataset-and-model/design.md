# Design

## Context

FoodVision is a Python web application (Streamlit) that classifies food images, running on Python 3.11 with TensorFlow/Keras. Data exploration, preprocessing, training, and evaluation are done in Jupyter notebooks under `notebooks/`; application data lives in MongoDB Atlas; version control is Git/GitHub. No dataset, trained model, or training pipeline exists yet. See `proposal.md` for motivation and `specs/food-dataset/spec.md` and `specs/food-model/spec.md` for the requirements this design satisfies.

## Goals / Non-Goals

**Goals:**
- An end-to-end data-to-model pipeline: public dataset → preprocessing/augmentation → leakage-free splits → MobileNetV2 transfer-learning training → evaluation → `.keras` export with class names JSON.
- The Streamlit app loads the exported `.keras` model and class names JSON at startup and classifies food images without retraining.
- Scope appropriate for a university project by a team of three (approximately 10–15 food classes).

**Non-Goals:**
- Training a custom CNN from scratch.
- Large-scale hyperparameter optimization or architecture search.
- More than 15 food classes in the initial set.
- Real-time video classification, user accounts, or dataset hosting.
- Inventing training results or metrics.

## Decisions

**MobileNetV2 as the transfer-learning base.**
- Rationale: pre-trained ImageNet features transfer well to food images, trains quickly on limited hardware, and fits a small team. Alternatives considered: EfficientNet (stronger but heavier) and a custom CNN from scratch (too long for this scope, more data needed).
- Approach: freeze the MobileNetV2 base and add a new classification head sized to the number of food classes.

**Keras `.keras` export format.**
- Rationale: native Keras v3 format, single file containing architecture and weights, directly loadable by `tf.keras.models.load_model`. Alternative: SavedModel directory (more verbose for this scope).

**Class names in JSON.**
- Rationale: decouples class index → name mapping from the model so the Streamlit app can display human-readable labels without retraining or hard-coding labels.

**Stratified train/validation/test split with augmentation on training data only.**
- Rationale: preserves class distribution across splits and prevents data leakage by ensuring no image appears in more than one split. Augmentation is applied only to training data so validation/test reflect real performance.

**Python 3.11 + TensorFlow/Keras, Jupyter notebooks.**
- Rationale: matches the existing stack in `requirements.txt` and `notebooks/`.

## Risks / Trade-offs

- [Dataset may have fewer than 10 usable classes] → Mitigate: target approximately 10–15 classes and adjust downward based on availability; record the final class list in class names JSON.
- [Class imbalance across foods] → Mitigate: use stratified splits; consider class weighting if imbalance is severe.
- [MobileNetV2 base may not capture food-specific features well] → Mitigate: start frozen; fine-tune later if needed (out of initial scope).
- [No training results available yet] → Mitigate: do not invent metrics; report actual measured values after training.

## Open Questions

- Exact public food image dataset to select.
- Exact final list of food classes (within 10–15).
- Exact train/validation/test split ratios.
