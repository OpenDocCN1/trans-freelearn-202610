![](bbox=[0.175, 0.145, 0.825, 0.542])

# Geocode

If our data is expressed as addresses, intersections, or other well-known places, we can geocode it (that is, match it with coordinates) with a local or remote geocoder configured for our particular set of fields, such as the standard fields in an address.

In this example, we will geocode it using the remote geocoder provided by Google. Perform the following steps:

+   1. Install the MMQGIS plugin.
+   2. If you don't already have some address data to work with, you can make up a delimited file that contains some standard address fields, such as street, city, state, and county (ZIP code is not used by this plugin). The data that I'm using comes from New Castle County, Delaware's GIS site (http://gis.nccde.org/gis_viewer/).