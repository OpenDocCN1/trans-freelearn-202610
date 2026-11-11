
## 行动时间 – 使用比例尺

使用比例尺是控制地图缩放级别的另一种方式。让我们来看一下。

1. 使用 minScale 和 maxScale 按如下方式创建地图对象：

```
map = new OpenLayers.Map('map_element', {
    controls: [
        new OpenLayers.Control.Navigation(),
        new OpenLayers.Control.PanZoomBar(),
        new OpenLayers.Control.LayerSwitcher()
    ],
    maxScale: 27683990.15625,
    minScale: 221471921.25
});
```

2. 你应该会看到类似这样的内容：

![](img/1b8cd151c9f278555f9cf255b20d244e_223_0.png)

3. 现在，让我们回到代码并重新创建地图对象。这次，我们将传入一个 scales 数组。

```
//Create a map with an empty array of controls
map = new OpenLayers.Map('map_element', {
    controls: [
        new OpenLayers.Control.Navigation(),
        new OpenLayers.Control.PanZoomBar(),
        new OpenLayers.Control.LayerSwitcher()
    ],
    scales: [ 55367980.3125, 27683990.15625, 13841995.078125, 6920997.5390625],
});
```

4. 你应该会看到类似这样的内容：

![](img/1b8cd151c9f278555f9cf255b20d244e_224_0.png)

## 发生了什么？

我们刚刚使用比例尺来确定地图的缩放级别。当你想要指定最大或最小缩放级别，以及想要指定硬编码的缩放级别值数组时（尤其是在使用缓存瓦片服务器时），使用比例尺或分辨率非常有用。现在让我们谈谈一些其他与缩放级别无关的属性。

## panMethod

```
panMethod: {Function} Default: OpenLayers.Easing.Expo.easeOut
```

此属性指定平移地图时使用的缓动动画类型。默认值是一个名为 OpenLayers.Easing.Expo.easeOut 的函数，它会导致动画在平移结束时“缓出”（或减速）。截至撰写时，可能的值为 OpenLayers.Easing.Expo、OpenLayers.Easing.Quad 和 OpenLayers.Easing.Linear。这三个基本函数各自又分为三种类型：easeOut（'out' 表示动画结束时）、easeIn（in 表示动画开始时）以及 easeInOut（表示动画在两者时间都会发生）。http://dev.openlayers.org/docs/files/OpenLayers/Tween-js.html 上的文档将包含最最新的可能值。

指定此属性时，请使用基类和子类的完整组合，例如 OpenLayers.Easing.Quad.easeInOut。每当地图被平移时都会出现动画效果，例如使用平移箭头按钮时、用键盘平移地图时，或调用 panTo() 时。

## panDuration

panDuration: {Integer} Default: 50

指定此属性将控制平移完成所需的时间。与 `panMethod` 属性一样，此属性仅影响地图的平移动画。

## 行动时间 – 使用平移动画

OpenLayers 让我们能够在用户平移地图时使用不同类型的动画。利用我们刚刚讨论的属性，让我们学习如何自定义平移动画。

1. 从第 1 章的模板创建一个新页面。按如下方式定义地图对象，传入 `panMethod` 和 `panDuration` 属性：
2. 通过点击左上角控件中的一个箭头来平移地图。在 Firebug 中，调用以下 `panTo` 函数来平移地图并传入坐标：
3. 平移地图时你应该会看到动画。

```
map = new OpenLayers.Map('map_element', {
    panMethod: OpenLayers.Easing.Quad.easeInOut,
    panDuration: 100
});
```

```
map.panTo(new OpenLayers.LonLat(-18,42));
```

## 发生了什么？

我们刚刚使用 `panMethod` 和 `panDuration` 属性来自定义地图的平移动画。更改 `panDuration` 会影响动画本身的持续时间，而更改 `panMethod` 则决定使用哪种类型的动画。通过使用这些属性，你可以模仿 Google Maps 或其他第三方地图的功能。

## 试一试英雄 – 使用不同的动画类型

利用上一节学到的知识，尝试将 `panMethod` 属性更改为各种不同的可能值（例如 `OpenLayers.Easing.Expo.easeInOut`），以查看不同的动画样式。另请更改 `panDuration`，以查看不同值如何影响动画速度。

