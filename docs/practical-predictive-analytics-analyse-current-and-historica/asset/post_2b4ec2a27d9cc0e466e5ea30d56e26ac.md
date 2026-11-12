
train our Cox regression model. After we created it, we just put it off to the side. We will demonstrate one way to use this hold out dataset to validate the training results obtained from CoxModel.

Locate the #predict=== label and run the following code:

```
#predict===
par(mfrow=c(1,1))
#
```

### Computing baseline estimates

We will first determine the baseline (or average) estimates for the regression model we have just run, using the basehaz() function. We will assign it to an object named base, and then we will print and plot it.

```
# Let's start by looking at the baseline estimates for each time period.
#
base <- basehaz(CoxModel.1)
print(base)
> print(base)
     hazard time
1  0.007174321  1
2  0.033151848  2
3  0.051088747  3
4  0.062057960  4
5  0.084305023  5
6  0.109773610  6
7  0.149493300  7
8  0.200891166  8
9  0.281904190  9
10 0.420681696  10
11 0.623702585  11
12 1.281690658  12
```

Recall that the hazard is the likelihood that an event (churn) will happen, given that it hasn't already happened. This terminology is slightly different from the term survival rate, which we originally discussed, which is the percentage of the population in which the event did not happen.

We can see that the likelihood of churn always increases as time progresses. Churn seems to increase linearly until month 7 or 8, and then begins to increase exponentially with the greatest churn at the end of 12 months. This could occur as customers begin to approach the end of any contract that is in place.

Plot the baseline hazard for each time period:

```
ggplot(base, aes(base$time, base$hazard)) + geom_bar(stat = "identity") +
ggtitle("Churn Baseline Hazard by Time") +
labs(x="Month", y="Hazard")
dev.copy(jpeg,'Ch5 - baseline hazard.jpeg');
dev.off()
```

![](img/672c0cf8ba3c000a6529b117ed96945e_328_0.png)

### Running the predict() function

Next, we will use the predict() function to score the test data in ChurnStudy.test, based upon the prediction model trained on ChurnStudy.

The newdata= label designates the new dataset to be stored. The lp type is one of several types used for prediction, and this method uses the linear predictor:

```
pred_validation <- predict(CoxModel.1, newdata=ChurnStudy.test, type = "lp")
```

The resulting predictions will be in log form.

### Predicting the outcome at time 6

The predictions are in log form. Take the exponent and multiply by the base hazard estimate to obtain the predicted value at 6 months:

```
#
head(pred_validation)
#
pred.val <- base[6,1]*exp(pred_validation)
```

Let's assume we want to predict the risk of churn that occurs halfway through the analysis period (time=6):

First, we will take the exponents of the prediction and multiply them by the base hazard estimate that was shown in the hazard column at time=6. This method, in effect, adds the predictive power of the coefficients to the base hazard rate:

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

| ID | Churn | Xeducation | Xgender | Xsatisfaction | Xsatisfaction2 | Xservice.calls | Xtenure2 | Xpurch.last.month | Xmonthly.charges | pred_validation | pred_val |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 1081 | 0 | Master's Degree | M | 3 | 3 | 0 | 9 | 1 | 115.098475 | -0.326359648 | 0.07920067 |
| 691 | 1 | Master's Degree | M | 4 | 2 | 0 | 9 | 1 | 372.688733 | 1.785420280 | 0.65447961 |
| 1413 | 0 | Bachelor's Degree | F | 3 | 5 | 0 | 10 | 0 | 120.539268 | -0.505061482 | 0.06624491 |
| 1699 | 0 | Bachelor's Degree | M | 3 | 1 | 1 | 7 | 1 | | -2.017806 | 0.03227968 |
| 1111 | 0 | Bachelor's Degree | M | 5 | 3 | 3 | 7 | 1 | 67.455601 | -0.545857524 | 0.06359677 |
| 1962 | 0 | Bachelor's Degree | M | 1 | 5 | 0 | 9 | 1 | 31.944160 | -0.742731950 | 0.05223163 |

Look at the last column, which contains the prediction. The prediction is essentially a risk score that ranges from 0 to 1.

Examine the predicted value (pred_val) with the actual churn output (Churn) to get an idea of how the predicted values vary with the outcomes. Also examine the relationship between the independent variables and the predicted values. For example, you can see within the first few rows that the churn=1 event contains a much higher monthly charge than the other surrounding rows.

