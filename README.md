# COVID-19 Daily Deaths and Excess Mortality Prediction — Omicron Wave (Jan 2022)

> Machine Learning project for predicting **COVID-19 daily deaths and excess mortality during the January 2022 Omicron wave**, using historical COVID-19 data from 2020–2021.

This project compares multiple machine learning regression models and evaluates their ability to forecast COVID-19 mortality-related outcomes across different geographical regions.

### Regions

* 🇬🇧 United Kingdom
* 🇩🇪 Germany
* 🇨🇭 Switzerland
* 🇩🇪 Hamburg, Germany

### Prediction Targets

* COVID-19 daily deaths
* Excess mortality

---

## Project Objective

The objective of this project was to investigate whether historical COVID-19 indicators could be used to predict mortality outcomes during the **January 2022 Omicron wave**.

The models were trained using historical data from 2020–2021 and evaluated on data from **January 2022**, providing a time-based forecasting setup rather than a random train/test split.

The project focuses on:

* Time-based model evaluation
* Feature engineering using lagged variables
* Comparison of baseline and ensemble regression models
* Model performance using MAE, RMSE, R² and MAPE
* Feature importance analysis
* Understanding model generalisation and overfitting

---

## Dataset

### Our World in Data

COVID-19 data was obtained from **Our World in Data (OWID)**.

The complete OWID dataset is too large to include in this repository, so a compact/sample dataset is provided where applicable.

* **Source:** Our World in Data — COVID-19 dataset
* **Rows:** 535,365
* **Columns:** 61
* **Main variables:** COVID-19 cases, deaths, hospitalisation, ICU patients, vaccination, government response indicators and excess mortality

### RKI Dataset

Regional data for Hamburg was provided through the university project.

* **Rows:** 4,640
* **Columns:** 13
* **Source:** RKI data provided for the university assessment

---

## Prediction Setup

The project uses a chronological train/test split.

```text
Historical COVID-19 data
        │
        │  2020–2021
        ▼
   Training Data
        │
        ▼
Machine Learning Models
        │
        ▼
 January 2022
   Test Period
        │
        ▼
 Predicted Mortality
```

### Training Period

Historical observations from 2020–2021 were used for model training.

### Test Period

The models were evaluated on:

**January 2022 — Omicron wave**

This approach prevents future observations from being randomly mixed into the training data.

---

## Data Preprocessing

The notebooks perform several preprocessing and feature-engineering steps:

* Converted date columns to datetime format
* Filtered data by geographical region
* Selected relevant COVID-19 variables
* Removed or handled missing values
* Created lagged features
* Created rolling averages
* Calculated case fatality rate (CFR)
* Created mortality trend features
* Added vaccination-related features
* Added hospital and ICU indicators
* Used a 14-day lag to represent delayed relationships between COVID-19 indicators and mortality

### Example engineered features

* `cases_lag_14`
* `hosp_lag_7`
* `hosp_lag_14`
* `deaths_trend_14`
* `cases_trend_7`
* `cases_rolling_7`
* `cfr`
* `vaccination_rate`
* `icu_patients`
* `hosp_patients`

For the UK model, additional experimental features included growth rates, acceleration and hospital-to-death ratios.

---

## Models Implemented

The project compares several regression models.

### 1. Linear Regression

Used as the baseline model.

### 2. Decision Tree Regressor

Different tree depths were evaluated to investigate overfitting and generalisation.

### 3. Random Forest Regressor

An ensemble of decision trees was used to improve robustness and reduce the variance of individual trees.

### 4. XGBoost Regressor

Gradient boosting was evaluated as an additional ensemble approach.

---

## Evaluation Metrics

The models were evaluated using:

| Metric   | Description                                                    |
| -------- | -------------------------------------------------------------- |
| **MAE**  | Mean Absolute Error — average absolute prediction error        |
| **RMSE** | Root Mean Squared Error — penalises larger errors more heavily |
| **R²**   | Coefficient of determination — measures explained variance     |
| **MAPE** | Mean Absolute Percentage Error                                 |

For MAE, RMSE and MAPE:

**Lower is better.**

For R²:

**Higher is better.**

---

# Results

## 🇬🇧 United Kingdom — COVID-19 Daily Deaths

The UK notebook predicts **daily COVID-19 deaths** using historical COVID-19 indicators.

The processed dataset contained **493 observations**.

### Model Performance

| Model                 |      MAE ↓ |     RMSE ↓ |       R² ↑ |     MAPE ↓ |
| --------------------- | ---------: | ---------: | ---------: | ---------: |
| **Linear Regression** | **47.189** | **52.482** | **-0.484** |     25.08% |
| Decision Tree         |    118.876 |    152.120 |    -11.466 |     61.74% |
| Random Forest         |     82.690 |    125.225 |     -7.448 |     39.52% |
| XGBoost               |     53.296 |     96.995 |     -4.068 | **24.89%** |

