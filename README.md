# Time Series Forecasting — Final Project

## Overview

This project focuses on forecasting the next 48 hours of electricity generation using historical time series data and weather information.

The objective is to develop a forecasting model that improves upon the Seasonal Naive benchmark in terms of point forecast accuracy while producing reliable 95% prediction intervals.

## Data

The project uses the following datasets:

- `train.csv` — historical time series data, including electricity generation and weather-related variables.
- `weather_forecast.csv` — weather forecasts for the next 48 hours, used as input features for the final prediction.

The target variable is electricity generation (`y`).

## Methodology

The forecasting approach combines seasonal patterns, historical observations, and weather information.

The main steps are:

1. Data preprocessing and preparation.
2. Feature engineering using calendar features, cyclical time encoding, and historical lags.
3. Seasonal baseline construction using the previous 24 hours.
4. CatBoost regression to model residuals and improve the baseline forecast.
5. Rolling validation using multiple historical forecasting windows.
6. Evaluation against the Seasonal Naive benchmark.
7. Calibration of 95% prediction intervals using empirical validation residuals.
8. Generation of the final 48-hour forecast.

## Model

The proposed model is:

**Seasonal Regression + CatBoost Residuals**

The model uses the previous 24-hour observation as a seasonal baseline and predicts the residual correction using CatBoost.

Weather variables used as model features include:

- Cloud cover
- Temperature
- Wind speed
- Pressure
- Humidity
- Daylight
- Air density

The final prediction is calculated as:

\[
\hat{y}_t = y_{t-24} + \widehat{e}_t
\]

where \(\widehat{e}_t\) is the predicted residual.

## Evaluation

The model is evaluated using:

- **RMSE** — Root Mean Squared Error
- **MAPE** — Mean Absolute Percentage Error
- **Winkler Score** — evaluates prediction interval quality
- **Coverage** — proportion of actual values contained within the prediction intervals

Lower values are better for RMSE, MAPE, and Winkler Score. Higher coverage indicates that more actual observations fall within the prediction intervals.

The model is compared with the Seasonal Naive benchmark.

## Results

The model achieved the following results on the 48-hour evaluation period:

| Model | RMSE | MAPE | Winkler | Coverage |
|---|---:|---:|---:|---:|
| Seasonal Naive | 120.404 | 51.088 | 568.464 | 1.000 |
| Seasonal Regression + CatBoost Residuals | 102.524 | 39.629 | 768.133 | 1.000 |

The proposed model improves both point forecasting metrics:

- RMSE is reduced by approximately 14.85%.
- MAPE is reduced by approximately 22.43%.

However, the Winkler Score is higher than that of the Seasonal Naive benchmark, indicating that the prediction intervals require further improvement.

## Final Forecast

The final model is fitted using the complete historical dataset and generates forecasts for the next 48 hours.

The output file is:

`final_forecast.csv`

It contains the following columns:

- `ds` — forecast timestamp
- `forecast` — predicted electricity generation
- `lower_95` — lower bound of the 95% prediction interval
- `upper_95` — upper bound of the 95% prediction interval

## Repository Structure

```text
time_series_final_project/
├── Nosulko_project.ipynb
├── Nosulko_report.pdf
├── train.csv
├── weather_forecast.csv
└── README.md
```

## How to Run

1. Clone the repository:

   ```bash
   git clone https://github.com/mnosulko/time_series_final_project.git
   ```

2. Open the project directory:

   ```bash
   cd time_series_final_project
   ```

3. Install the required Python packages:

   ```bash
   pip install pandas numpy scikit-learn catboost matplotlib
   ```

4. Open the notebook:

   ```bash
   jupyter notebook Nosulko_project.ipynb
   ```

5. Run all notebook cells in order.

The notebook reads the input datasets, trains and evaluates the forecasting model, and generates the final forecast.

## Reproducibility

The model uses a fixed random seed to ensure reproducibility.

All data files required to run the notebook are included in the repository.

## Use of LLMs

ChatGPT was used as an auxiliary tool during the development of this project. It assisted with model selection, feature engineering, code implementation, debugging, and documentation. The model evaluation, interpretation of results, and final decisions were reviewed by Mariia Nosulko.
