The newdata=Â label designates the new dataset to be stored. The lp type is one of several types used for prediction, and this method uses the linear predictor:

```
pred_validation <- predict(CoxModel.1, newdata=ChurnStudy.test, t
```

The resulting predictions will be in log form.

# Predicting the outcome at time 6

The predictions are in log form. Take the exponent and multiply by the base hazard estimate to obtain the predicted value at 6 months:

```
#
head(pred_validation)
#
pred.val <- base[6,1]*exp(pred_validation)
```

Letâs assume we want to predict the risk of churn that occurs halfway through the analysis period (time=6):

First, we will first take the exponents of the prediction and multiply them by the base hazard estimate that was shown in the hazard column at time=6. This method, in effect, adds the predictive power of the coefficients to the base hazard rate:

```
pred.val <- base[6,1]*exp(pred_validation)
```

We will now merge the predicted values in with the raw values from the test dataset and view the results after verifying that the number of rows is what we expect:

```
combine <- cbind(ChurnStudy.test,pred_validation,pred.val)
> nrow(combine)
[1] 496
View(combine)
```

The View function opens up the dataframe so that it can be examined: