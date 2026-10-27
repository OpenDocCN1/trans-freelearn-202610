![](bbox=[0.345, 0.126, 0.656, 0.386])

When we include more variables, it becomes a multivariate relationship. Let's try to plot a multivariate relationship between various variables from the Cars93 dataset.

```
```r
> library(corrplot)
> t<-
  cor(Cars93[,c("Price","MPG.city","RPM","Rev.per.mile","Width","Weight","Horsepower","Length")])
> corrplot(t,method = "ellipse")
```
```

![](bbox=[0.254, 0.572, 0.747, 0.816])

There are various methods that can be passed as an argument to the correlation plot. They are "circle", "square", "ellipse", "number", "shade", "color", and "pie".