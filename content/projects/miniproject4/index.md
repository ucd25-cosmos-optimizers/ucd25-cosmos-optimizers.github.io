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

| Exchange (venue code)                       | Contract unit        | Price quote   | Typical trading hours (local)         | Warehouse/delivery system                                   | Tick size                                                      |
| ------------------------------------------- | -------------------- | ------------- | ------------------------------------- | ----------------------------------------------------------- | -------------------------------------------------------------- |
| **London Metal Exchange – LME Copper (CA)** | 25 metric tonnes     | USD per tonne | 01:00-19:00 London (ring + LMEselect) | Global LME-approved sheds; *“warrants”* transferable        | USD 0.10/tonne ([Lme][1])                                      |
| **Shanghai Futures Exchange – SHFE 铜 (CU)** | 5 t                  | CNY per ton   | 09:00-15:00 & 21:00-02:00 Beijing     | Mainland China warehouses; VAT & import‐quota rules apply   | ¥10/ton (≈USD 1.4) ([tsite.shfe.com.cn][2], [Barchart.com][3]) |
| **CME Group / COMEX Copper (HG)**           | 25 000 lb (≈11.34 t) | USD ¢ per lb  | 18:00-17:00 ET (electronically)       | U.S. COMEX-licensed warehouses; deliverable Grade 1 cathode | USD 0.0005/lb ([CME Group][4], [CME Group][5])                 |

[1]: https://www.lme.com/en/metals/non-ferrous/lme-copper/contract-specifications?utm_source=chatgpt.com "Contract specifications | London Metal Exchange"
[2]: https://tsite.shfe.com.cn/eng/market/futures/metal/cu/index.html?utm_source=chatgpt.com "Copper"
[3]: https://www.barchart.com/futures/quotes/VC%2A0/profile?utm_source=chatgpt.com "SHFE Copper Aug '25 Futures Contract Specifications - Barchart.com"
[4]: https://www.cmegroup.com/markets/metals/base/copper.contractSpecs.html?utm_source=chatgpt.com "Copper Futures Contract Specs - CME Group"
[5]: https://www.cmegroup.com/markets/metals/base/copper.html?utm_source=chatgpt.com "Copper Futures Overview - CME Group"
        |

[1]: https://www.lme.com/en/metals/non-ferrous/lme-copper/contract-specifications?utm_source=chatgpt.com "Contract specifications | London Metal Exchange"
[2]: https://tsite.shfe.com.cn/eng/market/futures/metal/cu/index.html?utm_source=chatgpt.com "Copper"
[3]: https://www.barchart.com/futures/quotes/VC%2A0/profile?utm_source=chatgpt.com "SHFE Copper Aug '25 Futures Contract Specifications - Barchart.com"
[4]: https://www.cmegroup.com/markets/metals/base/copper.contractSpecs.html?utm_source=chatgpt.com "Copper Futures Contract Specs - CME Group"
[5]: https://www.cmegroup.com/markets/metals/base/copper.html?utm_source=chatgpt.com "Copper Futures Overview - CME Group"


## Data
We utilized various different datasets that consisted of **historical daily closing prices** of copper futures obtained from different exchanges. Our datasets included **data from several years**, allowing our model to **learn from past behaviors** and thus **adapt to both long-term patterns and short-term volatility**. We mainly used **Yahoo Finance** via its **yfinance API**, incorporating its **adjusted closing price (`Adj close`)**.

## Methodology
### Preprocessing
- Handled any **missing values**
- **Normalized prices** through MinMax scaling on a [0,1] range
- Created **sequence generation** for supervised learning
- Chose a **sliding window of 30**
- **Split data** in a 70:30 ratio of training and testing sets respectively to **ensure no shuffling**

### Model Design

We used a **Bidirectional LSTM neural network** as we believed it would be beneficial in **understanding both forward and backward time dependencies and variables**, which is important especially in time-series data like financial forecasting. 

### Model Structure
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
