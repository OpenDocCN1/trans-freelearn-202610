
Confusion Matrix and Statistics

Reference
Prediction | cold | hot
cold | 16 | 5
hot | 4 | 20

Accuracy : 0.8
95% CI : (0.654, 0.9042)
No Information Rate : 0.5556
P-Value [Acc > NIR] : 0.0005445

Kappa : 0.597
Mcnemar's Test P-Value : 1.0000000

Sensitivity : 0.8000
Specificity : 0.8000
Pos Pred Value : 0.8333
Neg Pred Value : 0.7619
Prevalence : 0.5556
Detection Rate : 0.4444
Detection Prevalence : 0.5333
Balanced Accuracy : 0.8000

'Positive' Class : hot

Resulting confusion matrix summary from gradient boosting machines.

## Ensemble

Ensemble methods are a way to combine ML models together to generate a prediction. Think of it as an ML model committee where each ML model casts its vote and the tallied result is the prediction.

There are various methods to tally the votes, and you will want to experiment with them to see if you can increase the performance of your ensemble model. However, this is outside the scope of this book.

Research and real world usages have shown that ensemble methods often perform better than any of the incorporated models alone. Ensemble methods are a way to improve real-world performance by reducing the prediction variation of any one model. They should be in your data science toolkit for IoT analytics.

## Anomaly detection using R

Anomaly detection is a way to use historical data to identify unusual observations without requiring a labeled training set. Modern anomaly detection methods take into account long-term trends and cyclical variation in the data while determining which observations to flag as anomalies.

Twitter has recently released an advanced open source anomaly detection package for R called *AnomalyDetection*. It is geared toward detecting anomalies in single value high frequency (less than a day) time series data; however, it is possible to set an option to handle datasets longer than a month. It can also be used on a vector of non-time series data.

