# Technical requirements

No hands-on experience of a language such as **Python** or **R** is necessary to follow along with the content from this chapter. However, we highly recommend that you read this chapter while being connected to your own **Amazon Web Services (AWS)** account and open the Amazon Forecast console to run the different actions on your end.

To create an AWS account and log in to the Amazon Forecast console, you can refer to the *Technical requirements* section of *Chapter 2, An Overview of Amazon Forecast*.

The content in this chapter assumes that you have a dataset already ingested and ready to be used to train a new predictor. If you don't, I recommend that you follow the detailed process detailed in *Chapter 3, Creating a Project and Ingesting your Data*. You will need the **Simple Storage Service (S3)** location of your three datasets, which should look like this (replace `<YOUR_BUCKET>` with the name of the bucket of your choice):

+   - **Target time series**: `s3://<YOUR_BUCKET>/target_time_series.csv`
+   - **Related time series**: `s3://<YOUR_BUCKET>/related_time_series.csv`
+   - **Item metadata**: `s3://<YOUR_BUCKET>/item_metadata.csv`

You will also need the **Identity and Access Management (IAM)** role you created to let Amazon Forecast access your data from Amazon S3. The unique **Amazon Resource Number (ARN)** of this role should have the following format: `arn:aws:iam::<ACCOUNT-NUMBER>:role/AmazonForecastEnergyConsumption`. Here, you need to replace `<ACCOUNT-NUMBER>` with your AWS account number.

With this information, you are now ready to dive into the more advanced features offered by Amazon Forecast.

# Choosing an algorithm and configuring the training parameters

In *Chapter 4, Training a Predictor with AutoML*, we let Amazon Forecast make all the choices for us and left all the parameters at their default values, including the choice of algorithm. When you follow this path, Amazon Forecast applies every algorithm it knows on your dataset and selects the winning one by looking at which one achieves the best average **weighted absolute percentage error (WAPE)** metric in your backtest window (if you kept the default choice for the optimization metric to be used).