现在让我们回到更多地图属性。

## projection

```
projection: {String} Default: 'EPSG:4326'
```

我们在第 4 章投影章节中已深入介绍过此内容，因此这里无需花费太多时间。基本上，如果你使用的是除 EPSG:4326 之外的投影，则可以在此处设置 EPSG 代码来指定地图的投影。所有图层都应与地图的投影相同。如果你在此处设置投影，通常还必须指定相关属性，如 maxExtent、maxResolution 和 units（如果投影不是以度为单位）。

## theme

```
theme: {String or null} Default: 'theme/default/style.css'
```

此属性指定地图主题样式文件的相对路径（即，相对于地图页面所在的路径）——一个 CSS 文件。如果你使用自己的主题文件，应在此处指定 CSS 文件的路径，并设置 OpenLayers.ImgPath（任何自定义地图图像的路径）。

此属性也可以设置为 null，在这种情况下，你只需在页面中包含 CSS 文件链接标签（或将样式信息放在 <style> 标签中）。有关使用 CSS 样式化地图的更多信息，请参阅第 7 章。

## tileSize

```
tileSize: {OpenLayers.Size} Default: (256, 256)
```

指定此项将覆盖默认瓦片大小，即 256 x 256 像素。此默认大小 (256, 256) 是来自 Google 等第三方 API 的瓦片大小。如果你不使用第三方 API，可能有很好的理由更改此值。瓦片越大，需要向服务器发出的请求越少——但每个瓦片传输所需的时间越长。一些瓦片缓存服务器可能以 512 x 512 像素缓存瓦片；在这种情况下，你需要将地图大小设置为匹配。尽管如此，在大多数情况下你无需更改此项，但请随意尝试更改值以找到应用程序的“最佳”点。例如：

```
tileSize: new OpenLayers.Size(512,512);
```

## unit

```
unit: {String} Default: 'degrees'
```

这指定了地图的单位。除非你使用非默认投影，否则可能无需更改此项。可能的值为 'degrees' 或 ('dd')、'm'、'ft'、'km'、'mi'、'inches'。

## 地图函数

我们已经使用了 Map 类的一些方法，例如 `addLayers` 和 `zoomToMaxExtent`。事实上，我们在前几章中至少使用过几次更常见的方法。我们尚未详细介绍它们，而且还有一些我们尚未遇到但仍然非常有用的函数，尤其是在开发具有某种自定义功能的地图时。

本节分为相关功能组。每个组都有自己的标题，然后按字母顺序列出函数，以便你能快速找到所需函数。

## 与控件相关

- `addControl( control )`: Parameters—control `{OpenLayers.Control}`. 此函数接收单个控件对象并将其添加到地图。
- `addControls( controls )`: Parameters—controls `{Array {OpenLayers.Control}}`. 此函数接收一组控件对象并将其添加到地图。
- `getControl( match )`: Parameters—match `{String}`. 接受一个参数，即控件的 ID。它将返回一个与传入 ID 匹配的控件对象（如果找到）。**返回：** `{OpenLayers.Control}` 或 null。如果未找到匹配项，则返回 null。
- `getControlsBy( property, match )`: Parameters—property `{String}`, match `{String or Object}`. 这是一个有时被忽视但非常有用的函数。如果你需要获取符合某些条件的控件对象，这就是要使用的函数。它接受两个参数，一个 **属性**（例如 `'active'`），和一个 **匹配**（例如 `true`）。**匹配** 参数可以是字符串、正则表达式或布尔值。属性是控件的任何属性的字符串名称——因此，如果你想获取所有活动的控件，将调用 `map.getControlsBy('active', true);`。**返回：** `{{Array of OpenLayers.Control objects}}` 或 null。如果未找到匹配项，则返回 null；否则，返回匹配的控件对象数组。
- `getControlsByClass( match )`: Parameters—match `{String or Object}`. 与 `getControlsBy` 类似，但将按类名进行匹配。调用此函数并传入包含类名（或正则表达式）的字符串将返回 null 或控件对象数组。**返回：** `{Array of OpenLayers.Control objects}` 或 null。
