+   2. Highlight the c7 SQLite database.

+   3. Execute the following query, ensuring that **Load as new layer** is selected:

```
SELECT * FROM c7_polygons WHERE building = 'yes' and amenity = 'university'
```

![](bbox=[0.17, 0.25, 0.83, 0.63])

+   5. Export the query layer to c7/data/original/delaware-latest-3875/buildings.shp with the EPSG:3857 projection.

# To tile and use UTFGrid with TileMill

Although TileMill is no longer under active production by its creator Mapbox, it is still useful for us to produce MBTiles tiled images rendered by Mapnik using CartoCSS and a UTFGrid interaction layer.