
- 1. 首先我们要删除 init() 函数中的所有内容。你的函数现在应该如下所示：
```
function init() {
}
```
- 2. 接下来，在 init() 函数内部，我们将像之前一样设置我们的地图对象：
```
map = new OpenLayers.Map('map_element', {});
```
- 3. 现在我们要创建第一个图层。我们将使用 WMS 图层并向 WMS 服务器请求图层 'basic'（WMS 服务上的一个图层）。我们还将显式地将其设置为基础图层。
```
var wms_layer_map = new OpenLayers.Layer.WMS(
  'Base layer',
  'http://vmap0.tiles.osgeo.org/wms/vmap0',
  {layers: 'basic'},
  {isBaseLayer: true}
);
```
- 4. 现在让我们创建一个第二个图层对象。它也将是 WMS 图层。这次，我们将从 WMS 服务请求几个不同的图层——一堆标签。我们还将把 transparent 属性设置为 true，这样服务器返回的地图图像将是透明的。我们还将把不透明度设置为 50%（通过将 opacity 设置为 .5）。该图层将是一个覆盖图层。
```
var wms_layer_labels = new OpenLayers.Layer.WMS(
  'Location Labels',
  'http://vmap0.tiles.osgeo.org/wms/vmap0',
  {layers: 'clabel,ctylabel,statelabel',
   transparent: true},
  {opacity: .5}
);
```
- 5. 现在是时候将图层添加到地图了。我们将使用 addLayers 函数并传入一个图层对象数组。
```
map.addLayers([wms_layer_map, wms_layer_labels]);
```
- 6. 现在，让我们添加一个图层切换控件，以便显示地图上的图层。
```
map.addControl(new OpenLayers.Control.LayerSwitcher({}));
```
- 7. 最后，我们需要设置地图中心信息。这个最后一步需要在整本书的后续所有示例中重复——你需要确保包含它，即使没有明确要求。我们的所有地图都需要以某种方式设置其范围，而这是一种标准方法（请参阅第 1 章了解更多解释）。
```
if (!map.getCenter()) {
  map.zoomToMaxExtent();
}
```
- 8. 保存文件，然后在你的网络浏览器中打开它（最好是 Firefox，因为我们将使用 Firebug）。由于我们只是使用 HTML 和 JavaScript，你不需要将其放在服务器上。你可以直接从文件夹中用网络浏览器打开该文件。你应该会看到类似这样的内容：

![](img/1b8cd151c9f278555f9cf255b20d244e_71_0.png)

## 发生了什么？

我们刚刚创建了一个地图，其中包含两个 WMS 图层和一个图层切换控件，可让我们打开和关闭图层。控件是 OpenLayers 提供的功能，使我们能够实际与地图和图层交互。要使用控件，我们需要从不同的 Control 类创建对象。默认情况下，所有地图都会获得一个 Navigation 控件对象，该对象允许我们平移和缩放地图。在第 6 章中，我们将深入介绍控件。

![](img/1b8cd151c9f278555f9cf255b20d244e_72_0.png)

> 在之前的示例中，我们的图层对象名为 `wms_base_layer` 和 `wms_overlay_layer`。在整本书中，我将使用此格式来命名对象——全部小写，使用下划线 (_)，没有空格。当然，你可以使用任何你喜欢的约定，只要你的命名方案保持一致即可。

## 创建图层对象

处理图层的过程包括两个步骤：

- 1. 创建图层对象。
- 2. 将图层对象添加到地图。你可以使用 `map.addLayer(layer)` 添加单个图层，或使用 `map.addLayers([layer1, layer2, ...])` 添加图层数组，如前例所示。

这两个步骤实际上可以合并为一个步骤（在调用 `addLayer` 函数时实例化图层对象——这可行，但我不推荐这样做，因为它会使处理图层对象变得稍微困难一些）。到目前为止，我们已经有一些从 WMS Layer 类实例化对象的经验。让我们看看创建我们的 `wms_base` 图层对象的代码。

```
var wms_layer_map = new OpenLayers.Layer.WMS(
  'Base layer',
  'http://vmap0.tiles.osgeo.org/wms/vmap0',
  {layers: 'basic'},
  {isBaseLayer: true}
);
```

括号内的每个项目，在 `OpenLayers.Layer.WMS(` 之后，都称为 **参数**，我们在创建对象时传入这些参数。但我怎么知道要传入哪些参数呢？

