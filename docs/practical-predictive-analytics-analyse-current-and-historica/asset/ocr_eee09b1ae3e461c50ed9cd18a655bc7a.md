Rather than simply plotting the distribution, let's plot a histogram of the distribution so that we can see what the approximate error bins will look like. In fact, let's generate two different histograms, one containing the actual counts and the other containing percentages:

```
par(mfrow=c(1,2))
h <- hist(x,freq=FALSE,right=FALSE,breaks=c(-.20, -.15,-.10,-.05,
print(h)

plot(h$mids,100*h$counts/sum(h$counts),type="h",lwd=25, lty=1, ma
```

In the plots below, you can see from the histogram on the left that there are no values beyond the limits specified. In fact, since the distribution was generated with a mean=0, a good number of the test elements will have no errors added to them. This is by design.

The bar chart on the right has been generated simply to show the relative percentage of each bar (rather than the straight frequency). From this, we can see, for example, that the probability of an adjustment of 5-10% is about 12%: