
## 发生了什么？

我们刚刚将 EPSG:4326 投影中的一个点转换到 EPSG:900913 投影中的一个点。让我们更仔细地查看我们在 point_to_transform 对象上调用的 transform 方法。

```
point_to_transform.transform(proj_4326, proj_900913);
```

这将把原始点从 proj_4326 投影转换到 proj_900913 投影。注意，我们调用的不是投影对象的 transform() 函数，而是 LonLat 对象的 transform() 函数。OpenLayers.LonLat 对象的 transform() 函数定义如下：

| 函数 | 描述 | 参数 |
|---|---|---|
| `transform(source, dest)` | 此函数原地转换一个点，意味着原始点将被转换；因此原始值将丢失。如果需要它的副本，请先使用 `.clone()` 方法。它返回自身。 | `source`: 源投影<br>`dest`: 目标投影。 |

在这种情况下，我们的源投影为 EPSG:4326，目标投影为 EPSG:900913。需要注意的是，如果不包含 Proj4js 库，使用 OpenLayers 只能对 EPSG:4326 和 EPSG:900913 这两种投影进行转换。

> 当创建地图时，你的所有栅格图层（基于图像的图层；几乎所有除矢量和图像图层之外的图层）必须与地图使用相同的投影。你可以使用坐标和矢量图层进行投影转换，但一旦 OpenLayers 从地图服务器获取到图像，它就无法自行重投影图像（这是地图服务器需要做的事）。

## Proj4js 库

Proj4js 库允许你将坐标从一个坐标系统转换到另一个坐标系统。Proj4js 网站位于 http://proj4js.org。只需在页面上包含 Proj4js 库（就像你使用 OpenLayers 一样），你就可以在 OpenLayers 中进行更多转换。并非所有 EPSG 代码都受支持，但有很多，如果你的目标代码不受支持，你可以为其添加投影定义。

> 网站 http://spatialreference.org 包含大多数 EPSG 代码的 Proj4js 定义。

理想情况下，你应该在整个地图中使用相同的投影，但在某些情况下，你可能希望以不同的投影显示坐标——例如使用矢量图层。让我们看看如何设置 Proj4js 库。

## 动手实践 – 设置 Proj4js.org

这一步与我们设置 OpenLayers 的方式类似。

- 1. 从 http://trac.osgeo.org/proj4js/wiki/Download 下载 Proj4js。截至撰写时，最新版本为 proj4js-1.0.1.zip，所以请继续下载（或下载最新版本）。
- 2. 解压它并将 proj4js 文件夹复制到你的根代码目录（即存放 OpenLayers.js 文件的文件夹）。
- 3. 在代码的 <head> 部分，在 OpenLayers 库包含代码之后添加以下行。
- 4. 现在，打开页面并启动 Firebug。输入并运行以下内容：
- 5. 你应该会看到如下输出：

```
<script type='text/javascript' src="proj4js/lib/proj4js-combined.js"></script>
```

```
var test_proj = new Proj4js.Proj('EPSG:4325');
console.log(test_proj);
```

```
Object { srsCodeInput="EPSG:4325", more...}
```

## 发生了什么？

我们刚刚包含了 Proj4js 库并测试它是否正常工作。如果你在尝试调用 new Proj4js.Proj() 时收到错误，这意味着 proj4js-combined.js 文件的位置错误。请确保目录设置正确（你的示例文件应与 proj4js 文件夹在同一目录中，而 proj4js 文件夹内应包含许多文件和文件夹，包括包含 proj4js-combined.js 文件的 lib 文件夹）。

## 定义自定义投影

现在 Proj4js 库已包含，你可以像在前一个示例中那样使用更多投影进行转换。并非所有投影都已定义；不过，你可以自行定义，例如：

```
Proj4js.defs["EPSG:27563"] = "+title=LAMB sud france +proj=lcc
+lat_1=44.1 +lat_0=44.1 +lon_0=0 +k_0=0.999877499 +x_0=600000
+y_0=200000 +a=6378249.2 +b=6356515 +towgs84=-168,-60,320,0,0,0,0
+pm=paris +units=m";
```

