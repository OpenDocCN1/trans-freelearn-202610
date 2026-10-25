The geoJSON data mentioned contains the coordinates that correspond to the points on
the map. These points will be used to construct vector shapes. The geoJSON data also lists
other properties such as NAME and CENSUSAREA. Let's use the previous JSON data as a
DataSource object in building the map using the kendoMap function:

```
$('#chart').kendoMap({
center: [30.2681, -97.7448],
zoom: 4,
layers: [{
type: 'shape',
dataSource: {
type: 'geojson',
transport: {
read: './geojson-us.json'
}
}
}]
});
```

Here, the type attribute is set as shape and dataSource is specified, which refers to the
file that contains the geoJSON data. The framework would parse the geoJSON data and
refer to the coordinates to construct the map.

We can use other properties present in the geoJSON data and add some functionality to the
map. For example, we could convert the CENSUSAREA field to a hex number and use this to
fill the shape:

```
$('#chart').kendoMap({
center: [30.2681, -97.7448],
zoom: 4,
layers: [{
type: 'shape',
dataSource: {
type: 'geojson',
transport: {
read: './geojson-us.json'
}
}
}]),
shapeCreated: function (e) {
var shapeColor = parseInt(e.shape.dataItem.properties.
CENSUSAREA, 16).toString();
shapeColor = '#' + ('FFFFFF' + shapeColor).slice(-6);
e.shape.fill(shapeColor);
}
});
```