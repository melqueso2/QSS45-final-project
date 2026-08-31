# Resource Wealth Without Water?
## Natural Resource Dependence and Drinking Water Access Across Africa

**Author:** Melton Sawadogo  
**Course:** QSS 45 — AI & Machine Learning for Social Science

## Project Overview

This project examines the relationship between natural-resource dependence and access to basic drinking water across African countries from 2000 to 2021.

The main research question is:

**How is dependence on natural-resource rents associated with access to basic drinking water across African countries, and can machine-learning models improve our understanding of this relationship beyond conventional linear regression?**

The final analytic dataset contains 1,092 country-year observations from 52 African countries.

I compare two modeling approaches:

1. Ordinary Least Squares (OLS)
2. XGBoost

I also use SHAP values to interpret the XGBoost model.

## Data

The project uses country-year indicators downloaded through Our World in Data. The underlying indicators come primarily from the World Bank and the WHO/UNICEF Joint Monitoring Programme.

The main variables are:

- Basic drinking-water access
- Total natural-resource rents as a percentage of GDP
- GDP per capita
- Electricity access
- Year

GDP per capita is log-transformed before modeling.

The analysis covers 2000 through 2021.

## Repository Structure

```text
QSS45-final-project/
├── README.md
├── code/
│   ├── 00_pull.ipynb
│   ├── 01_merge.ipynb
│   ├── 02_ols.ipynb
│   └── 03_xgboost_shap.ipynb
├── data/
│   ├── water.csv
│   ├── resource_rents.csv
│   ├── gdp.csv
│   ├── electricity.csv
│   └── analysis_data.csv
└── output/
    ├── OLSEstimate_Burkina.png
    ├── SHAPEstimate_Burkina.png
    ├── ols_coefficients.csv
    ├── ols_performance.csv
    ├── xgboost_performance.csv
    └── shap_importance.csv

## Notebook 00: Pull Data

### Input
Public Our World in Data CSV endpoints for drinking water, natural-resource rents, GDP per capita, and electricity access.

### Function
Downloads each source dataset and saves a local copy in the `data/` directory.

### Output
- `water.csv`
- `resource_rents.csv`
- `gdp.csv`
- `electricity.csv`

## Notebook 01: Merge and Clean Data

### Input
The four raw CSV files created by `00_pull.ipynb`.

### Function
Renames variables, identifies African countries, merges the datasets by country, country code, and year, restricts the sample to 2000–2021, checks missing values, and creates log GDP per capita.

Diagnostic information is printed before and after data merges.

### Output
- `analysis_data.csv`

Final dataset:
- 1,092 observations
- 52 African countries
- 2000–2021

## Notebook 02: OLS Analysis

### Input
`analysis_data.csv`

### Function
Uses a chronological train-test split, standardizes predictors using the training data, and estimates an OLS model with robust standard errors.

### Output
- `OLSEstimate_Burkina.png`
- `ols_coefficients.csv`
- `ols_performance.csv`

### Results
- Training R²: 0.726
- Test R²: 0.680
- Test RMSE: 8.87

## Notebook 03: XGBoost and SHAP

### Input
`analysis_data.csv`

### Function
Fits an XGBoost regression model using the same predictors and chronological train-test split as OLS. SHAP values are used to interpret model predictions.

### Output
- `SHAPEstimate_Burkina.png`
- `xgboost_performance.csv`
- `shap_importance.csv`

### Results
- Training R²: 0.936
- Test R²: 0.671
- Test RMSE: 8.99

SHAP feature importance ranks:
1. Electricity access
2. Log GDP per capita
3. Year
4. Natural-resource rents

## Main Takeaway

Greater natural-resource dependence is negatively associated with basic drinking-water access in the OLS specification, while infrastructure and economic development are stronger predictors.

XGBoost fits the training data much more closely than OLS but does not outperform the simpler OLS model on the held-out 2018–2021 observations.

These results describe associations and should not be interpreted as evidence that natural-resource extraction directly causes changes in drinking-water access.