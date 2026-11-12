```
#
sat.fit.high <- survfit(CoxModel.1, newdata=subset(ChurnStudy[1:3
plot(sat.fit.high,col=c('blue') ,lty=1,xlab="Months", ylab="Hazar
title(main = "sat (Highest) Survival")
dev.copy(jpeg,'Ch5 - CoxModell sat.jpg'); dev.off()
savehistory (file="Ch5 CoxModel Contrast Curves.txt")
```

# Comparing gender differences

In the comparison of males (left plot) with females (right plot), we can see
that there is slightly more density in the upper left quadrant for males than
there is for females. This supports the hypothesis that the males will churn
earlier:

![](bbox=[0.113, 0.394, 0.706, 0.815])

# Comparing customer satisfaction differences