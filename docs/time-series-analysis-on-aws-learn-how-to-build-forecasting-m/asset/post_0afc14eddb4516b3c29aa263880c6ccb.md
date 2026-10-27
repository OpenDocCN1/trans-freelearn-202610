
## Generating explainability for your forecasts

Since this chapter was written, Amazon Forecast has also deployed a new feature to provide some level of explainability for your predictions. In the previous paragraph, we walked through an error analysis to understand what can be done to improve the forecast accuracy. In this paragraph, we are going to explore this new explainability feature. Explainability is a set of practices and capabilities that help you understand the predictions made by a statistical, machine learning, or deep learning model. In essence, the goal of explainability is to open what can be perceived as a black box to the end users.

Amazon Forecast computes a specific metric (called the **impact score**) to quantify the impact (does it increase or decrease a given forecast value?) that each attribute of your dataset has on your time series. In this paragraph, we are going to look at how to generate such insights and then how to interpret them.

> **Important Note**
>
> Since this chapter was written, Amazon Forecast deployed the **AutoPredictor** feature: as mentioned in earlier chapters, when training a predictor with this feature, Amazon Forecast will analyze your data on your behalf to ensemble multiple algorithms and build better forecasts for each individual time series. However, depending on your data, you may still have to perform the type of error analysis we just went through in this paragraph: this practice will always be critical whichever forecasting tool, algorithm, and approaches you select in your line of business.

> **Note**
>
> As this was a rather new feature at the time of writing, the dataset we selected won't be the best example to explore the outputs from these new insights provided by Amazon Forecast. However, I will walk you through the process to generate and visualize them. Feel free to have a look at the explainability documentation (`https://docs.aws.amazon.com/forecast/latest/dg/forecast-explainability.html`) or this step-by-step guide if you're comfortable with running Python code in a Jupyter notebook: `https://github.com/aws-samples/amazon-forecast-samples/blob/main/notebooks/advanced/Item_Level_Explainability/Item_Level_Explainability.ipynb`.

### Overview

![](img/e4ee48104c730845e796d8c469dfeecc_201_0.png)

Figure 6.17 – Accessing the explainability feature from the main dashboard

5. You will be presented with a list of insights previously generated. Click on the **Create explainability** button at the top right of the screen.

6. On the **Create explainability** screen, you will need to enter the following information:

    - **Explainability name:** `london_energy_explainability`

    - **Select forecast:** From the drop-down list, select the name of the forecast you previously created. If you don’t see your forecast in the list and only see a **Forecast not supported** message, this means that your forecast is associated with a model which was not trained with the AutoPredictor (P-Code) feature. You will need to convert your legacy predictor to an AutoPredictor (P-Code) one and return to this screen to generate your explainability insights.

    - **Time duration details:** You can either request explainability for the whole forecast horizon or for specific time ranges.

    - **S3 location:** Fill in the S3 location of the CSV file you uploaded previously. The format of this S3 path should be `s3://BUCKET_NAME/timeseries_explainability.csv` where `BUCKET_NAME` should be replaced by the name of your bucket.

    - **IAM Role:** Select `Enter a custom IAM role ARN`.

    - **Custom IAM role ARN:** Paste the ARN of the AmazonForecastEnergyConsumption role that you created earlier in the IAM service (see *Chapter 3, Creating a Project and Ingesting Your Data*). The format of the ARN should be `arn:aws:iam::<ACCOUNT-NUMBER>:role/AmazonForecastEnergyConsumption`, where `<ACCOUNT-NUMBER>` needs to be replaced by your AWS account number.

    - **Data schema:** This schema should be the same as the target time series dataset schema (you should see `item_id` with a `string` type already filled in on your behalf). If you have other dimensions, you can add them here.

7. Scroll down to the bottom of the screen and click on the **Create** button to launch the explainability generation process.

You are brought back to the list of insights screen where you can see the explainability requests in progress. Once the report is ready, you can click on its name to visualize it.

### Visualizing forecast explainability

