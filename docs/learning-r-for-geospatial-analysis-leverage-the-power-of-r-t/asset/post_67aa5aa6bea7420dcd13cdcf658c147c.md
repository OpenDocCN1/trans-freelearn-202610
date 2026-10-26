
Examining the table will also reveal that it contains subtotal entries, for entire states, in addition to the county entries. These entries are marked with the value 0 in the county column. Since we are interested in counties, not states, these entries need to be removed:

```
> dat = dat[dat$county != 0, ]
```

With the first and second steps now complete, we are going to create a key with which the county layer and the dat table will be joined. The least problematic option is to use the FIPS codes as a key since county names may be slightly different among datasets.

We already have a FIPS column in the county layer. For example, the FIPS codes of the first 10 features are as follows:

```
> county$FIPS[1:10]
  [1] "09005" "09003" "09013" "09015" "06093" "06015" "06049"
  [8] "09011" "09001" "09007"
```

These five-digit codes encompass the state code (digits 1 to 2) and county code (digits 3 to 5). As we can see, when the state or county code is fewer than two or three digits, it is preceded by zeros. For example, the first county polygon has the FIPS code 09005, which means its state code is 9 (state of Connecticut) and county code is 5 (Litchfield County). However, in the dat table, the state and county FIPS codes are kept as separate numeric values without leading zeros (9 and 5). In order to get matching values with the county layer, we need to perform the following steps:

+   1. Add leading zeros in the state column to get uniform two-digit codes.
+   2. Add leading zeros in the county column to get uniform three-digit codes.
+   3. Paste the state and county columns together to get the FIPS codes.

The first two steps can be performed using the formatC function. This function can deal with several formatting tasks including the one we need — padding the values of a vector with leading zeros, to obtain a common character length. In this particular case, we need to specify three arguments: the vector to work upon, the required final character count (the width parameter), and the format modifier "0" (the flag parameter, where "0" marks the pad with leading zeros scenario; see ?formatC):

```
> dat$state = formatC(dat$state, width = 2, flag = "0")
> dat$county = formatC(dat$county, width = 3, flag = "0")
```

We can examine the first few values of the modified state and county columns to make sure the expected outcome was obtained:

```
> dat$state[1:10]
[1] "01" "01" "01" "01" "01" "01" "01" "01" "01" "01"
> dat$county[1:10]
[1] "001" "003" "005" "007" "009" "011" "013" "015" "017" "019"
```

Note that the numeric values have been automatically converted to characters since a numeric value cannot have leading zeros.

Finally, we will paste the state and county codes into county FIPS codes and assign them to a new column called FIPS:

```
> dat$FIPS = paste0(dat$state, dat$county)
```

Now, we have in dat$FIPS an identical format as seen in county$FIPS:

```
> dat$FIPS[1:10]
[1] "01001" "01003" "01005" "01007" "01009" "01011" "01013"
[8] "01015" "01017" "01019"
```

We are ready to move on to the fourth step—joining the attribute table of county with the dat table. In fact, we are going to use a subset of dat, containing only two columns: FIPS (since the join will be based upon it) and census2010pop (since these are the data we are interested in) because the state and county columns are of no use to us at this stage. The join operation is done using the join function from the plyr package (see Chapter 3, Working with Tables):

```
> library(plyr)
> county@data = join(county@data,
+ dat[, colnames(dat) %in% c("FIPS", "census2010pop")],
+ by = "FIPS")
```

> Using county@data as the first argument in join and the type of join that is being used ("left", by default), ensures that all entries in county@data are preserved in their original order (regardless of whether they have a match in dat). This is extremely important since the rows of county@data correspond to the county polygons. Thus, changing the order of rows would result in a discrepancy between the attribute table and the spatial features. Concerning vector layers, operations such as join(county@data, x, type="left") are safe to perform, while manual modification of the @data component involving altering the row order (for example, deleting a single row) should be generally avoided.

As a result, we now have the matching census2010pop entries in the attribute table of county:

```
> head(county@data)
      NAME_1      NAME_2 TYPE_2  FIPS      area census2010pop
1 Connecticut Litchfield County 09005 2451.876      189927
2 Connecticut   Hartford County 09003 1941.110      894014
3 Connecticut    Tolland County 09013 1077.789      152691
4 Connecticut    Windham County 09015 1350.476      118428
5 California   Siskiyou County 06093 16416.572      44900
6 California   Del Norte County 06015 2626.707      28610
```

We can check and see that only one entry in the county layer could not be matched with a census2010pop entry from dat (and thus, has NA in the census2010pop column). This entry corresponds to Clifton Forge City, Virginia:

```
> county@data[is.na(county$census2010pop),
+ c("NAME_1", "NAME_2")]
      NAME_1          NAME_2
2591 Virginia Clifton Forge City
```

Our fifth and final step will be to calculate population densities for each county, by dividing the population size (the census2010pop column) by county area (the area column). The result can be assigned to a new column, named density, in the attribute table of county:

```
> county$density = county$census2010pop / county$area
```

Examining the attribute table shows that the new column, holding average population density per km², has indeed been added to the county layer:

```
> head(county@data)
      NAME_1      NAME_2 TYPE_2  FIPS      area census2010pop
1 Connecticut Litchfield County 09005 2451.876      189927
2 Connecticut   Hartford County 09003 1941.110      894014
3 Connecticut    Tolland County 09013 1077.789      152691
4 Connecticut    Windham County 09015 1350.476      118428
5 California   Siskiyou County 06093 16416.572      44900
6 California   Del Norte County 06015 2626.707      28610
density
1 77.461920
2 460.568482
3 141.670638
4 87.693548
5 2.735041
6 10.891965
```

Preparing a map of population densities per county is postponed until we reach Chapter 9, Advanced Visualization of Spatial Data.

## Summary

In this chapter, we covered the basic operations involved in working with vector layers in R. First, we reviewed the classes used to represent spatial vector layers in R, and explored two ways to bring spatial vector data into R (geocoding and reading from a file). Second, we discussed how to examine and modify the attribute tables of vector layers, how to create subsets of layers according to their attribute tables, and how to join new data to an attribute table (either from a separate table or from another vector layer). Third, the major types of geometry-related operations with vector layers, including the calculation of geometrical properties, evaluation of relations between layers, and the creation of new layers based on the existing ones, were presented.

In the next chapter, we will delve into rasters in more detail, examining geometry-related modification of rasters (such as reprojection), utilizing elevation rasters (such as DEMs), and working with spatio-temporal raster data (such as time series data from satellite images). Later, in Chapter 7, Combining Vector and Raster Datasets, we are going to discuss several common procedures associated with interrelations between raster and vector data.

## 6
Modifying Rasters and
Analyzing Raster Time Series

In this chapter, we will continue with the material presented in *Chapter 4, Working with Rasters*, moving on to more advanced operations. These involve either the modification of the geometric properties of a raster or direction- and distance-related calculations on rasters; mostly using additional functions in the `raster` package. Examples related to the analysis of spatio-temporal remote sensing data will be presented in the last section of this chapter, to demonstrate how more complex, custom-made procedures of raster processing can be constructed.

In this chapter, we will use objects that we previously created in *Chapter 3, Working with Tables*, and *Chapter 4, Working with Rasters*.

In this chapter, we'll cover the following topics:

+   - Modifying the geometry of raster layers
+   - Applying focal filters
+   - Clumping patches of connected cells
+   - Resampling and reprojection of rasters
+   - Performing topography-related calculations on elevation data
+   - Aggregating spatio-temporal raster data
