You will need to do the following:

+   - Give a *name* to your import. This will help you distinguish between different import jobs. We will call it `energy_consumption_20210519` (I added a timestamp to know when I imported it).

> **Important Note**
> Note that *successive import jobs are not aggregated*: only the most recent import will be considered when you use your datasets to train a predictor.

+   - Enter the S3 URI for the *target time series* that you copied earlier when you uploaded the CSV file to Amazon S3.

+   - Select **Enter a custom IAM role ARN** and paste the ARN of the `AmazonForecastEnergyConsumption` role that you created earlier in the IAM service. The format of the ARN should be `arn:aws:iam::<ACCOUNT-NUMBER>:role/AmazonForecastEnergyConsumption`, where `<ACCOUNT-NUMBER>` needs to be replaced by your AWS account number.

Once you're done, you can click on **Start** to start the ingestion process. Your ingestion process begins.

![](bbox=[0.11, 0.5, 0.85, 0.68])

Figure 3.26 – Target time series ingestion in progress

If you click on your dataset name, you can get the status with an estimated time remaining for ingestion. You also have the ability to stop or delete the ingestion if you made a mistake and don't want to wait for the process to finish before you can actually restart it.