# ARIMA-Based Gold Price Forecasting & Trading Strategy

## Overview

This project implements a time series forecasting and algorithmic trading system based on the ARIMA model. 
It predicts next-day gold prices using historical data and generates long or short trading signals based on the direction of the forecast. 
The performance of the strategy is evaluated by backtesting it on unseen historical data.

## Methodology

### Data Processing

The dataset consists of daily gold prices from 2010 onwards, sourced from [Kaggle](https://www.kaggle.com/datasets/rizkykiky/gold-price-dataset). 
The time series is preprocessed by cleaning missing values, converting the date column into a proper time index, and applying time series decomposition to extract the underlying trend component. 
The final dataset is split into training and testing sets, with the last five years reserved for backtesting.

### Forecasting Model

The forecasting model is uses an ARIMA model. 
The optimal model parameters are selected using the `auto_arima` function, which minimises information criteria such as AIC and BIC. 
A walk-forward forecasting approach is used, where the model is updated iteratively with each new observation without full retraining. 
This allows the model to generate one-step-ahead forecasts for each trading day in the test period.

### Trading Strategy

The trading strategy is based on the direction of the forecasted price movement. 
A long position is taken when the predicted price is higher than the previous close, while a short position is taken otherwise. 
Each trade uses a fixed allocation of 10% of the cumulative portfolio value. 
The simulation begins with an initial capital of USD 1 mil.

### Backtesting Results

The results of backtesting the strategy are summarised below:

![PnL chart](images/output.png)

- Total return: **54.59%**
- Win rate: **63.75%**
- Profit factor: **3.37**
- Average win/loss ratio: **1.68**
- Sharpe ratio: **6.47**

## Notes

No transaction costs or slippage are included in the simulation, and execution is assumed to occur at closing prices. 
As a result, the reported performance reflects an idealised backtest environment. 
The strategy is based purely on historical data, and its performance may not generalise to live trading conditions.
