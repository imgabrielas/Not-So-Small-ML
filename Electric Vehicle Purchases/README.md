# Electric Vehicle Purchases Prediction
**Status: ongoing / work in progress**

Binary classification project for a current Kaggle competition
([Playground Series S6E9](https://www.kaggle.com/competitions/playground-series-s6e9/overview),
September 2026), predicting whether a person will buy an electric vehicle
(`Will_Buy_EV`) from demographic, financial, and lifestyle features (age,
income, commute distance, cars owned, charging station access,
environmental concern, home charging availability, subsidy availability,
range anxiety, and more).

This is an active, month-long competition, so the notebook here is a
work in progress. Progress will be shared throughout the month as the
project develops, and the full solution will be shared once the
competition is finished.

## Data

Place the competition files in `Electric-Vehicle-Purchases-Data/`
(gitignored):

- `train.csv` — labeled training data
- `test.csv` — unlabeled test data
- `sample_submission.csv` — expected submission format (`id`, `Will_Buy_EV`)

## Models

All models run through a shared `ColumnTransformer` (scaling numeric
features, one-hot encoding nominal/binary categoricals, ordinal encoding
`Range_Anxiety_Level`) and are compared on validation ROC-AUC:

| Model          | Validation ROC-AUC |
|----------------|--------------------:|
| **XGB (tuned)** | **0.9417** |
| XGB             | 0.9384 |
| LogReg          | 0.9380 |
| RF              | 0.9345 |

`XGBClassifier`, tuned via `RandomizedSearchCV` (`StratifiedKFold`, 3 folds,
10 candidates) over `n_estimators`, `max_depth`, `learning_rate`,
`subsample`, and `colsample_bytree`, is the best model so far.

**Current Kaggle leaderboard position: #2456.**

## Project structure

- `Electric-Vehicle-Purchases.ipynb` — working analysis / modeling notebook
- `Electric-Vehicle-Purchases-Data/` — raw competition data (gitignored, not tracked)
