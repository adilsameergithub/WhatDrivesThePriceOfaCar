# What Drives the Price of a Car?
### Used Car Price Analysis — Summary of Findings

**Author:** Adil Sameer  
**Notebook:** `prompt_II_Adil_completed.ipynb`  
**Dataset:** `vehicles.csv` — 426,880 used car listings (Kaggle) - **uploaded as a zip file due to size limitations, please unzip before loading the file in the notebook.**   
**Framework:** CRISP-DM

---

## Project Overview

This project uses regression modeling to identify the key factors that influence used car resale prices, with the goal of providing actionable inventory and pricing recommendations to a used car dealership.

---

## Methodology (CRISP-DM)

| Phase | Summary |
|---|---|
| **Business Understanding** | Framed as a regression problem: predict used car price from observable vehicle attributes |
| **Data Understanding** | Explored 426k listings; identified significant missing data (size 72%, cylinders 42%, condition 41%) and price outliers |
| **Data Preparation** | Filtered price ($500–$150k), year (1980–2023), odometer (≤400k miles); engineered `age` feature; log-transformed price |
| **Modeling** | OLS Linear Regression (baseline), Ridge with GridSearchCV, Lasso with GridSearchCV |
| **Evaluation** | All three models achieved MAE ≈ $6,847; Lasso coefficients interpreted for feature importance |
| **Deployment** | Findings translated into concrete dealer recommendations |

---

## Techniques Used

- **OLS Linear Regression** — multiple regression with non-numeric features
- **Ridge Regression + GridSearchCV** — L2 regularization, automated alpha tuning
- **Lasso Regression + GridSearchCV** — L1 regularization with feature shrinkage
- **StandardScaler** — required for regularized models
- **sklearn Pipelines + ColumnTransformer** — clean preprocessing chains
- **5-fold cross-validation** — model validation and hyperparameter selection

---

## Evaluation Metric: MAE (Mean Absolute Error)

**MAE** was chosen as the primary metric because:
- It is directly interpretable in **dollar terms** — "predictions are off by $X on average"
- It is **robust to outliers** — unlike MSE/RMSE, it does not square errors
- It is the most **business-friendly** metric for a dealership audience

---

## Key Findings

### Top Price Drivers
*Ranked by Lasso coefficient magnitude*

1. **Vehicle Age** — strongest predictor; sharp depreciation in years 1–8
2. **Odometer Reading** — higher mileage consistently lowers price
3. **Cylinders / Engine Size** — V6/V8 trucks and SUVs price highest
4. **Fuel Type** — diesel trucks and hybrids command premiums
5. **Title Status** — clean title is essential; salvage/rebuilt depresses price significantly
6. **Transmission** — automatic commands a small consistent premium

---

## Model Performance

| Model | CV MAE (log) | Test MAE (log) | Test MAE ($) |
|:---|:---:|:---:|:---:|
| OLS Linear Regression | 0.4313 ± 0.0023 | 0.4344 | $6,847 |
| Ridge (α=0.01, GridSearchCV) | 0.4313 | 0.4344 | $6,847 |
| Lasso (α=0.0001, GridSearchCV) | 0.4313 | 0.4344 | $6,847 |

All three models perform equally — predictions are within **$6,847 on average**. Against a median sale price of ~$15,900, this provides a useful pricing anchor. Adding `age²` as a polynomial feature is the recommended next modeling step to reduce MAE further.

---

## Recommendations for Dealers

- **Stock** 3–8 year old V6/V8 trucks/SUVs with under 80k miles and clean titles
- **Prefer automatic transmission** and diesel/hybrid fuel types when available
- **Avoid salvage/rebuilt titles** — very difficult to price competitively
- **Deploy the Ridge model** as a real-time pricing tool — average error of ~$6,847

---

## Files

```
├── prompt_II_Adil_completed.ipynb   ← Complete Jupyter notebook
├── vehicles.csv                     ← Dataset (426,880 listings)
└── README.md                        ← This file
```
