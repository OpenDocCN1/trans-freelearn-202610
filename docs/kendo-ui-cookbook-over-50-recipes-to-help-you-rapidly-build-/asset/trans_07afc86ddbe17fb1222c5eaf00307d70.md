

其他两个属性 horizontalSeparation 和 verticalSeparation 用于指定水平和垂直排列的两个节点之间的距离。

下一步是在 DataSource 对象中为节点创建形状。在本示例中，我们将使用 shapeDefaults 选项向图表添加形状，如以下代码片段所示：

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

这里，visual 属性的值是一个函数，该函数返回需要在图表上呈现的可视元素。在我们的示例中，我们将创建一个包含模型中 name 和 designation 属性的可视元素。该函数首先通过创建 kendo.dataviz.diagram.Group 的实例来创建一个组元素。Group 属性是一个不可见的可视元素，用于包含和分组其他可视元素。然后，我们创建 kendo.dataviz.diagram.Rectangle 和 kendo.dataviz.diagram.TextBlock 的实例并将其追加到组中。该函数随后返回组对象。该函数被递归调用，节点被添加到图表中。

# 工作原理...

当布局子类型为 down 时，根节点位于顶部，子节点被添加，树向下生长，如以下屏幕截图所示：

![](img/2a66507445090790d34dc5588b56b24c_230_0.png)

当将子类型更改为 left 时，根节点位于右侧，节点被添加，使树从右向左生长。

![](img/2a66507445090790d34dc5588b56b24c_230_1.png)

将子类型更改为 mindmapHorizontal 后，根节点位于中心，子节点被添加到两侧。

![](img/2a66507445090790d34dc5588b56b24c_231_0.png)

您可以尝试将子类型更改为 radial 和 tipOver，以查看图表中节点的位置。

# 使用 OpenStreet 地图创建显示地理空间信息的地图

该库提供了 API，您可以使用它在页面上显示特定位置的地图。在本示例中，我们将使用 kendoMap 函数创建地图，并提供一些配置详细信息。我们将使用 OpenStreet 地图来获取地图图像。

# 操作方法...

要创建地图，请在选定的 DOM 节点上调用 kendoMap 函数，并提供以下代码片段中提到的配置选项：

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

在前面的代码片段中，center 选项用于指定地图中心位置的坐标。纬度和经度信息，连同提供的地图宽度和高度，用于在地图容器中居中地图。zoom 选项用于设置地图的缩放级别。layers 属性用于指定将在地图上显示的各种图层；在前面的示例中，图层类型指定为 tile，用于显示来自 OpenStreet 地图的数据。type 的其他可能值包括 bing，用于从 Bing 地图检索地图；marker，用于向地图添加标记；以及 shape，用于显示矢量形状图层。当数据绑定到 geoJSON 数据时，会使用这些值。urlTemplate 属性指定从中检索地图数据的 URL。它会考虑地图设置的缩放级别以及 x 和 y 坐标，即 center 属性和 subdomains 属性中指定的纬度和经度信息。请注意，在 urlTemplate 选项之后，指定了 subdomains 属性。它是一个数组，提及应用于获取地图图像的子域。浏览器对每个域可并行检索的图像数量有限制。使用子域时，图像会并行获取；因此，缩短了呈现整个地图所需的时间。attribution 字段用于指定图层的归属信息。

现在让我们向地图添加一些标记。标记是另一种可以添加的图层类型：

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

# Kendo UI DataViz - 高级图表

在前面的代码片段中，图层类型指定为 `marker`，`locationField` 属性指定的值为 `latlng`。这引用了 `dataSource` 选项中的属性。`dataSource` 属性指定标记详细信息，即必须添加标记的纬度和经度信息，以及用户将鼠标悬停在标记上时显示的提示信息。`tooltip` 选项包含一个 `content` 属性，其值是一个函数，用于从 `dataSource` 属性返回提示信息。

# 工作原理...

当您呈现页面时，会显示地图。地图数据从指定的 URL 获取，即从 OpenStreet 地图使用各种子域获取。

![](img/2a66507445090790d34dc5588b56b24c_233_0.png)

请注意，地图在左上角显示了导航器和缩放控件。此外，归属文本显示在地图的右下角。添加图层标记后，地图现在将在指定的纬度和经度坐标处包含图钉，如以下屏幕截图所示：

![](img/2a66507445090790d34dc5588b56b24c_234_0.png)

在前面的屏幕截图中，当用户将鼠标悬停在标记上时，会显示提示信息。

# 更多...

在前面的示例中，导航器、缩放和归属控件位于屏幕的左上角和右下角。可以通过包含 controls 选项并指定这些控件的位置来更改这些控件的位置：

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

现在当您呈现页面时，控件将相应地定位。

![](img/2a66507445090790d34dc5588b56b24c_235_0.png)

position 属性可以具有以下值之一：topLeft、topRight、bottomLeft 和 bottomRight。也可以通过将其值设置为 false 来隐藏控件：

```
controls: {
    navigator: false
},
```

通过将指定的控件设置为 false，该控件将不会在页面上显示。

# 通过绑定 GeoJSON 数据创建地图

GeoJSON 是一种对多种地理数据结构进行编码的格式。它是一种开放标准格式，用于使用 JSON 对一组简单地理特征及其非空间属性进行编码。在本示例中，我们将创建一个绑定到 geoJSON 数据的地图，该数据不仅包含坐标，还包含该区域的其他非空间信息。

# 操作方法...

要创建与点或坐标对应的形状，请将图层类型设置为 shape。让我们先参考包含坐标和其他非空间数据的 geoJSON 数据：

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

