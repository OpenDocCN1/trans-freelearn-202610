

The other two attributes, horizontalSeparation and verticalSeparation, are used to specify the distance between the two nodes that are arranged horizontally and vertically.

The next step is to create shapes for the nodes in the DataSource object. In this example, we will use the shapeDefaults option to add shapes to the diagram, as shown in the following code snippet:

```
$("#chart").kendoDiagram({
.
.
shapeDefaults: {
visual: function (data) {
var group = new kendo.dataviz.diagram.Group();

//create a rectangle shape
group.append(new kendo.dataviz.diagram.Rectangle({
width: 130,
height: 75,
background: '#1696d3',
stroke: {
width: 0
}
})); //add text 'name' inside the rectangle
group.append(new kendo.dataviz.diagram.TextBlock({
text: data.dataItem.name,
x: 10,
y: 10,
color: '#fff'
})); //add text 'designation' inside the rectangle
group.append(new kendo.dataviz.diagram.TextBlock({
text: data.dataItem.designation,
x: 10,
y: 30,
color: '#fff'
}));
return group;
}
},
.
});
```

Here, the visual attribute’s value is a function that returns a visual element that needs to be rendered on the diagram. In our example, we will create a visual element that includes the name and designation properties from the model. The function first creates a group element by creating an instance of kendo.dataviz.diagram.Group. A Group attribute is an invisible visual element, which is used to contain and group other visual elements. We then create instances for kendo.dataviz.diagram.Rectangle and kendo.dataviz.diagram.TextBlock and append it to the group. The function then returns the group object. This function is invoked recursively and nodes are added to the diagram.

# How it works...

When the layout subtype is down, the root node is placed at the top and the child nodes are added, growing the tree downwards, as shown in the following screenshot:

![](img/2a66507445090790d34dc5588b56b24c_230_0.png)

When you change the subtype to left, the root node is placed on the right-hand side and the nodes are added such that the tree grows from right to left.

![](img/2a66507445090790d34dc5588b56b24c_230_1.png)

After changing the subtype to mindmapHorizontal, the root node is placed at the center and the child nodes are added on either side.

![](img/2a66507445090790d34dc5588b56b24c_231_0.png)

You can try changing the subtype to radial and tipOver to see the placement of nodes in the diagram.

# Creating a map to display geospatial information using an OpenStreet map

The library provides APIs that you can use to display a map of a particular location on the page. In this recipe, we will create a map by using the kendoMap function, providing some configuration details. We will use OpenStreet maps to fetch images for the map.

# How to do it...

To create a map, invoke the kendoMap function on the selected DOM node, providing the configuration options mentioned in the following code snippet:

```
$('#chart').kendoMap({
    center: [30.2681, -97.7448],
    zoom: 4,
    layers: [{
        type: 'tile',
        urlTemplate: "http://#= subdomain #.tile2.opencyclemap.org/transport/#= zoom #/#= x #/#= y #.png",
        subdomains: ['a', 'b', 'c'],
        attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap contributors</a>.' +
            'Tiles courtesy of <a href="http://www.opencyclemap.org/">Andy Allan</a>'
    }],
    ...
});
```

In the preceding code snippet, the center option is used to specify the coordinates of the map's center location. The latitude and longitude information, along with the provided map's width and height, is used to center the map in the container. The zoom option is used to set the zoom level of the map. The layers attribute is used to specify various layers that will be displayed on the map; in the previous example, the type of layer is specified as tile, which is used to display data from the OpenStreet map. The other possible values for type are bing, which is used to retrieve maps from Bing maps; marker, which is used to add markers to the map; and shape, which is used to display a vector-shape layer. These values are used when the data is bound to the geoJSON data. The urlTemplate attribute specifies the URL from which the map data has to be retrieved. It takes into account the zoom level set for the map and the x and y coordinates, that is, the latitude and longitude information specified in the center attribute and in the subdomains property. Notice that after the urlTemplate option, the subdomains property is specified. It is an array that mentions the subdomains that should be used to get map images. The browser has a limit on the number of images that can be retrieved in parallel per domain. When using subdomains, the images are fetched in parallel; hence, it reduces the time required to render the entire map. The attribution field is used to specify the attribution for the layer.

Let's now add some markers to the map. A marker is another layer type that can be added:

```
$('#chart').kendoMap({
.
.
layers: [{
type: 'tile',
.
.
}, {
type: 'marker',
locationField: 'latlng ',
tooltip: {
content: function (e) {
return e.sender.marker.dataItem.tooltip;
}
},
dataSource: {
data: [{
latlng: [40.6700, -73.9400],
tooltip: 'New York'
}, {
latlng: [34.0500, -118.2500],
tooltip: 'Los Angeles'
}]]
}});
```

# Kendo UI DataViz - Advance Charting

In the preceding code snippet, the type of layer is specified as `marker` and the `locationField` attribute specifies the value as `latlng`. This refers to the property in the `dataSource` option. The `dataSource` attribute specifies the marker details, that is, the latitude and longitude information where the marker must be added and the tooltip to be shown when the user hovers over the marker. The `tooltip` option contains a `content` attribute, whose value is a function that returns the tooltip information from the `dataSource` attribute.

# How it works...

When you render the page, a map is shown. The map data is fetched from the specified URL, that is, from OpenStreet maps using various subdomains.

![](img/2a66507445090790d34dc5588b56b24c_233_0.png)

Notice that the map shows you the navigator and zoom controls at the top-left corner of the map. Also, the attribution text is shown in the bottom-right corner of the map. After adding layer markers, the map would now contain pins at the specified latitude and longitude coordinates, as shown in the following screenshot:

![](img/2a66507445090790d34dc5588b56b24c_234_0.png)

In the preceding screenshot, a tooltip is shown when the user hovers over the marker.

# There's more...

In the earlier examples, the navigator, zoom, and attribution controls were placed at the top-left and at the bottom-right corners of the screen. The position of these controls can be changed by including the controls option and specifying the position of these controls:

```
$('#chart').kendoMap({
.controls: {
navigator: {
position: "bottomRight"
},
zoom: {
position: "bottomLeft"
},
},
```

```
attribution: {
    position: "topRight"
},
.
});
```

Now when you render the page, the controls will be positioned accordingly.

![](img/2a66507445090790d34dc5588b56b24c_235_0.png)

The position attribute can have one of the following values: topLeft, topRight, bottomLeft, and bottomRight. It is also possible to hide the controls by setting its value to false:

```
controls: {
    navigator: false
},
```

By setting the specified control to false, the control would not be shown on the page.

# Creating a map by binding it to the GeoJSON data

GeoJSON is a format for encoding a variety of geographic data structures. It is an open-standard format for encoding a collection of simple geographical features along with non-spatial attributes using JSON. In this recipe, we will create a map that is bound to the geoJSON data, containing not only the coordinates, but also other non-spatial information about the area.

# How to do it...

To create shapes that correspond to points or coordinates, set the layer type as shape. Let's first refer to the geoJSON data that contains the coordinates and also other non-spatial data:

```
{
"type": "FeatureCollection",
"features": [
{
"type": "Feature",
"properties": {
"GEO_ID": "0400000US23",
"STATE": "23",
"NAME": "Maine",
"LSAD": "",
"CENSUSAREA": 30842.923
},
"geometry": {
"type": "MultiPolygon",
"coordinates": [
[
[-67.619761, 44.519754],
[-67.61541, 44.521973],
[-67.587738, 44.516196],
.
.
.
],
.
.
.
]
}
}
]
}
```

