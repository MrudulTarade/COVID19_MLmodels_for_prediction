# Predicting COVID-19 Daily Deaths and Excess Mortality for Omicron Wave (Jan 2022)

> The following repository contains a project created for university assessment.
It contains ML model comparison for **Predicting COVID-19 Daily Deaths and Excess Mortality for Omicron Wave (Jan 2022)** based on previous years' data (2020 -21).
> Countries: United Kingdom, Germany, Switzerland
> City: Hamburg

## Dataset
- Source: Our World In Data (COVID19), RKI Data provided by Professor.
- Rows: 535,365 (OWID) / 4,640 (RKI)
- Columns: 61 / 13
- Target Variable: new_deaths , excess_mortality

## Technologies Used
- Python
- NumPy
- Pandas
- Matplotlib / Seaborn
- Scikit-learn
- Jupyter Notebook

## Data Preprocessing
- Splitting the data according to the time frame. (Test = Jan 2020 - Dec 2021 , Test = Jan 2022)
- Separating data according to regions, features and target variables
- Removing NaN values
- Using lagged data (14 day lag) for all feature variables

## Models Implemented
- Logistic Regression
- Random Forest
- XGBoost

## Metrics observed
- Accuracy
- MAPE
- MAE 
- R² / RMSE

# Made by
## Mrudul Madhukar Tarade
www.linkedin.com/in/mrudul-tarade-b4270a279
