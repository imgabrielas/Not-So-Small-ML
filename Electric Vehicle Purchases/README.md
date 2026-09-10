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

## Project structure

- `Electric-Vehicle-Purchases.ipynb` — working analysis / modeling notebook
- `Electric-Vehicle-Purchases-Data/` — raw competition data (gitignored, not tracked)
