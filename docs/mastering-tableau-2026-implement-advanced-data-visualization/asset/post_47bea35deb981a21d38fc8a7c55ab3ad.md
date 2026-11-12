
- 10. Close the dialog box and press Ctrl + V (Command + V for Mac) to create a new dataset in Tableau. Rename the resulting dataset (now called Clipboard_[timestamp]) to Lat Long. Also name the worksheet Lat Long.

- 11. In the Lat Long worksheet, rename Latitude (generated) and Longitude (generated) to Lat and Long.

- 12. Return to the Miles worksheet and, within the Transit Data data source, create two calculated fields: one called LAT, containing the code AVG([Lat Long].[Lat]), and one called LONG, containing the code AVG([Lat Long].[Long]).

- 13. Remove Latitude (generated) and Longitude (generated) from Text on the Marks card.

- 14. Place Measure Names on the Columns shelf and the Filter shelf and select Lat and Long. Also, place Measure Values on the Text shelf. Now, we have the ability to treat latitude and longitude as true measures.

- 15. Create two more calculated fields: one called Lookup Lat, containing the code Lookup(Lat, -1), and one called Lookup Long, containing the code Lookup(Long, -1). Place the two newly created calculated fields on the Measure Values shelf.

- 16. Create a calculated field named Great Circle Distance Formula with the following code:

```
3959 * ACOS (
SIN(RADIANS([Lat])) * SIN(RADIANS([Lookup Lat]))+ COS(RADIANS([Lat])) *
COS(RADIANS([Lookup Lat])) * COS(RADIANS([Lookup Long]) - RADIANS([Long]))
)
```

For kilometers, change 3959 to 6378. This number represents the radius of the Earth, and it needs to be updated from miles to kilometers.

- 17. Place the newly created calculated field Great Circle Distance Formula on the Measure Values shelf.

- 18. Change the calculation for Great Circle Distance Formula so that it computes using City by clicking on the field itself and selecting Compute Using: City

- 19. Adjust the following calculations accordingly

- Lookup Lat: IFNULL(LOOKUP(Lat,-1), LOOKUP(Lat,1))

- Lookup Long: IFNULL(LOOKUP(Long,-1), LOOKUP(Long,1))

- 20. Select the Map worksheet and set the Marks type to Line.

- 21. Place Trip ID on the Detail shelf.

- 22. Drag City to the bottom of the Marks card.

- 23. Place the Great Circle Distance Formula field on the Tooltip shelf. Double-check that it is still set to Compute Using: City.

![](img/66cf6b89349fda3581a4f196cb3c98a4_388_0.png)

Figure 9.6: Australia distances

- 24. Create two more calculated fields, one called Source City, containing the following code:

```
{ FIXED [Trip ID]:MIN(IF [Dest/Orig]='Source' THEN City END)}
```

The second calculated field should be called Destination City and contain the following code:

```
{ FIXED [Trip ID]:MIN(IF [Dest/Orig]='Destination' THEN City END)}
```

- 25. Use the newly created calculated fields to format as desired. In particular, notice that in the following screenshot, **Source City** is on the **Color** shelf and **Destination City** is used on the **Tooltip** shelf:

![](img/66cf6b89349fda3581a4f196cb3c98a4_389_0.png)

Figure 9.7: Flights

To sum up what we have done here, initially, we added a dataset to Tableau. We then needed to copy the latitudes and longitudes per city out of the workbook in order to create a second data source, which we then used as a blend to look up latitude and longitude values. With the help of LOD calculations, we could identify the source and destination cities, and lastly, we were able to measure the distance between cities by using the so-called great circle formula. Each color in the visualization represents a starting city; for example, all green lines measure distances from Perth to other cities. By hovering over a line, the source and destination city, as well as the distance measured in miles (or kilometers), are shown.

If you want to analyze shipment routes or any other distances between two points, you now know exactly how to visualize this kind of data in Tableau.

