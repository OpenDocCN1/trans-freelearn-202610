It would be a much better fit if a regression problem involved sales prediction from inventory. Low bias models tend to result in over fitting and high bias models have lower accuracy but are simpler to explain.

Variance is related to how a model would change when supplied with different data. Low variance is also desirable.

Decision trees are an example of an algorithm, which tends to have low bias (over fits) but can come up with completely different results when given a new training sample (high variance). Even adding one additional observation to an existing decision tree model can result in a totally different tree.

To develop a good predictive model, you acknowledge that there ultimately will be a compromise or tradeoff between bias and variance. To learn more about the bias-variance trade-off, check out some of the external references in *Wikipedia*.

# Description of specific models

Here are some short descriptions of some of the models that we will be covering, with some short code examples.

# **Poisson (counts)**

A Poisson model is used to model counts of things. That could be the number of insurance claims filed in a given month, the number of calls which are received in a call center in a given minute, or the number of orders which are sold for a particular item. The Poisson distribution is the appropriate way for modeling count data since all data is positive and the range of the distribution is bound by 0 and infinity. The classic way of modeling a Poisson model is through the R `glm()` function using a `poisson` link function:

```
model.poisson <- glm(count ~ v1+v2+v3, data=inputdata, family=poi
```

Note that the preceding model specified merely shows the model in a generalized form. Do not try to run it since there are no variables other than the existing ones `v1`, `v2`, `v3` or `count`. However, what the model specification