
ArcGIS is available as a desktop application or as a cloud service. You can sign up for a 60-day free trial (https://www.arcgis.com/features/free-trial.html). You can use ArcGIS to do many different kinds of analytics, including geoprocessing your own custom shapefile. The following example shows how ArcGIS can be used to define polygon areas within a 4-minute drive time of a set of location points on a typical Friday afternoon at 5 pm:

![](img/9f2a55db1236f995dcd9392acbc82910_268_0.png)

Drive time area analysis example. Source: ArcGIS

## QGIS

QGIS is open source and very powerful desktop GIS software. It is similar to ArcGIS but not to the full scope of capability as the paid ESRI software. But the price is right, and it still has a wide variety of capabilities. It can also be manipulated with Python code. There is a vast trove of documentation on it and many useful books on how to use it.

You can download and install QGIS from the project site (http://www.qgis.org/en/site/forusers/download.html). If you are unable to get ArcGIS, make sure to keep QGIS handy. QGIS was used to create many of the images in this book. You can use both QGIS and ArcGIS to connect directly to geospatial databases, such as PostGIS and Oracle. The following image shows an example of what can be created with QGIS:

![](img/9f2a55db1236f995dcd9392acbc82910_269_0.png)

QGIS example screenshot. Source: anitagraser.com

## ogr2ogr

ogr2ogr is part of the GDAL library. It is a command-line tool used to convert files from one OpenGIS Simple Features Reference Implementation (OGR) spatial format to another. It is a no-frills tool but is used heavily by geospatial analysts. The general format for a command-line conversion follows the following example:

```
ogr2ogr -f "file_format" destination_data source_data
```

You can use it to convert PostGIS data into shapefiles or load shapefiles into PostGIS for example. It supports conversion into over 90 file formats for vector data alone.

## PostGIS spatial functions

PostGIS contains dozens of spatial functions that can be referenced in a standard SQL query. The following table provides an overview of some commonly used functions:

| Function | Description |
| --- | --- |
| `ST_GeomFromText` | This returns a specified ST_Geometry value from WKT representation. |
| `ST_GeomFromWKB` | This creates a geometry instance from a WKB geometry representation and optional SRID. |
| `ST_Buffer` | This returns a geometry covering all points within a given distance from the input geometry. |
| `ST_ConvexHull` | The convex hull of a geometry represents the minimum convex geometry that encloses all geometries within the set. |
| `ST_Intersection` | This returns a geometry that represents the shared portion of geomA and geomB. |
| `ST_Simplify` | This returns a simplified version of the given geometry using the **Douglas-Peucker** algorithm. |
| `ST_Boundary` | This returns the closure of the combinatorial boundary of this geometry. |
| `ST_Transform` | This returns a new geometry with its coordinates transformed to a different spatial reference. |
| `ST_Centroid` | This returns the geometric center of a geometry. |
| `ST_ClosestPoint` | This returns the 2-dimensional point on g1 that is closest to g2. This is the first point of the shortest line. |
| `ST_Contains` | This returns true if and only if no points of B lie in the exterior of A, and at least one point of the interior of B lies in the interior of A. |
| `ST_Covers` | This returns 1 (TRUE) if no point in geometry B is outside geometry A. |
| `ST_Crosses` | This returns TRUE if the supplied geometries have some, but not all, interior points in common. |

AWS also has a managed petabyte-scale data warehouse service called **Redshift**. This was introduced in `Chapter 3, IoT Analytics for the Cloud`. Redshift does not support geometry fields directly but does support Python UDFs. You can create UDFs using Python code and the *shapely* package, then call them from Redshift SQL statements. A similar strategy can be used for both Hive and Spark.

ESRI supports an open source project called **GP tools for AWS** that allows ArcGIS users to connect to Amazon EMR and S3 data sources. The project is hosted on GitHub (`https://github.com/Esri/gptools-for-aws`).

## Solving the pollution reporting problem

From what you have learned in this chapter, you can now solve the IoT pollution sensor data by congressional districts problem introduced earlier. Follow these general steps using either Python code or spatial query functions in a database such as PostGIS:

- Download a shapefile for U.S. Interstates such as the U.S. National Transportation Atlas Interstate Highways shapefile available from the University of Iowa (`ftp://ftp.igsb.uiowa.edu/gis_library/USA/us_interstates.htm`).
- Download a shapefile for US congressional districts such as the TIGER/Line Shapefile available from the US Census (`https://www.census.gov/cgi-bin/geo/shapefiles/index.php?year=2016&layergroup=Congressional+Districts+%282815%29`).
- Load the shapefiles into a geospatial database using `ogr2ogr` or into Python using the *fiona* package.
- Add a 1 km buffer to the Interstates MultiLineString using the *shapely* package or `ST_Buffer` in PostGIS.
- Use a mapping API such as Google Maps to geocode each device address in order to get the latitude and longitude point pairs. This was discussed in `Chapter 7, Decorating Your Data - Adding External Datasets to Innovate.`
- Load the IoT device locations as Point geometries.
- Optionally, apply R-tree indexes to the points and the congressional districts polygons to speed up calculations.
- Find Points contained within the buffered Interstates roadways polygon. The `contains` function would be one way to do this. Tag those points as within the bounds. Tag the remaining points as outside the range so they can be used as comparisons.
- For each point, search for the congressional district polygon that contains it and track the results. This would be a great place to leverage the spatial indexing created earlier.
- Calculate the pollution values for each device and combine with the congressional district and the in/out of Interstates buffer tags.
- Pivot and compare the data to compile the report.
- Go ask for a raise, you have earned it!
- You can also use the erosion option in the `buffer` function to create Interstate polygons that identify several distance ranges. Start with the largest range as the buffer, then erode your way back down for each range level. Create a buffered polygon for each range. Search the smallest buffered polygon first, the next biggest, then the next, and so on. The points that are not in any of those are outside the range.
- Go ask for another raise!

## Summary

In this chapter, you learned about how to use geospatial analytics to find insights and answer complex questions about IoT data. The importance of geospatial analysis for geographically distributed IoT devices was discussed. The concept of CRS was introduced along with haversine distance and its limitations.

The world is not a perfect sphere. Methods to adjust for that in order to accurately measure distance was covered. Python functions for geospatial analytics, such as `buffer` and `contains`, were discussed, along with some examples.

Storing and processing geospatial data requires some specialized handling. Some geospatial databases and GIS software tools were reviewed. PostGIS spatial functions were also reviewed. We went over some tips for leveraging geospatial analytics in a big data world.

Geospatial analytics offers a huge opportunity to analyze IoT data in new and innovative ways. It can help discover patterns in noisy data. New services can then be created as another way to extract value from your data.

## Data Science for IoT Analytics

"Revenues are up 5% due to your little geospatial search trick," the VP of Connected Services says, "You know your former boss's position is still open. Maybe we should fill it from the inside..."

Your pulse quickens, you were hoping he might come to this conclusion. You deserve a promotion after what your analytics has brought to the company. You now have one person working for you focusing on geospatial analysis. You can just imagine what you could do with a whole team.

"There is something that we have been toying around with though," he continues, "With all this data we are collecting, we should be able to tap into machine learning models to predict equipment failures. Some think we should be hiring an outside consulting company to handle all of it. Sounds expensive to me. I sure wish we could coordinate this ourselves, work with data scientists of our own choosing... know anyone that might be up to it?"

He winks and walks off, hands behind his back, whistling a Brahms tune.

In this chapter, we introduce data science techniques such as machine learning, deep learning, and forecasting using ARIMA. Special focus is given on how to use these methods with IoT data. The core concepts for each will be reviewed along with examples in R. Deep learning will be described along with where to go to create an Amazon EC2 instance with TensorFlow.

This chapter covers a lot of ground, so hold onto your hats.
