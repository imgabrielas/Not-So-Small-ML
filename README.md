# Smartphone Addiction Prediction (Kaggle Playground Series S6E8)
**Status: ongoing / work in progress**

Binary classification project for the Kaggle Playground Series competition
[*Predicting Smartphone Addiction*](https://www.kaggle.com/competitions/playground-series-s6e8/data)
(Season 6, Episode 8). The goal is to predict whether a person is addicted
to their smartphone (`addicted_label`) from behavioral and demographic
features.

## Data

Place the competition files in `smartphone_addiction_data/` (gitignored):

- `train.csv` — 691,369 rows, labeled
- `test.csv` — 296,302 rows, unlabeled
- `sample_submission.csv` — expected submission format (`id`, `addicted_label`)


### Key EDA findings

- **Target is imbalanced but not severely**: ~71% addicted (`1`) vs. ~29% not
  addicted (`0`).
- **Every feature has missing values**, ranging from ~4% (`age`) up to ~19%
  (`social_media_hours`) of rows, spread across all numeric and categorical
  columns — no column is complete.
- Numeric features (screen time, social media/gaming/work-study hours, sleep,
  notifications, app opens) are roughly unimodal and don't show extreme
  outliers or skew that would need special handling.
- All three categorical features have a small, fixed set of categories
  (`gender`: Male/Female/Other, `stress_level`: Low/Medium/High,
  `academic_work_impact`: Yes/No) with no unexpected values.

## Preprocessing

Missing values and feature encoding are handled entirely through
`scikit-learn` pipelines, fit only on the training split and reused
(via `.transform`, not refit) on the validation and test sets to avoid
leaking their statistics into training.

Features are grouped by type, each with its own imputation + encoding
pipeline:

- **Numeric features** (`age`, screen time / usage hour columns,
  notifications, app opens): missing values are imputed with the **median**
  of the column, then the column is standardized with `StandardScaler`.
- **Nominal feature** (`gender`): missing values are imputed with the
  **most frequent** category, then one-hot encoded (unknown categories at
  inference time are ignored rather than raising an error).
- **Ordinal feature** (`stress_level`): missing values are imputed with the
  most frequent category, then encoded as an ordered integer following the
  natural order Low < Medium < High.
- **Binary feature** (`academic_work_impact`): missing values are imputed
  with the most frequent category, then encoded as 0/1 (No/Yes).

These four pipelines are combined with a `ColumnTransformer` so each group
of columns is routed to the right imputer/encoder in a single fit/transform
call, producing a fully numeric, missing-value-free feature matrix for
modeling.

## Models

A stratified 80/20 train/validation split (stratified on `addicted_label`
to preserve the class balance) is used to compare models before touching
the real test set. Each model is wrapped together with the preprocessing
step in one `Pipeline`, and compared by ROC-AUC on the validation split:

- **Logistic Regression** — linear baseline model.
- **Random Forest** — bagged tree ensemble.
- **XGBoost** — gradient-boosted tree ensemble.

### Results (validation ROC-AUC)

| Model | ROC-AUC |
|---|---|
| XGBoost | 0.9623 |
| Random Forest | 0.9411 |
| Logistic Regression | 0.9102 |

XGBoost is currently the best-performing model and the leading candidate
for the final submission, pending hyperparameter tuning.

## Project structure

- `notebook.ipynb` — working analysis / modeling notebook (gitignored, will be shared after competition)
- `smartphone_addiction_data/` — raw competition data (gitignored, not tracked)

