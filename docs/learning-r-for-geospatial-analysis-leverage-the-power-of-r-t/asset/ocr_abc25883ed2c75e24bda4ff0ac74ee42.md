The following screenshot shows the graphical output:

![](bbox=[0.21, 0.175, 0.79, 0.62])

The first thing to note is that the `lattice` syntax is analogous to the base R plotting, in the sense that we have distinct plotting functions for each type of plot (such as `plot` or `wireframe`), and each function has a set of parameters covering all modifiable elements of the respective plot type. This is very different from the `ggplot2` approach, where a plot is constructed layer by layer using numerous function calls rather than just one.