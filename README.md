# Air Quality Forecasting with FBProphet

## Project Overview
This project focuses on forecasting air quality metrics using historical data from the 'Air Quality' dataset. The primary goal is to demonstrate data processing techniques, handle missing values, and apply the FBProphet library for time series forecasting.

## Dataset
The dataset used is the 'Air Quality' dataset, available from the UCI Machine Learning Repository: [https://archive.ics.uci.edu/ml/datasets/air+quality](https://archive.ics.uci.edu/ml/datasets/air+quality)

### Key Features:
*   **Date** and **Time**: Timestamp information.
*   **CO(GT)**: True hourly averaged concentration of CO in mg/m^3 (reference analyzer).
*   **PT08.S1(CO)**: Tin Oxide sensor response (CO).
*   **NMHC(GT)**: True hourly averaged overall Non-Methane HydroCarbons concentration in \mu g/m^3 (reference analyzer).
*   **C6H6(GT)**: True hourly averaged Benzene concentration in \mu g/m^3 (reference analyzer).
*   **PT08.S2(NMHC)**: Titania sensor response (NMHC).
*   **NOx(GT)**: True hourly averaged NOx concentration in ppb (reference analyzer).
*   **PT08.S3(NOx)**: Tungsten Oxide sensor response (NOx).
*   **NO2(GT)**: True hourly averaged NO2 concentration in \mu g/m^3 (reference analyzer).
*   **PT08.S4(NO2)**: Tungsten Oxide sensor response (NO2).
*   **PT08.S5(O3)**: Indium Oxide sensor response (O3).
*   **T**: Temperature in Â°C.
*   **RH**: Relative Humidity (%).
*   **AH**: Absolute Humidity (g/m^3).

## Data Processing and Cleaning
The following steps were performed to prepare the data for forecasting:

1.  **Loading Data**: The dataset was loaded using pandas, specifying the semicolon (`;`) as a separator and comma (`,`) as the decimal indicator.
2.  **Removing Unnamed Columns**: Two unnamed columns at the end of the dataset were identified and removed as per the dataset documentation.
3.  **Handling Corrupted Data**: Rows after index 9357 were discarded as they were indicated to be corrupted in the dataset documentation.
4.  **Missing Value Imputation**: 
    *   Missing values were explicitly marked as `-200` in the dataset. These were replaced with `np.nan`.
    *   All `np.nan` values were then imputed using the mean of their respective columns.
5.  **Date and Time Conversion**: 
    *   The 'Date' column was converted to datetime objects, specifically handling the `DD/MM/YYYY` format.
    *   The 'Time' column's format (e.g., `18.00.00`) was standardized to `HH:MM:SS`.
    *   'Date' and 'Time' columns were combined into a single datetime column (`ds`), which is required by FBProphet.
6.  **Target Variable Selection**: The 'RH' (Relative Humidity) column was chosen as the target variable for forecasting and assigned to the `y` column as required by FBProphet.

## Forecasting with FBProphet

### Model Setup:
*   An FBProphet model was initialized with `yearly_seasonality=False`. (Note: Given the dataset only spans about a year, `yearly_seasonality` might be difficult to infer, though for longer datasets, it's typically set to `True`).
*   The model was fitted using the processed `data` DataFrame, which contains the `ds` (datestamp) and `y` (target variable) columns.

### Prediction:
*   A future DataFrame was created to forecast 365 days into the future, at a daily frequency (`freq='D'`).
*   The `model.predict()` function was used to generate forecasts, which include `yhat` (the predicted value), `yhat_lower`, and `yhat_upper` (the lower and upper bounds of the uncertainty interval).

### Visualization:
*   The `model.plot(forecast)` function was used to visualize the historical data and the forecasted values, along with their uncertainty intervals.
*   `model.plot_components(forecast)` was used to display the individual components of the forecast (e.g., trend, weekly seasonality, etc.).

This project provides a comprehensive example of preparing a real-world time series dataset and applying a powerful forecasting library like FBProphet to predict future trends.