After an explainability report is ready, you will see its status as **Active** on the insights screen:

![](img/e4ee48104c730845e796d8c469dfeecc_202_0.png)

Figure 6.18 – List of available explainability reports

Click on the name of the report to bring up a detailed page. After the **Explainability details** section (where you can find the name of your report, how long it took to generate it, and when it was created), you will find the key section of interest, **Attribute increasing impact score**. This section includes two horizontal bar charts:

1. At the top, you will find the attributes that increase the normalized impact scores for a selected time series and time points. You can select an aggregated view to display the impact scores for either all the time series and/or all the time points. The following screenshot is extracted from step-by-step notebooks mentioned earlier. On this screenshot, you can see, for instance, that the `Customer_Request` attribute has twice the impact of `Loss_Rate`:

   ![](img/e4ee48104c730845e796d8c469dfeecc_203_0.png)

   Figure 6.19 – Attribute increasing impact score

   > **Important Note**
   >
   > The impact scores only measure the relative impact of the scores and cannot be used to improve model accuracy. They can only be used to explain the contribution each attribute has on a given forecast value.

2. At the bottom, you will see a similar bar chart but including only the attributes that decrease the impact scores.

Note that the console plots only display *normalized* impact scores (between -1 and 1) whereas direct API calls can also give you raw impact scores (based on Shapley values). Raw impact scores can be used to combine or compare scores across different time series and time points. However, these scores cannot be compared or combined when they come from different forecasts.

Congratulations, with this last feature, you have reached the end of this chapter! It's time to conclude and move on to the last chapter, dedicated to Amazon Forecast.

## Summary

In this chapter, you learned how to generate, export, and visualize the predictions for the models you trained in the previous chapters. You also learned how you could use the forecast export features to monitor the performance of your predictions at the time series level (and not only at the aggregated level as provided by the predictor evaluation dashboard).

## Generating New Forecasts

Generating forecasts will be your key activity once a model is trained. It's important that you practice this step, which includes establishing a sound error analysis process. Using error analysis at the item level is indeed critical to understand the areas where your forecasts can be improved and where a significant change in your data distribution impacts its performance.

In the next chapter, you will set up fully automated solutions that will perform even more heavy lifting for you so that you can focus on improving your forecast accuracy over time. You will also have a look at how you can put together the key components of a sound model monitoring strategy.

### Technical requirements

In this chapter, we will be tackling more advanced topics and as such, you will need some hands-on experience in a language such as Python to follow along in more detail. You will also need broader skills in several AWS cloud services that are beyond the scope of this book (such as **AWS CloudFormation** or **Amazon SageMaker**).

We highly recommend that you read this chapter while connected to your own AWS account and open the different service consoles to run the different actions on your end.

To create an AWS account and log into the Amazon Forecast console, you can refer to the *Technical requirements* section of *Chapter 2, An Overview of Amazon Forecast*.

The content in this chapter assumes that you have a predictor already trained; if you don't, we recommend that you follow the detailed process presented in *Chapter 4, Training a Predictor with AutoML*.

This chapter also assumes you have already generated a forecast from a trained predictor; if you haven't already, we recommend that you follow the detailed process presented in *Chapter 6, Generating New Forecasts*.

To follow along with the last section of this chapter (*Serverless architecture orchestration*), you will also need to download the following archive and unzip it:

```
https://packt-publishing-timeseries-on-aws-michaelhoarau.s3-eu-west-1.amazonaws.com/part1-amazon-forecast/energy_consumption_serverless.zip
```

### Deep diving into forecasting model metrics

When training a forecasting model, Amazon Forecast will use different machine learning metrics to measure how good a given model and parameters are at predicting future values of a time series. In this section, we are going to detail what these metrics are, why they are important, and how Amazon Forecast uses them.

In *Chapter 4, Training a Predictor with AutoML*, you trained your first predictor and obtained the following results:

![](img/e4ee48104c730845e796d8c469dfeecc_206_0.png)

Figure 7.1 – Results page: Accuracy metrics
