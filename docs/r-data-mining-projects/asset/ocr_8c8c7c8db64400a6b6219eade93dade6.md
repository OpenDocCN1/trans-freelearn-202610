# Pie chart

A pie chart is a representation of categorical variables when the label for each categorical variable is less than 10. If it exceeds 10, then it is suggested to look at a histogram or barplot for comparison. Using the ggplot library, the pie chart can be created. The script is as follows:

```
> #creating pie charts
> pp <- ggplot(Cars93, aes(x = factor(1), fill = factor(Type))) +
+ geom_bar(width = 1)
> pp + coord_polar(theta = "y")
```

![](bbox=[0.08, 0.37, 0.92, 0.78])

```
> # 3D Pie Chart from data frame
> library(plotrix)
> t <- table(Cars93$Type);par(mfrow=c(1,2))
> pct <- paste(names(t), "\n", t, sep="")
> pie(t, labels = pct, main="Pie Chart of Type of cars")
> pie3D(t,labels=pct,main="Pie Chart of Type of cars")
```