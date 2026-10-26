This time, we can easily notice, for example, that the minimum temperatures in 2010 were higher than average in southern Spain while the maximum temperatures were lower than average in the north of the country.

This is the final product we were looking for, according to the goals specified at the beginning of this section. However, we are not done yet. In the next chapter, we will use spainT as one of the sample datasets while discussing how to produce plots with the ggplot2 package. Since ggplot2 is a general-purpose graphical package, the input data must come as a data.frame object. Moreover, the data table should be tidy (see the previous chapter) in order to conveniently exploit all of the possibilities this package offers. What does that mean in the present case? It means that we need to have a data.frame object with all the data currently held in spainT, with each column corresponding to a variable and each row corresponding to an observation. In this case, the variables are as follows:

+   - x-coordinate
+   - y-coordinate
+   - Variable
+   - Year
+   - Predicted temperature

Each row in such a data.frame object will correspond to one pixel of an individual layer in spainT.

Luckily, the as.data.frame function (that we encountered in the previous chapter, when converting a matrix to a data.frame) also has a method to convert rasters to the data.frame objects. By setting xy=TRUE, we are specifying that we would like to have a table not only with the raster values (from all of its layers) but also with the respective spatial coordinates:

```
> spainT = as.data.frame(spainT, xy = TRUE)
> head(spainT)
          x      y mmnt_2006 mmnt_2007 mmnt_2008 mmnt_2009
1 -11957.925 4857128        NA        NA        NA        NA
2  -8357.925 4857128        NA        NA        NA        NA
3  -4757.925 4857128        NA        NA        NA        NA
4  -1157.925 4857128        NA        NA        NA        NA
5   2442.075 4857128        NA        NA        NA        NA
6   6042.075 4857128        NA        NA        NA        NA
  mmnt_2010 mmxt_2006 mmxt_2007 mmxt_2008 mmxt_2009 mmxt_2010
1        NA        NA        NA        NA        NA        NA
```