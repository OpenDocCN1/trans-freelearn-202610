would be the area where advantageous conditions converge: high true positives with low false positives.

AUC is a good measure of the tradeoffs involved in classification errors.
However, it should not be considered as absolute. Costs of misclassification
should be taken into account when using the AUC.

# Computing an ROC curve using the titanic dataset

Here is an example of plotting an ROC curve on the `titanic` dataset, using a
simple logistic regression model to predict survival:

```
install.packages("titanic")
install.packages("ROCR")

library(titanic)
library(ROCR)

titanic <- titanic_train[complete.cases(titanic_train), ]

model <- glm(as.factor(Survived) ~ Sex+Age+Pclass, data=titanic,
             pred <- prediction(predict(model), titanic$Survived)
             perf <- performance(pred,"tpr","fpr")
             plot(perf)
             abline(a=0,b=1)
```

The AUC curve is plotted along with the various cutoff values for the
probability of the predicted outcome. The diagonal reference line represents a
random model. The area under the logistic model curve looks to be about
75% of the total area: