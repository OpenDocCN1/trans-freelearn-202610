The following screenshot summarizes the forecasting problem scope:

![](bbox=[0.117, 0.128, 0.847, 0.365])

Figure 2.1 – Time series forecasting components

Let's dive into several of the key areas of this screenshot, as follows:

+   - **Historical data:** The historical data is the part of your time series that will be used to train a forecasting model. In *Figure 2.1*, this is the regular line plot located on the left of the graph.
+   
+   - **Historical data frequency:** Your historical time series data can be available at a regular or irregular sampling rate. If your raw data is not regularly sampled or if its frequency is different from the forecast granularity, you may have to aggregate your data at the desired unit. In retail, each product sale is recorded when it happens, but not every product is sold every day, leading to a sparse time series with spikes that correspond to days when a sale has happened. Aggregated sales for each product at a weekly or monthly level will smooth out the original signal.
+   
+   - **Forecast horizon:** This is the prediction length you can expect for a forecast. This is usually measured in the number of time steps using the desired unit, which might be different from the original time series. If you expect daily data as a result of your forecast and need predictions for the coming week, your sampling unit will be at the daily level and your forecast horizon will be 7 days.