之后，你就可以像使用之前的示例中的 EPSG:4326 和 EPSG:900913 一样使用 EPSG:27563 进行投影转换。

有一些已经定义的投影，你可以在 http://proj4js.org 上更广泛地查看它们。更完整的列表（包含几乎任何 EPSG 代码的 Proj4js 定义）可以在 http://spatialreference.org/ 上找到。

## 总结

在本章中，我们讨论了投影。我们介绍了它们是什么以及各种不同类型的投影。经度、纬度和其他地理概念也被讨论过。虽然我们只是触及了这些相当复杂主题的表面，但你应该拥有足够的基础信息来理解如何使用投影。

我们还讨论了 Projection 类，以及如何转换坐标和使用 Proj4js 库。你经常会处理 EPSG:4326 以外的坐标系统中的数据，了解如何在其他投影中工作和转换数据非常重要。

在下一章中，我们将看看 Google Maps 和其他第三方 API，并将我们最近获得的关于投影的知识付诸实践。

## 5

## 与第三方 API 交互

*网络地图今天非常流行，并且日益流行。在 Google Maps 推出后，出现了交互式网络地图的爆炸式增长。Google 提供了用于与其地图服务交互的 API，其他公司现在也提供了类似的 API，而 OpenLayers 与其中大多数都能很好地配合使用。我们不仅可以将这些第三方 API 与 OpenLayers 一起使用，还可以在其上“mashup”其他图层。*

在本章中，我们将学习：

- 第三方地图 API 的概念
- 使用第三方 API 的图层类：Google、Microsoft 和 Yahoo！
- 如何使用 OpenStreetMap
- 使用球面墨卡托投影
- 组合不同的图层类

## 第三方地图 API

基于网络的地图今天已司空见惯。网络地图爆炸式增长的催化剂是 Google Maps 的推出。在此之前网络地图就已存在，但它们并不快速也不利于开发者。2005 年 6 月，Google 发布了 Google Maps 的 API，该 API 提供了前端客户端（OpenLayers 所扮演的角色）以及对后端地图服务器的访问。

这使得任何人不仅可以在其网站上插入 Google 地图，还可以添加自己的点数据并以其他方式操作地图。Google Maps 越来越受欢迎，其他公司，如 Microsoft 和 Yahoo！，纷纷效仿，创建了自己的网络地图 API。

## 地图 Mashup

术语 **mashup** 指的是将各种不同的数据源和功能组合在一起的应用程序。**map mashup** 是将不同图层和数据组合在一起的地图。第三方地图 API，如 Google 和 Yahoo！ Maps，使得人们更容易创建这些“map mashups”。例如，一个以 Google Maps 为底图并叠加标记以追踪你去过的地方的地图可以被视为一个地图 mashup。

OpenLayers 并未引入地图 mashup，但它能让我们轻松创建非常强大的 mashup。使用 OpenLayers 将 Google 的地图图层、WMS 图层和矢量图层组合起来非常简单。

## OpenLayers 与第三方 API

OpenLayers 允许你在地图应用程序中使用第三方地图 API，让你可以在自己的地图中使用它们的地图。主要限制是，截至撰写时，第三方 API 地图必须是底图层（但正如我们之前讨论的，你可以使用任意多个底图层）。

OpenLayers 可以通信的三个大型商业地图 API 是 **Google Maps**、**Yahoo! Maps** 和 **Bing (Microsoft) Maps**。还有另一个免费开源的 API，OpenLayers 与其配合良好（并且以与前三个类似的方式工作），称为 **OpenStreetMap**。

我们首先将逐一查看这四个图层类，然后再介绍如何将它们与其他图层类组合。

## Google Maps

Google Maps 背后的软件由客户端和服务器组成。客户端是你访问 `http://maps.google.com` 时使用的，它与 Google Maps 后端服务器通信。Google 提供了让你使用其客户端和后端服务器的 API，但由于我们使用 OpenLayers 作为客户端，我们只对与 Google 的后端地图服务器交互感兴趣。

> 根据你计划如何使用 Google Maps，可能存在法律限制。完整限制可在 `http://code.google.com/apis/maps/terms.html` 和 `http://www.google.com/intl/en_ALL/help/legalnotices_maps.html` 找到。