It is good at handling the effects of trends and seasonality - although seasonality, in this case, is at the minutes to days level not yearly. The GitHub page is located here (https://github.com/twitter/AnomalyDetection), and it can be installed easily as an R package using the following R code. Make sure to spell Anomaly with a capital A. *anomalyDetection* is a different package (welcome to the world of R):

```
install.packages("devtools")
devtools::install_github("twitter/AnomalyDetection")
```

The following example shows how anomaly detection can be used on sensor data. Anomaly detection is useful in itself, but the frequency of anomalies detected in a time window (hourly or daily) can have good predictive value when used as an input into other models. For example, the frequency of pressure sensor anomalies in the last hour combined, with an average temperature sensor reading, could be highly predictive of a future generator failure.

The following example code takes the **Wind** values from the imputed dataset and looks for anomalies. The search can be either forward or backward, or both:

```
#load library
library(AnomalyDetection)

#This uses the vector method since no timestamp is present. If timestamp is
present, use AnomalyDetectionTs
res = AnomalyDetectionVec(cs_example_data$Wind, max_anoms=0.05, period=60,
direction='both', only_last=FALSE, plot=TRUE)
res$plot
```

The code creates a plot, which is shown in the following figure. The points that it determines are possible anomalies are circled in blue:

![](img/9f2a55db1236f995dcd9392acbc82910_310_0.png)

Anomaly detection on Wind values in our example dataset

## Forecasting using ARIMA

Sometimes, you will have the need to forecast future values of a time series. For example, this could be a requirement to estimate the next several months of active IoT devices; or, it could be a need to project the usage hours of remote oil well pumps. One of the most popular methods to forecast time series is AutoRegressive Integrated Moving Average (ARIMA).

ARIMA is not one model but a collection of related methods that attempt to describe autocorrelations in the data in order to forecast future values. ARIMA is a combination of moving average and autoregressive techniques. Autoregressive means that the forecasting of future values of a variable is based on the linear combination of the past values of variables.

ARIMA incorporates both trend and seasonality effects into future forecasts. It can model both seasonal and nonseasonal data with a range of methods.

### Using R to forecast time series IoT data

The forecast package contains ARIMA functions in R. You can install it with the following code:

```
install.packages("forecast")
```

The Arima function has settings for the nonseasonal component of a forecast and the seasonal component (if needed). Each is composed of three numbers that represent the order of the autoregressive part, the degree of differencing to be used, and the order of the moving average part. This is commonly notated as (p,d,q) in R documentation. As with all things in data science, the right place to start is to look at your data, note any unusual trends, and perform some diagnostic statistical analysis to understand what are the appropriate values to use for (p,d,q) for both the nonseasonal and seasonal components.

Fortunately, and also dangerously (has that been mentioned before?), R can handle a lot of this for you and automatically determine the appropriate values based on a series of tests. This is done using the auto.arima() function. You should also analyze the results and the residual values to determine if your forecast model is viable.

The following example code takes the Wind values from the New York air quality dataset that we have been using in this chapter and fits an ARIMA model:

```
#load forecast library
library(forecast)

#create a time series on the Solar radiation data. The air quality data was captured from May 1, 1973 to September 30, 1973
windTS <- ts(completed_example_data$Wind,start = c(1973,5,1), frequency = 365.25)

#show values over time
plot.ts(windTS)

#fit an arima model
```

```
fit <- auto.arima(windTS)

#plot forecast for the next month
plot(forecast(fit, h=30))
```

The resulting forecast plot shows some wide variation in projected **Wind** values. You should not quit your job to be a weatherman based on this simple R code ARIMA example:

![](img/9f2a55db1236f995dcd9392acbc82910_312_0.png)

ARIMA forecast of Wind values. Note the auto.arima function chose a 3,1,1 model (p,d,q notation).

## Deep learning

**Deep learning** is an area of data science that is experiencing rapid advancement and generating a lot of excitement. Some deep learning models are better at certain types of image recognition than humans. When stories in the media mention artificial intelligence, they are usually referring to deep learning models.

Deep learning models are very complex although several of the concepts are similar to the ML concepts we have discussed so far in this chapter (such as the bias-variance tradeoff). Deep learning models can have millions of features and can take days or weeks to train.

### Use cases for deep learning with IoT data

Deep learning can do wonders for complex data, with thousands to millions of features and a large history of labeled examples to use as training sets. The rapid advancements in image recognition has as much to do with the vast trove of identified images that Google and others have collected over the years, as to the advances in the deep learning algorithms used.

For IoT data, this can limit the usefulness of deep learning techniques. Most IoT data is relatively new without a long history of labeled examples. Many IoT devices have only a few sensors, so the feature set is not as complex. In these situations, many of the ML techniques discussed previously can do as good, if not better, job of prediction than deep learning techniques. Deep learning is also more computationally expensive in both time and compute power (that is, higher cost).

However, if the IoT data flow includes a large number of features and hundreds of thousands to millions of labeled training data is available, deep learning techniques may be able to provide a significant boost in predictive power. This is clearly the case in autonomous vehicle development. It could also do wonders where images are captured as part of the device functionality (static or video).

Deep learning packages tend to be interacted with best by Python as opposed to R. They are also relatively new, so expect documentation and tutorials to be limited. This makes them a little more finicky to work with than the prior examples in this chapter. You will need more time and expertise to develop deep learning models than for more well-trodden ML models, such as random forest.

Use what method makes the most sense for the individual use case and available training data. Consider the effectiveness requirements and see whether using a deep learning modeling technique is likely to provide enough boost in accuracy to warrant the expense.

### A Nickel Tour of deep learning

Deep learning is a big umbrella that covers many projects and neural network configurations using a variety of methods. The following chart can give you an idea of the variety of architectures available. There is typically a trade-off between the accuracy obtainable and required computing time to train the models that has to be considered. The right trade-off depends on the use case. The y axis represents accuracy and the x axis represents compute requirements:
