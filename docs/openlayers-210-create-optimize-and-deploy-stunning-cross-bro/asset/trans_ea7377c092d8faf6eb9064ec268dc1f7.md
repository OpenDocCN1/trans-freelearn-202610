
```
allOverlays: true,
    controls: [new OpenLayers.Control.Navigation(),
new OpenLayers.Control.PanZoom(),
new OpenLayers.Control.LayerSwitcher()]
});
```

2. 现在，让我们创建几个图层，以便了解 allOverlays 属性的工作方式。我们假设在今后的示例中你会向地图添加 WMS 图层。

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

3. 由于在实例化地图对象时没有指定 div，我们必须调用 render() 函数将地图放置在 div 中。该函数接受 HTML 元素或元素 ID 的字符串，因此让我们传入元素的 ID。

```
map.render('map_element');
```

4. 最后，检查是否已设置中心点。如果没有，则缩放至最大范围。我们假设你在今后的示例中也会执行此步骤。如果你愿意，可以将对 render() 函数的调用放在这段代码之后，它仍然会缩放至中心或最大范围——调用 render() 和设置地图位置的顺序由你决定。

```
if (!map.getCenter()) {
    map.zoomToMaxExtent();
}
```

5. 查看地图并打开图层切换器——你应该能够关闭两个图层并看到现在为空的地图。因为我们没有底图，并且所有叠加图层默认都是禁用的，所以在你启用图层之前你只会看到空白地图。

![](img/1b8cd151c9f278555f9cf255b20d244e_214_0.png)

## 发生了什么？

我们刚刚查看了一些我们提到的属性——`allOverlays`、`controls`，以及（省略）`div` 属性。我们还看到了用户通过关闭所有图层可能完全看不到地图的方式。

地图不会自动渲染，因为我们没有传入 HTML 元素。相反，我们在创建地图对象后调用了 `render()` 方法并传入了 HTML 元素。可能有时候你希望延迟渲染地图，例如等待用户点击按钮，在这种情况下这是一种很好的做法。我们将在本章后面更详细地讨论地图方法，但让我们回到地图属性的讨论。

### 事件监听器

`eventListeners: {Object}`

此属性接受一个匿名对象，其中包含事件类型（作为键）和在触发这些事件时调用的函数（作为值）的键值对。我们在第 6 章中简要介绍了此属性，这里它的行为方式相同。一个示例是：

```
var map = new OpenLayers.Map('map_element', {
    eventListeners: { 'moveend': my_moveend_function }
});
```

上面的代码假设存在一个名为 `my_moveend_function` 的函数。我们将在本章的事件部分中更详细地介绍此属性以及可能的事件类型。

### 穿透

```
fallThrough: {Boolean} Default: true
```

fallThrough 属性决定 OpenLayers 是否允许地图上的事件向下传播（即“穿透”）到页面上的其他元素。

让我们通过一个示例来说明其含义。假设你的 map_element div 上附加了一个事件，当你点击它时会显示一个警告。这个事件在 OpenLayers 之外。现在，假设你创建一个地图对象并将其附加到该 map_element div。如果 fallThrough 设置为 true（默认情况下是这样），你通过点击地图会收到原始警告——鼠标点击事件将被允许“穿透”。如果将其设置为 false，你将不会收到警告，因为 OpenLayers 将会消耗该鼠标点击事件。

### 图层

```
layers: {Array {OpenLayers.Layer}}
```

我们已经在一定程度上遇到过此属性。地图实例化后，我们可以通过调用 map.layers 来访问其图层。到目前为止，我们一直是先创建地图对象，然后创建图层对象，然后使用 addLayer() 将图层添加到地图。然而，我们也可以在实例化地图时使用 layers 属性并传入图层对象的数组。

在创建地图对象时使用此属性传入图层或调用 addLayers 本质上做的是同一件事——使用你最熟悉的方法即可。一个示例是：

```
var map = new OpenLayers.Map('map_element', {
    layers: [my_layer_1, new OpenLayers.Layer.WMS({ ... })]
});
```

你可以传入已创建的图层对象（例如 my_layer_1，假设已创建了该名称的图层对象），或在调用中实例化图层对象（例如 new OpenLayers.Layer.WMS({...})）。

![](img/1b8cd151c9f278555f9cf255b20d244e_215_0.png)

> 在上面的示例中，... 将被 WMS 图层的选项替换。

### 最大范围

maxExtent: {OpenLayers.Bounds} 默认值：以十进制度为单位的范围，(-180, -90, 180, 90)

设置此项将指定地图的最大范围。超出最大范围的瓦片不会被请求，用户也不能平移到位于 maxExtent 之外的坐标。如果你使用的是与默认投影不同的投影，你需要更改此设置以反映所需投影中世界的坐标。

maxExtent 属性的数据类型是 OpenLayers.Bounds 对象。OpenLayers.Bounds 类用于创建“bounds”对象，其中包含构成边界框的四个坐标。这四个坐标是最小 x（左）、最小 y（底）、最大 x（右）和最大 y（顶）。要实例化一个 bounds 对象，只需按以下顺序传入坐标：

```
var bounds_object = new OpenLayers.Bounds(minx, miny, maxx, maxy);
```

### 最小范围

minExtent: {OpenLayers.Bounds}

设置 minExtent 将指定地图的最小范围。此属性没有默认值。你通常不需要设置它，除非你使用 maxResolution 指定自定义分辨率——在这种情况下，你通常会将值设置为 (-1, -1, 1, 1)。

### 受限范围

restrictedExtent: {OpenLayers.Bounds}

此属性指定用户可以导航的范围——也就是说，用户只能在 restrictedExtent 属性指定的范围（如果给出，并且限制其导航是可能的）内平移。

设置此属性仍允许用户放大以查看超出受限范围的地图部分，但地图的中心将被设置为受限范围。如果你想限制缩放级别，可以设置 resolutions 数组、maxResolution 或 scales 属性（在后面的页面中介绍）。此属性类似于 maxExtent，然而，restrictedExtent 之外的瓦片仍会被请求，但用户不一定能够平移到它们。

### 缩放级别数

numZoomLevels: {Integer} 默认值：16

numZoomLevels 属性指定地图将拥有的可能缩放级别的数量。对于许多应用来说，给用户 16 个缩放级别并不总是理想的。在大多数情况下，只需指定 numZoomLevels（如果使用不同的投影，则连同 maxExtent）即可设置缩放级别的数量。随着我们查看其他属性，我们将看到更多指定缩放级别数量的方法（有些方法根本不使用此属性）。然而，让我们看看如何使用此属性来设置缩放级别的数量——非常简单！

## 动手实践 – 设置缩放级别和最大范围

1. 通过指定 numZoomLevels 并添加 PanZoomBar 控制来创建你的地图对象。我们还将 numZoomLevels 属性设置为 8，意味着只有八个缩放级别可用。最后，我们将 maxExtent 属性设置为仅包含世界的一部分，如下所示：

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

2. 打开页面，你应该会看到类似这样的内容：

![](img/1b8cd151c9f278555f9cf255b20d244e_217_0.png)
