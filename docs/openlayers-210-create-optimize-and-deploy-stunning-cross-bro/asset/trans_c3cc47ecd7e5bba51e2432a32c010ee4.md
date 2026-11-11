
## 三个类的简要概述

这些类协同工作，帮助我们定义矢量图层的行为。我们很快会更详细地介绍每个类，但首先让我们对其进行概述。

## Protocol 类

**Protocol** 类控制矢量图层如何与数据源进行通信。与 Geometry 类类似，我们实际上使用的是 Protocol 类的子类。两个常用的子类是 `Protocol.HTTP` 和 `Protocol.WFS`。HTTP 协议类允许我们直接与包含矢量数据的源进行通信，而 WFS 协议类则让我们能够与 WFS 服务通信。

## Format 类

**Format** 类让我们可以使用一种文件类型（如 KML 文件），并将该文件中的数据转换为地图中的实际要素对象。我们还可以将现有的要素对象转换为 KML、GeoJSON 等文件格式。OpenLayers 开发者的设想是为 Vector 类支持更多的 Format 和 Protocol，而不是像 WFS（已弃用）这样的特定子类。其他图层类，如 WMS 或 Google Maps 图层，不使用 Format 类——这是因为它们是栅图像层，我们无需担心数据格式。

更技术地说，**Format** 类处理要素数据的序列化和反序列化。序列化是程序员使用的术语，基本意思是从代码中提取对象并将其转换为类似文件或人类可读的字符串，以便这些对象可以在代码之外访问。反序列化则相反——它将文件或字符串转换为代码可用的对象。

那么，在 OpenLayers 的上下文中，这意味着什么？使用 format 类，我们可以对来自各种不同文件格式的要素数据进行序列化和反序列化——例如 KML、GeoJSON、GeoRSS、ArcXML 等。

## Strategy 类

最后，**Strategy** 类用于控制如何设置对服务器的请求以及对服务器返回的数据做什么处理。Strategy 类也是一个基类，我们与它的子类进行交互。我们可以使用众多子类，例如 `Strategy.BBOX` 和 `Strategy.Cluster`。与 Protocol 和 Format 类不同，对于每个类只能使用其中一个子类，使用 Strategy 类时我们可以同时使用多个策略。

为了澄清，让我们看看 `Strategy.BBOX` 类的作用。Strategy 类用于设置请求并确定对响应做什么处理。`Strategy.BBOX` 类是一个处理第一部分的子类。例如，使用 WFS 服务器时，我们可以使用 `Strategy.BBOX` 类向服务器发送请求，仅请求在可视地图范围内的數據。因此，BBOX 类决定如何设置请求。

Strategy 类的另一个作用是控制服务器返回的数据会发生什么。使用像 Strategy.Cluster 这样的类，当图层收到数据后，它会将返回的数据分组为簇。因此，它有助于控制对服务器响应的结果的处理。

我们可以使用 BBOX 类和 Cluster 类一起——即设置图层向 WFS 服务器请求地图范围内的數據，然后图层接收数据并将其转换为簇。

## 这三个类如何交互

现在，你可能已经开始了解这些类如何交互。Protocol 类决定数据如何与客户端进行通信。该数据以某种格式存在，由 Format 类进行序列化和反序列化。最后，Strategy 类告诉 Vector 类如何设置对服务器的请求以及对响应做什么处理。

## 示例实例化

在深入示例或进一步讨论之前，让我们看看如何使用这三个类创建矢量图层。以下代码将从 GeoJSON 文件加载要素数据。该文件必须位于同一服务器上——如果它位于不同的服务器上，或如果您希望指向外部 URL，则需要代理主机（我们稍后会介绍）。

```
var vector_layer = new OpenLayers.Layer.Vector('More Advanced Vector Layer',{
    protocol: new OpenLayers.Protocol.HTTP({
        url: 'some_data.json',
        format: new OpenLayers.Format.GeoJSON({})
    }),
    strategies: [new OpenLayers.Strategy.Fixed()]
});
```

上述代码将使用 HTTP Protocol 类并使用 GeoJSON 格式。注意格式实际上是协议对象的一部分。Fixed Strategy 类也被使用，它基本上只是导致矢量图层请求一次要素并且不再请求新要素。

实际上，这就是全部内容！不同格式将使用不同的 Format 类，如果您想使用其他策略，只需将它们传递到 strategies 数组中即可。

这有很多内容需要消化。让我们通过两个示例来了解如何将这三个类一起使用。

## 动手实践 – 创建矢量图层

在这个第一个示例中，我们将使用与上面看到的类似的代码——利用这三个类构建我们的矢量图层，该图层将从 URL 请求数据。

这个示例有两个注意事项。第一个是它假设您在地图文件同一目录下有一个名为 ex5_data.json 的文件。您可以在本书的源代码文件中找到它，网址为 http://vasir.net/openlayers_book/files/。

其次，除非您在服务器上运行，否则它将无法工作。它需要一个代理主机，我们将在此示例后立即介绍。如果您无法访问服务器，您可以在上述 URL 查看示例的运行情况。

- 1. 使用第 1 章的基础模板启动一个新地图文件。我们将使用 Protocol、Format 和 Strategy 类。因此，首先让我们为每个类创建对象。让我们从格式对象开始，因为协议对象将使用它。我们将使用 GeoJSON 格式。
  
  ```js
  //Create a Format object
  var vector_format = new OpenLayers.Format.GeoJSON({});
  ```
  
- 2. 现在，让我们使用刚刚创建的格式对象创建一个协议对象：
  
  ```js
  var vector_protocol = new OpenLayers.Protocol.HTTP({
      url: 'ex5_data.json',
      format: vector_format
  });
  ```
  
- 3. 最后，我们将创建一个要使用的策略变量，它实际上是策略对象的数组。
  
  ```js
  var vector_strategies = [new OpenLayers.Strategy.Fixed()];
  ```
  
- 4. 现在，将其组合起来创建矢量图层并将其添加到地图中：
  
  ```js
  //Create a vector layer that contains a Format, Protocol, and Strategy class
  vector_layer = new OpenLayers.Layer.Vector('More Advanced Vector Layer',{
      protocol: vector_protocol,
      strategies: vector_strategies
  });
  
  map.addLayer(vector_layer);
  ```
  
- 5. 如果您在服务器上运行，可以打开页面，您应该会看到类似以下内容：

![](img/1b8cd151c9f278555f9cf255b20d244e_287_0.png)

## 发生了什么？

我们刚刚演示了如何使用 **Format**、**Protocol** 和 **Strategy** 类设置矢量图层。如果我们想使用 KML 文件，比如说，我们只需使用 `Format.KML` 类并更改 URL 以指向 KML 文件。

现在，如果您只是从文件夹运行，前面的示例将无法工作——它需要在服务器上。设置服务器超出了本书的范围，但让我们谈谈为什么如果直接从文件夹打开，前面的示例不起作用。

## 跨服务器请求

JavaScript 使用 `XMLHttpRequest` 发出 AJAX 请求。使用 `Protocol` 类时，OpenLayers 会发出 `XMLHttpRequest` 从 URL 获取要素数据。如果该数据文件与您的地图不在同一服务器上，请求将无法工作。JavaScript 有安全限制，阻止其进行此类跨域请求。因此，为了绕过这个问题，我们必须使用 **proxy**。其他图层类不需要这样做，因为其他栅图像层请求的是图像，但不使用 AJAX——因此跨域请求限制不适用。
