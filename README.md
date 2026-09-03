# Not-So-Small-ML

A collection of small, end-to-end machine learning projects, each living in its own folder within this single repository.

Companion to [How-To-Make-ML](https://github.com/gabrielaslomiany/How-To-Make-ML), that repo covers general ML concepts and workflow, this one is where those ideas get applied to actual projects (including Kaggle competitions). Keeping every project here instead of in separate repos makes them easier to browse, compare, and maintain.

Each project folder is self-contained: its own notebook(s), data, README, and (where needed) dependencies.

## Projects

| Project                                                   | Description | Status |
|-----------------------------------------------------------|---|---|
| [`alzheimer-detection/`](alzheimer/README.md)             | Classifying Alzheimer's disease diagnosis from MRI volumetric measurements, cognitive scores, and clinical data. Iterative workflow across notebooks, from a baseline Random Forest to feature-selected, tuned models. | Complete |
| [`smartphone-addiction/`](smartphone-addiction/README.md) | Kaggle Playground Series (S6E8) binary classification competition predicting smartphone addiction from behavioral and demographic features. Sklearn preprocessing pipelines, model comparison (Logistic Regression, Random Forest, XGBoost), hyperparameter tuning. | Complete |

## Structure

Each project follows roughly the same shape:

```text
<project-name>/
├── data/            # raw/processed data (may be gitignored per-project)
├── *.ipynb           # notebook(s), often numbered for iterative steps
├── README.md         # project-specific writeup: goal, approach, results
└── requirements.txt  # project-specific dependencies, if they diverge from the rest
```
