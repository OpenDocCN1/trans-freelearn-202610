
```
return false;
};
```

7. 现在我们只需注册事件：

```
map.layers[1].events.register('beforefeatureadded', this, before_feature_added);
```

8. 尝试向地图添加要素。该要素不应被添加，并且控制台应记录：

```
feature will not be added! Object { feature=Object,  more...}
```

## 发生了什么？

我们刚刚演示了如何在矢量图层上注册和注销一个事件，该事件在要素添加到图层之前触发。让我们再看一个涉及事件的示例，然后继续讨论 Vector 图层类的工作方式。

## 实践时间 – 处理更多事件

在本示例中，我们将检查与向地图添加要素相关的事件。我们将查看要素已添加事件类型，该事件在要素被添加到地图时触发（无论是通过编程还是用户操作），以及可由 EditingToolbar 控制触发的草图事件。

- 1. 打开本章的第一个示例。同样，我们将使用 Firebug。
- 2. 这次，我们将演示 featureadded、sketchstarted 和 sketchcomplete 事件。让我们创建三个合适的监听函数：
```
function feature_added(feature) { console.log('feature added', feature); };
function sketch_start(feature){ console.log( 'sketch started:', feature); };
function sketch_complete(feature){ console.log( 'sketch done:', feature); };
```
- 3. 现在，注册要素已添加事件：
```
map.layers[1].events.register('featureadded', this, feature_added);
```
- 4. 查看地图并添加一个要素。在添加点或添加多边形/线（草图）后，你应该会看到一条消息：
```
feature added Object { feature=Object,  more...}
```

5. 现在让我们注册草图事件。它们将在使用任何 EditingToolbar 控制时触发。

```
map.layers[1].events.register('sketchstarted', this, sketch_start);
map.layers[1].events.register('sketchcomplete', this, sketch_complete);
```

6. 现在，在添加点或创建多边形/线时，在完成添加要素后，你应该会看到总共三条消息：

```
sketch started Object { feature=Object,  more...}
sketch done Object { feature=Object,  more...}
feature added Object { feature=Object,  more...}
```

## 发生了什么？

我们刚刚演示了几种更多事件类型。你也可以通过查看控制台日志来了解事件是如何传播的（哪些事件先执行）。从上面的示例可以看出，sketchcomplete 事件在 featureadded 事件之前执行。

即使我们先注册了 featureadded 事件，注册事件的顺序也不会改变事件传播的顺序。我们将在整章中使用更多矢量图层事件，但它不会比前两个示例复杂多少。那么，让我们继续吧！

## Geometry and Feature classes

在本章中，我们一直在使用要素对象，但并没有真正讨论 Feature 类本身。我们也提到了 Geometry 类，但到目前为止还没有深入细节。本节将更详细地介绍这两个类，以便你能更自信地使用它们。别担心——很简单，你已经接触过这两个类了。

在进入 Feature 类之前，我们应该先介绍 Geometry 类，因为它用于创建构成要素对象的实际几何对象。

## Geometry class

The **Geometry** class is, more or less, the foundation of the feature objects we've seen so far. Specifically, up to this point, we have been using the `Feature.Vector` class as the base class for our feature objects. We'll talk more about that shortly, but for now it is just important to know that the `Feature.Vector` class uses the `Geometry` class to store geometry information about a feature.

但 **Geometry** 类到底是什么？简单来说，它存储地理信息。还记得本章前面添加要素的示例吗？代码如下：

```
var feature_point = new OpenLayers.Feature.Vector(
    new OpenLayers.Geometry.Point(-72, 42)
);
```

我们传入了 Geometry Point 对象（我们稍后会介绍子类）。现在，如果执行并在地图上查看，我们会在坐标 -72, 42 处看到一个点。然后，如果我们通过类似 `map.layers[1].features[0]` 的方式获取要素信息，我们会看到有关该要素的信息。查看时，我们会看到一个 geometry 属性。

展开它，或使用 `map.layers[1].features[0].geometry`，我们会发现它具有 `bounds` 属性，以及 `x`、`y` 和 `ID` 属性。这就是要素如何在几何对象中存储地理信息的方式。

让我们再次看看实例化几何对象的代码：`new OpenLayers.Geometry.Point(-72, 42)`。正如你可能猜到的，我们正在使用 Geometry 类的 Point 子类。

## Geometry subclasses—Theory

在使用 Geometry 类时，我们实际上几乎总是使用它的某个子类。我们是什么意思？想想我们在整本书中使用的 Layer 类——我们一直在使用基 Layer 类的子类（`Layer.WMS`、`Layer.Image` 等都是 `OpenLayers.Layer` 类的子类）。

就像我们处理图层一样，当我们想要一个 Geometry 对象时，我们几乎总是使用 `OpenLayers.Geometry` *子类*。到目前为止，我们唯一明确调用的是 `Geometry.Point` 子类，它接受单个 X,Y 坐标并基于传入的坐标创建几何。还有许多其他子类让我们能够做比仅仅处理点更复杂的事情。

在介绍子类之前，让我们快速了解一下通过基 `Geometry` 类可用于*所有*子类的一些方法。所有这些方法都可用于任何 Geometry 子类，因为所有子类都继承自 Geometry 类。

## Geometry class methods

- **atPoint(lonlat, toleranceLon, toleranceLat)**: 这将返回一个 `{Boolean}`，指示几何对象是否位于传入的 lonlat。lonlat 参数是一个包含要检查坐标的 `{OpenLayers.LonLat}` 对象，而 toleranceLon、toleranceLat 参数指定一个 `{Float}` 数字，分别设置经度和纬度的阈值。如果一个点位于经度和纬度的阈值范围内，将返回 `true`——否则返回 `false`。确定几何是否位于传入点的计算是基于几何对象边界的近似值。
- **calculateBounds()**: 调用此函数将重新计算几何的边界。它不返回任何内容。如果想查看几何对象的边界，请参阅 `getBounds()`。
- **clearBounds()**: 此函数会将几何对象的边界对象设为 null。
- **clone()**: 此函数创建几何的副本并返回一个克隆的 `{OpenLayers.Geometry}` 对象。
- **destroy()**: 调用此函数将销毁几何对象。注意——如果在已属于要素对象的几何对象上调用此函数，它不会从地图中移除几何对象。
- **distanceTo(geometry, options)**: 调用此函数将计算当前几何对象与传入几何对象之间的最近距离。options 参数可以包含额外的计算选项，具体取决于几何子类而有所不同。有一个 details 选项，它是一个 `{Boolean}`，可为所有子类指定。如果在 options 参数中将 details 设置为 `true`，此函数将返回一个包含距离以及表示两个几何对象坐标的 x0、y0 和 y1、y0 值的 `{Object}`。如果未设置 details（或设置为 `false`），此函数默认将返回包含距离的 `{Number}`。请注意地图的单位。
- **extendBounds(bounds)**: 此函数将扩展几何的边界，包括传入的边界对象，该对象必须是 `{OpenLayers.Bounds}` 对象。如果调用此函数时几何没有设置边界，则会设置边界。
- **getArea()**: 返回一个包含几何对象覆盖面积的 `{Float}`。此函数在各种子类中被重新定义，但可由所有子类调用。
- **getBounds()**: 以 `{OpenLayers.Bounds}` 对象的形式返回几何的边界。如果未设置边界，则会计算边界。这将返回单个几何对象的边界——如果你想获取图层上所有要素的边界，请参阅 Vector 类的 getDataExtent 方法。
