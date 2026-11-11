
//Show all the selected features
document.getElementById('map_feature_log').innerHTML += 'All selected features: ';

//Now, loop through the selected feature array
for(var i=0; i<vector_layer.selectedFeatures.length; i++){
    document.getElementById('map_feature_log').innerHTML +=
    vector_layer.selectedFeatures[i].attributes.location + ' | ';
}
}

13. 我们现在需要一个在要素取消选择时调用的函数。它将做与之前函数类似的事情——显示用户点击的要素，然后显示所有已选择的要素。

```
function unselected_feature(event){
    var display_text = event.feature.attributes.location + 'unselected!' + '<hr />';
    document.getElementById('map_feature_log').innerHTML = display_text;

    //Show all the selected features
    document.getElementById('map_feature_log').innerHTML += 'All selected features: ';

    //Now, loop through the selected feature array
    for(var i=0; i<vector_layer.selectedFeatures.length; i++){
        document.getElementById('map_feature_log').innerHTML +=
        vector_layer.selectedFeatures[i].attributes.location + ' | ';
    }
}
```

14. 现在只剩最后一件事了！我们必须注册事件，以便在要素被选择或取消选择时调用这些函数：

```
vector_layer.events.register('featureselected', this, selected_feature);
vector_layer.events.register('featureunselected', this, unselected_feature);
```

15. 全部完成！打开你的地图并选择一些要素（使用 Shift 选择多个要素）。你应该会看到日志更新：

![](img/1b8cd151c9f278555f9cf255b20d244e_280_0.png)

## 发生了什么？

我们刚刚演示了如何使用 SelectFeature 控制以及一些要素数据。在本书的剩余部分，我们将对 SelectFeature 控制和要素做更多的工作，所以如果你对它还不是完全熟悉，也不用太担心。尝试创建自己的函数和要素。为此，你需要对 SelectFeature 类及其一些属性有更多的了解。让我们快速回顾一下。

## Control.SelectFeature 类

首先我们将介绍属性，然后查看 SelectFeature 类的一些方法。

## SelectFeature 控制属性

- **box:** `{Boolean}` 指定是否可以通过绘制框来选择要素。默认为 `false`。如果设置为 `true`，在激活 `SelectFeature` 控制对象时，你将能够绘制一个框来选择要素，而不是通过点击要素来选择它们。
- **clickout**: `{Boolean}` 确定在点击任何要素之外时要素是否会被取消选择。默认为 `false`。
- **geometryType**: `{Array{String}}` 指定要素能够选择的唯一几何类型的字符串数组。每个字符串应为 Geometry 类的名称。默认为此属性指定 `null`，意味着所有几何类型都可以被选择。例如，如果你希望控制只允许选择 Point 对象，你可以这样传入此属性：
  ```
  geometryTypes: ['OpenLayers.Geometry.Point']
  ```
- **handlers**: `{Object}` 这包含对处理程序对象实例的引用。
- **highlightOnly**: `{Boolean}` 指定要素是否可以被选择，或只能被高亮。默认为 `false`。如果 `hover` 设置为 `true`，则此设置无效。
- **hover**: `{Boolean}` 默认为 `false`。如果设置为 `true`，当用户将鼠标悬停在要素上时，要素将被选中（并添加到 `vector_layer.features` 数组中）——点击要素不会起作用，只有悬停才有效。
- **layer**: `{OpenLayers.Layer.Vector}` or `{Array{OpenLayers.Layer.Vector}}` 指定 selectFeature 控制关联的矢量图层（或图层数组）。
- **multiple**: `{Boolean}` 控制是否可以通过点击选择多个要素。默认为 `false`。这并不意味着永远不能选择多个要素（即使此设置为 `false`，你也可以使用 `multipleKey` 来选择多个要素）。如果设置为 `true`，点击其他要素时要素不会被取消选择。
- **multipleKey**: `{String}` 指定用于允许多选要素的键。按住该键时可以选中多个要素。例如 `multipleKey: 'shiftKey'` 或 `'altKey'`。默认值是 `null`。
- **onBeforeSelect**: `{Function}` 在要素被选择前调用的函数。默认为空函数。
- **onSelect**: `{Function}` 在要素被选择时调用的函数。默认为空函数。
- **onUnselect**: `{Function}` 在要素被取消选择时调用的函数。默认为空函数。
- **renderIntent**: `{String}` 用于从图层的样式映射中获取要使用的样式。样式在第 10 章中介绍。
- **selectStyle**: `{Object}` 包含样式对象。样式将在本章后面讨论。
- **toggle**: `{Boolean}` 确定在点击要素时是否取消选择已选择的要素。默认值为 `false`。如果 `hover` 属性设置为 `true`，则此设置无效。
- **toggleKey**: `{String}` 指定一个键，按住时将 `toggle` 属性设置为 `true`。释放键时，`toggle` 属性将恢复为 `false`。例如 `multipleKey`: `'altKey'`。默认值为 `null`。

