# NIFTY 50 Volatility Forecasting using GARCH Model

## Project Overview

This project aims to analyze the historical NIFTY 50 index data, identify volatility clustering, and forecast future volatility using a GARCH (Generalized Autoregressive Conditional Heteroskedasticity) model. Financial time series often exhibit heteroskedasticity, meaning their volatility changes over time. GARCH models are particularly suited for capturing such dynamics.

## Table of Contents

1.  [Data Acquisition and Pre-processing]
2.  [Exploratory Data Analysis (EDA)]
3.  [Stationarity Test (ADF Test)]
4.  [ARCH Effects Test]
5.  [GARCH Model Fitting and Forecasting]

## 1. Data Acquisition and Pre-processing

**Objective**: To download historical NIFTY 50 data and prepare it for analysis.

*   **Source**: NIFTY 50 index data is downloaded using the `yfinance` library for the period from 2010-01-01 to 2023-12-31.
*   **Data Extraction**: Only the 'Close' price is extracted.
*   **Log Returns Calculation**: Daily log returns are calculated using the formula: `log(P_t / P_{t-1})`. Log returns are preferred in financial modeling as they are approximately symmetric and additive over time.
*   **Missing Values**: Rows with NaN values (introduced by the `shift` operation) are dropped.

## 2. Exploratory Data Analysis (EDA)

**Objective**: To understand the characteristics of NIFTY 50 prices and log returns.

*   **Close Price Plot**: Visualizes the NIFTY 50 close price over the entire period to observe overall trends.
*   **Log Returns Plot**: Shows the daily log returns, which typically exhibit volatility clustering (periods of high volatility followed by periods of low volatility).
*   **Descriptive Statistics**: Provides summary statistics (mean, std, min, max, quartiles) for the log returns, giving insights into their distribution.
*   **Distribution Plot**: A histogram with a Kernel Density Estimate (KDE) is used to visualize the distribution of log returns, often revealing fat tails (leptokurtosis) compared to a normal distribution.
*   **Autocorrelation and Partial Autocorrelation (ACF/PACF) Plots**: These plots help identify the presence of autocorrelation in the log returns and squared log returns, which is crucial for GARCH modeling. For log returns, we typically expect little to no significant autocorrelation, but for squared log returns, significant autocorrelation indicates ARCH effects.

## 3. Stationarity Test (ADF Test)

**Objective**: To determine if the log return series is stationary.

*   **Augmented Dickey-Fuller (ADF) Test**: This statistical test is used to check for the presence of a unit root in the time series. A stationary series has statistical properties that do not change over time (mean, variance, and autocorrelation structure are constant).
*   **Result**: The NIFTY 50 log returns series is found to be stationary (p-value < 0.05), which is a prerequisite for applying GARCH models to the returns.

## 4. ARCH Effects Test

**Objective**: To determine if there are ARCH effects present in the log return series.

*   **ARCH-LM Test (Lagrange Multiplier Test)**: This test checks for autoregressive conditional heteroskedasticity. The null hypothesis is that there are no ARCH effects.
*   **Result**: The test indicates the presence of significant ARCH effects (p-value < 0.05), suggesting that a GARCH model is appropriate for modeling the volatility of NIFTY 50 log returns.

## 5. GARCH Model Fitting and Forecasting

**Objective**: To fit a GARCH(1,1) model to the log returns and forecast future volatility.

*   **Model Specification**: A GARCH(1,1) model is chosen for its simplicity and effectiveness in capturing volatility dynamics. The `arch_model` function from the `arch` library is used.
    *   `mean='Constant'`: Assumes a constant mean for the log returns.
    *   `vol='GARCH', p=1, q=1`: Specifies a GARCH(1,1) volatility model.
    *   `dist='StudentsT'`: The Student's T-distribution is often preferred for financial returns due to its ability to account for fat tails (leptokurtosis) better than a normal distribution.
    *   `rescale=True`: Automatically scales the data, which can aid optimization.
*   **Model Fitting**: The model is fitted to the `log_return` data.
*   **Model Summary**: The summary provides details about the estimated parameters (omega, alpha, beta, nu), their standard errors, t-statistics, and p-values, along with overall model fit statistics like Log-Likelihood, AIC, and BIC.
*   **Diagnostic Plots**: Plots of standardized residuals and conditional volatility are generated to assess the model's fit and identify any remaining patterns in the residuals.
*   **Volatility Forecasting**: The `forecast` method is used to predict conditional variance (and thus volatility) for a specified horizon (e.g., 30 days).
*   **Annualized Volatility**: The forecasted conditional variances are annualized (multiplied by 252 for trading days and square-rooted) to provide a more interpretable measure of future volatility.
```
git add README.md
git commit -m "Add detailed README"
git push
