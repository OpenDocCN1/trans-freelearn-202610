The following three AWS services we dedicate a part to in this book also assume that you will provide raw time series datasets as inputs:

+   - **Amazon Forecast**: To perform time series forecasting
+   - **Amazon Lookout for Equipment**: To deliver anomalous event forewarning usable to improve predictive maintenance practices
+   - **Amazon Lookout for Metrics**: To detect anomalies and help in **root-cause analysis (RCA)**

Other open source packages and AWS services can process transformed time series datasets. Let's now have a look at the different transformations you can apply to your time series.

# Summarizing time series into tabular datasets

The first class of transformation you can apply to a time series dataset is to compact it by replacing each time series with some metrics that characterize it. You can do this manually by taking the mean and median of your time series or computing standard deviation. The key objective in this approach is to simplify the analysis of a time series by transforming a series of data points into one or several metrics. Once summarized, your insights can be used to support your EDA or build classification pipelines when new incoming time series sequences are available.

You can perform this summarization manually if you know which features you would like to engineer, or you can leverage Python packages such as the following:

+   - `tsfresh`: (http://tsfresh.com)
+   - `hctsa`: (https://github.com/benfulcher/hctsa)
+   - `featuretools`: (https://www.featuretools.com)
+   - `Cesium`: (http://cesium-ml.org)
+   - `FATS`: (http://isadoranun.github.io/tsfeat)

These libraries can take multiple time series as input and compute hundreds, if not thousands, of features for each of them. Once you have a traditional tabular dataset, you can apply the usual ML techniques to perform dimension reduction, uncover feature importance, or run classification and clustering techniques.