我们没有编写该类，所以我们不知道它期望接收什么。因此，就像几乎任何第三方库一样，我们必须参考文档来查看该类期望接收哪些参数。OpenLayers 拥有出色的文档，我们将在整本书中使用它。

由于我们使用的是 WMS Layer，让我们查看一下 WMS 类的文档，网址为 http://dev.openlayers.org/docs/files/OpenLayers/Layer/WMS-js.html。

## Layer.WMS 类

API 文档中有很多信息，但让我们具体查看标题为 **构造函数** 的部分。这将告诉我们如何创建 WMS 图层对象。不同类型的图层过程各不相同，但让我们先关注 WMS 类。查看 **参数** 子部分——它指定了该特定类期望接收的参数：

| 参数 | 描述 |
|------------|-------------|
| name | {String} 图层的名称。 |
| url | {String} WMS 的基础 URL（例如 http://vmap0.tiles.osgeo.org/wms/vmap0）。 |
| params | {Object} 一个包含键/值对的对象，表示 GetMap 查询字符串参数及其参数值。 |
| options | {Object} 要附加到图层的额外选项的哈希表。 |

这些参数告诉我们要传入参数的顺序，以及每个参数的含义。花括号 ({ }) 之间的词指的是参数的数据类型。所以，`{string}` 表示参数应为字符串。

让我们看看 `wms_base_layer` 图层的实例化代码，看看我们如何使用这四个参数。

## WMS 图层参数：

四个 WMS 图层参数如下：

### 名称

第一个参数是图层的 **名称**，它应为字符串。我们传入 `'Base Layer'`。注意我们如何用引号将 `Base Layer` 括起来——这就是表示它是一个字符串的方式。标题可以是你喜欢的任何内容——如果你有图层切换控件，标题将显示在其中。请记住，闭合引号后有一个逗号，这意味着我们已完成第一个参数，可以继续下一个参数。

这个 **名称** 参数几乎存在于所有 Layer 类中。

### URL

**URL** 是第二个参数，它也应为字符串。它指定 Web 地图服务器的 URL。我们传入 `'http://vmap0.tiles.osgeo.org/wms/vmap0'`。这个 URL 参数存在于大多数 Layer 类中。

### Params

Params 是第三个参数，它是一个由键:值对组成的匿名对象。该参数指定影响 WMS 服务器返回的地图图像的服务器端设置。你在此处传入的键:值对将被（或多或少地）附加到 OpenLayers 在向地图服务器发出请求时生成的 URL。

例如，在之前的示例中创建 `wms_layer_labels` 时，我们传入了 `{layers: 'clabel,ctylabel,statelabel'}`。在这种情况下，我们要求 WMS 服务器返回一张地图图像，其中服务器端图层 `'clabel,ctylabel,statelabel'` 已打开。我们指定了多个服务器端图层——不过请求什么或多少个服务器端图层并不重要，因为在客户端，OpenLayers 仍将 WMS Layer 对象视为单个图层对象。这个 params 参数存在于大多数 Layer 类中。

### 可能的 params 键和值

这个 params 对象的可能键和值取决于你正在使用的地图服务器。不幸的是，涵盖所有图层类型的所有内容超出了本书的范围。我们在整本书中只会使用几个 WMS 参数——layers（指定 WMS 服务应提供哪些图层）、transparent（请求透明图像，例如标签图层）和 srs（指定投影）。

目前，至少，我们的主要关注点只是弄清楚 WMS 服务器上的图层名称。为此，你可以在 URL 中发出 GETCAPABILITIES 请求（还必须指定 SERVICE=WMS）。例如，要获取我们迄今为止使用的 WMS 服务的可能图层，请访问以下 URL：

```
http://vmap0.tiles.osgeo.org/wms/vmap0?SERVICE=WMS&REQUEST=GETCAPABILITIES
```

如果你对 WMS 有更多兴趣，其规范可以在以下位置找到

```
http://www.opengeospatial.org/standards/wms.
```

### 选项

Options 是最后一个参数，它是一个匿名对象，指定图层对象的设置。你不需要传入此参数。

options 对象包含客户端 OpenLayers Layer 对象的属性。这些是图层对象本身的设置，因此所有 Layer 类都有此参数。要定义 OpenLayers Layer 对象的各种属性，我们使用此 options 参数。属性包括诸如 isBaseLayer、opacity 和 visibility 等设置。由于图层属性是客户端设置，WMS 服务器（或图层使用的任何地图服务器）都不知道这些设置。
