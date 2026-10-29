# Airline Satisfaction

Binary classification project focused on predicting whether an airline passenger is satisfied with their flight experience.

## Overview
This project uses customer experience and demographic features to predict the target variable `satisfaction` in a tabular dataset. The notebook is currently in the data-preparation and modeling-setup phase, where the data is being explored, cleaned, and transformed before benchmarking classifiers.

## Dataset
The project loads separate train and test CSV files from the `data/` folder.

- Training set: 699,635 rows
- Test set: 299,844 rows
- Target: `satisfaction` (`True` / `False`)

The dataset includes flight-service ratings, delay information, trip details, and customer metadata such as age, gender, customer type, travel type, and class.

## Current stage of work
The notebook already includes the following work:

- Loading the training and test data
- Checking dataset shape and basic structure
- Inspecting columns, dtypes, and unique values
- Confirming the target variable and identifying feature groups
- Finding missing values in the dataset
- Preparing preprocessing pipelines for:
  - numeric features using median imputation and standard scaling
  - ordinal features such as `Class`
  - binary categorical features such as `Gender`, `Customer Type`, and `Type of Travel`
- Splitting the data into training and validation sets with stratification

The notebook also imports the main ML libraries used for the task, including:

- pandas and numpy
- matplotlib and seaborn
- scikit-learn preprocessing and modeling tools
- XGBoost

## Feature structure
The data contains a mix of:

- continuous variables: age, flight distance, delay times, service ratings
- ordinal categorical variables: flight class
- binary categorical variables: gender, customer type, travel type

This makes the project a good example of preprocessing mixed-type tabular data for classification models.

## Status
In progress.

The current notebook has completed the exploratory analysis and built the preprocessing foundation. The next step is to train baseline models, compare performance, and tune the best-performing approach (likely Logistic Regression, Random Forest, or XGBoost) using validation metrics such as ROC-AUC.
