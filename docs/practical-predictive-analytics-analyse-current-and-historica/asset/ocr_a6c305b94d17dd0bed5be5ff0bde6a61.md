![](bbox=[0.156, 0.095, 0.819, 0.482])

As you can see, for the ALL AGES category, the non-covered percentage is seen to be increasing over the time period, until 2010 (where the affordable care act was enacted), and then the percentage begins to decrease. However, for the same time period, the under 18 age group shows a decrease in the proportion of non-insured relative to the population size.

We can take a closer look at the top and bottom four categories, along with the ALL AGES category, to examine this more closely. This time, we will add our own trend line using the geom_smooth parameter, which will add a linear regression trend line:

```
# declining enrollment
.df2 <- rbind(head(.df, (4*14)) , tail(.df, (4*14)) , .df[.df$z == "1
.plot2 <- ggplot(data = .df2, aes(x = x, y = y, colour = 1, shape
+ geom_point()
+ scale_shape_manual(values = seq(0, nrow(.df2)))
+ scale_y_continuous(expand = c(0.1, 0))
+ scale_x_continuous(expand = c(0.1, 1))
+ facet_wrap(~z)
+ xlab("Year.1")
```