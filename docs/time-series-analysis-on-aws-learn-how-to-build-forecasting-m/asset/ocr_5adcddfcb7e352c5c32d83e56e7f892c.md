Last but not least, you will define which forecast types you are interested in:

```
Forecast:
  ForecastTypes:
    - "0.10"
    - "0.50"
    - "0.90"
```

You will find a copy of the complete YAML file in the archive downloaded earlier.

Navigate to the Amazon S3 bucket you took note of earlier and upload this YAML file to this bucket. Also, click on Create folder and create a folder named train. Your S3 bucket root looks like the following (a folder named glue was created during the initial deployment and should already be present):

![](bbox=[0.115, 0.34, 0.85, 0.555])

Figure 7.18 – S3 bucket initial configuration

Your solution is now configured and ready to be used!

# Using the solution

After deploying the Improving Forecast Accuracy with Machine Learning solution, you are now ready to use it. In this section, you will learn how to trigger and monitor the main workflow run by the solution and how to visualize the results.

# Monitoring the workflow status

Navigate to the train directory of your S3 bucket and upload the three CSV files you extracted in the Data preparation component section.