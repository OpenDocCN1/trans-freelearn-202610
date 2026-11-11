# Params

Params is the third parameter, and it is an anonymous objector consisting of key:value pairs. This parameter specifies server side settings that affect the map image, which the WMS server returns. The key:value pairs you pass in here will be appended (more or less) to the URL that OpenLayers generates when it makes requests to the map server.

For example, in the previous example when creating the `wms_layer_labels` we passed in `{layers: 'clabel,ctylabel,statelabel'}`. In this case, we are asking the WMS server to give us back a map image with the server side layers called `'clabel,ctylabel,statelabel'` turned on. We specify multiple server side layers—it doesn't matter what or how many server side layers we request though, because the WMS Layer object on the client side is still considered by OpenLayers to be a single layer object. This params parameter is present in most Layer classes.

# Possible params keys and values

The possible keys and values for this params object depend on the map server you are working with. Unfortunately, covering them all for all layer types is outside the scope of this book. We'll only be using a few WMS parameters throughout this book—layers (to specify what layers the WMS service should give us), transparent (to ask for transparent images, for things such as label layers), and srs (to specify the projection).

For now, at least, our main concerns are just figuring out what layer names are on the WMS server. To figure this out, you can issue a GETCAPABILITIES request in the URL (SERVICE=WMS must also be specified). For instance, to get the possible layers from the WMS service we've been using so far go to the following URL:

```
http://vmap0.tiles.osgeo.org/wms/vmap0?SERVICE=WMS&REQUEST=GETCAPABILITIES
```

If you are interested in more information about WMS, the specifications can be found at

```
http://www.opengeospatial.org/standards/wms.
```

# Options

Options is the last parameter and is an anonymous object that specifies the layer object's settings. You are not required to pass in this parameter.

The options object contains properties for the client side OpenLayers Layer object. These are the settings for the layer object itself, so all Layer classes have this parameter. To define various properties of the OpenLayers Layer object, we use this options argument. Properties include settings such as isBaseLayer, opacity, and visibility. Since the layer properties are client side settings, the WMS server (or whichever map server the layer uses) doesn't know about them.