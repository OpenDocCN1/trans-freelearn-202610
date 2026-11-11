
```
allOverlays: true,
    controls: [new OpenLayers.Control.Navigation(),
new OpenLayers.Control.PanZoom(),
new OpenLayers.Control.LayerSwitcher()]
});
```

2. Now, let's create a couple of layers so we can see how the allOverlays property works. We'll assume that you will add WMS layers to your maps for future examples.

```
var wms_layer_all = new OpenLayers.Layer.WMS(
    'OpenLayers WMS',
    'http://vmap0.tiles.osgeo.org/wms/vmap0',
    {layers: 'basic'},
    {}
);
```

```
var wms_layer_labels = new OpenLayers.Layer.WMS(
    'Labels',
    'http://vmap0.tiles.osgeo.org/wms/vmap0',
    {layers: 'clabel,ctylabel,statelabel',
transparent:true},
    {}
);
```

```
map.addLayers([wms_layer_all, wms_layer_labels]);
```

3. Since we didn't specify a div when instantiating our map object, we'll have to call the render() function to place the map inside a div. The function takes in either an HTML element or the string of the element's ID, so let's pass in the element's ID.

```
map.render('map_element');
```

4. Finally, check to see if a center is set. If it is not, zoom to the max extent. We'll assume you do this step in future examples. You can place the call to the render() function after this bit if you'd like, and it will still zoom to the center or the max extent—the order in which you call render() and set the map's location is up to you.

```
if (!map.getCenter()) {
    map.zoomToMaxExtent();
}
```

5. Take a look at the map and open the layer switcher—you should be able to turn off both layers and see an empty map now. Because we don't have a base layer and all the Overlayers are disabled by default, you'll just see a blank map until you enable a layer.

![](img/1b8cd151c9f278555f9cf255b20d244e_214_0.png)

## What Just Happened?

We have just looked at a few of the properties we brought up—`allOverlays`, `controls`, and (leaving out) the `div` property. We also saw how the user can potentially see no map at all by turning off all the layers.

The map will not automatically render, because we did not pass in an HTML element. Instead, we called the `render()` method and passed in an HTML element after our map object was created. There may be times when you wish to delay rendering the map, such as waiting for a user to click a button, and in those cases this is a good way to do it. We'll talk more about map methods later in this chapter, but let's get back to discussing map properties.

### eventListeners

`eventListeners: {Object}`

This property accepts an anonymous object containing key:value pairs of event types (as the key) and functions to call (the values) when those events are fired. We saw this property briefly in Chapter 6, and it behaves the same way here. An example would be:

```
var map = new OpenLayers.Map('map_element', {
    eventListeners: { 'moveend': my_moveend_function }
});
```

The above code assumes that a function named `my_moveend_function` exists. We will cover this property in much more detail, along with the possible event types, later in this chapter in the section on events.

### fallThrough

```
fallThrough: {Boolean} Default: true
```

The fallThrough property determines whether or not OpenLayers will allow events on the map to propagate downwards (i.e., 'fall through') to other elements on your page.

Let's go over the meaning with an example. Say your map_element div has an event attached to it that will show an alert when you click on it. This event is outside of OpenLayers. Now, let's assume you create a map object and attach it to that map_element div. If fallThrough is set to true (it is by default), you will receive the original alert by clicking on the map—the mouse click event will be allowed to 'fall through'. If it is set to false, you will not receive the alert, as OpenLayers will consume that mouse click event.

### layers

```
layers: {Array {OpenLayers.Layer}}
```

We've encountered this property, to some degree, already. After the map is instantiated, we can access its layers by calling map.layers. So far, we've been creating our map object, then our layer objects, then adding the layers to the map with addLayer(). We can, however, use the layers property when instantiating our map and pass in an array of layer objects.

Passing in layers using this property when we create our map object or calling addLayers essentially does the same thing—use whichever method you're most comfortable with. An example would be:

```
var map = new OpenLayers.Map('map_element', {
    layers: [my_layer_1, new OpenLayers.Layer.WMS({ ... })]
});
```

You can pass in layer objects that have already been created (e.g., my_layer_1, assuming a layer object with that name has been created), or by instantiating a layer object in the call itself (e.g., new OpenLayers.Layer.WMS({...})).

![](img/1b8cd151c9f278555f9cf255b20d244e_215_0.png)

> In the example above, the ... would be replaced with the options for the WMS layer.

### maxExtent

maxExtent: {OpenLayers.Bounds} Default: bounds in decimal degrees, (-180, -90, 180, 90)

Setting this will specify the maximum extent of your map. Tiles that fall outside of the maximum extent will not be requested, nor can users pan to a coordinate that lies outside the maxExtent. If you are using a different projection than the default projection, you will need to change this to reflect the world's coordinates in your desired projection.

The maxExtent property's data type is an OpenLayers.Bounds object. The OpenLayers.Bounds class is used to create a 'bounds' object, which contains four coordinates that make up a bounding box. The four coordinates are minimum x (left), minimum y (bottom), maximum x (right), and maximum y (top). To instantiate a bounds object, you just pass in the coordinates in the order of:

```
var bounds_object = new OpenLayers.Bounds(minx, miny, maxx, maxy);
```

### minExtent

minExtent: {OpenLayers.Bounds}

Setting the minExtent will specify the minimum extent of the map. There is no default value for this property. You will usually not need to set this, unless you are specifying custom resolutions using maxResolution—in such case, you will usually set the values to (-1, -1, 1, 1).

### restrictedExtent

restrictedExtent: {OpenLayers.Bounds}

This property specifies the bounds the user can navigate in—meaning that the user will be able to only pan around inside the bounds specified by the restrictedExtent property (if it is given, and if restricting their navigation is possible).

Setting this property will still allow the user to zoom out and see parts of the map that are outside the restricted extent, but the map's center will be set to the restricted extent. If you want to limit the zoom level, you could set the resolutions array, maxResolution, or scales properties (covered in the following pages). This property is similar to maxExtent, however, tiles outside the restrictedExtent will still be requested, but the user won't necessarily be able to pan to them.

### numZoomLevels

numZoomLevels: {Integer} Default: 16

The numZoomLevels property specifies the amount of possible zoom levels that your map will have. For many applications, giving users 16 zoom levels is not always desirable. In most cases, just specifying the numZoomLevels (along with maxExtent, if using a different projection) is all you need to do to set the number of zoom levels. We'll see, as we look at other properties, more ways to specify the number of zoom levels (some without using this property at all). However, let's look at how to use this property to set the number of zoom levels—it's pretty easy!

## Time for Action – setting zoom levels and maxExtent

1. Create your map object, by specifying the numZoomLevels and adding a PanZoomBar control. We'll also set the numZoomLevels property to 8, meaning that only eight zoom levels will be available. Lastly, we'll set the maxExtent property to include just a subset of the world, like the following:

```
map = new OpenLayers.Map('map_element', {
    controls: [
        new OpenLayers.Control.Navigation(),
        new OpenLayers.Control.PanZoomBar(),
        new OpenLayers.Control.LayerSwitcher()
    ],
    numZoomLevels: 8,
    maxExtent: new OpenLayers.Bounds(-100, -30, 40, 30)
});
```

2. Open up the page, and you should see something like this:

![](img/1b8cd151c9f278555f9cf255b20d244e_217_0.png)
