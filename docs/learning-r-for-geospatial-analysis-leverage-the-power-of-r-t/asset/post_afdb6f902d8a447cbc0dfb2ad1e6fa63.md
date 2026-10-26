
```
+ v_mod =
+ autofitVariogram(
+ formula = form,
+ input_data = dat)$var_model}
+ # (3) Create gstat object
+ g = gstat(formula = form, data = dat, model = v_mod)
+ # (4) Perform cross-validation
+ cv = gstat.cv(g)
+ # (5) Calculate RMSE and assign to cv_results
+ cv_results$rmse[i] = rmse(cv)
+ }
```

Note that, in step 2, nested conditional statements are used in order to determine form and v_mod according to the interpolation method that is being used. We can write down the procedure this particular code section performs in the form of a decision tree:

+   - If the method is "IDW", then form is value~1 and v_mod is NULL
+   - Otherwise:
+     - If the method is "OK", then form is value~1
+     - If the method is "UK", then form is value~elevation
+     - The v_mod is calculated according to form

Once the loop execution is completed (this may take a few moments), the rmse column of cv_results will be filled with the resulting RMSE values instead of NA values:

```
> head(cv_results)
     variable year method    rmse
1      mmnt 2001    IDW 1.942872
2      mmxt 2001    IDW 1.594996
3      mmnt 2002    IDW 1.880490
4      mmxt 2002    IDW 1.570574
5      mmnt 2003    IDW 1.912887
6      mmxt 2003    IDW 1.605938
```

Note that, for instance, the RMSE value at the third row table (1.880490), which corresponds to the IDW interpolation of "mmnt" for 2002 as expected, is identical to the result we have previously obtained manually. Several interesting comparisons can be made using this RMSE table. As previously stated, we are mainly interested in seeing which interpolation method is the most accurate, on average, across 10 years and the two variables. To find out, we can use tapply:

```
> tapply(cv_results$rmse, cv_results$method, mean)
        IDW        OK        UK
1.773243 1.908957 1.458997
```

The result shows that, on average, UK yields the lowest prediction error among the three methods. Interestingly, OK is less accurate, on average, than IDW although in the particular example we previously saw it was the other way around. Based on this result, UK is going to be our method of choice to produce annual temperature maps of Spain.

> It should be noted that comparing cross-validation RMSE is only meaningful among different spatial prediction models of the same data. Otherwise, RMSE may be higher or lower not only because the prediction error is different but also because the predicted values themselves are on different scales. For example, RMSE of the precipitation amount ("tpcp") prediction would have been much higher than RMSE of temperature prediction in our case (you can try and see this), simply because the generally observed annual precipitation amounts in Spain (in mm) are higher by an order of magnitude than temperatures (in Celsius degrees). However, this does not mean that the prediction models of precipitation are less accurate with respect to the scale of the measured variable itself.

We are ready to move on and produce annual temperature maps using another loop that calculates the predicted values (rather than performing cross-validation, which we just did). Once again, a well-organized way of doing this is to construct a table describing the set of parameters we need for each spatial interpolation, then go through that table and perform the required operations in turn. We will once again use the expand.grid function, producing all combinations of just two variables this time: year (the 5 years 2006-2010) and variable ("mmnt" and "mmxt"). The method will be left unspecified as we have already decided it should be invariably UK.

```
> spainT_tab = expand.grid(
+ year = 2006:2010,
+ variable = c("mmnt", "mmxt"))
```

The resulting table, named `spainT_tab`, looks as follows:

```
> spainT_tab
  year variable
1  2006      mmnt
2  2007      mmnt
3  2008      mmnt
4  2009      mmnt
5  2010      mmnt
6  2006      mmxt
7  2007      mmxt
8  2008      mmxt
9  2009      mmxt
10 2010      mmxt
```

We are ready to create the second loop that will go through the rows of `spainT_tab` and spatially interpolate the respective temperature data. Since our results are going to be rasters (rather than numeric values, as was the case with the cross-validation loop) in this case, they cannot be appended to the table itself. Instead, we are going to stack the results in a multiband raster where each layer is going to correspond to a row in `spainT_tab`. In other words, `spainT_tab` is going to function as a supplementary table for the resulting multiband raster (much as `dates` served the same purpose for the raster `r`; see Chapter 4, Working with Rasters). To construct the raster, we will first create an empty `RasterStack` to which the interpolation results will be appended during loop execution:

```
> spainT = stack()
```

The loop will go through the rows of `spainT_tab`, each time performing the following set of operations:

+   1. Creating a point layer named `dat` with the variable and year set according to the current row of `spainT_tab`.
+   2. Fitting a variogram model (`v`).
+   3. Creating a `gstat` object (`g`) according to `dat` and `v`.
+   4. Calculating the raster of predicted values (`z`) with `interpolate`, based on `g` and `dem_spain`.
+   5. Masking `z` according to `dem_spain` and attaching the result to `spainT`.

When we are interested in interannual differences only, the most reasonable course of action is to find out what is the characteristic annual temperature state, find out the deviation of each individual year's state from the characteristic state, and then compare the deviation images rather than the original ones. Calculating deviations from a general trend is a very common practice to highlight patterns of interest in our data, and there are many ways to find both the general trend and the deviations of each observation from it. Here, we will employ the simplest possible approach—the characteristic temperature in each pixel will be defined as the 5-year average observed in that pixel, and the deviations will be equal to arithmetic differences of each observation from the average.

Since we need to treat the minimum and maximum temperature data separately, we will first create two vectors identifying the relevant `spainT` layers for each case:

```
```r
> mmnt_layers = which(spainT_tab$variable == "mmnt")
> mmnt_layers
[1] 1 2 3 4 5
> mmxt_layers = which(spainT_tab$variable == "mmxt")
> mmxt_layers
[1]  6  7  8  9 10
```
```

We will also create a new two-band RasterStack, named means, to hold the minimum and maximum mean temperature pattern in its first and second layers, respectively. The two layers will also be named accordingly ("mmnt" and "mmxt"):

```
```r
> means = stack(mean(spainT[[mmnt_layers]]),
+ mean(spainT[[mmxt_layers]]))
> names(means) = c("mmnt", "mmxt")
```
```

Now that we have the means, all we need to do is subtract the appropriate mean from each set of layers in spainT (the mean of the minimum temperatures from layers 1-5 and the mean of the maximum temperatures from layers 6-10). There are numerous ways to do this. For example, we can divide the spainT raster into two, subtract the respective mean from each substack, and then combine them once again. A more elegant way, extendable to any number of categories, however, is to create a new means raster with the same number of layers as spainT, by means of duplication, with the relevant means occupying its layers. In other words, we need a raster with means[[1]] duplicated five times (occupying layers 1-5) and means[[2]] duplicated five times (occupying layers 6-10). To do this, we can utilize the fact that raster layers can also be selected by their names. The following expression therefore takes the "mmnt" and "mmxt" layers and duplicates them in agreement with the spainT_tab$variable column:

```
```r
> means = means[[spainT_tab$variable]]
```
```
