+   - extent: {OpenLayers.Bounds} The extent of the image.
+   - size: {OpenLayers.Size} The size of the image, in pixels.
+   - options: {Object} An anonymous object containing the layer property settings.

Let's take a look at our image_layer object's extent, size, and options arguments:

```
new OpenLayers.Bounds(-180,-112.5,180,112.5),
new OpenLayers.Size(1920,1200),
{numZoomLevels:7, maxResolution:.625}
```

The extent argument is an {OpenLayers.Bounds} object, which is created via OpenLayers.Bounds(-180,-112.5,180,112.5). We pass in the min x, min y, max x, and max y values to create the bounding box. For images, you may have to play around with the values depending on your image's dimensions. The ratio of our image (1920/1200) is 1.6, so I've set the bounds to have a similar ratio (180 / 112.5 = 1.6).

The size argument, new OpenLayers.Size(1920,1200), is an {OpenLayers.Size} object, and we pass in the width and height of the image.

For the next argument, the options, we pass in {numZoomLevels:7, maxResolution:.625}. The numZoomLevels property can be anything you'd like, but higher values may allow you to zoom in too far to the image. The value of maxResolution property is set to .625, which is a value I arrived at by dividing 1200/1920 (the width divided by the height)—just a rough calculation that, in case, makes the map look just about right. Documentation for this layer class is at http://dev.openlayers.org/docs/files/OpenLayers/Layer/Image-js.html.

# Have a Go Hero – make your own image based maps

Using the previous example's code, find other high resolution images and create a similar 'image viewer' with them. Add as many layers as you'd like, and play around with the opacity setting to see how the image layers interact with each other.

# Layer.MapGuide

This is the layer for the open source MapGuide platform. We won't be covering MapGuide in this book, but it is a popular project and, since it is free and open source, you're welcome to install it on your own computer. It behaves similar to the Layer.WMS class and has the same parameters. It also inherits from Layer.Grid. The documentation for this layer class can be found at http://dev.openlayers.org/docs/files/OpenLayers/Layer/MapGuide-js.html. More information on MapGuide can be found at http://mapguide.osgeo.org/.