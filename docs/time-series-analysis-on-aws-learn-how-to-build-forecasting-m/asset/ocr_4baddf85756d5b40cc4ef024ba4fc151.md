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

# Weighted absolute percentage error (WAPE)

This accuracy metric is a method that computes the error made by the forecasting algorithm with regard to the actual values (the real observations). As this metric is averaged over time and over different items (household items in our case), this metric doesn't differentiate between these items. In other words, the WAPE does not assume any preference between which point in time or item the model predicted better. The WAPE is always a positive number and the smaller it is, the better the model.

The formula to derive this metric over a given backtesting window is as follows:

WAPE = Σᵢ,ₜ |yᵢ,ₜ − ŷᵢ,ₜ| / Σᵢ,ₜ |yᵢ,ₜ|