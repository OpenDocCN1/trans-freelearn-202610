It is already apparent, from the last few examples, that the print method applied to such an object induces drawing it in a graphical window. Assignment of a complete ggplot2 plot, or individual layers (which we will see later), may serve at least two useful purposes:

+   - We can save a plot with ggsave providing the plot object as an argument so that we do not have to wait for the plot to be drawn in the graphical window for no good reason. This way, we also do not need to keep in mind which plot was produced last. For example, after the tmax_line object has been created, we can save the plot it describes as follows:

```
> ggsave(plot = tmax_line,
+ filename = "C:\\Data\\4367OS_09_01.png",
+ width = 5.5,
+ height = 3.5)
```

+   - We can define useful combinations of layers and/or settings, which we will repeatedly use to save us the trouble of typing the whole collection of components when producing a series of plots that have some components in common. Complete themes, such as theme_bw, also serve this purpose. For example, the following expression produces the same plot we saw in the first image in this chapter:

```
> tmax1 = ggplot(dat, aes(x = time, y = tmax))
> tmax2 = geom_line()
> tmax1 + tmax2
```

# Plotting spatial data

So far in this chapter, we introduced the ggplot2 package and briefly reviewed its syntax using a few simple, nonspatial examples. In the next two sections, we will move on to slightly more complicated examples, now involving spatial data. What you will see right away is that producing a map with ggplot2 is conceptually no different from producing any other kind of plot (because spatial data, in turn, is not conceptually different from any other type of data).