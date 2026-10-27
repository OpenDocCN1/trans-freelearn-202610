![](bbox=[0.15, 0.13, 0.82, 0.42])

The input dataset contains the individual ratings; normalization function reduces the individual rating bias by centering the row, which is a standard z-score transformation. Subtracting each element from the mean and then dividing by standard deviation. The following graph shows normalized ratings for the preceding dataset:

```
> hist(getRatings(normalize(data)),breaks=100,col="blue4")
```

![](bbox=[0.22, 0.58, 0.78, 0.87])