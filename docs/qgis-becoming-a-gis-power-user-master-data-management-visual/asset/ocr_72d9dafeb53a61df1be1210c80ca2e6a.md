4. Remove the `nz-coastlines-and-islands` layer from QGIS and add it again. Now, New Zealand and Chatham islands will be displayed correctly, as follows:

![](bbox=[0.25, 0.19, 0.75, 0.45])

It is worth mentioning that the `ST_Shift_Longitude` function is also available in PostGIS.

# How it works...

When we enable 'on the fly' CRS transformation, every geometry is reprojected to the given CRS (in our case Pacific centered) so that coordinates now have the same sign.

The `ogr2ogr` tool with the `-wrapdateline` option splits all geometries that cross the dateline and write them to the output file. Geometries that do not cross the dateline are copied without changes.

The `ST_Shift_Longitude` function translates negative longitudes by 360 and as a result, all the data will be in the range of 0-360 degrees and displayed correctly.

# See also

+   - http://docs.qgis.org/2.2/en/docs/user_manual/working_with_vector/supported_data.html#vector-layers-crossing-180-degrees-longitude