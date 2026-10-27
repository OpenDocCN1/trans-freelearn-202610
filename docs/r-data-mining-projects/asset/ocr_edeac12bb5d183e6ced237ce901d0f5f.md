![](bbox=[0.14, 0.13, 0.82, 0.51])

From the preceding image, we can conclude that the price variable is positively skewed because of the presence of some outlier values on the right-hand side of the distribution. The mean of the price variable is inflated and greater than the mode because the mean is subject to extreme fluctuations.

Now let's try to understand a case where normal distribution can be used to answer any hypothesis.

Suppose the variable mileage per gallon on a highway is normally distributed with a mean of 29.08 and a standard deviation of 5.33. What is the probability that a new car would provide a mileage of 35?

```
> pnorm(35,mean(Cars93$MPG.highway),sd(Cars93$MPG.highway),lower.tail = F)
[1] 0.1336708
```

Hence the required probability that a new car would provide a mileage of 35 is 13.36%, since the expected mean is higher than the actual mean; the lower tail is equivalent to false.