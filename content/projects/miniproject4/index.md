+++
title = 'Mini Project 4: Copper Futures Price Forecasting Using BiLSTM'
date = 2025-07-29T14:00:00-04:00
weight = 485
description = 'Predicting the stock price of the copper future'
tags = []
pageName = "miniproject4"
icon = 'chart'
draft = false
+++

## Introduction

Copper plays an **extremely important** role in the global economy, with uses spanning from infrastructure to electronics. The **purpose** of our study is to **better understand** the behavior of copper within our stock market, specifically the **copper future prices**. We chose to utilize a **Bidirectional Long Short-Term Memory (`BiLSTM`)** model in order to accurately take into consideration both forward and backward moving variables, based on historical data from three sources: **CME Group / COMEX Copper (`HG`), Shanghai Futures Exchange (`CU`), and London metal exchange (`CA`)**.

## Data
We utilized various different datasets that consisted of **historical daily closing prices** of copper futures obtained from different exchanges. Our datasets included **data from several years**, allowing our model to **learn from past behaviors** and thus **adapt to both long-term patterns and short-term volatility**. We mainly used **Yahoo Finance** via its **yfinance API**, incorporating its **adjusted closing price (`Adj close`)**.

## Methodology
### Preprocessing:
- Handled any **missing values**
- **Normalized prices** through MinMax scaling on a [0,1] range
- Created **sequence generation** for supervised learning
- Chose a **sliding window of 30**
- **Split data** in a 70:30 ratio of training and testing sets respectively to **ensure no shuffling**

### Model Design:

We used a **Bidirectional LSTM neural network** as we believed it would be beneficial in **understanding both forward and backward time dependencies and variables**, which is important especially in time-series data like financial forecasting. 

### Model Structure:
- The model was built to take in a 30-day window of past copper prices and **predict the next day's price**. 
- It uses a single Bidirectional LSTM (`BiLSTM`) layer with **64 hidden units**, which helps the model **learn patterns from both past and future directions in the time series**. 
- A **simple linear layer** at the end outputs the predicted price.
- Training was done over 50 epochs with a batch size of 64. 
- The model used the **Mean Squared Error (`MSE`)** as its loss function and the **Adam optimizer**, which is well-suited for time-series problems like this.

## Discussion
The BiLSTM model **demonstrated extremely strong predictive ability**:
- **MSE**: 5.33e-06
- **RMSE**: 0.0023
- **MAE**: 0.0019
- **R²**: 0.9887
  
These results show that the model **performs very well**. The predicted prices are **accurate with the actual values** in the test set, suggesting that the BiLSTM effectively **captures patterns and trends** in the copper market. A strong indication of accuracy is the **R² score of 0.9887**, which means the model accounts for **nearly 99% of the variation** in prices. 

Nevertheless, it is important to **remain cautious about potential overfitting**, especially with time-series data where future values can be hard to predict reliably.

### Future Opportunities

- **Long-Term Forecasting**: While the model currently focuses on predicting just the next day’s price, we will look in future versions to **extend the forecast window**. Predicting prices **over multiple days** could offer **more practical value** for traders and analysts.
  
- **Additional Features**: Improving performance by bringing in additional features such as **macroeconomic indicators** like the U.S. dollar index or manufacturing activity, which often **influence commodity prices**.

- **Real-Time Analysis**: By evaluating how well the model **generalizes to new data**, we plan to use **time-aware validation methods** like walk-forward analysis. This would give a **more realistic picture** of how the model performs in **real-world trading scenarios**.
