# maxExtent

maxExtent: {OpenLayers.Bounds} Default: bounds in decimal degrees, (-180, -90, 180, 90)

Setting this will specify the maximum extent of your map. Tiles that fall outside of the maximum extent will not be requested, nor can users pan to a coordinate that lies outside the maxExtent. If you are using a different projection than the default projection, you will need to change this to reflect the world's coordinates in your desired projection.

The maxExtent property's data type is an OpenLayers.Bounds object. The OpenLayers.Bounds class is used to create a 'bounds' object, which contains four coordinates that make up a bounding box. The four coordinates are minimum x (left), minimum y (bottom), maximum x (right), and maximum y (top). To instantiate a bounds object, you just pass in the coordinates in the order of:

```
var bounds_object = new OpenLayers.Bounds(minx, miny, maxx, maxy);
```

# minExtent

minExtent: {OpenLayers.Bounds}

Setting the minExtent will specify the minimum extent of the map. There is no default value for this property. You will usually not need to set this, unless you are specifying custom resolutions using maxResolution—in such case, you will usually set the values to (-1, -1, 1, 1).

# restrictedExtent

restrictedExtent: {OpenLayers.Bounds}

This property specifies the bounds the user can navigate in—meaning that the user will be able to only pan around inside the bounds specified by the restrictedExtent property (if it is given, and if restricting their navigation is possible).

Setting this property will still allow the user to zoom out and see parts of the map that are outside the restricted extent, but the map's center will be set to the restricted extent. If you want to limit the zoom level, you could set the resolutions array, maxResolution, or scales properties (covered in the following pages). This property is similar to maxExtent, however, tiles outside the restrictedExtent will still be requested, but the user won't necessarily be able to pan to them.

# numZoomLevels

numZoomLevels: {Integer} Default: 16