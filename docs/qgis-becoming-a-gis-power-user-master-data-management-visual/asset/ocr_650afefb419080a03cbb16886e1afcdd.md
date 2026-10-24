# Getting ready

You need to load a vector layer of points with a numeric ID indicating order, and an identifier of unique lines or polygons that is shared between points of the same geometry. For example, you can use `census_wake_2000_points` loaded into SpatiaLite with the geometry field called geom.

# How to do it...

Using **DB Manager Plugin** (comes with QGIS and is in the **Database** menu), the **QspatiaLite** plugin, or an alternate SpatiaLite SQL application (command line or GUI), the following SQL examples will perform the conversions between vector types.

# Points to lines

+   1. Create a table with points grouped by common ID:

```
––Create table grouping points with shared stfid into lines
CREATE Table census_pts2lines AS
SELECT stfid,Make_Line(geom) as geom
FROM census_wake_2000_points
GROUP BY stfid;
```

The following screenshot shows what the screen will look like:

![](bbox=[0.176, 0.517, 0.822, 0.818])