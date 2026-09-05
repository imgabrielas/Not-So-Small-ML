# Gold Price Prediction
**Status: ongoing / work in progress**

Regression project exploring gold price prediction from related financial
market indicators. The dataset spans 2008–2018 (2,290 trading days) and
tracks the Gold ETF price alongside other market signals that tend to move
with it.

## Data

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

## Setup

Install dependencies from the repository-level `requirements.txt`:

```bash
pip install -r ../requirements.txt
```

## Project structure

- `notebook.ipynb` — data loading, dataset overview, and data cleaning checks (duplicates, missing values). Modeling steps are still to come.
- `gold_price_data.csv` — raw dataset used by the notebook.
