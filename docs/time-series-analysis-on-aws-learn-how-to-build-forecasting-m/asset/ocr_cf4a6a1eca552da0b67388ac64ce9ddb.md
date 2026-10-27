+   - If your historical time series contains 1,000 days of data but at an hourly granularity (24 x 1,000 = 24,000 time steps), your maximum forecast horizon will be 500 hours (if you decide to keep the same sampling rate of 1 hour per time step).

Amazon Forecast is a univariate forecasting service—the target dataset that contains the metrics of interest can only include a single field to forecast for. If you have a multivariate dataset and want to deliver predictions for multiple fields in parallel, you would have to build several individual models with Amazon Forecast. Depending on your use case, this may not be suitable, as dependency between different variables may contain valuable insights to forecast future values for all these variables.

Last but not least, your data must have a compatible time interval—although Amazon Forecast can deal with datasets that have a time interval ranging from 1 minute to 1 year, the more common intervals are hours, days, and weeks. *High-frequency datasets* (for example, sensor data collected at a 10-millisecond sampling rate) will require you to aggregate the data at a minimum of 1 minute. If you are looking at predictive fast events, this is not a suitable approach (I would argue that a forecasting formulation might not be the right one). On the other end of the spectrum, *long periods* of time—such as decades—in environmental science will lead to a sparsity of your dataset, which may generate a low-quality forecast.

# Use-case requirements

Throughout this chapter, you have read through multiple forecasting examples. You have also seen how much heavy lifting Amazon Forecast performs on your behalf. There are, however, situations and use cases for which Amazon Forecast is not the best match, such as the following:

+   - **Small data**: Use cases similar to financial planning, where you need to forecast monthly revenue for a company for the next 5 years based on the last 3 years (36 data points).
+   - **Long periods of time with sparse data**: This generally yields small data.
+   - **Random walk**: Random processes such as stock market indices where future values are heavily impacted by external factors with low (to no) predictive power in past historical data alone.
+   - **High-frequency data**: Datasets with a sampling rate of less than 1 minute or even less than 1 second are not suitable for Amazon Forecast.
+   - **Multivariate data**: Although Amazon Forecast can forecast univariate data while leveraging insights coming from related time series data, the service is not built to provide multivariate forecasts in parallel for several interdependent time series.