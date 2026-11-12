```
# library(forecast)
iterations <- 0
for (i in seq(from = 1, to = 999, by = 14)) {
j <- i + 13
# pull out the next category. It will always be 14 records.
x4 <- df[i:j, ]
x <- ts(x4$Not.Covered.Pct, start = c(1999), end = c(2012), frequu

# assign the first 10 records to the training data, and the next
# test data.
```

```
trainingdata <- window(x, start = c(1999), end = c(2008))
testdata <- window(x, start = c(2009), end = c(2012))
```

```
par(mfrow = c(2, 2))

# first fit the training data, then the test data.
# Use simple exponential smoothing
fit <- ets(trainingdata, model = "ANN")
# summary(fit)
fit2 <- ets(testdata, model = fit)
onestep <- fitted(fit2)
```

```
iterations <- iterations + 1
onestep.df[iterations, 1] <- paste(x4$cat[1])
onestep.df[iterations, 2] <- accuracy(onestep, testdata)[, 2] #RM
onestep.df[iterations, 3] <- accuracy(onestep, testdata)[, 3] #MA
onestep.df[iterations, 4] <- accuracy(onestep, testdata)[, 5] #MA
onestep.df[iterations, 5] <- accuracy(onestep, testdata)[, 7] #AC

if (iterations == 24)
break
}
```

Take a look at some of the rows from one of the categories extracted from the
for loop. It is easy to look at the last group processed since since all
intermediate object created in the loop are still intact.

First let's look at the actual data from the raw group:

```
tail(x4)
```

This shows a partial output of the last group UNDER 18 YEARS. NOTE: To see