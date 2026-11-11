# Multiple maps and custom events

Let's create a slightly more complicated application using multiple maps and adding event listeners. One map will show a zoomed out view of a country, and the map next to it will show a close up view. Whenever one map is moved, the other map's center point will be immediately updated. This type of mapping application can be quite useful for things such as data entry processes, where two large maps might be more usable than one large map with an overview map.

Let's put together some of the concepts we've learned in this chapter and create a similar type of application.

# Time for Action – creating a multiple map and custom event application

In this example, we'll make an application that contains two maps, side by side, that update one another when moved.

+   1. Create a copy of the previous example. We'll be working on it as the basis for this example.
+   
+   2. Let's recreate the first map objects. For this example, we'll make the first map zoomed in a bit and set a maxResolution so the map cannot be zoomed out more than a few times. We'll also use two control—Navigation and PanZoomBar.
+   
+   3. Let's also change the center point. We'll create a variable to store the starting center coordinate (which will also be used by the second map) and change the map.zoomToMaxExtent line. After the layer has been added to the map, use the following code:

```
map_1 = new OpenLayers.Map('map_1_element', {
    controls: [
        new OpenLayers.Control.Navigation(),
        new OpenLayers.Control.PanZoomBar()
    ],
    maxResolution:0.0054931640625,
    minResolution:0.00034332275390625
});
```

```
var map_center = new OpenLayers.LonLat(-120, 34);
if (!map_1.getCenter()) {
    map_1.setCenter(map_center);
}
```