If we want to calculate the area of each feature (each county, in this case), rather than the area of the layer as a whole, we need to specify byid=TRUE. The byid parameter determines whether we wish to perform the calculation by ID, that is, for each feature separately. This parameter is present in many of the functions in the rgeos package with the same functionality, as we shall see in the subsequent examples. The following expression returns a numeric vector with the area of each feature in the county layer in km². The vector is immediately assigned to a new column in the county layer, named area:

```
> county$area = gArea(county, byid = TRUE) / 1000^2
```

Now the attribute table of a county contains an extra column with the area size for each county. We can confirm this by printing the first few rows of the attribute table:

```
> head(county@data)
      NAME_1      NAME_2 TYPE_2  FIPS    area
0 Connecticut Litchfield County 09005 2451.876
1 Connecticut   Hartford County 09003 1941.110
2 Connecticut    Tolland County 09013 1077.789
3 Connecticut    Windham County 09015 1350.476
4  California   Siskiyou County 06093 16416.572
5  California  Del Norte County 06015 2626.707
```

As an example of an operation where a new spatial layer is created, we will dissolve the county polygons into state polygons. For simplicity, we will perform the dissolving on a subset of county, including only two states: Nevada and Utah. At first, we will create the subset and assign it to a new object named county_nv_ut:

```
> county_nv_ut = county[county$NAME_1 %in% c("Nevada", "Utah"), ]
```

Now, we will dissolve the county_nv_ut polygons using the gUnaryUnion function. The two arguments transferred to this function are the layer to be dissolved and the ID, a vector defining the features that should be aggregated (all features with identical levels will be dissolved into one). If the id argument is omitted, all polygons are dissolved into one, as we shall see in subsequent examples. Here is the code for dissolving our current layer:

```
> states = gUnaryUnion(county_nv_ut, id = county_nv_ut$NAME_1)
```