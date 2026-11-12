
Now that we have simulated the variables separately for the two groups, we will recombine them and then remove some of the NAs:

```
# bind them back together ===
d1 <- data.frame(xchurn.list)
d2 <- data.frame(xnochurn.list)
ChurnStudy <- rbind(d1,d2)
ChurnStudy <- na.omit(ChurnStudy)
summary(ChurnStudy)
nrow(ChurnStudy)
savehistory(file="ch5 generate churn data")
```

The `str`, `summary`, and `nrow` functions will output to the console, and the `savehistory()` function will save all of the commands that were run to an external file:

```
'data.frame': 1984 obs. of 9 variables:
 $ Churn : num 1 1 1 1 1 1 1 1 1 1 ...
 $ xeducation : Factor w/ 3 levels "Bachelor's Degree",...: 1 3 3 1 3 2 1 3 1 3 ...
 $ xgender : Factor w/ 2 levels "F","M": 2 2 1 1 2 2 2 2 2 ...
 $ xatisfaction : chr "3" "4" "2" "4" ...
 $ xatisfaction2 : chr "3" "4" "2" "4" ...
 $ xservice.calls : num 0 0 0 0 1 0 0 0 ...
 $ Xtenure2 : int 12 5 13 2 6 7 6 1 10 ...
 $ xpurch.last.month: int 11 11 13 1 11 3 ...
 $ xmonthly.charges : num 107.91 69.98 74.66 100.68 87.39 ...
 - attr(*, "na.action")=Class 'omit' Named int [1:16] 186 38 98 544 564 676 743 993 996 1207 ...
  ..- attr(*, "names")= chr [1:16] "98" "86" "308" "554" ...
 > summary(xchurn3)
     Churn           xeducation    xgender    xatisfaction    xsatisfaction2   xservice.calls   xtenure2       xpurch.last.month  xmonthly.charges
 Min. :0.0   Bachelor's Degree:1593 F: 4519    1:419       Length:1984        Min. :0.0000   Min. : 1.000   Min. : 0.000   Min. :97.53
 1st Qu.:0.0   Doctorate degree : 122 M:1532     2:448       Class :character   1st Qu.:0.0000   1st Qu.: 7.000   1st Qu.: 1.000   1st Qu.: 67.80
 Median :0.5   Master's Degree : 269            3:1304       Mode :character   Median :0.0000   Median : 9.000   Median : 1.000   Median:131.48
 Mean :0.3                      Mean   : 4.458                  Mean   :0.4718   Mean   : 8.449   Mean   : 1.479   Mean   :127.44
 3rd Qu.:1.0                      4:457                  Median :0.0000   Mean   : 8.449   Median : 1.000   Median :131.48
 Max. :1.0                      3rd Qu.:1.0000   Max. : 10.000   Max. : 381.82
 > nrow(xchurn3)
[1] 1984
 > savehistory(file="ch5 generate churn data")
 > 
```

Notice that the summary output shows satisfaction as a factor and `satisfaction2` as a character variable. We will alter the format and the values of `satisfaction2` later in the chapter when we discuss the follow-up survey.

Try plotting some of the histograms which show the difference in tenure and the number of purchases last month for the two groups, the churners and those who remained active:

```
par(mfrow=c(2,2))
hist(xchurn$Xtenure2,main="Churners Tenure")
hist(xnochurn$Xtenure2,main="Non-Churners Tenure")
hist(xchurn$Xpurch.last.month, col = "grey", labels = FALSE,main=
hist(xnochurn$Xpurch.last.month, col = "black", labels = FALSE,ma
dev.copy(jpeg,'Ch5 - Plots after dataset creation.jpg'); dev.off()
```

Compare the churners to the non-churners with respect to tenure and number of purchases in the last month:

![](img/672c0cf8ba3c000a6529b117ed96945e_291_0.png)

![](img/672c0cf8ba3c000a6529b117ed96945e_291_1.png)

![](img/672c0cf8ba3c000a6529b117ed96945e_291_2.png)

![](img/672c0cf8ba3c000a6529b117ed96945e_291_3.png)

We can see from the preceding plots that the churners generally have a lower number of purchases last month than those who remained active. For the churners, there seems to be some churn activity after 1 or 2 months, but this is a premature assessment, since we do not know at what point in time the customer came aboard.

## Creating matrix plots

If we want to explore tenure a bit further for the churners, we could also produce a matrix plot showing the association between tenure and any other variables we choose. In this example we will use the `ggpairs()` function to plot the matrix plots for `tenure`, `gender`, `satisfaction`, and `monthly charges`:

```
str(xchurn)
install.packages("GGally")
library(GGally)
library(ggplot2)
ggpairs(xchurn,c(3,4,7,8),lower=list(combo=wrap("facethist",binwi
```

![](img/672c0cf8ba3c000a6529b117ed96945e_292_0.png)

