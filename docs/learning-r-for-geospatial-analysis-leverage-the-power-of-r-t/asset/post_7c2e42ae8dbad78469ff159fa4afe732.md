
If we try to match the `airports` attribute table entries to `nm` (with `over(nm, airports)`), only the first airport that intersects each county will be returned in cases where there are multiple matches. If we wish to preserve all matches, we need to specify `returnList=TRUE` in the `over` function call. However, the returned object will be a `list` object since holding sets of elements with variable lengths requires a `list` rather than a `data.frame` object. Many of the more advanced uses of R that require such flexibility involve `list` objects. However, the subject is beyond the scope of this book. More information on lists can be found in the official R's introduction document at http://cran.r-project.org/doc/manuals/r-release/R-intro.pdf and in most introductory books on R.

As an example of querying polygon-polygon relations, we will use another example involving three polygonal Shapefiles:

- Administrative areas of England and Wales (`CTYUA_DEC_2013_EW_BFE.shp`)
- Buildings in Greater London (`london_buildings.shp`)
- Natural areas in Greater London (`london_natural.shp`)

The datasets were downloaded from freely accessible online resources, either from the Office of National Statistics at https://geoportal.statistics.gov.uk/ (administrative areas) or OpenStreetMap (buildings and natural areas).

First, we will read the files into R and bring them to a common CRS. We will begin with the administrative areas layer, reading it from the disk and naming it boundary:

```
> boundary = readOGR("C:\\Data", "CTYUA_DEC_2013_EW_BFE")
```

Using the `proj4string` function reveals the CRS of boundary — the British National Grid. Note that the resulting string is abbreviated in the following output since it does not fit in a single line:

```
> proj4string(boundary)
[1] "+proj=tmerc +lat_0=49 +lon_0=-2 +k=0.9996012717 +x_0=400000 ..."
```

Next, we will read the two OpenStreetMap layers:

```
> buildings = readOGR("C:\\Data", "london_buildings")
> natural = readOGR("C:\\Data", "london_natural")
```

Both these layers are defined in a geographical CRS, as the following output demonstrates:

```
> proj4string(buildings)
[1] "+proj=longlat +datum=WGS84 +no_defs +ellps=WGS84 +towgs84=0,..."
> proj4string(natural)
[1] "+proj=longlat +datum=WGS84 +no_defs +ellps=WGS84 +towgs84=0,..."
```

We will reproject the buildings and natural layers to the CRS of boundary:

```
> buildings = spTransform(buildings, CRS(proj4string(boundary)))
> natural = spTransform(natural, CRS(proj4string(boundary)))
```

Now that the preliminary preparations are complete, we can continue with the exercise. Our goal will be to assign the distance to the River Thames to each of the buildings within the City of London. For this, we will go through the following intermediate steps:

1. Create a subset of buildings with only those in the City of London.
2. Create a subset of natural with only the riverbanks.
3. Dissolve the riverbank features into a single one.
4. Calculate the distance between each feature in buildings and the riverbank.

