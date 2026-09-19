# Phishing Websites — Analysis Summary

Dataset: [UCI Phishing Websites](https://archive.ics.uci.edu/dataset/327/phishing+websites) (id=327) — 11,055 rows, 30 integer-coded features (`{-1, 0, 1}`), binary target `result` (`1` = legitimate, `-1` = phishing).

## Data quality

- No missing values; every feature already scaled into a small ordinal range — no imputation or encoding needed.
- **47% of rows are exact duplicates** (5,270 of 11,055 share identical features with another row; 5,206 of those also share the same label).
- **357 rows are conflicting duplicates**: identical features, opposite labels (64 distinct feature-vectors affected). No feature-based model can classify both copies of a conflicting pair correctly, which caps achievable accuracy on this feature set at roughly **96.8%**.
- Two near-constant features found and confirmed low-value by Random Forest feature importance: `rightclick` (96% one value) and `iframe` (91%).

## Feature reduction

- `favicon` and `popupwindow` are the most correlated pair in the dataset (r = 0.94) — dropped both, keeping the other 28 features unchanged.
- Correlation heatmap also surfaced a broader redundant cluster (`port`, `submitting_to_email`, `on_mouseover`, `rightclick`, `iframe`) worth revisiting if further trimming is needed.
- Random Forest feature importance confirms `sslfinal_state` and `url_of_anchor` dominate (~55% of total importance combined), followed by `having_sub_domain`, `web_traffic`, `prefix_suffix`.

## Methodology note: train/test leakage

A plain random `train_test_split` on the raw (duplicate-heavy) data let identical rows land on both sides of the split, inflating the baseline Logistic Regression's apparent performance (0.929 accuracy / 0.981 AUC). After deduplicating (11,055 → 5,817 unique rows) and re-splitting, the honest baseline dropped to 0.917 accuracy / 0.971 AUC. **All model comparisons below use the deduplicated split.**

## Model comparison (deduplicated split)

| model | recall (phishing) | recall (legit) | accuracy | ROC AUC |
|---|---|---|---|---|
| Logistic Regression | 0.906 | 0.929 | 0.917 | 0.971 |
| Random Forest | 0.937 | 0.959 | 0.948 | 0.988 |
| Gradient Boosting | 0.929 | 0.957 | 0.942 | 0.987 |
| Random Forest (tuned, GridSearchCV) | 0.925 | 0.961 | 0.942 | 0.989 |
| XGBoost | 0.945 | 0.955 | 0.950 | 0.992 |
| **XGBoost (tuned, GridSearchCV)** | **0.954** | **0.959** | **0.956** | **0.992** |

- `class_weight='balanced'` on Logistic Regression traded ~1.6pt of legitimate-site recall for ~1.6pt of phishing recall — a reasonable swap if false negatives (missed phishing) are costlier than false positives.
- Random Forest tuning (`max_depth`, `n_estimators`) found best CV params of `max_depth=12, n_estimators=300` — barely better than the untuned default, since 28 features don't need much depth to fit well.
- XGBoost tuning (`max_depth`, `n_estimators`, `learning_rate`) found `max_depth=4, n_estimators=600, learning_rate=0.1`, giving the best model overall.

## Conclusion

Tuned XGBoost is the strongest model (95.6% accuracy, 0.992 ROC AUC), within ~1.2 points of the ~96.8% ceiling set by the dataset's own label noise. Further gains from this feature set would likely require resolving the 357 conflicting-label rows rather than additional model tuning.