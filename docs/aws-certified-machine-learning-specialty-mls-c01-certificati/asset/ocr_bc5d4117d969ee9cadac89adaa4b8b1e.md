Two of the main hyperparameters of DeepAR are context_length, which is used to control how far in the past the model can see during the training process, and prediction_length, which is used to control how far in the future the model will output predictions.

DeepAR can also handle missing values, which, in this case, refers to existing gaps in the time series. A very interesting functionality of DeepAR is its ability to create derived features from time series. These derived features, which are created from basic time frequencies, help the algorithm learn time-dependent patterns. The following table shows all the derived features created by DeepAR, according to each type of time series that it is trained on:

| Frequency of the Time Series | Derived Features |
|---|---|
| Minute | Minute-of-hour, hour-of-day, day-of-week, day-of-month, day-of-year |
| Hour | Hour-of-day, day-of-week, day-of-month, day-of-year |
| Day | Day-of-week, day-of-month, day-of-year |
| Week | Day-of-month, week-of-year |
| Month | Month-of-year |

Figure 7.16 – DeepAR derived features per frequency of time series

We have now completed this section about forecasting models. Next, we will have a look at the last algorithm regarding supervised learning; that is, the Object2Vec algorithm.

# Object2Vec

Object2Vec is a built-in SageMaker algorithm that generalizes the well-known word2vec algorithm. Object2Vec is used to create embedding spaces for high multidimensional objects. These embedding spaces are, per the definition, compressed representatiosn of the original object and can be used for multiple purposes, such as feature engineering or object comparison:

![](bbox=[0.28, 0.72, 0.75, 0.9])

Figure 7.17 – A visual example of an embedding space