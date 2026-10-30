We refer to time series with constant mean and variance across a period of time as **stationary**. In general, time series *with* trend and seasonality are *not* stationary. It is possible to apply data transformations to the series to transform it into a stationary time series so that the modeling task tends to be easier. This type of transformation is known as **differentiation.**

While you are exploring a time series, you can check stationarity by applying hypothesis tests, such as **Dickey-Fuller**, **KPSS**, and **Philips-Perron**, just to mention a few. If you find it non-stationary, then you can apply differentiation to make it a stationary time series. Some algorithms already have that capability embedded.

# Exploring, exploring, and exploring

At this point, I am sure I don't have to remind you that exploration tasks happen all the time in data science. Nothing is different here. While you are building time series models, you might want to have a look at the data and check if it is suitable for this type of modeling.

**Autocorrelation plots** are one of the tools that you can use for time series analysis. Autocorrelation plots allow you to check the correlations between lags in the time series. The following graph shows an example of this type of visualization:

![](bbox=[0.22, 0.52, 0.75, 0.75])

Figure 7.15 – Autocorrelation plot

Remember, if you're playing with univariate time series, your time series just has one variable, so finding autocorrelation across the lags of your unique variable is crucial to understanding whether you can build a good model or not.