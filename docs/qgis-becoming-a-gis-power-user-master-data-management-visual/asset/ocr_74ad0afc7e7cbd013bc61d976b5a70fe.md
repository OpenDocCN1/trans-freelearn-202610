This technique is not limited to just tile services. Many other formats that GDAL works with can be wrapped for easier usage in QGIS. This is a similar method to **Virtual Raster Tables (VRT)** layers mentioned in the *Creating raster overviews (pyramids)* recipe in *Chapter 2, Data Management.*

Lastly, you may ask why a new plugin using this method doesn't replace the OpenLayers plugin. Such an idea has been under discussion for a while; the key sticking point is that accessing some layers, such as Google, Bing, and so on, with this method may violate the Terms of Service as they do not keep the Copyright, Trademark, and Logo in the correct place. Also, caching and printing such layers may not be legal. In general, avoid using proprietary data when possible to reduce licensing issues.

# See also

+   - This recipe and method has actually been known and discussed in many QGIS venues. The most frequently cited example is available at http://www.3liz.com/blog/rldhont/index.php?post/2012/07/17/OpenStreetMap-Tiles-in-QGIS.
+   - The full explanation of options for GDAL can be found at http://www.gdal.org/frmt_wms.html.

# Serving web maps with the QGIS server

QGIS and the Web is not all about consuming data, it can also be used to serve data over the Web for others to view online or consume in other web clients (such as QGIS). Keep in mind that setting up your own web service is not the easiest way to make a web map (refer to the *Hooking up web clients* recipe in this chapter). This is, however, a great way to transition all the hard work that you've put into a QGIS project file into something other people can see and use.

# Getting ready

For this recipe, you need a working installation of the QGIS server. This involves running a standard web server (such as Apache or Nginx). There are many ways to set up the server, so please see the official documentation at http://hub.qgis.org/projects/quantum-gis/wiki/QGIS_Server_Tutorial.

Once you have the QGIS server running, then you just need a QGIS project with the configuration outlined in this recipe.