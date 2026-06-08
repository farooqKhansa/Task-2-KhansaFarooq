# Task 2 — Short-Term Stock Price Prediction

Predict the **next day's closing price** of a chosen stock from its historical market data, using regression models trained on Open / High / Low / Close / Volume features.

---

## Overview

This project fetches historical stock data from Yahoo Finance via the `yfinance` library and frames a short-term forecasting problem: *given today's market data, what will tomorrow's closing price be?* It trains and compares two regression models and visualizes how closely their predictions track reality.

Because stock data is a **time series**, the project uses a chronological train/test split (train on the past, test on the most recent period) rather than a random shuffle — preventing look-ahead leakage.

## Objectives

- Select a stock (default: Apple / `AAPL`).
- Download historical price data via an API.
- Predict the next `Close` from `Open`, `High`, `Low`, `Volume`, and engineered features.
- Train and compare **Linear Regression** and **Random Forest** models.
- Plot actual vs predicted prices.

## Features

| Step | What it does |
|------|--------------|
| **Fetch** | Downloads adjusted OHLCV data with `yfinance`; flattens single-ticker column structure |
| **Feature engineering** | Adds daily range, open–close change, and 5- / 10-day moving averages |
| **Target** | Sets the label to the *next* day's `Close` (`Close.shift(-1)`) |
| **Split** | Chronological split — no shuffling — to respect time ordering |
| **Models** | Linear Regression and Random Forest Regressor |
| **Metrics** | MAE, RMSE, and R² for each model |
| **Plots** | Actual vs predicted closing prices; Random Forest feature importance |

## Requirements

```bash
pip install yfinance pandas numpy scikit-learn matplotlib
```

> Requires an internet connection to reach Yahoo Finance.

## Usage

```bash
# Default: Apple, 3 years of history
python task2_stock_prediction.py

# Custom ticker and period
python task2_stock_prediction.py --ticker TSLA --period 5y

# Adjust the test fraction (most-recent slice held out)
python task2_stock_prediction.py --ticker MSFT --test-size 0.15
```

### Arguments

| Flag | Default | Description |
|------|---------|-------------|
| `--ticker` | `AAPL` | Stock symbol (e.g. `TSLA`, `MSFT`, `GOOGL`) |
| `--period` | `3y` | History window (`1y`, `3y`, `5y`, `max`, …) |
| `--test-size` | `0.2` | Fraction of the most recent data used for testing |

## Output

| File | Description |
|------|-------------|
| `<TICKER>_actual_vs_predicted.png` | Actual vs predicted closing prices over the test period |
| `<TICKER>_feature_importance.png` | Random Forest feature importances |

Console output reports MAE, RMSE, and R² for each model and names the best performer by RMSE.

## Notes & Limitations

- This is an **educational** short-horizon model, **not** financial advice. Markets are noisy and only weakly predictable; treat results as a modeling exercise.
- Lagged price features (like `Close` and moving averages) carry most of the predictive signal — expect them to dominate feature importance.
- For more realistic evaluation, consider walk-forward validation and additional features (returns, volatility, technical indicators).

## Skills Demonstrated

- Time-series data handling
- Regression modeling and model comparison
- Data fetching via the `yfinance` API
- Plotting predictions against real data
