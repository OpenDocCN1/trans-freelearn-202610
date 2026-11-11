```
'OpenLayers WMS',
'http://vmap0.tiles.osgeo.org/wms/vmap0',
{layers: 'basic,clabel,ctylabel,statelabel', transparent: true},
{isBaseLayer: false,
opacity: .7}
);
```

5. Now let's add a Vector layer. It will be in the EPSG:900913 projection as well, since it inherits the projection from the map object. We could, however, specify a different projection for the vector layer and re-project vector data on the fly (we can't do this on raster layers, like the WMS layer)—but we'll save this topic for Chapter 9, our Vector layer chapter. Create the vector layer object:

```
var vector_layer = new OpenLayers.Layer.Vector(
'Editable Vectors');
```

6. We'll add an EditingToolbar control now, which lets us add points and polygons to a vector layer via a toolbar that will appear in the upper right corner of the map (by default). When creating the control object, we simply pass in the vector layer object we wish the control to use:

```
//Add a vector editing control
map.addControl(new
OpenLayers.Control.EditingToolbar(vector_layer));
```

7. Finally, we just need to add the layers to the map.

```
map.addLayers([google_streets, wms_layer, vector_layer]);
```