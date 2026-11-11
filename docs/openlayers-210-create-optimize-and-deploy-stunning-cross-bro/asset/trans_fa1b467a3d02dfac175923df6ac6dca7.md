
## Strategy.Filter

**Strategy.Filter** 类用于限制添加到矢量图层的要素。它使用 **OpenLayers.Filter** 类对要素数据进行比较。要使用此策略，您必须创建一个 **OpenLayers.Filter** 对象，详细内容将在下一章介绍。

## Strategy.Fixed

此策略用于请求要素一次且不再请求。有个 `preload` 属性会在图层可见之前加载数据，默认值为 `false`。如果设为 `true`，数据会在要素绘制前加载，但可能会减慢地图速度。

## Strategy.Paging

使用此策略，要素可以分组为“页”，这意味着您会收到数据的分组。如果数据量很大，这是一个非常有用的策略。有个 `length` 属性接收一个 `{Integer}`，指定每页的要素数量。默认值是 `10`。

另一个属性 `num` 包含当前页码的 `{Integer}`。要使用此策略，您需要调用 `pageNext()` 和 `pagePrevious()` 方法来翻页。

## Strategy.Refresh

此策略会刷新矢量图层，导致要素被重新请求并重绘。它可以手动调用，也可以设置间隔并按传入的间隔刷新。

`interval` 属性是一个以毫秒为单位的 `{Float}`，如果设为大于 0 的数字，则指定自动刷新矢量图层的频率。默认情况下，它设为 `0`，不会自动刷新。

还有一个 `force` 属性，也会强制刷新图层。默认情况下设为 `false`。

有几个相关的方法可以使用：

- **refresh()**: 导致图层被刷新
- **reset()**: 重置计时间隔（如果已设置）
- **start()**: 根据 `interval` 属性开始自动刷新行为
- **stop()**: 结束自动刷新

## Strategy.Save

使用此策略，要素可以保存到服务器。服务器必须允许保存，例如使用事务的 WFS 服务器。当调用 `save()` 方法时，更改将尝试保存到服务器。

还有一个 `auto` 属性，可以设为 `{Boolean}` 或 `{Float}`（以秒为单位）。如果设为 `true`，则在要素被修改（或删除）后立即保存。如果设为数字，则要素将在传入的间隔时保存。

## Summary

本章关于矢量图层类的介绍到此结束。我们讨论了矢量图层类是什么以及它如何工作。我们还介绍了矢量图层类的属性、方法和事件，并通过示例进行了演示。Feature 和 Geometry 类也涵盖了其属性和方法。最后，我们学习了 Strategy、Protocol 和 Format 类以及如何使用它们创建矢量图层。

在下一章中，我们将继续深入使用矢量图层。我们将学习如何为矢量图层设置样式，并学习如何使用 Rule 和 Filter 类来完全控制矢量图层的外观。

## 10 矢量图层样式指南

> *到目前为止，您应该已经对 OpenLayers 相当熟悉了。*
> *我们谈过如何自定义控件（UI 元素）的外观，但我们还没有太多谈论如何自定义地图本身的外观。*

在本章中，我们将让事物看起来更漂亮（或多或少）。我们将：

- Learn how to style Vector layers in OpenLayers
- Discuss the Style class and demonstrate how to use it
- Go over the StyleMap class
- Work with the Rules class
- Learn about the Filter class

Let's get to it!

## 为矢量图层设置样式

在上一章中，您看到了矢量图层的强大功能。在本章中，我们将更深入地探讨如何自定义矢量图层中要素的外观。矢量图层样式的工作方式类似于我们自定义控件对象外观的方式，但不同于使用 CSS，我们使用 Style 和 StyleMap 对象与矢量图层一起使用。我们可以这样为矢量图层设置样式，因为矢量图层不使用栅格图像——它使用可以设置样式的 SVG（或 Canvas 或 VML）元素，类似于 HTML 元素可以用 CSS 设置样式。

更改要素样式，例如将要素颜色从默认的橙色更改为蓝色，非常容易。但更改矢量图层要素的颜色也许只是我们可以做的最基本的事情之一。通过使用 **Rule** 和 **Filter** 类，我们可以指定 *如何*（以及什么）要素应该被设置样式。例如，我们可以根据要素的地理位置或其 `attributes` 中包含的属性值来为点着不同颜色。

我们能做的远不止修改颜色。我们可以更改大小、不透明度、线型以及各种其他属性。样式属性可以任意设置，也可以基于单个要素对象本身的属性。

### 应用样式

我们可以做很多事，但怎么做呢？有三种不同的方法可以将样式应用于矢量图层。

- Use the StyleMap and Style objects.
- Apply a symbolizer (an anonymous object with style properties) directly to the layer.
- Apply a symbolizer directly on a feature. This primarily only occurs when reading in data from a source like a KML file.

The first way is what we'll be using throughout this chapter. A **StyleMap** is, essentially, a mapping of **Style** objects to various 'states', or **intents**, of feature objects. For example, the `default` intent is what a feature object is normally set as. When you select it, it then receives the `select` intent, and any styles mapped to that intent.

The **Style** object contains a style symbolizer, or numerous style properties and values. We use symbolizers to define style, and we can use symbolizers on Style objects, directly on layer objects, or directly on an individual feature.

### 什么是 symbolizer？

A **symbolizer** is just an anonymous object consisting of `key:value` pairs that contains style properties, similar to CSS. For example, { `fillColor`: '#336699', `strokeWidth`: 4 } is a symbolizer which tells the feature to have a fill color of #336699 (light blue), and a stroke width of 4 pixels. We'll cover a full list of valid properties later in this chapter.

We use symbolizers when we create **Style** objects to specify the style properties to apply to our features. We can apply a symbolizer directly to the vector layer object, or read in style information from a source file (such as a KML file).

Before we dive into the **StyleMap** and **Style** classes, let's first take a look at an example so we can better wrap our heads around what we intend to do in this chapter.

## 动手实践 – 应用一些基本样式

Applying styles is pretty easy, as you'll see:

- 1. We'll be doing a few things in this example. First, we'll create a vector layer. Then, we'll add some features to it. Nothing new so far. Then, we'll style the layer by creating a StyleMap and Style object and applying them to the vector layer.
- 2. Add a WMS layer to the map (like in previous chapters) then create and add a vector layer to the map:
- 3. Let's add some feature points. We'll create a loop which will create ten randomly placed feature objects and add them to the vector layer. These coordinates will be different each time you refresh the page, as they are being randomly generated:
- 4. Now, let's add some style to our vector layer. We'll be using a StyleMap object and apply a style to the 'default' intent (the 'normal state' of the features) so all features will receive the style. First, we'll need to create a Style object which will be used by the StyleMap object that gets applied to the layer. The Style object consists of a symbolizer, as we previously mentioned, that contains key:value pairs of styles to apply to the features (similar to CSS):

```
vector_layer = new OpenLayers.Layer.Vector('Basic Vector Layer');
map.addLayer(vector_layer);
```

```
for(var i=0; i<10; i++) {
    vector_layer.addFeatures([new OpenLayers.Feature.Vector(
        new OpenLayers.Geometry.Point(
            (Math.floor(Math.random() * 360) - 180),
            (Math.floor(Math.random() * 180) - 90)
        )
    )]);
}
```

```
var vector_style = new OpenLayers.Style({
    'fillColor': '#669933',
    'fillOpacity': .8,
    'strokeColor': '#aaee77',
    'strokeWidth': 3,
    'pointRadius': 8
});
```
