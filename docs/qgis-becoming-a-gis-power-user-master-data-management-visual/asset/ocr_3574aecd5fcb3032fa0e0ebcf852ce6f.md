# # Working offline

The Internet is an awesome resource, but sometimes you just don't have access to it. For field work, in places with intermittent services, on an airplane, or even in a meeting room, you just might not be able to access all the stuff that you need. By stuff, we're referring to documentation (user and developer), but more importantly database layers (for example, PostGIS) and web service-based layers (for example, WMS, WFS, the OpenLayers plugin, and so on).

This recipe is about caching local copies of the files that you need on your computer before you leave for an unconnected place.

# ## Getting ready

For this recipe, you will need to open a PostGIS database or WFS and enable the **Offline Editing** plugin that ships with QGIS.

# ## How to do it...

+   1. Load your layer from PostGIS or WFS.
+   2. Make sure to activate the **Offline Editing** option.
+   3. In the **Database** menu, you should see **Offline Editing**, choose **Convert to offline project**:

![](bbox=[0.342, 0.541, 0.657, 0.605])

+   4. Choose the local file to use to store the data.
+   5. Then, select all of the layers to convert (vector layers only):

![](bbox=[0.353, 0.674, 0.648, 0.83])