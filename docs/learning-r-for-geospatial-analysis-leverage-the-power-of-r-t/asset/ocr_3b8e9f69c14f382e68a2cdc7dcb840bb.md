The graphical output is shown in the following screenshot:

![](bbox=[0.23, 0.17, 0.77, 0.53])

What we see here is the elevation map of Spain (the `dem_spain` raster), with station locations on top (`stations` point layer). The stations seem to cover the whole area of the country, more or less evenly. Note that some of the stations are not shown since they are located on the Canary Islands and thus they are beyond the scope of the `dem_spain` raster.

To extract the elevation values underlying each station, we employ the `extract` function with the raster and the points as the first two arguments, respectively. Since the returned object is going to be a numeric vector with the respective elevation values for each station, we can directly assign it to a new column in `stations` named `elev_dem`:

```
> stations$elev_dem = extract(dem_spain, stations)
```