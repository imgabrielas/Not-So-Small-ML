# Airline Satisfaction

Binary classification project predicting whether an airline passenger is satisfied with their flight, built for the Kaggle competition [Predicting Airline Satisfaction (Playground Series, Season 6 Episode 10)](https://www.kaggle.com/competitions/playground-series-s6e10).

## Competition result
- **Leaderboard position:** 3161
- **Submitted model:** XGBoost with default hyperparameters (200 estimators), refit on the full training set
- **Validation ROC-AUC:** 0.9571

The model was submitted without any hyperparameter tuning, so the score is a baseline. Tuning (e.g. `RandomizedSearchCV` with `StratifiedKFold`) is the clear next step for improving the result.

## Data
- Training set: 699,635 rows, 23 columns
- Test set: 299,844 rows, 22 columns
- Target: `satisfaction` (`True` / `False`)

Features cover passenger demographics (age, gender, customer type), trip details (type of travel, class, flight distance), delays, and 0 to 5 ratings for 14 service areas such as wifi, seat comfort and online boarding.

### Ignored data
- **292 training rows dropped** with missing `Arrival Delay in Minutes` (0.04% of the training set)
- **`id` column excluded** from the features, as it is only a row identifier
- The 130 missing `Arrival Delay in Minutes` values in the test set were kept and filled with the median by the preprocessing pipeline, since test rows cannot be dropped from a submission

## Approach
1. Brief EDA: structure, dtypes, unique values and missing data
2. Preprocessing with a scikit-learn `ColumnTransformer`:
   - numeric features: median imputation and standard scaling
   - `Class`: ordinal encoding (`Eco` < `Eco Plus` < `Business`)
   - `Gender`, `Customer Type`, `Type of Travel`: binary one-hot encoding
3. Stratified 80/20 train/validation split
4. Comparison of three baseline models on validation ROC-AUC

| Model | Validation ROC-AUC |
|---|---|
| XGBoost | 0.9571 |
| Random Forest | 0.9540 |
| Logistic Regression | 0.9206 |

## Tech stack
pandas, NumPy, Matplotlib, seaborn, scikit-learn, XGBoost

## Status
Baseline submitted. Possible improvements: hyperparameter tuning, cross-validation instead of a single split, and feature engineering (e.g. aggregated service scores, total delay).