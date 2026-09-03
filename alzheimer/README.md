# Alzheimer's Disease Detection using Machine Learning

A machine learning project exploring Alzheimer's disease classification using MRI volumetric measurements, cognitive assessment scores, and clinical information. The project demonstrates a complete end-to-end machine learning workflow, including data preprocessing, feature engineering, model training, evaluation, and model optimization.

## Project Overview

The goal of this project is to classify patients based on Alzheimer's disease diagnosis by combining data from multiple clinical datasets. Rather than building a single model, the project follows an iterative workflow where each notebook improves upon the previous one.

The workflow includes:

- Data cleaning
- Missing value handling
- Feature selection
- Categorical encoding
- Model training
- Model evaluation
- Hyperparameter tuning

## Dataset

The project combines three datasets:

- **MRI volumetric measurements**
- **Cognitive assessment scores**
- **Clinical diagnosis data**
- **Merged** - used for notebook 2

The datasets are merged using the patient identifier (`RID`) to create a single dataset for machine learning.

## Data Preprocessing

The preprocessing pipeline includes:

- Removing unnecessary features
- Converting incorrect data types
- Handling missing values
- Feature selection
- One-Hot Encoding of categorical variables
- Train-test split
- Preparing the data for machine learning models

## Machine Learning Pipeline

1. Load datasets
2. Merge datasets
3. Clean and preprocess data
4. Feature selection
5. Split into training and testing sets
6. Encode categorical variables
7. Train the model
8. Evaluate model performance
9. Optimize the model

## Technologies Used

- Python
- Pandas
- NumPy
- Scikit-learn
- Seaborn
- Matplotlib
- Jupyter Notebook

## Results

The project follows an iterative approach, where each notebook focuses on improving the previous model.

| Notebook                       | Description | Training Accuracy | Testing Accuracy |
|--------------------------------|-------------|------------------:|-----------------:|
| **01 - Baseline Model**        | Initial Random Forest using all selected features | **100.00%** |       **50.00%** |
| **02 - Model Improvement**     | Feature selection and improved preprocessing | **100.00%** |       **80.60%** |
| **02 - Hyperparameter Tuning** | Optimized Random Forest | **—** |       **78.88%** |

The second notebook demonstrates that improving the preprocessing pipeline and reducing the feature space significantly improved the model's ability to generalize, increasing the testing accuracy from **50.00%** to **80.60%**.

## Future Improvements

Possible directions for further improving performance include:

- Hyperparameter tuning
- Cross-validation
- Recursive Feature Elimination (RFE)
- Dimensionality reduction (PCA)
- Better handling of class imbalance
- Testing additional machine learning algorithms
- Larger and more diverse datasets

## Repository Structure
```text
.
├── data/
│   ├── cognitive.csv
│   ├── data.csv
│   └── diagnosis.csv
│
├── notebooks/
│   ├── 01_baseline_model.ipynb
│   ├── 02_model_improvement.ipynb
│   └── 03_hyperparameter_tuning.ipynb
│
├── README.md
└── requirements.txt
```

## Key Takeaways

This project demonstrates an iterative machine learning workflow for Alzheimer's disease classification. Starting with a baseline model that suffered from severe overfitting, subsequent notebooks focus on improving preprocessing, reducing the feature space, and optimizing the model to achieve better generalization on unseen data.
