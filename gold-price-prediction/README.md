# Gold Price Prediction
**Status: modeling complete**

Regression project exploring gold price prediction from related financial
market indicators. The dataset spans 2008–2018 (2,290 trading days) and
tracks the Gold ETF price alongside other market signals that tend to move
with it.

## Data

- Source: Gold Price Data. Kaggle / Mohammed Youssef
- Samples: 2,290 trading days (2008 to 2018)
- Features: 5 numeric cross-asset prices

`gold_price_data.csv` (included in this folder) has 2,290 rows and 6 columns:

| Column    | Description          |
|-----------|-----------------------|
| `Date`    | Trading date |
| `SPX`     | S&P 500 index |
| `GLD`     | Gold ETF price (target) |
| `USO`     | Oil ETF price |
| `SLV`     | Silver ETF price |
| `EUR/USD` | Euro to US Dollar exchange rate |

### Key EDA findings

- No missing values across any column.
- No duplicate rows.
- All numeric columns are complete `float64`, so no type conversion is
  needed before modeling.

## Models

Engineered `Day`, `Month`, `Year`, and `DayOfWeek` (1=Monday, 7=Sunday) from
`Date`, scaled the features, and split 80/20 into train/test. Four regressors
were trained and evaluated, then the two best performers (`Random Forest` and
`Gradient Boosting`) were tuned via `GridSearchCV` over `n_estimators`,
`max_depth`, and `min_samples_leaf`.

| Model                      | MSE   | RMSE | MAE  | R²   | MAPE (%) |
|-----------------------------|------:|-----:|-----:|-----:|---------:|
| **Gradient Boosting (Tuned)** | 1.57 | 1.25 | 0.86 | 1.00 | 0.73 |
| Random Forest                | 1.74 | 1.32 | 0.89 | 1.00 | 0.76 |
| Random Forest (Tuned)        | 1.75 | 1.32 | 0.89 | 1.00 | 0.76 |
| Gradient Boosting             | 4.73 | 2.17 | 1.59 | 0.99 | 1.34 |
| Decision Tree                 | 5.11 | 2.26 | 1.28 | 0.99 | 1.07 |
| Linear Regression             | 45.60 | 6.75 | 5.26 | 0.92 | 4.36 |

`SLV` (silver ETF price) is by far the strongest predictor of `GLD`, followed
by `Year`; the remaining features contribute only marginally.

Tuning improved both ensembles and swapped their ranking: `Random Forest`
was the best base model, but `Gradient Boosting (Tuned)` ended up the best
model overall.

### Sample predictions (Gradient Boosting, Tuned)

| Predicted | Actual | Abs. Error |
|----------:|-------:|-----------:|
| 133.36 | 132.85 | 0.51 |
| 118.89 | 119.22 | 0.33 |
| 172.03 | 171.14 | 0.89 |
| 88.78  | 88.28  | 0.50 |
| 87.49  | 87.01  | 0.48 |

## Setup

Install dependencies from the repository-level `requirements.txt`:

```bash
pip install -r ../requirements.txt
```

## Project structure

- `notebook.ipynb` — data loading, EDA, date feature engineering, model training, hyperparameter tuning, model comparison, and sample predictions.
- `gold_price_data.csv` — raw dataset used by the notebook.
