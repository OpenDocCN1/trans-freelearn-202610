
Note that we have once again utilized our `sp_minimal` custom-made theme to eliminate the axis annotation. The `geom_contour` function has been used to generate contour lines, similar to the Haifa DEM elevation contours, only that now we specified the exact vector of breaks, specifying where contour lines will be generated. Using `facet_grid(variable~year)`, we arranged the facets in rows according to `variable` and in columns according to `year`.

## Adding static maps from the Web

Nowadays, cartographers and spatial analysts are lucky to have a variety of online resources for map creation. We already used freely available satellite imagery, DEMs, and administrative border datasets, for example. However, to recover and process such datasets is not always a quick and straightforward task. We may need to figure out the spatial location in question, search for available resources, import them into R, and so on. On the other hand, several applications, such as Google Maps and OpenStreetMap, provide readily available maps of the entire earth with a variety of features that can be unreasonable to collect ourselves each time we want to create a background for a quick visualization (satellite and aerial imagery, roads, borders, town names, and so on).

The `ggmap` package is an extension to `ggplot2`, providing a simple interface to download static maps from such online resources and easily incorporate them as the background when creating maps within the `ggplot2` framework. We already used the `ggmap` package for geocoding (see Chapter 5, *Working with Points, Lines, and Polygons*), and mentioned the introductory paper on this package by Kahle and Wickham (2013).

> By static maps, we refer to maps that cannot be manipulated by scrolling, zooming in and zooming out, and so on, as opposed to dynamic maps that respond to such user feedback. The whole `ggplot2` framework, in that sense, may be referred to as static plotting. It should be noted that dynamic plots and maps (and even entire web applications; see the `shiny` package) can also be produced in R. As for dynamic maps, for instance, we have already seen a short example of using the Google Earth software as a platform for dynamic spatial data display from R using the `plotKML` package (see Chapter 4, *Working with Rasters*). Another example is the `plotGoogleMaps` package, which can be used to display spatial data from R on top of interactive Google Maps in a web browser.

The static maps incorporation with ggmap is quite simple in practice. Using the `get_map` function of the `ggmap` package, we first need to download the required background image. Then, we can use that image as a background layer with the `ggmap` function of the same package. Any additional layers and components can be added to the plot initiated with `ggmap` the same way as to a plot initiated with `ggplot`.

Our first example featuring ggmap will be to display the City of London buildings and their distances to the River Thames, as promised in Chapter 5, *Working with Points, Lines, and Polygons*. Since layers obtained with `ggmap` are always defined in a geographical CRS, we must first reproject the two relevant layers that we will plot to such a CRS as well. The layers we are going to plot, in this case, are as follows:

- buildings: City of London buildings
- city: City of London boundary polygon

The following expressions reproject these layers and save them as new objects `buildings_geo` and `city_geo`, respectively:

```r
> buildings_geo = spTransform(buildings,
+ CRS("+proj=longlat +datum=WGS84"))
> city_geo = spTransform(city,
+ CRS("+proj=longlat +datum=WGS84"))
```

To specify the location for which we would like to download a map with `get_map`, we can use the coordinates of the buildings layer centroid, obtained in the following manner:

```r
> buildings_ctr = coordinates(gCentroid(buildings_geo))
> buildings_ctr
```

|   | x | y |
|---:|---:|---:|
| 1 | -0.09222641 | 51.51499 |

Next, we will fortify the `buildings_geo` layer, to make the data available in the form of a `data.frame` object. The region in this case is the `osm_id` attribute table column, which holds unique building identifiers:

```r
> buildings_f = fortify(buildings_geo, region = "osm_id")
> head(buildings_f)
```

|   | long | lat | order | hole | piece | group | id |
|---:|---:|---:|---:|---:|---:|---:|---:|
| 1 | -0.09962729 | 51.51428 | 1 | FALSE | 1 | 100684524.1 | 100684524 |
| 2 | -0.09952849 | 51.51429 | 2 | FALSE | 1 | 100684524.1 | 100684524 |
| 3 | -0.09942449 | 51.51429 | 3 | FALSE | 1 | 100684524.1 | 100684524 |
| 4 | -0.09941299 | 51.51424 | 4 | FALSE | 1 | 100684524.1 | 100684524 |
| 5 | -0.09951839 | 51.51423 | 5 | FALSE | 1 | 100684524.1 | 100684524 |
| 6 | -0.09961579 | 51.51422 | 6 | FALSE | 1 | 100684524.1 | 100684524 |

