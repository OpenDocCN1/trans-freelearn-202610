To clarify things, it is best to show an example. The following code produces two histogram facets, according to the grouping variable `cover`, using `facet_grid`. Since we have only one grouping factor, we mark the column grouping as `.` to obtain a vertical ribbon (later in this chapter, in the Spain temperature maps example, we will use `facet_grid` with two grouping factors):

```
```r
> ggplot(slopes, aes(x = slope)) +
+ geom_histogram() +
+ facet_grid(cover ~ .) +
+ scale_x_continuous("Slope (radians)") +
+ scale_y_continuous("Count") +
+ theme_bw()
```
```

The resulting plot is shown on the *left* in the following screenshot. Since in this case we have a single grouping variable, the same kind of plot can also be produced with `facet_wrap`. Replacing `facet_grid(cover~.)` with `facet_wrap(~cover,ncol=1)` produces an identical plot, except for a slightly different facet labeling scheme; the latter plot is shown on the *right* in the following screenshot. Note that in both cases, the facets share a common *x* axis.

![](bbox=[0.221, 0.448, 0.778, 0.792])