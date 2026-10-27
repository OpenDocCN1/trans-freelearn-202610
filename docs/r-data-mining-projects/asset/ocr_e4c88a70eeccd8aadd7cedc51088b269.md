![](bbox=[0.105, 0.15, 0.905, 0.62])

Using the `facet_grid()` option, we can display the bi-variate relationship between two categorical variables using the ggplot2 library:

```
> #bi-variate plotting using ggplot2
> gg<-gg+facet_grid(color~cut)
> gg
```