

```
}
dev.copy(jpeg,'Ch5 - Coxmodel1 zph.jpg');
dev.off()
savehistory (file="Ch5 CoxModel zph.txt")
```

# First, look at the output of the `print(temp)` command:

```
> print(temp)
```

| Variable | rho | chisq | p |
|---|---|---|---|
| XeducationDoctorate Degree | 0.02283 | 0.40067 | 0.5267 |
| XeducationMaster's Degree | 0.06161 | 2.88733 | 0.0893 |
| XgenderM | -0.05593 | 2.36508 | 0.1241 |
| Xsatisfaction2 | -0.01517 | 0.17429 | 0.6763 |
| Xsatisfaction3 | -0.06778 | 3.47926 | 0.0621 |
| Xsatisfaction4 | 0.01274 | 0.12252 | 0.7263 |
| Xsatisfaction5 | -0.00120 | 0.00111 | 0.9735 |
| Xservice.calls | 0.02364 | 0.41897 | 0.5175 |
| Xpurch.last.month | 0.00987 | 0.10354 | 0.7476 |
| Xmonthly.charges | 0.01265 | 0.07633 | 0.7823 |
| GLOBAL | NA | 11.43649 | 0.3245 |

For the printed output, you will notice the chi-square (column 4) and associated p-value (column 5), which test the assumption that each regression covariate is dependent on time. For the model to pass the assumption, you want to see higher p-values, all of which are above a critical significance level. In this case, they are all above p=.05, so all of the variables pass the assumptions of proportional hazards.

# Proportional hazard plots

The plots produced also show that all of the variables are dispersed in a uniform fashion as time increases. Any change due to time would be indicated by a pronounced line that slopes upward or downward:

![](img/672c0cf8ba3c000a6529b117ed96945e_320_0.png)

## Obtaining the cox survival curves

We can obtain survival curves for the Cox model in a similar fashion as we did for the KM models. To obtain the data points for the curve, use `summary(survfit(CoxModel.1))`, which will also display the confidence intervals, count of the number of members who `churned (n.event)`, and the number of members at risk `(n.risk)`:

```
> summary(survfit(CoxModel.1))
Call: survfit(formula = CoxModel.1)
```

| time | n.risk | n.event | survival | std.err | lower 95% CI | upper 95% CI |
|---|---|---|---|---|---|---|
| 1 | 1488 | 15 | 0.993 | 0.00185 | 0.989 | 0.996 |
| 2 | 1455 | 52 | 0.967 | 0.00404 | 0.960 | 0.975 |
| 3 | 1393 | 34 | 0.950 | 0.00505 | 0.940 | 0.960 |
| 4 | 1342 | 20 | 0.940 | 0.00559 | 0.929 | 0.951 |
| 5 | 1315 | 39 | 0.919 | 0.00655 | 0.906 | 0.932 |
| 6 | 1245 | 42 | 0.896 | 0.00752 | 0.881 | 0.911 |
| 7 | 1156 | 60 | 0.861 | 0.00884 | 0.844 | 0.879 |
| 8 | 1020 | 68 | 0.818 | 0.01033 | 0.798 | 0.838 |
| 9 | 850 | 89 | 0.754 | 0.01233 | 0.731 | 0.779 |
| 10 | 665 | 115 | 0.657 | 0.01500 | 0.628 | 0.687 |
| 11 | 435 | 105 | 0.536 | 0.01789 | 0.502 | 0.572 |
| 12 | 225 | 112 | 0.278 | 0.02226 | 0.237 | 0.325 |

## Plotting the curve

The curve can be plotted using the generic plot function:

```
plot(survfit(CoxModel.1),col=c('red','blue') ,lty=1,xlab="Months"
title(main = "Model Survival Curve")
dev.copy(jpeg,'Ch5 - Coxmodel1 plot.jpg'); dev.off()
```

You can also plot a prettier version of the curve using ggplot and ggfortify:

```
library(ggplot2)
library(ggfortify)

autoplot(survfit(CoxModel.1), surv.linetype = 'dashed', surv.colo
conf.int.fill = 'dodgerblue3', conf.int.alpha = 0.5, cen
```

Here is a side by side comparison of the curves produced by the native plot function (left plot) and autoplot functions (right plot). The major difference is that the shaded bands replace the confidence interval dotted lines:

![](img/672c0cf8ba3c000a6529b117ed96945e_322_0.png)

# Partial regression plots

Partial regression plots are useful in regression to observe the effect of the individual variables independently of the other variables. Use the `termplot()` function to view the variables in the model separately. The termplot curves roughly correspond to the slopes of the regression line, so you can visually observe the effect of each variable as the levels or continuous values change.

For the regression that we just ran, males, satisfaction levels 3 and 4, and monthly charges are all marked as significant in the regression output, so we can observe their individual slopes by looking at the plots.

