# Conveyor Fault Prediction

A multiclass machine learning project for predicting conveyor fault types from sensor-based operational data.

## Overview

This project analyzes a structured industrial dataset to classify different conveyor faults using machine learning. The workflow combines exploratory data analysis, statistical feature assessment, dimensionality reduction, model comparison, interpretability analysis, and robustness checks.

The main goal is to determine whether sensor variables such as speed, load, temperature, vibration, and current can reliably distinguish among multiple fault categories, and to identify which modeling strategy performs best.

## Problem Statement

Conveyor systems are critical in industrial operations, and fault detection is essential to reduce downtime, improve maintenance planning, and prevent costly failures. In this project, the task is formulated as a **multiclass classification problem**, where each sample must be assigned to one of several fault categories based on sensor measurements.

The project compares models trained on the original feature space against models trained on a reduced PCA representation in order to evaluate the trade-off between predictive performance and dimensionality reduction.

## Dataset

- **Source:** [Kaggle - Operational Conveyor Fault Dataset](https://www.kaggle.com/datasets/ziya07/operational-conveyor-fault-dataset)
- **Samples:** 1,209
- **Features:** 5 numerical sensor variables
- **Target:** fault type
- **Missing values:** none
- **Class balance:** nearly perfectly balanced

### Target Classes

- belt slippage  
- ball bearing  
- idler roller fault  
- pulley  
- drive motor  
- central shaft  

### Input Features

- `speed_rpm`
- `load_kg`
- `temperature_c`
- `vibration_ms2`
- `current_a`

## Project Workflow

The project is organized into the following stages:

1. **Exploratory Data Analysis**
   - dataset inspection
   - class distribution analysis
   - descriptive statistics
   - feature distributions and outlier inspection

2. **Feature Analysis**
   - correlation heatmap
   - ANOVA across fault classes
   - class-wise boxplots and scatter plots

3. **Dimensionality Reduction**
   - PCA
   - explained variance analysis
   - PCA loadings interpretation
   - comparison of original features vs PCA-transformed data

4. **Modeling**
   - Random Forest
   - XGBoost
   - Neural Network
   - hyperparameter tuning with `RandomizedSearchCV`

5. **Evaluation**
   - accuracy
   - precision
   - recall
   - F1-score
   - confusion matrices
   - class-level error analysis

6. **Interpretability and Robustness**
   - permutation importance
   - ablation study
   - misclassification analysis

## Main Results

The best-performing model was **XGBoost trained on the original feature space**.

### Test Set Performance

| Model | Data Representation | Accuracy | Macro F1 |
|------|----------------------|----------|----------|
| XGBoost | Raw features | **0.9050** | **0.9021** |
| Random Forest | Raw features | 0.8967 | 0.89 |
| Neural Network | Raw features | 0.8595 | 0.86 |
| Random Forest | PCA (2 components) | 0.8099 | 0.80 |
| Neural Network | PCA (2 components) | 0.7851 | 0.78 |
| XGBoost | PCA (2 components) | 0.7521 | 0.74 |

### PCA Findings

PCA showed that the first principal component explains **75.37%** of the variance, while the first two components explain **85.92%**. Although PCA was useful for visualization and dimensionality reduction, models trained on only two principal components consistently underperformed compared with models trained on the original variables.

## Key Insights

- The dataset contains a clear and meaningful relationship between sensor behavior and fault type.
- **Vibration** was the most informative variable, followed by **load**, **current**, and **temperature**.
- **Speed** was the least informative feature.
- The ablation study showed that removing speed had almost no impact on performance, while using only one feature caused a strong drop in predictive quality.
- The model’s errors were concentrated in a few specific class pairs, especially:
  - **ball bearing → pulley**
  - **pulley → idler roller fault**
  - **ball bearing → belt slippage**

These patterns suggest that some fault types have partially overlapping sensor signatures.

## Ablation Study Summary

| Feature Set | Accuracy | Macro F1 |
|------------|----------|----------|
| all_features | **0.9050** | **0.9021** |
| without_speed | 0.9050 | 0.9020 |
| top_3 | 0.8306 | 0.8252 |
| only_vibration | 0.6240 | 0.5734 |
| only_current | 0.5620 | 0.5240 |

This confirms that the best results are obtained when the model uses the **full feature set**, even though speed contributes relatively little.

## Repository Structure

```bash
Data_Science_Projects/
└── conveyor_fault_prediction/
    ├── data/
    ├── models/
    │   ├── nn_model.h5
    │   ├── nn_model_pca.h5
    │   ├── rf_model.pkl
    │   ├── rf_model_pca.pkl
    │   ├── xgb_model.pkl
    │   └── xgb_model_pca.pkl
    ├── src/
    │   └── main.ipynb
    └── requirements.txt
    