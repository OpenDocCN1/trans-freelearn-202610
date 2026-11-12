# # Time series data

Time series data is usually a set of ordered data collected over equally spaced intervals. Time series data occurs in most business and scientific disciplines, and the data is closely tied to the concept of forecasting, which uses previously measured data points to predict future data points based upon a specific statistical model.

Time series data differs from the kind of data that we have been looking at previously; because it is a set of ordered data points, it can contain components such as trend, seasonality, and autocorrelation, which have little meaning in other types of analysis, such as "Cross-sectional" analysis, which looks at data collected at a static point in time.

Usually, time series data is collected in equally spaced intervals, such as days, weeks, quarters, or years, but that is not always the case. Measurement of events such as natural disasters is a prime example. In some cases, you can transform uneven data into equally spaced data. In other cases, you can use specialized techniques such as Croston's method for forecasting intermittent or unexpected demand for goods and services in certain cases.

# ## Exploring time series data

Many time series studies start out by exploring one data metric which has been measured across equally spaced time intervals. From a data science perspective, we could be interested in identifying various segments of a time series which could be exhibiting interesting trends, or cyclical or seasonal patterns. So, we always begin time series by looking at the data graphically, and producing aggregate measures before proceeding with the modeling.