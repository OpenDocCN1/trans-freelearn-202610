however Xsatisfaction2 is substituted for Xsatisfaction to account for the change in satisfaction due to the follow-up study. The new time0 variable is also substituted for the original tenure variable, and Churn2 is substituted for the original churn response variable:

```
CoxModel.2 <- coxph(Surv(time0,Xtenure2, Churn2) ~
 Xeducation + Xgender + Xsatisfaction2 + Xse
 Xpurch.last.month + Xmonthly.charges,
 data=SURV2)
```

Avoid overwriting existing variable names. Either create new variables within an existing dataframe or preserve the old one and create a new frame.

After the model is run, we can use the `stargazer` command (or
`summary(CoxModel.2)`) to view the model output.

The asterisks after gender, satisfaction, and monthly charges indicate that
they are the most significant variables in the model.

The R-square of .13 is not a standard linear regression type measure, but is a
pseudo-R square measure that we discussed earlier, which is a number
between 0 and 1 that measures the difference between the fitted model and a
null model. If you are curious to see how this particular pseudo R-square is
calculated for a cox regression, you can issue the following command:

```
getS3method("summary", "coxph")
```

However, it is not as high as the original `CoxModel.1`. This is possibly due to
the artificial changing of the data. In any case, it is not a good idea to rely on
a single measure, and we will see shortly how the model does improve the
retention.

```
library(stargazer)
stargazer(CoxModel.2,single.row=TRUE,type="text")
```

```
> stargazer(CoxModel.2,single.row=TRUE,type="text")
```

```
============================================
Dependent variable:
```