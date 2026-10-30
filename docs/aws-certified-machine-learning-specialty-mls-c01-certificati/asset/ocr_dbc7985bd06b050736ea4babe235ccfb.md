# Exploring other regression metrics

There are many more metrics that are suitable for regression problems aside from the ones that we have just learned. We will not be able to cover most of them here, but there are a few more metrics that might be important for you to know.

One of these metrics is known as the Mean Absolute Percentage Error (MAPE). As the name suggests, MAPE will compute the absolute percentage error of each prediction and then take the average value. The following formula shows how this metric is computed:

![](bbox=[0.13, 0.3, 0.85, 0.48])

MAPE is broadly used on forecasting models since it is very simple to interpret, and it provides a very good sense of how far (or close) the predictions are from the actual values (in terms of a percentage).

We have now completed this section on regression metrics. Next, we will talk about model optimizations.

# Model optimization

As you know, understanding evaluation metrics is very important in order to measure your model's performance and document your work. In the same way, when we want to optimize our current models, evaluating metrics also plays a very important role in defining the baseline performance that we want to challenge.

The process of model optimization consists of finding the best configuration (also known as hyperparameters) of the machine learning algorithm for a particular data distribution. We don't want to find hyperparameters that overfit the training data in the same way that we don't want to find hyperparameters that underfit the training data.

You learned about overfitting and underfitting in Chapter 1, Machine Learning Fundamentals. In the same chapter, you also learned how to avoid these two types of modeling issues.