The administrative boundaries layer comes with an explanatory file (Product Specification for CTYUA_2013_EW_BFE.docx, which is also provided on the book's website), where we can find out that the names of the different administrative areas are defined in the CTYUA13NM column of the attribute table of this layer. Using this column, we will create a subset, named city, containing only the polygon defining the city of the London administrative area:

```
> city = boundary[boundary$CTYUA13NM == "City of London", ]
```

Using the city polygon, we will create a subset of only those buildings that are within the City of London. If we were interested in intersection (in other words, buildings that are completely or incompletely within the city polygon), we could use the [ or over methods as already shown earlier. To evaluate other types of relationships, the rgeos package provides a dozen additional functions: gContains, gContainsProperly, gCovers, gCoveredBy, gCrosses, gDisjoint, gEquals, gEqualsExact, gIntersects, gTouches, gOverlaps, and gWithin. The names of these functions already provide a clue for the relationship they are used to evaluate; the exact definitions are provided in the respective help page of each function. Since in our example we are interested in creating a subset of the `buildings` features that are contained within `city`, we are going to use the function that evaluates containment—`gContains`.

According to the help page of gContains, the expression `gContains(city,buildings)` will return TRUE for buildings features that are contained within city. Specifying `byid=TRUE` is necessary to evaluate the relation separately for each feature:

```r
> in_city = gContains(city, buildings, byid = TRUE)
```

The result, `in_city`, is a two-dimensional matrix with rows corresponding to the buildings features (210,937 buildings in Greater London) and columns corresponding to the city features (there is only one, the City of London; if we had more than one feature in `city`, we would have had more columns):

```r
> class(in_city)
[1] "matrix"
> head(in_city)
     119
0 FALSE
1 FALSE
2 FALSE
3 FALSE
4 FALSE
5 FALSE
```

The first (and only) column of this matrix is a logical vector specifying the containment status of each building within the city polygon. Using this vector as a rows index for `buildings` creates a subset of only those buildings within city:

```r
> buildings = buildings[in_city[, 1], ]
```

The first step is now complete. Returning to the natural layer, we will now subset the polygons of type `"riverbank"` (which in the vicinity of the City of London corresponds to the River Thames):

```r
> river = natural[natural$type == "riverbank", ]
```

As the third step, we will dissolve the separate river segments since we will be interested in the shortest distance to any section of the river, rather than the distances to its specific parts. As previously shown in the county example, dissolving can be achieved with `gUnaryUnion`. This time, the `id` parameter is left unspecified so that all geometries will be dissolved into a single one:

```r
> river = gUnaryUnion(river)
```

## Creating new geometries

The rgeos package provides four functions to create new layers based on a pair of existing ones: `gDifference`, `gIntersection`, `gSymdifference`, and `gUnion`. The usage of these functions is very similar to that of functions to query relationships since their main parameters are also a pair of layers and the byid parameter. The difference is that they do not return logical values or matched attribute table entries (based on whether the examined relationship holds), but rather a new layer. The following diagram demonstrates how new geometries are generated in each case:

![](img/ebfd6294770b225fdeceda9a7b8b560e_179_0.png)

Note that the gUnion function operates in a similar way to gUnaryUnion—just that the gUnion function dissolves geometries from two different layers, while the gUnaryUnion function operates upon the geometries of a single layer.

## Working with Points, Lines, and Polygons

Our next example will utilize two of these functions: `gIntersection` and `gDifference`. We will also use three new layers: buildings, natural areas, and administrative borders in Haifa. The buildings and natural areas layers originate from OpenStreetMap data, the same way as in the London example, while the administrative borders of Israel will be downloaded from a global administrative borders dataset directly through R. The buildings and natural areas layers will be named `haifa_buildings` and `haifa_natural` in order to not be confused with the analogous objects `buildings` and `natural` from the London example.

Our goal will be to create a polygon encompassing the natural areas in the vicinity of the buildings in Haifa, excluding those natural areas that are within 50 meters of the nearest building. We will follow four intermediate steps:

1. Read the layers into R and bring them to a common CRS.
2. Create a bounding polygon (convex hull) encompassing the buildings.
3. Clip the natural areas according to the bounding polygon.
4. Remove the natural areas that are 50 meters away from buildings.

As a first step, we will read the Haifa buildings and natural areas layers:

```r
> haifa_buildings = readOGR("C:\\Data", "haifa_buildings")
> haifa_natural = readOGR("C:\\Data", "haifa_natural")
```

The third layer involved, the administrative boundaries, will be downloaded from the GADM database of Global Administrative Areas at `http://www.gadm.org/`, which is accessible using the getData function from the raster package:

```r
> israel_adm = getData("GADM", country = "ISR", level = 1)
```

The hereby used arguments of getData are as follows:

- `name`: The dataset name (for example, "GADM", which stands for the GADM dataset; using another dataset, "SRTM", will be demonstrated in the next chapter)
- `country` (relevant for the `name="GADM"` option): The country ISO3 code (a list of country codes can be obtained with `getData("ISO3")`)
- `level` (relevant for the `name="GADM"` option): The level of administrative subdivision (0: country, 1: first subdivision, and so on)