## Partitioning into training and test data

Next, we will generate test and training datasets so that we can validate any models produced. There are many ways of generating test and training sets.

In earlier chapters, we used the createDataPartition function. For this example, we will generate the test and training data using native R functions. Please refer to the outline of the code here, and then run the code that follows:

- Set a variable corresponding to the percentage of the data to designate as training data (TrainingRows). In this example, we will use 75%.
- Use the sample() function to randomize the rows and assign to a new dataframe named ChurnStudy.
- Then select the first TrainingRows rows. Since the df dataframe has already been sampled, selecting a percentage of rows sequentially from a random sample is a convenient and valid way to select a training sample.
- The remaining rows (TrainingRows+1 to the end) will be the testing dataset. Assign it to ChurnStudy.test.

Once we have generated the test and training validation dataset, we will use the stargazer package (which we used in Chapter 1, Getting Started with Predictive Analytics) to generate HTML output for the original ChurnStudy dataset, as follows:

```
#divide into train and test===
TrainingRows <- round(.75 * nrow(ChurnStudy))
TrainingRows
set.seed(1020)
#randomize rows
df <- ChurnStudy[sample(nrow(ChurnStudy)), ]
rows <- nrow(df)
ChurnStudy <- df[1:TrainingRows, ] #training set
ChurnStudy.test <- df[(TrainingRows+1):rows, ] #test set
nrow(ChurnStudy)
nrow(ChurnStudy.test)
```

```
str(ChurnStudy)
str(ChurnStudy.test)
library(stargazer)
stargazer(ChurnStudy[1:10,1:9], out = c("ch 5 AnalysisDataFrame.html"))
browseURL("ch 5 AnalysisDataFrame.html")
savehistory(file="ch5 generate test and train")
```

| Churn | Xeducation | Xgender | Xsatisfaction | Xsatisfaction2 | Xservice.calls | Xtenure2 | Xpurch.last.month | Xmonthly.charges | SurvObj |
|---|---|---|---|---|---|---|---|---|---|
| 1 | Bachelor's Degree | F | 1 | 5 | 0 | 7 | 1 | 295.502908 | 7 |
| 1 | Bachelor's Degree | F | 3 | 5 | 0 | 9 | 1 | 215.769268 | 9 |
| 1 | Bachelor's Degree | M | 4 | 5 | 0 | 6 | 1 | 230.142292 | 6 |
| 0 | Bachelor's Degree | M | 3 | 2 | 4 | 11 | 1 | 84.775160 | 11+ |
| 1 | Bachelor's Degree | F | 4 | 3 | 1 | 11 | 1 | 167.735122 | 11 |
| 0 | Bachelor's Degree | M | 3 | 1 | 0 | 1 | 0 | 174.883747 | 1+ |
| 1 | Bachelor's Degree | M | 1 | 5 | 0 | 4 | 1 | 154.264568 | 4 |
| 1 | Doctorate Degree | M | 4 | 3 | 0 | 12 | 1 | 212.801643 | 12 |
| 1 | Bachelor's Degree | M | 4 | 2 | 0 | 12 | 1 | 291.822605 | 12 |
| 1 | Bachelor's Degree | M | 4 | 2 | 1 | 3 | 10 | 318.343811 | 3 |
| 0 | Bachelor's Degree | M | 2 | 4 | 0 | 12 | 3 | 71.934505 | 12+ |

## Setting the stage by creating survival objects

Coding survival analysis in R usually starts with creating what is known as a survival object using the `Surv()` function. A `survival` object contains more information than a regular dataframe. The purpose of the `survival` object is to keep track of the time and the event status (0 or 1) for each observation. It is also to designate what the response (dependent) variable is.

At a minimum, you need to supply a single time variable and an event when defining a `survival` object. In our case, we will use the tenure time (`Xtenure2`) as the time variable, and a formula that designates the defining event. In our case, this will be `Churn == 1`, since that means that the customer churned in that month:

```
install.packages("survival")
library(survival)
ChurnStudy$SurvObj <- with(ChurnStudy, Surv(Xtenure2, Churn == 1))
```

As I mentioned in earlier chapters, I always like to issue a `str()` command after I create a new dataframe, just to make sure the results are as expected:

```
> str(ChurnStudy$SurvObj)
Surv [1:1488, 1:2] 7 9 6 11+ 11 1+ 4 12 12 3 ...
 - attr(*, "dimnames")=List of 2
  ..$ : NULL
  ..$ : chr [1:2] "time" "status"
 - attr(*, "type")= chr "right"
```

From the preceding output we can see that the `survival` object created contains 1488 rows, with columns named time and status. Note that the `+` sign after the number of months means that the member was censored at the end of the study period, that is, the subscriber remained active and did not churn. The `attr(*, "type")` line shows the model is incorporating right censoring.

Once the `survival` object is created, we can perform some rudimentary