Similar to what we saw in the county example, `fortify` removes the attribute table; therefore, we need to attach it back manually:

```r
> colnames(buildings_f)[which(colnames(buildings_f) == "id")] =
+ "osm_id"
> buildings_f = join(buildings_f, buildings@data, "osm_id")
> head(buildings_f)
```

|   | long | lat | order | hole | piece | group | osm_id | name | type | dist_river |
|---:|---:|---:|---:|---:|---:|---:|---:|---|---:|---:|
| 1 | -0.09962729 | 51.51428 | 1 | FALSE | 1 | 100684524.1 | 100684524 | Temple Bar | &lt;NA&gt; | 378.7606 |
| 2 | -0.09952849 | 51.51429 | 2 | FALSE | 1 | 100684524.1 | 100684524 | Temple Bar | &lt;NA&gt; | 378.7606 |
| 3 | -0.09942449 | 51.51429 | 3 | FALSE | 1 | 100684524.1 | 100684524 | Temple Bar | &lt;NA&gt; | 378.7606 |
| 4 | -0.09941299 | 51.51424 | 4 | FALSE | 1 | 100684524.1 | 100684524 | Temple Bar | &lt;NA&gt; | 378.7606 |
| 5 | -0.09951839 | 51.51423 | 5 | FALSE | 1 | 100684524.1 | 100684524 | Temple Bar | &lt;NA&gt; | 378.7606 |
| 6 | -0.09961579 | 51.51422 | 6 | FALSE | 1 | 100684524.1 | 100684524 | Temple Bar | &lt;NA&gt; | 378.7606 |

As the output shows, each building is now, once again, associated with a `dist_river` value.

Finally, we will `fortify` the `city_geo` polygon as well. Since there is no attribute data of interest along with it, we do not have to join anything to the resulting data frame object in this case:

```r
> city_f = fortify(city_geo, region = "CTYUA13NM")
> head(city_f)
```

|   | long | lat | order | hole | piece | group | id |
|---:|---:|---:|---:|---:|---:|---:|---:|
| 1 | -0.09671385 | 51.52319 | 1 | FALSE | 1 | City of London.1 | City of London |
| 2 | -0.09669776 | 51.52316 | 2 | FALSE | 1 | City of London.1 | City of London |
| 3 | -0.09664868 | 51.52317 | 3 | FALSE | 1 | City of London.1 | City of London |
| 4 | -0.09662369 | 51.52318 | 4 | FALSE | 1 | City of London.1 | City of London |
| 5 | -0.09646984 | 51.52282 | 5 | FALSE | 1 | City of London.1 | City of London |
| 6 | -0.09601742 | 51.52295 | 6 | FALSE | 1 | City of London.1 | City of London |

Examining the group column in this particular case will reveal that it has the same value ("City of London.1") in all rows since the city layer is composed of a single polygon.

The layers are ready. What is left to be done is download the background and produce the map. To download the background, we will employ the `get_map` function. In order to efficiently work with this function, we need to provide arguments for just these three parameters:

- location: The center of the map to be downloaded, which can be a longitude/latitude pair.

- maptype: The type of map to be downloaded. This is source-specific, but for Google Maps (the default data source), "terrain", "satellite", "roadmap", and "hybrid" are applicable. See the following screenshot for an example of a "hybrid" map; for a "satellite" example, see the screenshot after the following screenshot.

  > The default data source, and the one we use here, is Google Maps. Note that the Google Static Maps API is used by `get_map` to download the images in such cases, and that by using this function, you are agreeing to the Google Maps API Terms of Service (https://developers.google.com/maps/terms). Other data sources can be selected by modifying the `source` parameter of `get_map`, with possible sources being Google Maps ("google"), OpenStreetMap ("osm"), Stamen Maps ("stamen"), or CloudMade maps ("cloudmade"). See `?get_map` for details.

- zoom: The zoom level; an integer between 3 and 21 (the default value is 10). The best zoom value for a given map is determined by trying higher or lower magnifications and examining the result.

In our case, to get a static map of the City of London composed of a photographic map with roads and other features marked on top (a "hybrid" map), we can use the following expression:

```r
> city_map = get_map(location = buildings_ctr,
+ maptype = "hybrid",
+ zoom = 14)
```
