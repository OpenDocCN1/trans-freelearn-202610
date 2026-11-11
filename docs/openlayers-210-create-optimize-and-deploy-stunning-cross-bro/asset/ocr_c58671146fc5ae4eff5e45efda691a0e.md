# Time for Action – using attributions

Let's take a look at the attribution control in action.

+   1. To make the **Attribution** control useful, we must specify an `attribution` property on our layer objects. So, let's create a WMS layer with this property set to `'Base WMS Layer'`:
+   ```js
+   var wms_base = new OpenLayers.Layer.WMS(
+     'OpenLayers WMS',
+     'http://vmap0.tiles.osgeo.org/wms/vmap0',
+     {layers: 'basic'},
+     {attribution: 'Base WMS layer'}
+   );
+   ```
+   2. Now, let's add another layer and set the `attribution` text to `'State Boundary'`.
+   ```js
+   var wms_state_lines = new OpenLayers.Layer.WMS(
+     'State Line Layer',
+     'http://vmap0.tiles.osgeo.org/wms/vmap0',
+     {layers: 'stateboundary'},
+     {attribution: 'State Boundary', isBaseLayer: false,
+      opacity: .2}
+   );
+   ```
+   3. Finally, add the layers to the map:
+   ```js
+   map.addLayers([wms_base, wms_state_lines]);
+   ```
+   4. You should see something like the following, with the layer attribution text at the bottom right hand of the map:

![](bbox=[0.32, 0.6, 0.68, 0.82])