Interactive visualization, over informative reference layers (for example, in Google Earth), is very helpful for the initial examination of spatial data we have at hand. For example, we can now see clearly that the NDVI gradient within the raster, from relatively high values towards north-west direction to relatively low values towards south-east direction, is due to its positioning in the transition zone between the relatively humid Mediterranean climatic region (where vegetation is more abundant) and the arid Negev desert (where vegetation is scarce).

The interested reader can refer to the paper in the Journal of Statistical Software by Hengl, Roudier, Beaudette, and Pebesma (2014), or to the online tutorial (http://gsif.isric.org/doku.php?id=wiki:tutorial_plotkml) for further details and inspirational examples on the wide range of methods the plotKML package offers.

# Accessing raster values as a vector

Returning to the subject of raster value access, the simplest way of doing that is with the [ operator, exactly the same way we would with a vector. When accessing the values of a raster with [, the values will be ordered from the top-left corner rightwards, then along the second row, and so on, until the lower-right corner is reached. For example, to find out the first five values of the first layer in the r raster, we will use the following expression:

```
> r[[1]][1:5]
[1] 0.4242 0.3995 0.4190 0.4272 0.4285
```

Note that indices referring to bands with [[ come first, and indices referring to cells with [ come second. Either can be omitted, and then all elements from the respective dimension will be returned rather than a subset (for example, as we have seen previously, r[[1]] returns the whole first band).

When the [ operator is used, but the cell value's index is omitted (as in []), we get a vector containing all of the raster values. We can use this vector, for example, to calculate the mean NDVI on the first date of acquisition (February 18, 2000).

```
> mean(r[[1]][], na.rm = TRUE)
[1] 0.2302056
```

The result is 0.23.