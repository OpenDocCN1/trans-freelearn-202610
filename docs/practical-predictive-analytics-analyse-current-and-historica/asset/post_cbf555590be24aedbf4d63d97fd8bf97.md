
```
# library(forecast)
iterations <- 0
for (i in seq(from = 1, to = 999, by = 14)) {
j <- i + 13
# pull out the next category. It will always be 14 records.
x4 <- df[i:j, ]
x <- ts(x4$Not.Covered.Pct, start = c(1999), end = c(2012), frequency = 1)

# assign the first 10 records to the training data, and the next
# test data.
```

```
trainingdata <- window(x, start = c(1999), end = c(2008))
testdata <- window(x, start = c(2009), end = c(2012))
```

```
par(mfrow = c(2, 2))

# first fit the training data, then the test data.
# Use simple exponential smoothing
fit <- ets(trainingdata, model = "ANN")
# summary(fit)
fit2 <- ets(testdata, model = fit)
onestep <- fitted(fit2)
```

```
iterations <- iterations + 1
onestep.df[iterations, 1] <- paste(x4$cat[1])
onestep.df[iterations, 2] <- accuracy(onestep, testdata)[, 2] #RMSE
onestep.df[iterations, 3] <- accuracy(onestep, testdata)[, 3] #MAE
onestep.df[iterations, 4] <- accuracy(onestep, testdata)[, 5] #MAPE
onestep.df[iterations, 5] <- accuracy(onestep, testdata)[, 7] #ACF1

if (iterations == 24)
break
}
```

Take a look at some of the rows from one of the categories extracted from the for loop. It is easy to look at the last group processed since all intermediate objects created in the loop are still intact.

First let's look at the actual data from the raw group:

```
tail(x4)
```

This shows a partial output of the last group UNDER 18 YEARS. NOTE: To see all of the years you need to run the command `tail(x4,14)` at the console instead:

```
> Source: local data frame [6 x 9]
>
> cat Avg.Total.Insured Avg.People Year Year.1
> (fctr) (dbl) (dbl) (fctr) (int)
> 1 UNDER 18 YEARS 66200.46 73752.5 2007 2007
> 2 UNDER 18 YEARS 66200.46 73752.5 2008 2008
> 3 UNDER 18 YEARS 66200.46 73752.5 2009 2009
> 4 UNDER 18 YEARS 66200.46 73752.5 2010 2010
> 5 UNDER 18 YEARS 66200.46 73752.5 2011 2011
> 6 UNDER 18 YEARS 66200.46 73752.5 2012 2012
> Variables not shown: Total.People (dbl), Total (dbl), Not.Covered, Not.Covered.Pct (dbl)
```

## Accuracy measures produced by onestep

The onestep.df object produced from the for loop contains all of the accuracy measure for all of the groups. Take a look at the first 6. You can see that the accuracy measure rmse, mae, and mape are captured for each of the categories:

```
head(onestep.df)
> cat rmse mae mape acf1
> 1 18 to 24 YEARS 0.013772470 0.009869590 3.752381 1.0000552
> 2 25 to 34 YEARS 0.004036661 0.003380938 1.217612 1.0150588
> 3 35 to 44 YEARS 0.006441549 0.004790155 2.231886 0.9999469
> 4 45 to 54 YEARS 0.004261185 0.003129072 1.734022 0.9999750
> 5 55 to 64 YEARS 0.005160212 0.004988592 3.534093 0.7878765
> 6 65 YEARS AND OVER 0.002487451 0.002096323 12.156875 0.9999937
cbind(fit$x, fit$fitted, fit$residuals)
> Time Series:
> Start = 1999
> End = 2008
> Frequency = 1
> fit$x fit$fitted fit$residuals
> 1999 0.11954420 0.1056241 0.0139200744
> 2000 0.10725759 0.1056255 0.0016320737
> 2001 0.10649619 0.1056257 0.0008705016
> 2002 0.10291788 0.1056258 -0.0027078918
> 2003 0.10393522 0.1056255 -0.0016902769
> 2004 0.09942592 0.1056253 -0.0061994140
> 2005 0.10320781 0.1056247 -0.0024169001
> 2006 0.11231138 0.1056245 0.0066869135
> 2007 0.10587369 0.1056251 0.0002485556
> 2008 0.09527547 0.1056252 -0.0103496921
```

```
mean(fit$residuals)
> [1] -6.056125e-07
```

Take a look at the residuals which resulted for the "UNDER 18 YEARS" category for the test dataset. The residuals seem to change for the year 2011, but the absolute value is still small across the board.

```
absresid <- abs(fit2$residuals)
plot(absresid)
```

![](img/672c0cf8ba3c000a6529b117ed96945e_497_0.png)

The fit2 is test data, along with the fitted values from the model developed from the training data (fit):

