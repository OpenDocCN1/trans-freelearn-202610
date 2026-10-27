
The following screenshot summarizes the forecasting problem scope:

![](img/e4ee48104c730845e796d8c469dfeecc_73_0.png)

Figure 2.1 – Time series forecasting components

Let's dive into several of the key areas of this screenshot, as follows:

- **Historical data:** The historical data is the part of your time series that will be used to train a forecasting model. In *Figure 2.1*, this is the regular line plot located on the left of the graph.
- **Historical data frequency:** Your historical time series data can be available at a regular or irregular sampling rate. If your raw data is not regularly sampled or if its frequency is different from the forecast granularity, you may have to aggregate your data at the desired unit. In retail, each product sale is recorded when it happens, but not every product is sold every day, leading to a sparse time series with spikes that correspond to days when a sale has happened. Aggregated sales for each product at a weekly or monthly level will smooth out the original signal.
- **Forecast horizon:** This is the prediction length you can expect for a forecast. This is usually measured in the number of time steps using the desired unit, which might be different from the original time series. If you expect daily data as a result of your forecast and need predictions for the coming week, your sampling unit will be at the daily level and your forecast horizon will be 7 days.
- **Expected output:** You can request a point-in-time forecast. In the plot shown in *Figure 2.1*, this is the dotted line located in the forecast horizon and it helps answer this type of question: *What will the value of the time series be by tomorrow?* A forecast can also emit a value for several time steps at once. This helps answer questions such as: *Which value will the time series take for every day of the coming week?* Your forecast may also be consumed by organizations or systems that expect a range of possible values; in this case, you will replace the point values with probabilistic forecasts that emit a range of possible values at each predicted time step (in the previous plot, this is represented by the area plot around the median forecast denoted by a dotted line, delimited by the lower bound and upper bound of the requested quantiles—p10 and p90 in this case).

You now know how to frame a good forecasting problem, so let's apply this fresh knowledge to Amazon Forecast!

## What is Amazon Forecast?

**Amazon Forecast** is one of the AI-/ML-managed services available on the AWS cloud platform. Flexible and accurate forecasts are key in many business areas to predict demand and ensure inventory or raw material are stocked accordingly or predict wait times and ensure your counters are staffed accordingly. While all organizations use some form of forecasting process today, traditional methods cannot leverage the increasing quantity and complexity of time series signals available to us.

**Managed services** are services where the end users only bring their data and configure some parameters to suit their needs. All other tasks, *considered as undifferentiated heavy lifting*, are performed on the users' behalf by the service. This includes the automation of all the infrastructure management: as an Amazon Forecast user, you don't have to provision and manage **virtual machines (VMs)**, configure user accounts, manage security, plan for scalability if your request volume increases, decommission unused resources, and so on.

In the case of **AI-/ML-managed services**, some *data preparation, ingestion tasks, and model management activities are also performed under the hood*, allowing you to focus primarily on the problem to solve. Amazon Forecast is a scalable managed service that automates the whole **end-to-end (E2E)** forecasting pipeline, from data ingestion to model deployment and serving forecasts. The service also deals with data preparation under the hood. Amazon Forecast can do the following:

- Fill missing values in your datasets while letting you customize the parameters to use to replace them.
- Prepare the time series so that it can be ingested by multiple algorithms—Amazon Forecast can prepare time series sequences to feed **deep learning (DL)** models or generate multiple random sequences to output probabilistic forecasts for proprietary algorithms such as **non-parametric time series (NPTS)**.
- Pick the best algorithms depending on your datasets—Amazon Forecast can use AutoML to automatically choose between statistical forecasting algorithms (such as **AutoRegressive Integrated Moving Average (ARIMA)**, **Exponential Smoothing (ETS)**, NPTS, or Prophet) and more advanced data-hungry neural network-based architectures (such as DeepAR+ or **Convolutional Neural Network-Quantile Regression (CNN-QR) deep neural networks (DNNs)**). You will have an overview of the different algorithms used by Amazon Forecast, along with the advantages and disadvantages of using each of them, in *Chapter 5, Customizing Your Predictor Training*.
- Segment your data and automatically apply the best ensemble of algorithms for each segment.
- Provide confidence intervals thanks to probabilistic forecast generation—traditional algorithms only output point forecasts (usually the mean value) with a confidence interval, relying on strong assumptions about the data distribution.
- Provide explainability scores to help you understand which factors influence the forecast of specific items and for which time duration.
- Manage multiple local statistical models for each of your time series—using ARIMA, ETS, NPTS, or Prophet to train on 100 time series means that you would have to manage 100 models on your own. Amazon Forecast transparently manages all these models for you and lets you easily export all forecasts at once as a single **comma-separated values (CSV)** file after inference as if it were just one model.
- Perform model evaluation on multiple time windows and provide evaluation results for all your time series data at once in a single CSV file digestible by the usual spreadsheet or **business intelligence (BI)** visualization software. Amazon Forecast uses backtesting techniques on your behalf (you will deep dive into this feature and how to customize it to suit your needs in *Chapter 5, Customizing Your Predictor Training*).
- Automatically compute multiple accuracy metrics when evaluating and selecting the best models, including traditional point forecast metrics such as **Weighted Absolute Percentage Error (WAPE)** and unbiased error metrics for probabilistic forecasts (namely, **Weighted Quantile Loss (wQL)**). You will discover the details behind these accuracy metrics in *Chapter 4, Training a Predictor with AutoML*.
- Perform hyperparameter optimization to find the best parameters to train your models.
- Add built-in features such as public holidays or weather data to improve your forecast accuracy. When relevant, Amazon Forecast also feeds features such as lagged values to capture seasonality in your datasets. For instance, if your samples are taken at an hourly frequency, Amazon Forecast will feed values from the last 1, 2, and 3 days to algorithms such as DeepAR+ that can benefit from these additional insights to build more accurate models.

Based on the same technology used at *Amazon.com*, Amazon Forecast drills down the extensive experience Amazon has on forecasting techniques to make it available to every developer and data scientist. It's a fully managed service that uses statistical and DL-based algorithms to deliver accurate time series forecasts. Although an ML practitioner or a data scientist will be able to leverage their skills to improve the service results and make the best of it, Amazon Forecast does not require any ML experience to deliver future time series predictions based on your historical data.

Amazon Forecast also includes the latest advances in DL to provide DL algorithms (namely **DeepAR+** and **CNN-QR**) that are able to learn more complex models and serve situations where you might need to build forecasts for millions of time series, address cold start situations, or easily build what-if scenarios. Amazon Forecast is also able to automatically segment your data and build an ensemble of models, increasing forecast accuracy with no additional heavy analysis at your end.

Up to now, you have seen how to frame a forecasting problem and how the Amazon Forecast service can help you solve this. You will now see how Amazon Forecast works, which will give you the keys to successfully apply this service to your business problems.

## How does Amazon Forecast work?

To build forecasting models, Amazon Forecast deals with the following concepts and resources:

- **Datasets:** A dataset is a container to host your data. Amazon Forecast algorithms use these datasets to train its models. Each dataset is defined by a schema whereby you can define the columns and their types. Amazon Forecast includes several domains with predefined schemas to help you get started.
- **Dataset groups:** Amazon Forecast algorithms can leverage several datasets to train a model. A dataset group is a container that packages a group of datasets used together to train a forecasting model.
