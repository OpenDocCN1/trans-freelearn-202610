# Time for Action – configuring the options parameter

The options parameter is something that is present in all Layer classes, so let's get a bit more familiar with it. I suggest opening up the Layer documentation and following along, and even trying to add in the options yourself from the possible list of layer properties (at http://dev.openlayers.org/docs/files/OpenLayers/Layer-js.html).

+   1. Use the template (or code from Chapter 1 with the WMS layer code removed). We'll be adding some WMS layers, and we'll refer to this file as `chapter_3_ex2_options_config.html`.
+   
+   2. First we'll add in a layer that contains the 'basic' layer from the WMS server, like in the previous example.
+   
+   3. Next we'll create another layer object. We'll create a layer using labels, like in the previous example. This time though, let's set the layer's options to include `visibility: false`. This will cause the layer to be hidden by default. The layer definition should now look like:
+   
+   4. Time to create another layer. We'll set the layer params to ask the WMS service for the stateboundary layer. Then, we'll specify the options so that it won't display in the layer switcher control (via `displayInLayerSwitcher: false`) and set a minimum scale at which it will be visible (via `minScale`). That means this layer will only show up once we've reached a certain scale. Add the following to your `init()` function.

```
// Setup our two layer objects
var wms_layer_map = new OpenLayers.Layer.WMS(
  'Base layer',
  'http://vmap0.tiles.osgeo.org/wms/vmap0',
  {layers: 'basic'},
  {isBaseLayer: true}
);
```

```
var wms_layer_labels = new OpenLayers.Layer.WMS(
  'Location Labels',
  'http://vmap0.tiles.osgeo.org/wms/vmap0',
  {layers: 'clabel,ctylabel,statelabel',
  transparent: true},
  {visibility: false, opacity:0.5}
);
```

```
var wms_state_lines = new OpenLayers.Layer.WMS(
  'State Line Layer',
  'http://labs.metacarta.com/wms/vmap0',
  {layers: 'stateboundary',
```