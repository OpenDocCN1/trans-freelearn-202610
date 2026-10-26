# Working with Points, Lines, and Polygons

Our next example will utilize two of these functions: `gIntersection` and `gDifference`. We will also use three new layers: buildings, natural areas, and administrative borders in Haifa. The buildings and natural areas layers originate from OpenStreetMap data, the same way as in the London example, while the administrative borders of Israel will be downloaded from a global administrative borders dataset directly through R. The buildings and natural areas layers will be named `haifa_buildings` and `haifa_natural` in order to not be confused with the analogous objects `buildings` and `natural` from the London example.

Our goal will be to create a polygon encompassing the natural areas in the vicinity of the buildings in Haifa, excluding those natural areas that are within 50 meters of the nearest building. We will follow four intermediate steps:

+   1. Read the layers into R and bring them to a common CRS.
+   2. Create a bounding polygon (convex hull) encompassing the buildings.
+   3. Clip the natural areas according to the bounding polygon.
+   4. Remove the natural areas that are 50 meters away from buildings.

As a first step, we will read the Haifa buildings and natural areas layers:

```
```r
> haifa_buildings = readOGR("C:\\Data", "haifa_buildings")
> haifa_natural = readOGR("C:\\Data", "haifa_natural")
```
```

The third layer involved, the administrative boundaries, will be downloaded from the GADM database of Global Administrative Areas at `http://www.gadm.org/`, which is accessible using the getData function from the raster package:

```
```r
> israel_adm = getData("GADM", country = "ISR", level = 1)
```
```

The hereby used arguments of getData are as follows:

+   - `name`: The dataset name (for example, "GADM", which stands for the GADM dataset; using another dataset, "SRTM", will be demonstrated in the next chapter)
+   - `country` (relevant for the `name="GADM"` option): The country ISO3 code (a list of country codes can be obtained with `getData("ISO3")`)
+   - `level` (relevant for the `name="GADM"` option): The level of administrative subdivision (0: country, 1: first subdivision, and so on)