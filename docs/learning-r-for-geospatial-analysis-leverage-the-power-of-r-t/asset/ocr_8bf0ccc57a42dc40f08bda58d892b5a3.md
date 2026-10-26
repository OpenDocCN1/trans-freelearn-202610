```
4 2006-01-04 14.4
5 2006-01-05 10.6
6 2006-01-06 12.8
```

We used the following expression to produce the simplest possible line plot of this time series with ggplot2:

```
> library(ggplot2)
> ggplot(dat, aes(x = time, y = tmax)) +
+ geom_line()
```

Although the following screenshot already appeared, in Chapter 2, Working with Vectors and Time Series, it is provided again here since we are going to discuss the example for some time now:

![](bbox=[0.195, 0.36, 0.805, 0.655])

We will now briefly go over the main characteristics of the ggplot2 syntax, referring to the previous example. Later in this chapter, we will produce additional plots using other datasets from the previous chapters. This way, by the end of this chapter, we'll have reviewed the most important concepts and methods of operation for visualization of spatial data with ggplot2.