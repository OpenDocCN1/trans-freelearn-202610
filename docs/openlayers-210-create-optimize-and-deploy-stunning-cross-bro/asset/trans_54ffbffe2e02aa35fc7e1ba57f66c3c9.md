
## Vector Layer class

向量图层本身与我们迄今为止讨论过的其他图层一样——但要充分利用它，我们将与其他类一起工作。

即使要让一个基本示例运行（就像本章中的第一个示例），我们也会使用其他几个类。具体来说，向量类使用 Feature 类在图层上显示对象。但 Feature 类要工作，需要使用 Geometry 类来创建几何对象。因此，你的向量图层中的实际向量对象是 Feature 对象，它们由 Geometry 对象组成——我们很快会介绍所有这些内容。

在此之前，让我们先介绍 Vector Layer 类本身。我们将首先介绍 Vector 类的属性，然后是方法（方式与第 3 章类似）。

## OpenLayers.Layer.Vector properties

让我们回顾一下 Vector 类包含的属性。其中一些是来自其他类的实例化对象，将在本章后面介绍。

- `drawn: {Boolean}`. 返回 `true` 或 `false`，取决于要素是否已被绘制。在实例化向量图层对象时不会设置此属性。
- `features: {Array {OpenLayers.Feature.Vector}}`. 这是属于向量图层的要素对象的数组。我们在之前的示例中见过它——通过访问此数组，我们可以了解向量图层包含的所有对象（要素）的信息。由于这是一个数组，你可以通过调用 `vector_layer.features[X]` 来访问单个要素，其中 X 是所需要素的索引。我们将在本章后面更深入地介绍如何与要素交互。
- `filter: {OpenLayers.Filter}`. 通过将过滤器对象分配给向量图层对象，你可以根据提供给过滤器对象的属性（正如名称所示）过滤掉某些数据。当你只想显示来自数据源的部分要素而不是全部时，这非常有用。我们将在第 10 章的 Filter 类部分详细讨论此属性。
- `isBaseLayer: {Boolean}`. 默认值为 `false`。指定图层是否为底图。你可以使用向量图层作为底图，从而拥有完全基于向量的地图，而不必依赖 WMS 或 Google Maps 图层作为底图。
- `isFixed: {Boolean}`. 默认值为 `false`。确定在拖动地图时向量图层是否会移动。如果例如你想在地图上放置一个始终保持在地图中心的标记，这会很有用（在这种情况下你会将此属性设置为 `true`）。
- `isVector: {Boolean}`. 如果图层是向量图层，则返回 `true`。在实例化向量图层时不会设置此属性。此属性主要用于检查已实例化的图层是否为向量图层。例如，如果你想遍历地图上的所有图层并确定某个图层是否为向量图层，你很可能会使用此属性。
- `protocol: {OpenLayers.Protocol}`. 指定向量图层使用的协议对象。在关于策略、协议和格式的部分中，我们将对此进行更深入的讨论。
- `renderers: {Array{String}}`. 指定包含要使用的渲染器的字符串数组。每个渲染器按顺序尝试，如果浏览器不支持，则尝试下一个渲染器。本章前面我们介绍了如何使用此属性。
- `rendererOptions: {Object}`. 渲染器将使用一个由属性组成的匿名对象。我们在本章中不会再谈论渲染器，但此对象可以包含的属性在 OpenLayers 文档中列出，每个渲染器类在 http://dev.openlayers.org/docs/files/OpenLayers/Renderer-js.html。
- `reportError: {Boolean}`. 默认值为 `true`。此属性指定如果渲染器加载失败是否报告错误消息。
- `selectedFeatures: {Array{OpenLayers.Feature.Vector}}`. 此属性包含图层中当前被选中的要素的数组。例如，可以通过单击要素来选择要素。此属性在关于与要素交互的部分中会更详细地讨论。
- `strategies: {Array{OpenLayers.Strategy}}`. 策略对象的数组。策略告诉向量图层如何行为，例如将要素聚类。在关于策略、协议和格式的部分中，我们将对此进行更深入的讨论。
- `style: {Object}`. 包含向量图层的样式信息。通过它，我们可以更改向量图层中要素的颜色、大小等。我们将在第 10 章中更详细地讨论这个问题。
- `styleMap: {OpenLayers.StyleMap}`. 定义向量图层将使用的样式的 stylemap 对象。第 10 章详细介绍了 Style 和 StyleMap 类。
- `unrenderedFeatures: {Object}`. 这包含未能渲染的要素的匿名对象（如果存在）。在实例化向量图层对象时不会设置此属性。

## OpenLayers.Layer.Vector methods

现在我们已经介绍了 Vector Layer 类的属性，让我们讨论可以调用的方法。我们只介绍特定于 Vector 图层的功能；从基础 OpenLayers.Layer 父类继承的功能可以在第 3 章中找到。

- `clone()`: 复制图层及其包含的要素。返回一个 `{OpenLayers.Layer.Vector}` 对象，即图层的副本。
- `getDataExtent()`: 此函数将返回一个 `{OpenLayers.Bounds}` 对象，其中包含包含图层所有要素的最大范围。
- `refresh(obj)`: 导致图层请求要素并重绘它们。如果图层可见且在地图范围之内，则会触发刷新事件。接受一个可选的 `obj` 对象，其中包含事件侦听器的属性。
- `assignRenderer()`: 根据图层的 `renderers` 属性为图层分配渲染器。
- `displayError()`: 显示一个警告，告知用户其浏览器不支持图层的渲染器。

## Working with features

我们将讨论的剩余方法涉及要素。正如我们之前看到的，要素是属于向量图层的对象。我们将先介绍方法，然后通过相关示例逐步说明如何正确使用它们。

**addFeatures(features, options)**: 调用此函数将向地图添加要素。你必须至少传入一个 `features` 对象数组，并且可以可选地传入一个 `options` 对象。`features` 参数是 `{Array{OpenLayers.Feature.Vector}}`，而 `options` 是一个匿名 `{Object}`。

## Time for Action – adding features

让我们手动向地图添加一些要素。

1. 复制第一个示例——我们只需要一个基本的 WMS 底图和一个向量图层。
2. 在 Firefox 中打开地图并启用 Firebug。我们将使用 JavaScript 控制台向向量图层添加要素。
3. 让我们先创建一些要素。为此，我们需要使用 OpenLayers.Geometry 类。让我们先创建一个 OpenLayers.Geometry.Point 对象，传入经度和纬度（在 Firebug 中输入并执行以下内容）：

```
var point = new OpenLayers.Geometry.Point(-72, 42);
```
4. 现在我们有了几何对象，可以从中创建一个要素对象。我们将使用 OpenLayers.Feature.Vector 类使用我们刚刚创建的点对象创建一个要素对象。实例化时，构造函数可以接受三个参数——geometry、attributes（可选）和 style（可选）。我们将在本章后面详细介绍 Feature 类。创建要素点，传入上面创建的几何对象：

```
var feature_point = new OpenLayers.Feature.Vector(point);
```
5. 好了，现在我们可以使用 addFeatures 将要素添加到地图了。即使我们只传入一个要素，仍然必须传入一个数组。像这样添加：

```
map.layers[1].addFeatures([feature_point]);
```
6. 你应该会看到点已添加到地图：

![](img/1b8cd151c9f278555f9cf255b20d244e_256_0.png)

## What Just Happened?

我们刚刚使用 addFeatures 方法将从几何对象创建的要素添加到地图。如果 OpenLayers.Geometry 类对你来说是新的，不用担心——我们将在本章中介绍它，并提供更多示例。
