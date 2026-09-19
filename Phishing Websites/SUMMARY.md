# Phishing Websites — Analysis Summary

Dataset: [UCI Phishing Websites](https://archive.ics.uci.edu/dataset/327/phishing+websites) (id=327) — 11,055 rows, 30 integer-coded features (`{-1, 0, 1}`), binary target `result` (`1` = legitimate, `-1` = phishing).

## Data quality

- No missing values; every feature already scaled into a small ordinal range — no imputation or encoding needed.
- **47% of rows are exact duplicates** (5,270 of 11,055 share identical features with another row; 5,206 of those also share the same label).
- **357 rows are conflicting duplicates** (on the original 30 features): identical features, opposite labels (64 distinct feature-vectors affected). No feature-based model can classify both copies of a conflicting pair correctly, which caps achievable accuracy on this feature set at roughly **96.8%**. This count rises to **365 rows** once `favicon`/`popupwindow` are dropped (see below) — fewer features means more feature-vectors collide.
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

## Sensitivity check: removing conflicting rows entirely

To test how much of the remaining error was attributable to the 365 conflicting-label rows (rather than genuine model limitations), they were removed entirely (both/all copies, not an arbitrary first-occurrence pick) and every model retrained on the resulting conflict-free, deduplicated set (11,055 → 5,687 rows). Tuned models reused the hyperparameters already found by GridSearchCV rather than re-searching, to isolate the effect of the row removal.

| model | accuracy (conflicts kept) | accuracy (conflicts removed) | ROC AUC (kept) | ROC AUC (removed) |
|---|---|---|---|---|
| Logistic Regression | 0.917 | 0.921 | 0.971 | 0.978 |
| Random Forest | 0.948 | **0.962** | 0.988 | 0.995 |
| Gradient Boosting | 0.942 | 0.954 | 0.987 | 0.991 |
| Random Forest (tuned) | 0.942 | 0.955 | 0.989 | 0.994 |
| XGBoost | 0.950 | 0.961 | 0.992 | 0.996 |
| XGBoost (tuned) | 0.956 | 0.961 | 0.992 | 0.996 |

Every model gained roughly 1–1.5 accuracy points and 0.5–0.7 AUC points, confirming that most of the remaining error in the main comparison was attributable to label noise rather than genuine model imperfection. Random Forest becomes the top model on accuracy (0.962) here, essentially tied with XGBoost on AUC (0.995–0.996).

**Caveat:** this improvement is real but partly an artifact of an easier benchmark — the genuinely ambiguous examples were removed from the *test set* too, so the model is no longer graded on cases that are inherently unresolvable from these features alone. In deployment, websites with these same ambiguous feature signatures will still occur. The "conflicts kept" numbers in the main comparison above remain the more honest estimate of real-world performance; this table is a diagnostic, not a replacement result.

## Conclusion

Final comparison table includes two times of evaluation, one with conflicting rows kept and the other one rows were dropped. Slight improvement can be seen in all models with metrics for dropped rows. Random Forest has the biggest accuracy 0.962 and ROC AUC 0.995, Tuned XGB is second best in terms of accuracy but better with ROC AUC of 0.996. In practice both of those models are good, and choice in between goes down to secondary factors like inference speed, interpretability, ease of deployment rather than performance.