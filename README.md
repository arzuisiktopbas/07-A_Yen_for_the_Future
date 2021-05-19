# A Yen for the Future
 *by Arzu Isik Topbas*
 
 <img src="https://image.freepik.com/free-photo/hand-holding-virtual-world-with-dollar-yuan-yen-euro-pound-sterling-sign-exchange-currency-concept_50039-1623.jpg" width="1000" height="400" />

[image source:freepik](https://www.freepik.com/premium-photo/hand-holding-virtual-world-with-dollar-yuan-yen-euro-pound-sterling-sign-exchange-currency-concept_9148152.htm#page=1&query=currency&position=2)

I tested the many time-series tools that I have learned in order to predict future movements in the value of the Japanese yen versus the U.S. dollar. I used two main models *Time Series Forecasting* and *Linear Regression Modeling*.

- - -

## 1 - Time-Series Forecasting
![Time-Series Forecasting](https://github.com/arzuisiktopbas/07-A_Yen_for_the_Future/blob/main/Images/Yen_Future_Settle_Prices.png)

In the long term, It seems that the Japanese Yen  is strengthening against the Dollar. On the other hand, there are a lot of short-term ups and downs.

### 1. Decomposition using a Hodrick-Prescott Filter

I used The Hodrick-Prescott Filter to decompose the "Settle" price into two seperate series: noise and trend. It seems that there's a lot of short term fluctuations that deviate around the trend.

![SettlevsTrend](https://github.com/arzuisiktopbas/07-A_Yen_for_the_Future/blob/main/Images/Settle%20vs%20Trend.png)

The Noise plot gave us a visualization of where the most noise is on this chart. These points can be correllated with volatile periods in the market.

![Noise](https://github.com/arzuisiktopbas/07-A_Yen_for_the_Future/blob/main/Images/Noise.png)



### 2. Forecasting Returns using an ARMA Model

I created an ARMA model and fit it to the returns data. Note. I set the AR and MA ("p" and "q") parameters to p=2 and q=1: order=(2, 1)

In the summary table, the p-values for each lag are not statistically significant. The model is not a good fit given the p-value exceeds 0.05.

![Arma](https://github.com/arzuisiktopbas/07-A_Yen_for_the_Future/blob/main/Images/ARMA_summary.png)

![Arma](https://github.com/arzuisiktopbas/07-A_Yen_for_the_Future/blob/main/Images/Arma_Five_Days_Returns.png)

### 3. Forecasting the Settle Price using an ARIMA Model



I created an ARIMA model by using the "Settle" price data. For autoregressive (AR) the parameter p was set to 5, for differences the parameter d was set to 1 and, for the moving average (MA) the parameter q was set to 1 making order=(5,1,1).

Again, based on the summary table, the p-values for each lag did not show to be statistically significant - none are less than 0.05.

![Arima](https://github.com/arzuisiktopbas/07-A_Yen_for_the_Future/blob/main/Images/ARIMA_summary.png)

Based on the ARIMA model, we can see that an increase in the Yen over the dollar is expected for the 5 day period.

![Arima](https://github.com/arzuisiktopbas/07-A_Yen_for_the_Future/blob/main/Images/Arima_Five_Days_Returns.png)


### 4. Forecasting Volatility with GARCH

I created a Garch model and fit it to the returns data. Note. Similar to ARMA, I set the AR and MA ("p" and "q") parameters to p=2 and q=1: order=(2, 1)
In the summary table,the p-values are greater than 0.05 and therefore statistically significant.

![Garch](https://github.com/arzuisiktopbas/07-A_Yen_for_the_Future/blob/main/Images/GARCH_summary.png)


Based on the GARCH forecast plot , volatility risk of the Yen will increase each day for the next 5 days.

![Garch](https://github.com/arzuisiktopbas/07-A_Yen_for_the_Future/blob/main/Images/Garch_forecast.png)

## Conclusions

According to ARMA and ARIMA models, the Japanese Yen will increase relative to the US dollar, however, there might be an unreliable coefficient that might cause misleading result because the p-values are greater than 0.05. Thus, I would not buy the Japanese Yen.

Based on the GARCH forcast, volatility risk of the Yen will increase each day.

Again, due to p- values are not less than 0.05, I do not feel confident in using these models for trading.

## 2 - Linear Regression Forecasting

I build a Scikit-Learn linear regression model to predict Yen futures ("settle") returns with *lagged* Yen futures returns and categorical calendar seasonal effects (e.g., day-of-week or week-of-year seasonal effects).

The model has a root mean square error of **0.415%** on out-of-sample data and **0.5962%** on in-sample data. Hence, the model performs better with data that it has not worked with before.

![Linear_reg](https://github.com/arzuisiktopbas/07-A_Yen_for_the_Future/blob/main/Images/Prediction_plot.png) 

The In-Sample data and the Out of Sample results is highly deviant from each other.Because the root mean square error of **0.415%** on out-of-sample data is smaller than the root mean square error of **0.5962%** on in-sample data, it is considered that the model is **Overfitting.**
