# # Forecasting using ALL AGES

The following code will perform the following steps:

+   1. First, it will filter the data so that it only includes the `ALL AGES` category.
+   2. Then, it creates a time series object.
+   3. Finally, it runs a simple exponential model, using the `ets()` function.

Note that we did not specify a smoothing factor. The `ets()` function calculates the optimal smoothing factor (alpha, shown via the `summary()` function (in bold below)), which in this case is .99, which means that model time series takes about 99% of the previous value to incorporate into the next time series prediction:

```
library(dplyr)
>
> Attaching package: 'dplyr'
> The following objects are masked from 'package:stats':
>
>     filter, lag
> The following objects are masked from 'package:base':
>
>     intersect, setdiff, setequal, union
library(forecast)
> Loading required package: zoo
>
> Attaching package: 'zoo'
> The following objects are masked from 'package:base':
>
>     as.Date, as.Date.numeric
> Loading required package: timeDate
> This is forecast 7.1
x4 <- x2[x2$cat == "ALL AGES", ]

# set up as a time series object
x <- ts(x4$Not.Covered.Pct, start = c(1999), end = c(2012), frequ

fit <- ets(x, model = "ANN")
```