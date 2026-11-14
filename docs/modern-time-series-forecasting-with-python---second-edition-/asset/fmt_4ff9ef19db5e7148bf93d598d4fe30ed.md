
# Simple Exponential Smoothing (SES)

**Simple exponential smoothing** (**SES**) is when you simply apply this smoothing procedure to the history. This is more suited for time series that have no trends or seasonality, and the forecast is going to be a flat line. The forecast is generated using the following formula:

![](media/file80.jpg)

# Double Exponential Smoothing (DES)

**Double exponential smoothing** (**DES**) extends the smoothing idea to model trends as well. It has two smoothing equations – one for the level and the other for the trend. Once you have the estimate of the level and trend, you can combine them. This forecast is not necessarily flat because the estimated trend is used to extrapolate it into the future. The forecast is generated according to the following formula:

![](media/file81.jpg)

First, we estimate the level (`l_t`) using the *Level Equation* with the available observations. Then, we estimate the trend using the *Trend Equation*. Finally, to get the forecast, we combine `l_t` and `b_t` using the *Forecast Equation*. Researchers have found empirical evidence that this kind of constant extrapolation can result in over-forecasts over the long-term forecast. This is because, in the real world, time series data doesn't increase at a constant rate forever. Motivated by this, an addition to this has also been introduced that dampens the trend by a factor of `φ`, such that when `φ = 1`, there is no damping, and it is identical to double exponential smoothing.

# Triple Exponential Smoothing (Holt-Winters / HW)

**Triple exponential smoothing** or **Holt-Winters** (**HW**) takes this one step forward by including another smoothing term to model the seasonality. This has three parameters (`α`, `β`, `γ`) for the smoothing and uses a seasonality period (`m`) as input parameters. You can also choose between additive or multiplicative seasonality. The forecast equations for the additive model are as follows:

![](media/file88.jpg)

These formulae are also used like in the double exponential case. Instead of estimating level and trend, we estimate level, trend, and seasonality separately.

# Exponential Smoothing Family

The family of exponential smoothing methods is not limited to the three that we just discussed. A way to think about the different models is in terms of the trend and seasonal components of these models. The trend can either be no trend, additive, or additive damped. The seasonality can be no seasonality, additive, or multiplicative. Every combination of these parameters is a different technique in the family, as shown in the following table:

![Table 4.1 – Exponential smoothing family](media/file89.jpg)

Table 4.1 – Exponential smoothing family

`NIXTLA` has an entire family of exponential smoothing methods.

# NIXTLA Implementation

Let's see how we can initialize the `ETS` model in **NIXTLA**:

```
from statsforecast.models import (SimpleExponentialSmoothing, Holt, HoltWinters, AutoETS)
exp_smooth = HoltWinters(error_type = 'A', season_length = 48)
```

Here the `error_type = 'A'` refers to additive error. The user has the option for either additive error or multiplicative error, which could be called using `error_type = 'M'`. `NIXTLA` models has an option to use `AutoETS()`. This model will automatically choose which exponential smoothing model is the best option: `SimpleExponentialSmoothing`, `Holt` (double exponential smoothing / Holt's Method), or `HoltWinters` (triple exponential smoothing / Holt-Winters Method). It will also choose which parameters and error types are best for each individual time series. Refer to the GitHub notebooks for examples on how to use `AutoETS()`.

Let's see what the forecast using `ETS` looks like in Figure 4.4:

![Figure 4.4 – Exponential smoothing forecast](media/file90.png)

Figure 4.4 – Exponential smoothing forecast

The forecast has captured the seasonality but has failed to capture the peaks. But we can see the improvement in `MAE` already.

Now, let's look at one of the most popular forecasting methods out there.

# ARIMA

**Autoregressive Integrated Moving Average** (**ARIMA**) models are the other class of methods that, like `ETS`, have stood the test of time and are one of the most popular classical methods of forecasting. The `ETS` family of methods is modelled around trend and seasonality, while `ARIMA` relies on **autocorrelation** (the correlation of `y_t` with `y_{t-1}`, `y_{t-2}`, and so on).

## AR(p) Models

The simplest in the family are the `AR(p)` models, which use **linear regression** with `p` previous timesteps or, in other words, `p` lags. Mathematically, it can be written as follows:

![](media/file94.jpg)

Here, `c` is the intercept and `ε_t` is the noise or error at timestep `t`.

## MA(q) Models

The next in the family are `MA(q)` models, in which instead of past observed values, we use the past `q` errors in the forecast (which is assumed to be pure white noise) to come up with a forecast:

![](media/file96.jpg)

Here, `ε_t` is white noise and `c` is the intercept. This is not typically used on its own but in conjunction with `AR(p)` models, which makes the next one on our list `ARMA(p,q)` models. `ARMA` models are defined as `ARMA(p,q)`.

## ARIMA(p,d,q) Models

In all the `ARIMA` models, there is one underlying assumption – the *time series is stationary* (we talked about stationarity in Chapter 1, *Introducing Time Series*, and will elaborate on this in Chapter 6, *Feature Engineering for Time Series Forecasting*). There are many ways to make the series stationary but taking the difference of successive values is one such technique. This is known as **differencing**. Sometimes, we need to do differencing once, while other times, we have to perform successive differencing before the time series becomes stationary. The number of times we do the differencing operation is called the *order of differencing*. The `I` in `ARIMA`, and the final piece of the puzzle, stands for *Integrated*. It defines the order of differencing we need to do before the series becomes stationary and is denoted by `d`.

So, the complete `ARIMA(p,d,q)` model says that we do `d`th order of differencing and then consider the last `p` terms in an autoregressive manner, and then include the last `q` moving average terms to come up with the forecast.

## Seasonal ARIMA

The `ARIMA` models we have discussed so far only handle non-seasonal time series. But using the same concepts we discussed, but on a seasonal cycle, we get **Seasonal ARIMA**. `p`, `d`, and `q` are slightly tweaked so that they work on the seasonal period, `m`. To differentiate them from the normal `p`, `d`, and `q`, we call the seasonal values `P`, `D`, and `Q`. For instance, if `p` meant taking the last `p` lags, `P` means taking the last `P` seasonal lags. If `y_t` is `y_{t-1}`, `y_{t-P}` would be `y_{t-m}`. Similarly, `D` means the order of seasonal differencing.

## Auto ARIMA

Picking the right `p`, `d`, and `q` and `P`, `D`, and `Q` values is not very intuitive, and we will have to resort to statistical tests to find them. However, this becomes a bit impractical when you are forecasting many time series. An automatic way of iterating through the different parameters and finding the best `p`, `d`, and `q`, and `P`, `D`, and `Q` values for the data is called **Auto ARIMA**. In Python, `NIXTLA` has implemented this method `AutoARIMA()`. **NIXTLA** also has a normal `ARIMA` implementation as well, which is much faster but requires `p,d,q` to be entered manually.

# Practical Considerations