![](img/66cf6b89349fda3581a4f196cb3c98a4_390_0.png)

Figure 9.8: U.S. Flight Departure Delays

In Figure 9.8, you can see another, similar visualization, which is definitely worth checking out (try to replicate it): https://www.tableau.com/solutions/workbook/big-data-more-common-now-ever.

In the next section, we will discuss how to leverage a combination of maps and polygons to further extend our Tableau toolbox.

## Creating custom polygons

Geographic areas for which Tableau natively provides polygons include worldwide airport codes, cities, countries, regions, territories, states, provinces, and some postcodes and second-level administrative districts (county-equivalents): U.S. area codes, Core-Based Statistical Areas (CBSAs), Metropolitan Statistical Areas (MSAs), congressional districts, and ZIP codes. This means, for example, that a filled map can easily be created for the countries of the world. Simply copy a list of the world's countries (Ctrl + C) and paste that list into Tableau by pressing Ctrl + V while your mouse is located on an empty worksheet in Tableau Desktop. A new data source will be added to the top right under Data. Next, set the View type in Tableau to Filled Map and place the country list on the Detail shelf. Tableau will automatically draw polygons for each of those data points. There are some geographic types for which Tableau will not automatically provide polygons. These include telephone area codes. For these geographic types, Tableau will draw a symbol map but not a filled map, like so:

![](img/66cf6b89349fda3581a4f196cb3c98a4_391_0.png)

Figure 9.9: Filled map failed

Even though the filled map **Marks** type is chosen, Tableau is not able to fill the map because the outlines of those areas are unknown to Tableau. Furthermore, special mapping needs may arise that require polygons to be drawn for areas that are not typically included on maps. For example, an organization may define sales regions that don’t follow the usual map boundaries. Or a Tableau author may import an image of a basketball court or football pitch into Tableau and draw polygons to represent particular parts of the playing area. An alternative, which we discussed in more detail in _Chapter 8, Going Beyond the Basics_, is mapping bookshelves in a store.

But other than drawing polygons yourself, there are also file types that support drawing polygons. One of those file types is .shp. In the next exercise, we will make use of such a file and create polygons for Texas.

By processing an example of the aforementioned .shp file type, Tableau can create polygons. Let’s try this out and show the population of Texas as polygons on a map of Texas:

- 1. Open the **Texas Department of Transportation** page: https://gis-txdot.opendata.arcgis.com/datasets/txdot-city-boundaries/explore?location=30.990445%2C-100.168292%2C6.68

- 2. Use the **Download** dropdown and download **Shapefile**

![](img/66cf6b89349fda3581a4f196cb3c98a4_392_0.png)

Figure 9.10: Texas data

In your download directory, you should find multiple files:

![](img/66cf6b89349fda3581a4f196cb3c98a4_392_1.png)

Figure 9.11: Texas city boundaries

If you were to open a .shp file in a text editor, you would see the following:

![](img/66cf6b89349fda3581a4f196cb3c98a4_393_0.png)

Figure 9.12: .shp in a text editor

- 3. Luckily, Tableau has native capabilities to read .shp files. In the workbook associated with this chapter, navigate to the City Limits worksheet and select Data | New Data Source | Text File.

- 4. Navigate to your downloads directory or the path where you saved the Texas shapefiles and select the .shp file.

![](img/66cf6b89349fda3581a4f196cb3c98a4_393_1.png)

Figure 9.13: Loading a spatial file

- 5. In the next screenshot, you can see how Tableau was able to read the .shp file and how it created an additional field called **Geometry** that indicates that we are looking at polygons and multipolygons in this file.

![](img/66cf6b89349fda3581a4f196cb3c98a4_394_0.png)

Figure 9.14: Geometry

- 6. Open a new worksheet and drag **Geometry** onto the empty canvas.

![](img/66cf6b89349fda3581a4f196cb3c98a4_394_1.png)

Figure 9.15: Adding geometry
