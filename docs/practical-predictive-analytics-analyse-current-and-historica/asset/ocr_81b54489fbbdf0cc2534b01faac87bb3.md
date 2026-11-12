Then we will use cbind() to display the data point, as well as the simple and exponential moving averages:

```
ExpMA <- EMA(x, n = 5, ratio = 0.8)
cbind(head(x, 15), head(MA, 15), head(ExpMA, 15))
> [,1] [,2] [,3]
> [1,] 0.1541247 NA NA
> [2,] 0.1574131 NA NA
> [3,] 0.1629424 NA NA
> [4,] 0.1609860 NA NA
> [5,] 0.1485338 0.1568000 0.1568000
> [6,] 0.1474000 0.1554551 0.1492800
> [7,] 0.1523271 0.1544379 0.1517177
> [8,] 0.1464598 0.1511414 0.1475114
> [9,] 0.1433967 0.1476235 0.1442196
> [10,] 0.1455136 0.1470194 0.1452548
> [11,] 0.1391090 0.1453612 0.1403382
> [12,] 0.1347953 0.1418549 0.1359039
> [13,] 0.1308892 0.1387408 0.1318922
> [14,] 0.1362041 0.1373023 0.1353417
ts.plot(x, ExpMA, gpars = list(xlab = "year", ylab = "Percentage
lty = c(1:2)))
title("Percentage of Non-Insured 1999-2012 - With EMA")
```

The following plot shows the data in graph form. You can see that each data point is closer to its EMA that to the SMA, which, as mentioned earlier, weights all previous data points equally within the lookback period. In this regard, EMAs react quicker to the recent data, while SMAs are slower moving and have less variability. Of course, both are affected by the parameters, especially the lookback period. Longer lookbacks will make for slower moving averages in both cases: