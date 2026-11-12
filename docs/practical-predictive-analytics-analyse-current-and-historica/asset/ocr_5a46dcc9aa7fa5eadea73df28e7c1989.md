# CoxModel.1

+   - We will then reduce the density of the plots by sampling 300 observations each from the males and females subsets
+   - Finally, we will plot the results

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
title(main = "FeMales (Sample) Survival Curve")
mtext(paste("Observations=",nrow(females.sample.fit)),side=3,line
#
dev.copy(jpeg,'Ch5 - CoxModell Gender.jpg'); dev.off()
sat.fit.low <- survfit(CoxModel.1, newdata=subset(ChurnStudy[1:30
plot(sat.fit.low,col=c('red') ,lty=1,xlab="Months", ylab="Hazard"
title(main = "Sat (Lowest) Survival")
```