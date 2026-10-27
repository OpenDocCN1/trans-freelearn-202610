# Technical requirements

No hands-on experience in a language such as **Python** or **R** is necessary to follow along with the content from this chapter. However, we highly recommend that you read this chapter while connected to your AWS account and open the Amazon Forecast console to run the different actions on your end.

To create an AWS account and log into the Amazon Forecast console, you can refer to the *Technical requirements of Chapter 2, An Overview of Amazon Forecast.*

The content in this chapter assumes that you have a predictor already trained and ready to be used to generate new forecasts: if you don't, we recommend that you follow the detailed process explained in *Chapter 4, Training a Predictor with AutoML*. Once you have a trained predictor, you are ready to generate new predictions.

# Generating a forecast

In *Chapter 4, Training a Predictor with AutoML*, and *Chapter 5, Customizing Your Predictor Training*, we focused on the first step of the forecasting process: looking back in historical data to establish a baseline and uncover trends that may continue in the future. In Amazon Forecast, this is done by training a predictor (your trained model). Once you have a model ready to be used, you can use Amazon Forecast to generate predictions for your time series: this is the highlighted area on the right side in the following figure:

![](bbox=[0.12, 0.55, 0.87, 0.81])

Figure 6.1 – Amazon Forecast overview