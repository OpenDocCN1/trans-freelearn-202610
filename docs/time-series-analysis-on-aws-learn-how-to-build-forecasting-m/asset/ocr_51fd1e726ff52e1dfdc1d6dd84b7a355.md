# Generating explainability for your forecasts

Since this chapter was written, Amazon Forecast has also deployed a new feature to provide some level of explainability for your predictions. In the previous paragraph, we walked through an error analysis to understand what can be done to improve the forecast accuracy. In this paragraph, we are going to explore this new explainability feature. Explainability is a set of practices and capabilities that help you understand the predictions made by a statistical, machine learning, or deep learning model. In essence, the goal of explainability is to open what can be perceived as a black box to the end users.

Amazon Forecast computes a specific metric (called the **impact score**) to quantify the impact (does it increase or decrease a given forecast value?) each attribute of your dataset has on your time series. In this paragraph, we are going to look at how to generate such insights and then how to interpret them.

> **Important Note**
> 
> Since this chapter was written, Amazon Forecast deployed the **AutoPredictor** feature: as mentioned in earlier chapters, when training a predictor with this feature, Amazon Forecast will analyze your data on your behalf to ensemble multiple algorithms and build better forecasts for each individual time series. However, depending on your data, you may still have to perform the type of error analysis we just went through in this paragraph: this practice will always be critical whichever forecasting tool, algorithm, and approaches you select in your line of business.

> **Note**
> 
> As this was a rather new feature at the time of writing, the dataset we selected won't be the best example to explore the outputs from these new insights provided by Amazon Forecast. However, I will walk you through the process to generate and visualize them. Feel free to have a look at the explainability documentation (`https://docs.aws.amazon.com/forecast/latest/dg/forecast-explainability.html`) or this step-by-step guide if you're comfortable with running Python code in a Jupyter notebook: `https://github.com/aws-samples/amazon-forecast-samples/blob/main/notebooks/advanced/Item_Level_Explainability/Item_Level_Explainability.ipynb`.