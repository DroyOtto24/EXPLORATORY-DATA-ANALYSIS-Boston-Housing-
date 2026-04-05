# 🏠 Boston Housing — Exploratory Data Analysis

 End-to-end EDA pipeline for the Boston Housing dataset built in Python. The notebook walks through data loading, quality assessment, cleaning, outlier detection, and univariate/bivariate analysis, finishing with a structured insights summary and actionable next steps for modelling.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Dataset](#dataset)
- [Project Structure](#project-structure)
- [EDA Pipeline](#eda-pipeline)
- [Key Findings](#key-findings)
- [Requirements](#requirements)
- [Usage](#usage)
- [Output Plots](#output-plots)
- [Next Steps](#next-steps)

---

## Overview

This project provides a reproducible, well-documented EDA workflow that can serve as a baseline for regression modelling tasks. Each section is implemented as a standalone Python function, making the pipeline modular and easy to extend.

---

## Dataset

**File:** `HousingData.csv`  
**Target:** `MEDV` — Median value of owner-occupied homes in $1,000s

| Feature | Description |
|---|---|
| `CRIM` | Per capita crime rate by town |
| `ZN` | Proportion of residential land zoned for large lots |
| `INDUS` | Proportion of non-retail business acres per town |
| `CHAS` | Charles River dummy variable (1 = bounds river) |
| `NOX` | Nitric oxide concentration (parts per 10 million) |
| `RM` | Average number of rooms per dwelling |
| `AGE` | Proportion of owner-occupied units built before 1940 |
| `DIS` | Weighted distances to Boston employment centres |
| `RAD` | Index of accessibility to radial highways |
| `TAX` | Full-value property-tax rate per $10,000 |
| `PTRATIO` | Pupil-teacher ratio by town |
| `B` | 1000(Bk − 0.63)² where Bk = proportion of Black residents |
| `LSTAT` | Percentage of lower-status population |
| `MEDV` | **Target** — Median home value in $1,000s |

---

## Project Structure

```
├── Exploratory_Data_Analysis.ipynb   # Main notebook
├── HousingData.csv                   # Dataset
├── outputs/                          # Auto-generated plots
│   ├── 01_missing_values_heatmap.png
│   ├── 02_boxplots_all_features.png
│   ├── 03_histograms_kde_all_features.png
│   ├── 04_correlation_matrix.png
│   ├── 05_scatter_top_features.png
│   └── 06_categorical_analysis.png
└── README.md
```

---

## EDA Pipeline

The notebook is structured as a sequential pipeline of eight modular functions:

1. **Data Loading** — Reads the CSV and reports shape, memory usage, and a data preview.
2. **Data Quality Assessment** — Checks data types, missing values, and duplicate rows; renders a missing-values heatmap.
3. **Data Cleaning** — Imputes numerical columns with the median (robust to outliers) and categorical columns with the mode; drops exact duplicate rows.
4. **Outlier Detection** — Applies the IQR method to flag outliers per feature; renders boxplots for all features. Outliers are *flagged, not removed*, since extreme housing values can be legitimate.
5. **Univariate Analysis** — Plots histograms with KDE overlays and annotates mean/median for every numerical feature; reports skewness for each.
6. **Bivariate Analysis** — Generates a Pearson correlation heatmap and scatter plots with regression trend lines for the top-4 features most correlated with `MEDV`.
7. **Categorical / Grouped Analysis** — Compares `MEDV` distributions across the `CHAS` binary variable and visualises the `RAD` highway accessibility index frequency.
8. **EDA Summary & Key Insights** — Prints a structured report covering strongest predictors, outlier flags, distribution shapes, the Charles River premium, and multicollinearity warnings.

---

## Key Findings

- **Strongest positive predictor:** `RM` (rooms per dwelling) — more rooms correlates strongly with higher home values.
- **Strongest negative predictors:** `LSTAT` (lower-status population %) and `NOX` (pollution) — both correlate with lower `MEDV`.
- **Charles River premium:** Homes adjacent to the Charles River (`CHAS = 1`) command a meaningfully higher median value than non-adjacent properties.
- **Skewed features:** `CRIM`, `ZN`, and `B` show strong right skew — log-transformation is recommended before modelling.
- **Multicollinearity:** `TAX` and `RAD` are highly correlated (r ≈ 0.91), which may destabilise linear regression models. Regularisation (Ridge/Lasso) is advised.

---

## Requirements

```
Python >= 3.8
numpy
pandas
matplotlib
seaborn
scipy
```

Install dependencies:

```bash
pip install numpy pandas matplotlib seaborn scipy
```

---

## Usage

1. Clone the repository and place `HousingData.csv` in the root directory.
2. Open `Exploratory_Data_Analysis.ipynb` in Jupyter or VS Code.
3. Run all cells — the pipeline executes automatically via `main()`.
4. All plots are saved to the `outputs/` directory.

```bash
jupyter notebook Exploratory_Data_Analysis.ipynb
```

---

## Output Plots

| File | Description |
|---|---|
| `01_missing_values_heatmap.png` | Heatmap showing location of missing values |
| `02_boxplots_all_features.png` | Boxplots for outlier visualisation across all features |
| `03_histograms_kde_all_features.png` | Histograms + KDE overlays for all numerical features |
| `04_correlation_matrix.png` | Pearson correlation heatmap (lower triangle) |
| `05_scatter_top_features.png` | Scatter plots + regression lines for top-4 predictors |
| `06_categorical_analysis.png` | MEDV by CHAS and RAD frequency distribution |

---

## Next Steps

- **Feature Engineering:** Log-transform right-skewed features (`CRIM`, `ZN`, `B`) to improve model fit.
- **Modelling:** Start with a Linear Regression baseline, then benchmark against Random Forest and XGBoost for non-linear patterns.
- **Validation:** Use k-fold cross-validation (k = 5 or 10) to ensure robust evaluation.
- **Metrics:** Evaluate on a held-out test set using RMSE, MAE, and R².