```
cbind(fit2$x, fit2$fitted, fit2$residuals)
> Time Series:
> Start = 2009
> End = 2012
> Frequency = 1
>
    fit2$x fit2$fitted fit2$residuals
> 2009 0.09745024 0.09451361 0.0029366252
> 2010 0.09785322 0.09451391 0.0033393083
> 2011 0.09397731 0.09451424 -0.0005369347
> 2012 0.08877369 0.09451419 -0.0057404940
```

```
mean(fit2$residuals)
> [1] -3.738216e-07
```

## Comparing the Test and Training for the "UNDER 18 YEARS" group

We can also examine the plots for the training and test groups for this sub segment to see if anything is really going on:

```
par(mfrow = c(1, 2))
plot(forecast(fit))
plot(forecast(fit2))
```

From the following plots we can see that although there is a decline in the not-covered percentage for this group, there is probably not enough data to discern trend so that the projections are set at the mean values.

For the test data plot we can also see why the absolute residual plot dropped in 2011 as it transitioned from a local peak in 2010 (when the Affordable Care Act was enacted) to the start of a decline in 2011:

![](img/672c0cf8ba3c000a6529b117ed96945e_498_0.png)

![](img/672c0cf8ba3c000a6529b117ed96945e_498_1.png)

## Accuracy measures

Using the residuals, we can measure the error from the predicted and actual values based upon three popular accuracy measures:

- Mean absolute error (MAE): This measure takes the mean of the absolute values of all of the errors (residuals)

- Root-mean-squared error (RMSE): The root mean square error measures the error by first taking the mean of all of the squared errors, and then takes the square root of the mean, in order to revert back to the original scale. This is a standard statistical method of measuring errors.

### Note

Both MAE and RMSE are scale-dependent measures, which means that that they can be used to compare problems with similar scales. When comparing accuracy among models with different scales, other scale-independent measures such as MAPE should be used.

- Mean percentage error (MAPE): This is the absolute difference between the actual and forecasted value, expressed as a percentage of the actual value. This is intuitively easy to understand and is a very popular measure:

```
M = \frac{100}{n} \sum_{t=1}^n \left| \frac{A_t - F_t}{A_t} \right|,
```

where $A_t$ is the actual value and $F_t$ is the forecast value.

We can look at the worst-performing models, in terms of a metric such as MAPE, by simply sorting the `onestep.df` object by the MAPE columns:

```
onestep.df %>% arrange(., desc(mape)) %>% head()
>
> cat rmse mae mape
> 1 MALE 65 YEARS AND OVER 0.002647903 0.002226841 17.440671 0.57
> 2 MALE 35 to 44 YEARS 0.039044319 0.024111701 14.218445 0.99
> 3 65 YEARS AND OVER 0.002487451 0.002096323 12.156875 0.99
> 4 MALE 25 to 34 YEARS 0.024195057 0.019901117 6.748294 0.99
> 5 MALE 45 to 54 YEARS 0.017711280 0.010388681 6.380865 0.99
> 6 FEMALE 55 to 64 YEARS 0.006749771 0.005610275 4.021224 0.50
```

Charting all of the MAPEs at once shows that the exponential model works better for the younger groups, and seems to degrade for older groups, especially for males:

```
lattice::barchart(cat ~ mape, data = onestep.df)
```

![](img/672c0cf8ba3c000a6529b117ed96945e_500_0.png)

## References

http://www.census.gov/data/tables/time-series/demo/health-insurance/historical-series/hib.html

## Summary

This chapter introduced time series analysis by reading in and exploring Health Care Enrollment Data from the CMS website. Then we moved on to defining some basic Time Series concepts such as Simple and Exponential Moving Averages. Finally we worked with the R "forecast" package to work with some exponential smoothed state space models, and showed you one way to produce automated forecasts for your data. We also showed various plotting methods using the ggplot, lattice package, as well as native R graphics.

## Chapter 9. Introduction to Spark Using R

> *Data! Data! Data! I can't make bricks without clay!*
>
> - Sir Arthur Conan Doyle

So far, we have learned how to perform analytics on what can be referred to as "small data". However, as the amount of data increases, so does the size and the problem of how to analyze the vast amounts of data that is produced arises. When that occurs, we begin to approach "big data" and new approaches to solving problems develop and sometimes, new tools are needed as well.

To some extent, nothing changes. You still want high quality data. You still want to be able to examine the relationships and cast the problem within a predictive analytic framework.

What does change are the steps needed to achieve that end, bearing in mind that the data is more difficult to manage and as a result new tools have evolved to help you do that.

One of the tools that has evolved in recent years is Apache Spark.

In this chapter, we will cover some basics of Spark. We will start with a known small dataset, perform some data cleaning, and then transform this dataset into something much larger which we can use to bring into the Apache Spark environment. We will also learn how to adapt tools that we are familiar with, in order to work with Spark, and learn some new tools along the way.
