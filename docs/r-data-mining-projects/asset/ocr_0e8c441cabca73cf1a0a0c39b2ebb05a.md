![](bbox=[0.13, 0.15, 0.85, 0.48])

In the preceding graph, the `cut` variable is used to show the relationship between the carat and the price. The number of rows selected is four, to represent the graphs in a clear manner. If we add one more variable to the `cut` variable, that is `clarity`, the graph would be more intuitive and insightful:

```
> #adding two variables as cut to display the relationship
> gg<-gg+facet_wrap(~cut+clarity, nrow=4)
> gg
```