### Key Findings

* Linear Regression achieved the lowest **MAE (47.189)** and **RMSE (52.482)**.
* XGBoost achieved the lowest **MAPE (24.89%)**.
* XGBoost substantially outperformed Random Forest in MAE.
* All models produced negative R² values on the January 2022 test period, indicating that the forecasting problem was challenging and that the models did not explain the test-period variance particularly well.
* The results demonstrate that a more complex model does not automatically outperform a simpler baseline.

### Feature Importance

The most consistently important features across the evaluated models were:

1. `hosp_patients`
2. `vaccination_rate`
3. `icu_patients`

This suggests that healthcare-system indicators and vaccination coverage were important predictors in the UK mortality model.

---

# 🇨🇭 Switzerland — COVID-19 Daily Deaths

The Switzerland notebook predicts **daily COVID-19 deaths**.

The processed dataset contained **517 observations**.

### Model Performance

| Model                 |      MAE ↓ |     RMSE ↓ |       R² ↑ |     MAPE ↓ |
| --------------------- | ---------: | ---------: | ---------: | ---------: |
| **Linear Regression** | **10.169** | **11.210** | **-5.709** | **87.99%** |
| Decision Tree         |     29.015 |     32.686 |    -56.045 |    230.05% |
| Random Forest         |     19.012 |     20.619 |    -21.700 |    154.30% |
| XGBoost               |     17.447 |     18.919 |    -18.111 |    145.25% |

### Key Findings

* Linear Regression achieved the best performance across all reported evaluation metrics.
* Linear Regression had the lowest MAE at **10.169 deaths**.
* XGBoost performed better than Random Forest but remained behind the Linear Regression baseline.
* The negative R² values indicate that the models struggled to generalise to the January 2022 test period.
* The results highlight the importance of establishing a simple statistical baseline before selecting a more complex model.

### Feature Importance

The most important features based on average model ranking were:

1. `hosp_patients`
2. `deaths_trend_14`
3. `icu_patients`

These features consistently ranked above other candidate predictors.

---

# 🇩🇪 Germany — Excess Mortality

The Germany notebook focuses on predicting **excess mortality** during January 2022.

The model used the following features:

* `vaccination_rate`
* `deaths_trend_14`
* `new_deaths_lag_14`
* `stringency`

The processed dataset contained **1,110 observations** before the final time-based train/test split.

### Model Performance

| Model             | Train MAE | Test MAE ↓ | Test RMSE ↓ |  Test R² ↑ |
| ----------------- | --------: | ---------: | ----------: | ---------: |
| Linear Regression |     0.727 |  **1.723** |   **1.789** | **-5.233** |
| Random Forest     |     0.255 |      2.257 |       2.361 |     -9.856 |

An additional XGBoost experiment reported:

> **XGBoost Test MAE: 2.04**

The notebook's final comparison table did not include the full RMSE/R²/MAPE values for XGBoost, so they are not reported here.

### Decision Tree Experiment

The decision-tree depth experiment produced:

| Maximum Depth |  Test MAE |
| ------------: | --------: |
|             1 | **0.556** |
|             2 |     1.561 |
|             3 |     2.160 |
|             4 |     2.266 |
|             8 |     2.320 |
|            16 |     2.217 |
|            32 |     2.320 |
|          None |     2.320 |

The shallow tree with **depth = 1** produced the lowest test MAE in this experiment.

### Key Findings

* Linear Regression outperformed the configured Random Forest model.
* Random Forest achieved a much lower training error (**0.255**) but a higher test error (**2.257**), demonstrating a degree of overfitting.
* The depth experiment showed that increasing tree complexity did not improve generalisation.
* XGBoost achieved a test MAE of **2.04**, improving on the reported Random Forest result, although the available notebook output does not provide a complete set of XGBoost metrics.

---

# 🇬🇧 United Kingdom — Excess Mortality

The project also includes **excess mortality prediction for the United Kingdom**.


| Model             | MAE | RMSE | R²   | MAPE |
| ----------------- | --: | ---: | -:   | ---: |
| Linear Regression |0.62 |0.705 |0.655 |96.12 |
| Decision Tree     |0.93 |1.307 |-0.184|184.94|
| Random Forest     |1.17 |1.447 |-0.450|226.76|
| XGBoost           |1.00 |1.246 |-0.076|191.29|

---

# 🇨🇭 Switzerland — Excess Mortality

