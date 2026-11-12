We will also generate two more curves, corresponding to the satisfaction and number of service calls variables:

```
par(mfrow=c(1,1))
plotsurv(ChurnStudy$Xsatisfaction,c(1:5),c('bars'))
title(main = "2 KM Curve Satisfaction")/
dev.copy(jpeg,'Ch5 - 2 KM Curve Satisfaction.jpg'); dev.off()
plotsurv(as.factor(ChurnStudy$Xservice.calls),c(1:6),c('none'))
dev.copy(jpeg,'Ch5 - 4 KM Curve Service Calls.jpg');
title(main = "4 KM Curve Service Calls")

dev.off()
```

The plots for the curves are sent to the output window, and are copied to the file specified in the preceding code:

![](bbox=[0.115, 0.385, 0.88, 0.705])

# User exercise:

Plot the preceding survival curves and run the log-rank test using survdiff() to see if there is a significant difference.

```
survdiff(SurvObj ~ Xsatisfaction, data = ChurnStudy)
survdiff(SurvObj ~ Xpurch.last.month, data = ChurnStudy)
survdiff(SurvObj ~ Xservice.calls, data = ChurnStudy)
```