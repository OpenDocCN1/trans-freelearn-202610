+   - **S3 forecast export location:** Paste an S3 location from the bucket you also created in *Chapter 3, Creating a Project and Ingesting Your Data*. The format of this S3 path should be `s3://BUCKET_NAME/forecasts/automl/` where `BUCKET_NAME` should be replaced by the name of your bucket.

+   10. Click on the **Start** button located at the bottom right of this form to launch the forecast export job. Your export creation is now started.

After a while, your data is available in the S3 bucket you selected earlier. Let's now fetch these files and visualize their content.

# Visualizing your forecast's results

The forecast export process output the predictions in Amazon S3. Let's open this service to collect these results and visualize them:

+   1. At the top left of the AWS console, you will see a **Services** drop-down menu that will display all the available AWS services. In the **Storage** section, look for the **S3** service and click on its name to go to the S3 console.
+   
+   2. Click on the name of the bucket you used to upload your datasets to visualize its content.
+   
+   3. On this screen, you will see a `forecasts/` folder, click on it, and then click on the `automl` folder name. You will end up in a directory with a collection of CSV files:

![](bbox=[0.137, 0.553, 0.868, 0.744])

Figure 6.11 – Forecast export results as stored on Amazon S3

Each CSV file has a name that follows this pattern:

```
`<export_name>_<yyyy-mm-ddTHH-MM-SSZ>_part<NN>`
```

`export_name` is the label you chose to name your backtest export job followed by a timestamp (the time at which the export was generated) and a part number.