The project also includes **excess mortality prediction for Switzerland**.


| Model             | MAE | RMSE | R²   | MAPE |
| ----------------- | --: | ---: | -:   | ---: |
| Linear Regression |1.012|1.162 |-2.329|    — |
| Decision Tree     |0.468|    — |  —   |    — |
| Random Forest     |2.048|2.292 |-11.96|    — |
| XGBoost           |2.071|    - |  —   |    — |

---

# 🇩🇪 Hamburg — COVID-19 Daily Deaths

Regional prediction was also performed for **Hamburg, Germany**, using the provided RKI dataset.

> Detailed evaluation metrics for the Hamburg experiment will be added here from the corresponding notebook.

| Model             | MAE | RMSE | R²  | MAPE |
| ----------------- | --: | ---: | -:  | ---: |
| Linear Regression |53.24|77.839|-39.3|    — |
| Decision Tree     |64.94|70.022|-31.6|    — |
| Random Forest     |24.79|27.491|-4.03|    — |
| XGBoost           |28.59|31.087|-5.43|    — |

---

# Overall Findings

The experiments demonstrate several important observations:

### 1. More complex models did not always perform better

The results show that Random Forest and XGBoost did not consistently outperform Linear Regression.

For example:

* **UK:** Linear Regression achieved MAE = 47.189 compared with XGBoost = 53.296.
* **Switzerland:** Linear Regression achieved MAE = 10.169 compared with XGBoost = 17.447.
* **Germany excess mortality:** Linear Regression achieved MAE = 1.723 compared with Random Forest = 2.257.

This demonstrates why model selection should be based on validation performance rather than model complexity.

### 2. Tree depth strongly affected generalisation

The Decision Tree experiments showed that highly complex trees could achieve extremely low training errors while producing significantly larger test errors.

This provided an opportunity to investigate the **overfitting/generalisation trade-off**.

### 3. Healthcare indicators were important predictors

Features such as:

* Hospitalised patients
* ICU patients
* Death trends
* Vaccination rate

frequently appeared among the most important predictors.

### 4. January 2022 was a challenging forecasting period

The negative R² values obtained in several experiments indicate that predicting the January 2022 mortality behaviour from earlier pandemic waves was difficult.

Rather than hiding these results, the project reports them as part of the model evaluation.

---

# Visualisations

The notebooks include:

* Actual vs. predicted mortality
* Model comparison charts
* Decision-tree depth analysis
* Feature importance rankings
* Residual analysis
* Training/test period visualisation
* Model performance comparisons

These visualisations are used to investigate both prediction accuracy and model behaviour.

---

# Technologies Used

* **Python**
* **NumPy**
* **Pandas**
* **Matplotlib**
* **Seaborn**
* **Scikit-learn**
* **XGBoost**
* **Jupyter Notebook**

---

# Project Structure

```text
.
├── bundesland/
│   └── hamburg_covid_prediction_v1.5_reference.ipynb
|
├── deaths/
│   └── Switzerland.ipynb
│   └── project_v1.7_UK_deaths.ipynb
|
├── excess_mortality/
│   └── Switzerland.ipynb
|   └── project_1.5.2_Germany-wip.ipynb
|   └── project_v1.7_UK_EM.ipynb
│
│
├── README.md
└──rki_merged_all_ages_ENG.csv
```

> The complete OWID dataset is not included because of its size.

---

# Limitations

The project has several limitations:

* The models were trained on historical pandemic data and evaluated specifically on January 2022.
* COVID-19 reporting behaviour can vary by country and over time.
* Mortality is affected by many factors that are not captured in the available features.
* Some datasets contain missing values and different reporting frequencies.
* The negative R² values in several experiments indicate limited generalisation to the January 2022 test period.
* Model performance may change significantly when evaluated on different pandemic waves or time periods.

---

# Future Improvements

Possible improvements include:

* Hyperparameter optimisation using time-series cross-validation
* More extensive feature engineering
* Additional lag and rolling-window features
* Incorporating weather and demographic information
* Testing additional gradient-boosting models
* Testing dedicated time-series forecasting methods
* Automated model retraining
* Model monitoring and drift detection
* Deployment through an API or dashboard

---

# AI Usage

AI tools were used only for:

* Markdown/README creation
* Bug fixing
* Comments and documentation within notebooks

The machine learning experiments, preprocessing and model evaluation were performed as part of the project work.

---

# Author

## Mrudul Madhukar Tarade

[LinkedIn](http://www.linkedin.com/in/mrudul-tarade-b4270a279)

---

*University Machine Learning Project — COVID-19 Mortality and Excess Mortality Prediction*
