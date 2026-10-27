![](bbox=[0.196, 0.147, 0.78, 0.567])

# Stacked bar chart

Stacked bar charts are just another variant of bar charts, where more than two variables can be plotted with different combinations of color. The following example codes show some variants of stacked bar charts:

```
> qplot(factor(Type), data=Cars93, geom="bar", fill=factor(Origin))
> 
> #or
> 
> ggplot(Cars93, aes(Type, fill=Origin)) + geom_bar()
```