The following code will loop through all the variables in the model and produce the following plots. The sixth term (monthly charges), which is a continuous variable, is produced with standard error bars. Notice the increasing variance as the monthly charges increase:

```
par(mfrow=c(3,3))
for(i in 1:5) termplot(CoxModel.1,term=i,col.se='grey',se=TRUE,pa
termplot(CoxModel.1,term=6,col.se='grey',se=TRUE,partial.resid =T
dev.copy(jpeg,'Ch5 - Coxmodell termplot.jpg');
dev.off()
```

Each `termplot()` call produces the following three plots in a 3*3 matrix format, as defined by the supplied `mfrow` specification:

![](img/672c0cf8ba3c000a6529b117ed96945e_323_0.png)

# Examining subset survival curves

As we did with the KM curves, we can also produce separate plots for subsets of the data. We will do this by applying the regression coefficients we obtained previously to subsets of the data, with each subset containing an approximately equal number of rows. That way, we will be able to compare the density across groups:

- In the following code, we will first subset our original dataset (ChurnStudy) by gender
- Then we will score the new subsets by applying the coefficients of

# CoxModel.1

- We will then reduce the density of the plots by sampling 300 observations each from the males and females subsets
- Finally, we will plot the results

Run the following code to produce some interesting subsets of the survival curves:

```
par(mfrow=c(1,2))
#
males <-subset(ChurnStudy,Xgender=='M')
males.nrow <- nrow(males)
females <-subset(ChurnStudy,Xgender=='F')
nrow(females)
females.fit <- survfit(CoxModel.1, newdata=females)
plot(females.fit,col=c('blue') ,lty=1,xlab="Months", ylab="Hazard
title(main = "Females Survival Curve")
mtext(paste("Observations=",nrow(females)),side=3,line=0,adj=1,ce
#
males.fit <- survfit(CoxModel.1, newdata=males)
plot(males.fit,col=c('red') ,lty=1,xlab="Months", ylab="Hazard")
title(main = "Males Survival Curve")
mtext(paste("Observations=",nrow(males)),side=3,line=0,adj=1,cex=
#
par(mfrow=c(1,2))
#
#sample the males population
#
males.sample <- sample(males)[1:300,]
males.sample.fit <- survfit(CoxModel.1, newdata=males.sample)
plot(males.sample.fit,col=c('orange') ,lty=1,xlab="Months", ylab=
title(main = "Males (Sample) Survival Curve")
mtext(paste("Observations=",nrow(males.sample.fit)),side=3,line=0
#
#sample the females population
#
females.sample <- sample(females)[1:300,]
females.sample.fit <- survfit(CoxModel.1, newdata=females.sample)
plot(females.sample.fit,col=c('red') ,lty=1,xlab="Months", ylab="
title(main = "Females (Sample) Survival Curve")
mtext(paste("Observations=",nrow(females.sample.fit)),side=3,line
#
dev.copy(jpeg,'Ch5 - CoxModell Gender.jpg'); dev.off()
sat.fit.low <- survfit(CoxModel.1, newdata=subset(ChurnStudy[1:30
plot(sat.fit.low,col=c('red') ,lty=1,xlab="Months", ylab="Hazard"
title(main = "Sat (Lowest) Survival")
#
sat.fit.high <- survfit(CoxModel.1, newdata=subset(ChurnStudy[1:3
plot(sat.fit.high,col=c('blue') ,lty=1,xlab="Months", ylab="Hazar
title(main = "sat (Highest) Survival")
dev.copy(jpeg,'Ch5 - CoxModell sat.jpg'); dev.off()
savehistory (file="Ch5 CoxModel Contrast Curves.txt")
```

# Comparing gender differences

In the comparison of males (left plot) with females (right plot), we can see that there is slightly more density in the upper left quadrant for males than there is for females. This supports the hypothesis that the males will churn earlier:

![](img/672c0cf8ba3c000a6529b117ed96945e_325_0.png)

# Comparing customer satisfaction differences

First, extract a sample for the highest and lowest satisfaction scores satisfaction=1 indicates more early churners than satisfaction=5, due to the density of the points:

```
sat.fit.low <- survfit(CoxModel.1, newdata=subset(ChurnStudy[1:30
plot(sat.fit.low,col=c('red') ,lty=1,xlab="Months", ylab="Hazard"
title(main = "Sat (Lowest) Survival")
#
sat.fit.high <- survfit(CoxModel.1, newdata=subset(ChurnStudy[1:3
plot(sat.fit.high,col=c('blue') ,lty=1,xlab="Months", ylab="Hazar
title(main = "sat (Highest) Survival")
dev.copy(jpeg,'Ch5 - CoxModell sat.jpg'); dev.off()
```

Here is the side-by-side comparison of the highest and lowest satisfaction scores:

![](img/672c0cf8ba3c000a6529b117ed96945e_326_0.png)

## Validating the model

Earlier, we created the `ChurnStudy.test` testing dataset and did not use it to

