
## Generating a Time Series with Trend and Seasonality

Let's generate a time series with trend and seasonality and see how it manifests, as follows:

```
# Sinusoidal Signal with Amplitude=1 & Frequency=0.25
signal=ts.signals.Sinusoidal(amplitude=1, frequency=0.25)
# White Noise with standard deviation = 0.3
noise=ts.noise.GaussianNoise(std=0.3)
# Generate the time series
sinusoidal_samples, regular_time_samples, _, _ = generate_timeseries(signal=signal, noise=noise)
# Regular_time_samples is a linear increasing time axis and can be used as a trend
trend = regular_time_samples*0.4
# Combining the signal and trend
ts = sinusoidal_samples+trend
plot_time_series(regular_time_samples,
                 ts,
                 "Sinusoidal with Trend and White Noise")
```

Here is the output:

![Figure 1.9 – Sinusoidal signal with trend and white noise](media/file10.jpg)

Figure 1.9 – Sinusoidal signal with trend and white noise

If you examine the time series in Figure 1.9, you will be able to see a definite trend and the seasonality, which together make the mean of the data distribution change wildly across different windows of time.

## Change in Variance Over Time

Non-stationarity can also present itself in the fluctuating variance of a time series. If the time series starts off with low variance and as time progresses, the variance keeps getting bigger and bigger, we have a non-stationary time series. In statistics, there is a scary name for this phenomenon—**heteroscedasticity**. The Air Passengers dataset, which is the 'iris dataset' of time series (most popular, over-used, and use-less) is a classic example of heteroscedastic time series. Let's look at the plot.

![Figure 1.10 – Air Passengers Dataset – Example of Heteroscedastic Time Series](media/file11.png)

Figure 1.10 – Air Passengers Dataset – Example of Heteroscedastic Time Series

In the figure, you can see that the seasonal peaks keeps getting wider and wider as we move along the time and this is a classic sign that the time series is heteroscedastic. But all heteroscedastic time series aren't easy to spot. We have statistical tests to check for each of the stationarity cases which we will cover in Chapter 7 – Target Transformations for Time Series Forecasting.

This book just tries to give you intuition about stationary versus non-stationary time series. There is a lot of statistical theory and depth in this discussion that we are skipping over to keep our focus on the practical aspects of time series.

Armed with the mental model of the DGP, we are at the right place to think about another important question: What can we forecast?

## What Can We Forecast?

Before we move ahead, there is another aspect of time series forecasting that we have to understand—the predictability of a time series. The most basic assumption when we forecast a time series is that the future depends on the past. But not all time series are equally predictable.

Let's take a look at a few examples and try to rank these in order of predictability (from easiest to hardest), as follows:

*   High tide next Monday
*   Lottery numbers next Sunday
*   The stock price of Tesla next Friday

Intuitively, it is very easy for us to rank them. High tide next Monday is going to be the easiest to predict because it is so predictable, the lottery numbers are going to be very hard to predict because these are pretty much random, and the stock price of Tesla next Friday is going to be difficult to predict, but not impossible.

> **NOTE**
>
> However, for people thinking that they can forecast stock prices with the advanced techniques covered in the book and get rich, that won't happen (most likely). Although it is worthy of a lengthy discussion, we can summarize the key points in a short paragraph.

Share prices are not a function of their past values but an anticipation of their future values, and this thereby violates our first assumption while forecasting. And if that is not bad enough, financial stock prices typically have a very low signal-to-noise ratio. The final wrench in the process is the **efficient-market hypothesis** (**EMH**). This seemingly innocent hypothesis proclaims that all known information about a stock price is already factored into the price of the stock. The implication of the hypothesis is that if you can forecast accurately, many others will also be able to do that, and thereby the market price of the stock already reflects the change in price that this forecast brought about.

The M6 competition chose to tackle this problem head-on to evaluate if the EMH holds true by conducting a year long forecasting and investment strategy competition. Although not conclusive, the results show that the EMH holds true for the vast majority of the participants, barring a few top teams. And even in that, they found out that in the top teams, there was no significant correlation between forecasting accuracies and the selection of stocks into the portfolio, i.e. the teams weren't choosing stocks which they were able to forecast better (Full report is linked in Further Reading).

Coming back to the topic at hand—predictability—three main factors form a mental model for this, as follows:

*   **Understanding the DGP**: The better you understand the DGP, the higher the predictability of a time series.
*   **Amount of data**: The more data you have, the better your predictability is.
*   **Adequately repeating pattern**: For any mathematical model to work well, there should be an adequately repeating pattern in your time series. The more repeatable the pattern is, the better your predictability is.

Even though you have a mental model of how to think about predictability, we will look at more concrete ways of assessing the predictability of time series in Chapter 3, Analyzing and Visualizing Time Series Data, but the key takeaway is that not all time series are equally predictable.

In order to fully follow the discussion in the coming chapters, we need to establish a standard notation and get updated on terminology that is specific to time series analysis.

## Forecasting Terminology

There are a few terminologies that will help you follow the book as well as other literature on time series. These are described in more detail here:

*   **Forecasting**

    Forecasting is the prediction of future values of a time series using the known past values of the time series and/or some other related variables. This is very similar to prediction in ML where we use a model to predict unseen data.

*   **Multivariate forecasting**

    Multivariate time series consist of more than one time series variable that is not only dependent on its past values but also has some dependency on the other variables. For example, a set of macroeconomic indicators such as **gross domestic product** (**GDP**), inflation, and so on of a particular country can be considered as a multivariate time series. The aim of multivariate forecasting is to come up with a model that captures the interrelationship between the different variables along with its relationship with its past and forecast all the time series together in the future.

*   **Explanatory forecasting**

    In addition to the past values of a time series, we might use some other information to predict the future values of a time series. For example, for predicting retail store sales, information regarding promotional offers (both historical and future ones) is usually helpful. This type of forecasting, which uses information other than its own history, is called explanatory forecasting.

*   **Backtesting**

    Setting aside a validation set from your training data to evaluate your models is a practice that is common in the ML world. Backtesting is the time series equivalent of validation, whereby you use the history to evaluate a trained model. We will cover the different ways of doing validation and cross-validation for time series data later.

*   **In-sample and out-sample**
