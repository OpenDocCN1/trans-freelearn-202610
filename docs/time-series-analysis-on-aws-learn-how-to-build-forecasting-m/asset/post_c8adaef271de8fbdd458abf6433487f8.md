
Using the dropdown on the top right of this section, you can select the algorithm for which you want to see the results. The results you obtained from your first predictor should be similar to the following ones:

|  | wQL[0.1] | wQL[0.5] | wQL[0.9] | WAPE | RMSE |
|---|---|---|---|---|---|
| CNN-QR (Winner) | 0.0993 | 0.2254 | 0.1183 | 0.2254 | 3.2904 |
| Deep_AR_Plus | 0.1100 | 0.2353 | 0.1253 | 0.2364 | 3.4374 |
| ARIMA | 0.1196 | 0.2456 | 0.1430 | 0.2456 | 3.6026 |
| Prophet | 0.1243 | 0.2636 | 0.1505 | 0.2617 | 3.8997 |
| NPTS | 0.1107 | 0.3329 | 0.1813 | 0.3618 | 5.1994 |
| ETS | 0.1636 | 0.2866 | 0.2084 | 0.2866 | 4.6343 |

Figure 7.2 – Algorithm metrics comparison

At the top of this table, you can see several metric names, such as wQL[0.1], wQL[0.5], wQL[0.9], WAPE, and RMSE. How are these metrics computed? How do you interpret them? How do you know whether a value is a good one? Let's dive deeper into each of these metrics.

## Weighted absolute percentage error (WAPE)

This accuracy metric is a method that computes the error made by the forecasting algorithm with regard to the actual values (the real observations). As this metric is averaged over time and over different items (household items in our case), this metric doesn't differentiate between these items. In other words, the WAPE does not assume any preference between which point in time or item the model predicted better. The WAPE is always a positive number and the smaller it is, the better the model.

The formula to derive this metric over a given backtesting window is as follows:

WAPE = Σᵢ,ₜ |yᵢ,ₜ − ŷᵢ,ₜ| / Σᵢ,ₜ |yᵢ,ₜ|

Here, we have the following:

- $y_{i,t}$ is the actual value observed for a given `item_id` at a point in time, $t$ (with $t$ ranging over the time range from the backtest period and $i$ ranging over all items).
- $\hat{y}_{i,t}$ is the mean forecast predicted for a given `item_id` at a point in time, $t$.

Let's have a look at the following plot, where we have drawn the forecast and the observations for the energy consumption of a given household from our dataset:

![](img/e4ee48104c730845e796d8c469dfeecc_208_0.png)

Figure 7.3 – WAPE metric

In the preceding plot, the vertical dotted lines illustrate the difference between the actual values and the forecasted values for a given household. To compute the WAPE metric, you can follow the steps listed here:

- 1. Take the length of every dotted line in this plot (each line length is the absolute value of the difference between the actual and forecasted values).
- 2. Sum all these lengths together and sum across all `item_id` in the dataset.
- 3. Divide this number by the sum of the observed values for every `item_id` over this backtest window.

This will give you an absolute value measured in percentage. For instance, a value of 10.0 could be read as *on average, the forecasted values are ±10% away from the observed values.* Reading this, you might notice one of the disadvantages of this metric, which is that with the WAPE being a percentage, it can be overly influenced by large percentage errors when forecasting small numbers. In addition, this metric is symmetrical: it does not penalize under-forecasting or over-forecasting more.

Note that the absolute error (the denominator in the previous formula) is normalized by the sum of all the values of all your time series. If you're a retailer and deal with the total demand of multiple products, this means that this metric puts more emphasis on the products that are sold frequently. From a business perspective, it makes sense to have a more accurate forecast for your best sellers. In an energy consumption use case (such as the one we have been using), this means that households with significantly higher consumption will have more impact on this metric. Although this makes sense to ensure you deliver the right amount of electricity through your grid, this may not be useful if you want to deliver personalized recommendations to each household.

## Mean absolute percentage error (MAPE)

Like the WAPE, this metric computes the error made by the forecasting algorithm with regard to the real observations. The formula to derive this metric over a given backtesting window is as follows:

$$MAPE_i = \frac{1}{n} \sum_{t} \left| \frac{y_{i,t} - \hat{y}_{i,t}}{y_{i,t}} \right|$$

Here, we have the following:

