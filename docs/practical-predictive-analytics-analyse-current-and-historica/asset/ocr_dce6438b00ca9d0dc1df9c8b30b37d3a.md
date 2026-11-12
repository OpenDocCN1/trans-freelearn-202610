Here we can look at all of the `deciles` of the data using the `quantile()` function and specifying that we want each breakpoint at each 10% of the data:

```
deciles <- data.frame(quantile(outlier.df$sales,
                                 prob=c(0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, .9,1)))
deciles
>deciles
quantile.outlier.df.sales..prob...c.0.1..0.2..0.3...0.4...0.5.
10%    5548.89
20%    7409.26
30%    8621.34
40%    9090.79
50%    9741.97
60%   10364.65
70%   11019.19
80%   12175.63
90%   13164.27
100%  15363.60
```

# Transforming the data

Another way to look at outliers is by first standardizing them to a normal distribution with a mean of 0 and standard deviation of 1. Using standard normal form is convenient, since the properties of the distribution never change, and some critical cut-off points can be committed to memory. For example, for a standard normal distribution, quartile 1 is always at -.67 and quartile 3 is always at +.67, so it is easy to compute the interquartile range to memory, which is 1.34. Using the interquartile range rule to identify outliers means that we will take 1.5 times this amount to derive the value of 2.01, so we will be considering any data point above .67+2.01=2.68, or -.67-2.01=-2.86, to be a possible outlier. This is an example of how we can use a statistical property of a distribution to identify an outlier. You could also use a rule such as identifying any data point more than three standard deviations from the mean, or by using a Chi-square test to determine if your grouped data differs significantly from the theoretical distribution. The outliers package also contains several other methods and statistical tests for detecting outliers, including the Grubbs test.

Here we will first normalize the data, and then look for data falling above or