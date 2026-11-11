You'll notice that each type is preceded by `google.maps.MapTypeID.`, which must be present. For example, to specify that you wish to use the hybrid layer (satellite imagery combined with the `ROADMAP` layer), you could use:

```
var google_hybrid_layer = new OpenLayers.Layer.Google(
    "Google Hybrid",
    {type: google.maps.MapTypeId.HYBRID}
);
```

Because of the projections Google Maps and the other third party APIs use, it can be little tricky to get other layers to work with them—we'll talk about this more in Chapter 4 and Chapter 5. Briefly though, it has to do with the fact that these third party APIs use a projection called `EPSG:900913`, which uses a different coordinate system than `EPSG:4326` (the projection our WMS layers have been in so far). Because of this, the coordinates don't match up. So, we can't just place our WMS layer (which is in the `EPSG:4326` projection) on top of the Google Layer (which is in the `EPSG:900913` projection), as the coordinates are very different. In the next chapters, we'll address how to solve this issue.

![](bbox=[0.2, 0.42, 0.26, 0.48])

> The Google Maps Version 3 API can be found at http://code.google.com/apis/maps/documentation/javascript/ and more information on possible Google Maps Layer types can be found at http://code.google.com/apis/maps/documentation/javascript/overlays.html.

# Layer.Grid

This is a base class that many other layers, such as WMS, inherit from. We won't be using it explicitly, but it is important to know that it exists. It works, basically, by constructing a grid and placing tiles (map images) inside the grid. It uses the HTTPRequest class to communicate with a map server to get the tiles it needs to build the map.

Classes that inherit from it, such as WMS, work in a similar way. So, if you're familiar with how to use the WMS layer, you'll be familiar with how to use nearly any class that inherits from the `Layer.Grid` class. The documentation for this class is at http://dev.openlayers.org/docs/files/OpenLayers/Layer/Grid-js.html.