To see the difference in the average scores between the churners and non-churners, we can take some mean aggregate functions and print the results. After running the following code, we can see difference between the churners (who have higher risk scores) and the non-churners:

```
y <- aggregate(combine$pred.val, by=list(combine$Churn), FUN=mean)
print (y)
> print (y)
  Group.1        x
1       0 0.0646759
2       1 0.2326843
```

### Determining concordance

A concordance index is another measure that is used in survival analysis to determine how well the model is able to discern between the observed and predicted responses. For our churn example, we would expect the churners to have higher hazard rates than those customers who remain active. If the concordance index is more than .5, that indicates that there is some predictive ability built into the model. To compute the index we will use the concordance.index() function from the survcomp package to measure the agreement of the pred_validation statistic, with the actual churn outcome. As in previous examples, one needs to supply the predictions, time variable, and event variable as arguments to the function. We will also print the associated confidence intervals:

```
library(survcomp)
cindex_validation = concordance.index(pred_validation, surv.time,
                                      surv.event=ChurnStudy.test)
print(cindex_validation$c.index)
print(cindex_validation$upper)
print(cindex_validation$lower)
```

For our example, the computed index is 70%, with confidence intervals showing that it can range between 60 and 77%:

```
> print(cindex_validation$c.index)
[1] 0.6973212
> print(cindex_validation$upper)
[1] 0.7736508
> print(cindex_validation$lower)
[1] 0.6082843
```

### Time-based variables

Up until now, we have treated all of our variables as static, that is, they maintained their original values measured from the beginning of the measurement period.

In reality, values such as age and marital status change over time, and these changes can be accounted for by the model. In the marketing context, surveys might be administered after the study has begun. Based upon changes in some of these variables, coupons and other incentives might be offered (interventions) with the purpose of changing customer behavior. In the model, these interventions can also be accounted for.

In our example, we will introduce a hypothetical second survey, which was introduced 6 months into the measurement period and measured the effect of treating some of the unsatisfied customers.

### Changing the data to reflect the second survey

The following code uses the survSplit function to create a new record at time period 6 that will reflect the response to a second hypothetical customer survey administered at that time.

Copy the following code and run it in a new script window:

```
library(survival)
SURV2 <- survSplit(data = ChurnStudy, id="ID.char", cut = 6, end="time")
SURV2$CustomerID <- as.integer(SURV2$ID.char)
SURV2 <- SURV2[order(SURV2$CustomerID), ]
```

In the R console, execute the following commands to see how the dataframe has grown larger. Because we are splitting records at the mid-point (6 months), we have almost doubled the original records:

```
> nrow(ChurnStudy)
[1] 1984
> nrow(SURV2)
[1] 3512
```

### How survSplit works

We will look a bit closer at the survSplit function and how it affects the satisfaction variable. Since we need to keep track of how customer satisfaction changed at time period 6, survSplit function will alter the dataframe by creating new rows and adjusting the time periods to reflect the change in customer satisfaction. In our example, that means that the function would create additional rows after the 6 months cutoff (cut=6) based upon the values of Xtenure2.

As an example of how this would work, first look at the original ChurnStudy dataframe and observe that the very first record has a tenure of 7 months. This customer would have been around long enough for a second survey:

```
ChurnStudy$seqid <- seq(1:nrow(ChurnStudy))
View(subset(ChurnStudy, select = c(seqid,Xtenure2,Xsatisfaction,
```

| | seqid | Xtenure2 | Xsatisfaction | Xsatisfaction2 | Churn |
|---|---|---|---|---|---|
| 43 | 1 | 7 | 1 | 5 | 1 |
| 219 | 2 | 9 | 3 | 6 | 1 |
| 44 | 3 | 6 | 4 | 6 | 1 |
| 1412 | 4 | 11 | 3 | 2 | 0 |
| 12 | 5 | 11 | 4 | 3 | 1 |
| 1458 | 6 | 1 | 3 | 1 | 0 |
| 519 | 7 | 4 | 1 | 5 | 1 |
| 562 | 8 | 12 | 4 | 3 | 1 |
| 668 | 9 | 12 | 4 | 2 | 1 |
| 135 | 10 | 3 | 4 | 2 | 1 |

Now look at the output produced by survSplit() function. You can see that the first record (CustomerID=1) has been split into two records, one record starting from the first month that the member was active (time0=0) and the other record from the sixth month onward (time 0=6).
