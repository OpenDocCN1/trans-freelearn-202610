# Spatial Interpolation of Point Data

This gives us the desired result, a 10-band raster with temperature means for either the minimum or maximum temperature, according to spainT_tab (and thus matching spainT). The layer names reveal that indeed the first five layers correspond to the minimum temperature and the last five layers correspond to the maximum temperature:

```
```r
> names(means)
[1] "mmnt.1" "mmnt.2" "mmnt.3" "mmnt.4" "mmnt.5" "mmxt.1"
[7] "mmxt.2" "mmxt.3" "mmxt.4" "mmxt.5"
```
```

What is left to be done is just to subtract one 10-band raster from the other:

```
```r
> spainT = spainT - means
```
```

The data we now have in the new spainT raster is the temperature deviations from the five-year average pattern of either the minimum or maximum temperature. We can check and see that the deviations are within the range of -4 and +4 degrees Celsius around the respective 5-year mean of 2006-2010:

```
```r
> range(spainT[], na.rm = TRUE)
[1] -4.020094  3.200148
```
```

Plotting these deviations will help us see more clearly where exactly the temperature was high or low, with respect to the average pattern, and what the magnitude of the departure was. This time, we will also indicate that contours should be drawn at 1°C intervals:

```
```r
> levelplot(spainT,
+ par.settings = "BuRdTheme",
+ layout = c(5,2),
+ contour = TRUE,
+ at = c(-4:-1,1:4))
```
```

The graphical output is shown in the following screenshot:

![](bbox=[0.224, 0.648, 0.775, 0.826])