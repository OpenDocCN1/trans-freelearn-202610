
## 9 Advanced Visualization of Spatial Data

Visualization of spatial data is vital both during intermediate analysis steps (to examine preliminary results and make sure we are on the right track) and as the final product (to present our results to colleagues or in a publication). In this chapter, we are going to visualize various datasets we created in the previous chapters, bringing closure to the previously presented case studies. While doing this, you will learn how to produce an elaborate and customized graphical output in R.

Most of this chapter is going to concentrate on the popular graphical package `ggplot2`. We will begin by presenting the logic behind the special syntax this package follows. Afterwards, we will review, through examples, the way spatial and nonspatial plots can be produced and customized using this package. The `ggmap` package, which automates downloading static maps from the Web and can be used to complement plots produced with `ggplot2`, will then be presented. Finally, we will experiment a little bit with **three-dimensional (3D)** visualization using the `lattice` package.

In this chapter, we'll cover the following topics:

- Using `ggplot2` to produce publication-quality plots
- Using `ggmap` to add static maps as the background
- Using `lattice` to produce 3D plots

> [ In this chapter, we are going to use objects created in the previous chapters. ]

### Plotting with ggplot2 and ggmap

In this section, you are going to learn how to use ggplot2 and ggmap to visualize spatial data. The section is structured as follows. First, we will review the ggplot2 framework using a simple example of a time series plot since (as you will see right away) the syntax is quite different from that of other plotting methods we used until now. After that, we will practice a little bit more with producing ordinary, nonspatial plots. Next, we will see how the ggplot2 plots can be saved for subsequent use, both within the R environment and in external files. Finally, the last two sections will deal with the most important material from this book's perspective. In these two sections, we will see how spatial data can be incorporated into ggplot2 visualizations to produce maps, and how we can conveniently download reference background images for such maps using ggmap.

Before going into the details of ggplot2, it is important to state that to use this package, our input data needs to be contained in a data.frame object and it needs to be tidy (see Chapter 7, Combining Vector and Raster Datasets, for more information). Therefore, in practice, there are two steps when plotting with ggplot2:

- Bringing the data to the appropriate form
- Creating the plot

In some of the following examples, when the data is already in the right shape, we will skip the first step and move on to creating the plot right away. Otherwise, we will first have to reshape our data (using methods already familiar to us from the previous chapters) and only then utilize ggplot2 to create the plot itself.

### An overview of ggplot2

The best way to understand the underlying logic of ggplot2 is through examples. Our first example is going to reproduce the time series plot we created in Chapter 2, Working with Vectors and Time Series, when demonstrating the difference between the three graphics systems in R. As you remember, we used the following data.frame object (although not knowing that it was a data.frame object at the time) that represents a time series of temperature measurements. The time column of this data.frame object contains dates, while the tmax column contains the daily temperature maxima:

```
> head(dat)
       time tmax
1 2006-01-01 13.3
2 2006-01-02 14.4
3 2006-01-03 15.6
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

![](img/ebfd6294770b225fdeceda9a7b8b560e_297_0.png)

We will now briefly go over the main characteristics of the ggplot2 syntax, referring to the previous example. Later in this chapter, we will produce additional plots using other datasets from the previous chapters. This way, by the end of this chapter, we'll have reviewed the most important concepts and methods of operation for visualization of spatial data with ggplot2.

The ggplot2 package is extremely flexible. Due to the abundance of functions and usage options, the variety of plots it is capable of producing is mainly limited by the users' knowledge and expertise. However, this abundance can be overwhelming for beginners. In addition, the ggplot2 syntax is hard to grasp at first, both conceptually and practically. Nevertheless, those who overcome the initial difficulties are greatly rewarded.

The purpose of this chapter is to present, through examples, some of the most important points to note when using ggplot2 to display spatial data. Obviously, we cannot cover all plot types and their optional modifications (even a standalone book cannot accomplish such a task). What we can do is provide the initial knowledge sufficient for orientation. The subsequent usage of ggplot2 will inevitably require trial and error, as well as looking for help online. In addition, the following sources of information on ggplot2 are highly recommended:

![](img/ebfd6294770b225fdeceda9a7b8b560e_298_0.png)

- The ggplot2 package is fortunately accompanied by a highly comprehensive collection of help pages (http://docs.ggplot2.org/), where all functions and arguments are reviewed – in most cases with code and graphical output examples.
- Winston Chang's book R Graphics Cookbook, O'Reilly Media, which was published in 2012, is notable for providing an extensive coverage in the form of precise recipes for visualization using ggplot2 (although there are relatively few examples involving spatial data).
- Readers interested in more information on the theoretical background of ggplot2 can refer to the absorbing book by the package author Wickham, H. ggplot2: Elegant Graphics for Data Analysis, Springer, 2009. Some of the code sections in that book are slightly outdated (the package has evolved since the book was published), but this does not affect the book's utility.

Each expression used to create a plot with ggplot2 is made up of several components as we shall see shortly. It starts with the ggplot function call (such as ggplot(dat, aes(x=time, y=tmax))), followed by additional layers' definitions and settings.

At the core of each plot created with ggplot2 are the layers, with each layer necessarily associated with a given geometry and the data used to draw the layer. A plot with no layers cannot be produced since there will be nothing to show:

```
> ggplot(dat, aes(x = time, y = tmax))
Error: No layers in plot
```

Layers are added to a plot using the + operator, and the data used to create the layer, as well as the aesthetic mapping (the link between the data and layers' aesthetic appearance), are specified as arguments either in the ggplot function (setting them as global arguments for this particular plot) or in the respective layer function (setting them as local arguments for the particular layer only).

The most straightforward way to create a layer is to use the layer function, where we can (rather verbosely) specify all of the layer characteristics. The preceding plot, for instance, can also be produced with the following expression, which demonstrates that geom_line is a layer with a predefined "line" geometry and "identity" statistical transformation (this means that there is no statistical transformation; the values are taken as is), among other default definitions that remain hidden (such as colour="black" for line color):

```
ggplot(data = dat, aes(x = time, y = tmax)) +
  layer(geom = "line", stat = "identity")
```

However, in practice, layers are most often specified using predefined layer functions rather than the layer function. These functions' names start with geom (such as geom_line) or stat (such as stat_contour). There is no conceptual difference between these two types of functions; the difference is just in their defaults—the latter emphasizes statistical transformations while the former does not; therefore, each type of function may be easier to use for a given purpose. However, it should be remembered that the settings of any given layer can be overridden, so in many cases the geom and stat layers are redundant in terms of the desired result. For example, exactly the same layer of contours can be produced with either geom_contour or stat_contour.

In general, each layer is composed of the following five components:

- The geometry the layer follows (this is already encompassed in the layer function name, for example, geom_line)
- The data.frame object where the data come from (for example, dat)
- The definition of the link between the data and layer appearance, that is, aesthetic mapping inside the aes function (for example, aes(x=time,y=tmax)) or the definition of appearance unrelated to the data, that is, aesthetic setting outside the aes function
- The statistical transformation that manipulates the data prior to aesthetic mapping (we will not use this here)
- A position adjustment to deal with overlapping graphical objects (we will not use this here)
