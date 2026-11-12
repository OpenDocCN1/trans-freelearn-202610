First, extract a sample for the highest and lowest satisfaction scores satisfaction=1 indicates more early churners than satisfaction=5, due to the density of the points:

```
```r
sat.fit.low <- survfit(CoxModel.1, newdata=subset(ChurnStudy[1:30
plot(sat.fit.low,col=c('red') ,lty=1,xlab="Months", ylab="Hazard"
title(main = "Sat (Lowest) Survival")
#
sat.fit.high <- survfit(CoxModel.1, newdata=subset(ChurnStudy[1:3
plot(sat.fit.high,col=c('blue') ,lty=1,xlab="Months", ylab="Hazar
title(main = "sat (Highest) Survival")
dev.copy(jpeg,âCh5 – CoxModell sat.jpgâ) ; dev.off()
```
```

Here is the side-by-side comparison of the highest and lowest satisfaction scores:

![](bbox=[0.12, 0.41, 0.87, 0.8])

# # Validating the model

Earlier, we created the `ChurnStudy.test` testing dataset and did not use it to