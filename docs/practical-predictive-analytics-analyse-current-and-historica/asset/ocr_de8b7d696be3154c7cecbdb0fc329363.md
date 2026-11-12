```
means.neg = colMeans(PimaIndians[PimaIndians$diabetes =='neg',1:8
means.all = colMeans(PimaIndians[,1:8],na.rm=TRUE)
barplot(means.all[c(1, 2,3,4,5,6,7,8)],cex.axis=.75,cex.names=.70,
```

The barplot below shows the calculated means for reference only. Â We are not comparing them since they all have different scale levels.

![](bbox=[0.15, 0.23, 0.85, 0.72])