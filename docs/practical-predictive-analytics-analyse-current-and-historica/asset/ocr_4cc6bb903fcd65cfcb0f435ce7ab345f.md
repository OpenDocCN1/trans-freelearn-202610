errors are normally distributed, and this is not true in the case of logistic regression. The error term is related to a conditional Bernoulli distribution.

However, logistic regression does share many of the features of regression models:

+   - It is a low variance algorithm, and as a result it is fairly simple to implement and explain, and does not have as many problems with overfitting as some of the other algorithms.
+   - It also has good capabilities for feature selection, and the addition of transformed variables and interactions which it shares with linear regression. However, what it *does* predicts (the likelihood of an event), sets it apart from standard regression techniques.

# The odds ratio

While linear regression will predict an actual outcome for the dependent variable, logistic regression does not predict actual outcomes or events. What it will predict is the likelihood, (or odds ratio) of one of the events occurring. This is slightly different since it is related to the probability of an event occurrence (rather than the prediction of the event itself).

The odds ratio is defined as the ratio of the probability of an event A occurring divided by the probability of an event not occurring. In functional form, this ratio is expressed as a natural logarithm:

```
ln\left( \frac{P}{1 - P} \right) = a + \beta_1X_1 + \beta_2X_2 + \dots \beta_1X_1
```

# The logistic regression coefficients

The coefficients returned from the logistic regression are standardized, in that