这些属性可以在实例化 `SelectFeature` 控制时设置。创建 `SelectFeature` 控制对象的调用形式为：

```javascript
var select_feature_control = new OpenLayers.Control.SelectFeature(
    vector_layer,
    {}
);
```

`{}` 是可选的 `options` 对象，可以用我们刚刚讨论的属性填充。现在，让我们来看一下 `SelectFeature` 控制的一些方法。

## SelectFeature 控制方法

让我们看一下在本书剩余部分会使用到的几个方法。

- **activate()**: 激活控制，允许我们使用它。
- **deactivate()**: 停用控制。调用后，要素在控制再次激活之前无法被选择。
- **highlight(feature)**: 使用要素的 `select` 样式绘制传入的 `feature`。
- **unhighlight(feature)**: 使用要素的正常样式绘制传入的 `feature`。
- **select(feature)**: 选择传入的 `feature` 对象，将其添加到图层的 `selectedFeature` 数组中，调用 `onSelect` 方法，并将要素渲染为已选择状态（应用任何样式等）。
- **unselect(feature)**: 取消选择传入的 `feature` 对象，从图层的 `selectedFeature` 数组中移除它，调用 `onUnselect` 方法，并将要素渲染回正常状态（应用任何样式等）。
- **unselectAll(options)**: 调用此方法将取消控制当前选择的所有要素。如果希望取消除特定要素外的所有要素，请将其作为 `except` 属性传递给 `options` 参数。

这结束了我们对 Feature 和 Geometry 类的讨论。接下来，我们将再次深入 Vector 类——但这次我们将关注更高级的使用。

## The Vector class, part two

现在我们对矢量图层的工作原理有了更多的了解，可以开始做一些更有趣的事情了。首先，让我们快速回顾一下到目前为止我们所涵盖的内容：

- Vector 类允许我们在地图上显示矢量对象，使用各种不同的渲染器（SVG、Canvas、VML）来实现这一点。
- 矢量图层中的对象，如点和多边形，都是 `OpenLayers.Feature.Vector` 类的对象。
- 每个 `Feature` 对象都包含一个包含空间信息的 `Geometry` 对象。
- 我们可以使用各种 Vector、Geometry 和 Feature 类方法来移动地图上的要素对象、添加要素以及获取地图上要素和几何对象的信息。

换句话说——到目前为止，我们主要研究了 Vector 类本身以及如何与我们添加的要素进行交互。那剩下的呢？

有趣的是你这么问。你可能已经注意到，我们并没有花很多时间来创建实际的矢量图层对象。本章的下一部分将主要关注这一点——创建矢量图层对象。

## Format, protocol, and strategy classes

在本章开头，我们提到要真正使用 Vector 类，我们实际上也会使用其他类。到目前为止，我们使用过 `Geometry` 和 `Feature` 类——但仅在矢量图层创建之后。现在让我们专注于创建更有用的矢量类。

为此，我们在实例化矢量图层对象时需要使用 Format、Protocol 和 Strategy 类。使用这三个类，我们可以进一步控制 Vector 图层的行为。

## Who invited these classes over?

那么，这些类是什么，为什么我们需要它们？有很多好的答案，本书无法全部涵盖——但以下是使用这三个类可以做到的一些事情的简短列表：

- 从外部源加载数据——KML 文件、GeoJSON 文件、WFS 服务等。
- 将我们创建的要素保存为 KML 或其他文件格式
- 使用 Cluster 策略将数百个点分组，就像我们在本章开头看到的那样
- 创建更具交互性的网络地图应用程序

这三个类如何相互关联，每个类的用途是什么？
