# Advanced Visualization of Spatial Data

It is important to understand that in order to produce a minimal plot, we need to specify only a dataset, a single layer, and mappings for the layers' required parameters (such as x and y in `geom_line`). Other layer parameters (such as colour or size), scales, faceting, and themes are optional. For example, the following expression adds a colour setting and two scale definitions, but it produces exactly the same plot as shown in the preceding screenshot, since all of the components we added were already specified by default:

```
ggplot(dat, aes(x = time, y = tmax)) +
geom_line(colour = "black") +
scale_x_date() +
scale_y_continuous()
```

The two scale settings used in the preceding expression (`scale_x_date()` and `scale_y_continuous()`) had no effect since a continuous variable (such as tmax) is by default plotted on a continuous scale, while a Date variable (such as time) is by default plotted on a dates scale. In addition, we have an aesthetic setting (`colour="black"`) that had no effect either. As opposed to an aesthetic mapping (which is always encompassed in the aes function), the aesthetic setting links the layer to a constant aesthetic value, irrespective of the data in a given column. In the preceding expression, we set the line color to black in the geom_line layer, with `colour="black"`. Again, since "black" is the default value for the line color in ggplot2, this setting does not affect the plot appearance.

The plot we produced still needs a little polishing if we would like to include it in a publication. In general, certain plot adjustments are not related to the data (for example, changing the background color), while others are indirectly related to the data (for example, changing the color scale of a raster). First of all, we would like to have proper axis titles (for example, "Time" instead of "time" for the x axis), preferably without changing the data column names themselves. In many cases a journal may require a cleaner appearance, so we may also wish to remove the gray background and grid lines. The latter two properties can be modified using the scales and themes specifications, respectively.

Through the `theme` functions, we can control the general appearance of the plots' nondata elements, such as plot title, axis labels style, tick marks length, background color, and so on. This can be done in two ways, which are not mutually exclusive but additive:

+   - The so-called complete themes can be specified to control all of the elements at once, in a predefined way. The predefined themes in ggplot2 currently include `theme_grey`, `theme_bw`, `theme_linedraw`, `theme_light`, `theme_minimal`, and `theme_classic`. You are welcome to replace `theme_bw` with each of these in the following code section, to see what each of them looks like.