- $y_{i,t}$ is the actual value observed for a given `item_id` at a point in time, $t$ (with $t$ ranging over the time range from the backtest period and $i$ ranging over all items).
- $\hat{y}_{i,t}$ is the mean forecast predicted for a given `item_id` at a point in time, $t$.
- $n$ is the number of data points in the time series associated with a given `item_id`.

As the formula hints, this metric is highly influenced by outliers: the MAPE is useful when your time series differ significantly over time. Compared to the WAPE, the MAPE is also unnormalized, making it useful when you want to consider time series with large values and others with small values equally.

## Mean absolute scaled error (MASE)

When your dataset is seasonal, the cyclical property of your time series may be better captured by this metric, which considers the seasonality by integrating it into a scaling factor. The formula to derive this metric over a given backtesting window for a given time series is the following:

$$MASE_i = \frac{\frac{1}{n} \cdot \sum_t |y_{i,t} - \hat{y}_{i,t}|}{\frac{1}{n - m} \cdot \sum_{t=m+1}^{n} |y_{i,t} - y_{i,t-m}|}$$

Here, we have the following:

- $y_{i,t}$ is the actual value observed for a given `item_id` at a point in time, $t$ (with $t$ ranging over the time range from the backtest period and $i$ ranging over all items).
- $\hat{y}_{i,t}$ is the mean forecast predicted for a given `item_id` at a point in time, $t$.
- $n$ is the number of data points in the time series associated with a given `item_id`.
- $m$ is the seasonality value: when $m$ is equal to 1, the denominator of this formula is actually the mean absolute error of the *naïve forecast method* (which takes a forecast equal to the previous timestep). Amazon Forecast sets this value, $m$, depending on the forecast frequency selected.

When your time series have a significant change of behavior over different seasons, using this metric as the one to optimize for can be beneficial. This metric is scale-invariant, making it easier to compare different models for a given `item_id` time series or compare forecast accuracy between different time series.

## Root mean square error (RMSE)

Like the WAPE, this accuracy metric is also a method that computes the deviation made by a predictor with regard to the actual values (the real observations). In the case of the RMSE, we average a squared error over time and over different items (the household in our case). This metric doesn't differentiate between these, meaning the RMSE does not assume any preference between which point or item the model predicted better. It is always a positive number and the smaller it is, the better the model.

The formula to derive this metric over a given backtesting window is as follows:

$$RMSE = \sqrt{\frac{1}{n \times T} \sum_{i,t} (\hat{y}_{i,t} - y_{i,t})^2}$$

On the numerator of the previous formula, you can see the following:

- $\tau \cdot max\left(y_{i,t}-q_{i,t}^{(\tau)},0\right)$ is strictly positive when the predictions are below the actual observations. This term penalizes under-forecasting situations.
- On the other hand, $(1-\tau)\cdot max\left(q_{i,t}^{(\tau)}-y_{i,t},0\right)$ is strictly positive when the predictions are above the actual observations. This term penalizes over-forecasting situations.

Let's take the example of $wQL[0.80]$. Based on the preceding formula, this metric assigns the following:

- A larger penalty weight to under forecasting: $0.8\cdot max\left(y_{i,t}-q_{i,t}^{(0.8)},0\right)$
- A smaller penalty weight to over forecasting: $(1-0.8)\cdot max\left(q_{i,t}^{(0.8)}-y_{i,t},0\right)$

Very often, in retail, being understocked for a given product (with the risk of missed revenue because of an unavailable product) has a higher cost than being overstocked (having a larger inventory). Using $wQL[0.8]$ to assess the quality of a forecast may be more informative to make supply-related decisions for these products.

On the other hand, a retailer may have products that are easy for consumers to substitute. To reduce excess inventory, the retailer may decide to understock these products because the occasional lost sale is outweighed by the inventory cost reduction. In this case, using $wQL[0.1]$ to assess the quality of the forecast for these products might be more useful.

Now that you have a good understanding of which metrics Amazon Forecast computes, let's see how you can use this knowledge to analyze your model accuracy and try and improve it.

## Understanding your model accuracy

Some items might be more important than others in a dataset. In retail forecasting, 20% of the sales often accounts for 80% of the revenue, so you might want to ensure you have a good forecast accuracy for your top-moving items (as the others might have a very small share of the total sales most of the time). In every use case, optimizing accuracy for your critical items is important: if your dataset includes several segments of items, properly identifying them will allow you to adjust your forecast strategy.
