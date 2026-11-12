Take this example in which you are measuring the effect of winning between an average of $1,000,000 or $1,000,001 in a lottery.

We will generate two probability samples:

+   - X contains 1 million observations with a mean value of 1,000,000
+   - Y also contains 1 million observations but with a mean value of 1,000,001 and it is only a 1-unit difference

Generate a dataframe with X and Y and print some summary statistics:

```
set.seed(1020)
lottery <- data.frame(
cbind(x=rnorm(n=1000000,1000000,100),y=rnorm(1000000,1000001,10
)
summary(lottery)
```

The output shows the simulated X and Y exactly as we expect. Both with the same number of observations, but with only a 1-unit difference in the mean values:

| x | Y |
|---|---|
| Min. : 999503 | Min. : 999513 |
| 1st Qu.: 999933 | 1st Qu.: 999934 |
| Median :1000000 | Median :1000001 |
| Mean :1000000 | Mean :1000001 |
| 3rd Qu.:1000067 | 3rd Qu.:1000068 |
| Max. :1000526 | Max. :1000489 |

If we run a statistical t-test on the difference between the means, the p-value shows a very significant difference based upon the p-value. But, for only a $1 difference:

```
t.test(lottery$x,lottery$y)
```

This is the output which is written to the console:

```
Welch Two Sample t-test

data:  lottery$x and lottery$y
t = -7.4518, df = 2000000, p-value = 9.209e-14
```