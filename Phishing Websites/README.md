# Phishing Websites

Binary classification task: predict whether a website is a phishing site (`result`) based on 30 URL/page-derived features.

## Data Source

[Phishing Websites Data Set](https://archive.ics.uci.edu/dataset/327/phishing+websites) — UCI Machine Learning Repository (id=327)

- **Instances:** 11,055
- **Features:** 30, all integer-encoded (no missing values)
- **Target:** `result` — 1 (legitimate) or -1 (phishing)
- **Collected from:** PhishTank archive, MillerSmiles archive, Google's search operators
- **Creators:** Rami Mohammad, Lee McCluskey (2012)
- **Reference paper:** Mohammad, R., Thabtah, F., & McCluskey, L. (2012). *An assessment of features related to phishing websites using an automated technique.* International Conference for Internet Technology and Secured Transactions.

### Loading

```python
# pip install ucimlrepo

from ucimlrepo import fetch_ucirepo

phishing_websites = fetch_ucirepo(id=327)
X = phishing_websites.data.features
y = phishing_websites.data.targets
```
