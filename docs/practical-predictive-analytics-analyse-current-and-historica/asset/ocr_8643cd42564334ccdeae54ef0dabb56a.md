means is that the independent variables are all dependent upon the previous values of that variable. Many data techniques assume that all values occur independently of each other, so special techniques were developed to deal with time-based data.

**Exponential smoothing** is based upon the simple concept that the most recent data is the best predictor of the future. Exponential smoothing techniques allow you to tweak the parameters so that you can decide how much weight is given to the recent data versus the older data. It is suitable for many time-series problems.

Time-series models can also be based upon cyclical, trend, seasonal, one-time occurrence, or causal factors. One of the more advanced time-series methods is known as **ARIMA** or **Box-Jenkins** models which incorporate elements of time-series and regression models.

# # Naive Bayes classifier

Naive Bayes originally became popular as a method for spam detection. It is quick and fast. Naive Bayes assumes that the variables are all independent and not related to each other (a bad assumption, but that is what makes it Naive!). It also has the advantage that it does not need to be retrained when adding new data. Naive Bayes has its roots in Bayes theorem.

This simple example shows Naive Bayes in action. Using the Iris dataset, Naive Bayes will make a prediction for the fifth column using the first four columns as independent variables:

```
#use 1st 4 columns to predict the fifth
library(e1071)
iris.nb<-naiveBayes(iris[,1:4], iris[,5])
table(predict(iris.nb, iris[,1:4]), iris[,5])
```

The results of the `table()` function will go to the console. This output is the confusion matrix which is as follows:

| | setosa | versicolor | virginica |
|---|---|---|---|
| setosa | 50 | 0 | 0 |
| versicolor | 0 | 47 | 3 |