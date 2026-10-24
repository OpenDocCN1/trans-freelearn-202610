# Converting lines to polygons

Run the following query:

```
CREATE VIEW line2poly AS
SELECT id,stid,ST_MakePolygon(geom) as geom
FROM pts2line;
```

![](bbox=[0.257, 0.253, 0.326, 0.309])

> Want to go straight from point to polygons? Try ST_MakePolygon(ST_MakeLine(geom)); the rest is as shown in the first example: points to lines query.

# Converting lines or polygons to points

Run the following query:

```
CREATE VIEW pts AS
SELECT ROW_NUMBER() over (order by a.id_0) as id,id_0 as
grid,(a.a_geom).path[2] as path,
ST_GeometryType((a.a_geom).geom), ((a.a_geom).geom) as geom
FROM (SELECT id_0, (ST_DumpPoints(geom)) as a_geom FROM
"census_wake2000") as a;
```

![](bbox=[0.267, 0.51, 0.314, 0.565])

> What's ROW_NUMBER() about? This is a trick to ensure a unique integer for each row. Some tools complain if you don't have this; for example, DB Manager won't preview or load the layer, even though direct loading in QGIS works fine.

# How it works...

Based on the common identifier specified in GROUP BY, the SQL statement aggregates multiple points into a new geometry of the specified type.

When dumping geometries to points, PostGIS actually dumps an array, including ID information. This is why the example query is actually a nested set of queries. The first is to dump the array of geometry information, and the second to extract the relevant parts of the results in the format that we want them in.