And yes, it turns out that, sometimes, it might happen that you don’t have a time series in front of you. Furthermore, no matter your efforts, you will not be able to model this data as a time series. This type of data is often known as **white noise.**

Another type of series that we cannot predict is known as **random walk.** Random walks are random by nature, but they have a dependency on the previous time step. For example, the next point of a random walk could be a random number between 0 and 1, and also the last point of the series.

> **Important note**
> Be careful if you come across those terms in the exam and remember to relate them to randomness in time series.

With that, we have covered the main theory about time series modeling. You should also be aware that the most popular algorithms out there for working with time series are known as **Auto-Regressive Integrated Moving Average (ARIMA)** and **Exponential Smoothing (ETS).** We will not look at the details of these two models. Instead, we will see what AWS can offer us in terms of time series modeling.

# Understanding DeepAR

The **DeepAR** forecasting algorithm is a built-in SageMaker algorithm that’s used to forecast a one-dimensional time series using a **Recurrent Neural Network (RNN).**

Traditional time series algorithms, such as ARIMA and ETS, are designed to fit one model per time series. For example, if you want to forecast sales per region, you might have to create one model per region, since each region might have its own sales behaviors. DeepAR, on the other hand, allows you to operate more than one time series in a single model, which seems to be a huge advantage for more complex use cases.

The input data for DeepAR, as expected, is one or more time series. Each of these time series can be associated with the following:

+   - A vector of static (time-independent) categorical features, controlled by the `cat` field
+   - a vector of dynamic (time-dependent) time series, controlled by `dynamic_feat`

> **Important note**
> Note that the ability to train and make predictions on top of multiple time series is strictly related to the vector of static categorical features. While defining the time series that DeepAR will train on, you can set categorical variables to specify which group each time series belongs to.