| 6 | 1245 | 42 | 0.896 | 0.00752 | 0.881 | 0.911 |
|---|---|---|---|---|---|---|
| 7 | 1156 | 60 | 0.861 | 0.00884 | 0.844 | 0.879 |
| 8 | 1020 | 68 | 0.818 | 0.01033 | 0.798 | 0.838 |
| 9 | 850 | 89 | 0.754 | 0.01233 | 0.731 | 0.779 |
| 10 | 665 | 115 | 0.657 | 0.01500 | 0.628 | 0.687 |
| 11 | 435 | 105 | 0.536 | 0.01789 | 0.502 | 0.572 |
| 12 | 225 | 112 | 0.278 | 0.02226 | 0.237 | 0.325 |

# # Plotting the curve

The curve can be plotted using the generic plot function:

```
plot(survfit(CoxModel.1),col=c('red','blue') ,lty=1,xlab="Months"
title(main = "Model Survival Curve")
dev.copy(jpeg,âCh5 - Coxmodel1 plot.jpgâ); dev.off()
```

You can also plot a prettier version of the curve using ggplot and ggfortify:

```
library(ggplot2)
library(ggfortify)

autoplot(survfit(CoxModel.1), surv.linetype = 'dashed', surv.colo
conf.int.fill = 'dodgerblue3', conf.int.alpha = 0.5, cen
```

Here is a side by side comparison of the curves produced by the native plot function (left plot) and autoplot functions (right plot). The major difference is that the shaded bands replace the confidence interval dotted lines: