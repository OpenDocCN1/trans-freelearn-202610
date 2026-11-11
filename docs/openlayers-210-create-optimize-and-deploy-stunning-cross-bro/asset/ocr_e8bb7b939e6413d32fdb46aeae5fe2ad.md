# Layer.ArcIMS

This layer class is similar to the ArcGIS93Rest class, and allows us to display data from ArcIMS Mapping Services. ArcIMS is another proprietary software product from ESRI. The documentation for this Layer class is at http://dev.openlayers.org/docs/files/OpenLayers/Layer/ArcIMS-js.html. More information about ArcIMS can be found at http://www.esri.com/software/arcgis/arcims/index.html.

# Layer.Google

This layer allows us to interact with Google Maps via the Google Maps API. The documentation for this layer class is located at http://dev.openlayers.org/docs/files/OpenLayers/Layer/Google-js.html.

# Time for Action – creating a Google Maps layer

Let's take a look at how to create a Google Maps layer.

+   1. From this point onward, I'm going to assume that when I say "create a new map" you will use the template from http://vasir.net/openlayers_book/, or create a copy of the example from Chapter 1 and remove the WMS layer calls (they both result in the same code). So, create a new map.
+   
+   2. The first thing we'll need to do is add in a script tag to the <head> section that references the Google API library. This allows us to actually use the Google Maps library:
+   ```html
+   <script src="http://maps.google.com/maps/api/js?sensor=false"></script>
+   ```
+   
+   3. Now, we'll need to create a Google Maps layer object. We won't pass in any additional settings, so the default settings will be used. By default, we'll see the Google 'Streets' Map (we could ask for the aerial imagery layer, the physical topology layer, etc—we'll cover this soon). Add the following in your init function:
+   ```javascript
+   var google_map_layer = new OpenLayers.Layer.Google(
+     'Google Map Layer',
+     {}
+   );
+   ```
+   
+   4. All we have to do now is add the layer to the map. Before you do this, make sure you've removed the existing line of code that adds the WMS layer to the map. We only want to have our map contain the google_map_layer.
+   ```javascript
+   map.addLayer(google_map_layer);
+   ```