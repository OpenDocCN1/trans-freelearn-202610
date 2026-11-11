
```
'strokeColor': '#ffffff'
})
//Create a style map object and set the 'default' intent to the
var vector_style_map = new OpenLayers.StyleMap({
  'default': vector_style,
  'select': vector_style_select
});
```

9. 进入最后部分——创建事件。我们的计划是在选择要素时显示一些照片信息。让我们先通过简单地创建一个将出现在地图下方的 div 来搭建该框架。在你的 HTML 代码中，添加以下内容：

```
<div id='photo_info_wrapper'></div>
```

现在让我们创建一个在选择要素时会被调用的函数。它需要使用所选集群中包含的照片的信息来更新 photo_info_wrapper div。我们需要创建一个循环，遍历所选集群中的每个要素，提取其属性，并更新 div 的 HTML。所有这些操作都将在选择要素时被调用的函数中发生，我们还需要创建一个在取消选择要素时清除该 div 的函数。我们需要从要素的 attributes 属性中获取属性，这些属性是在创建矢量图层时由之前的 extractAttributes 参数填充的。我们还将访问要素的 style 属性（该属性由我们之前使用的 extractStyles 参数填充）以获取 externalGraphic 位置。让我们创建这些函数：

```
function on_select_feature(event) {
  //Store a reference to the element
  var info_div = document.getElementById('photo_info_wrapper');
  info_div.innerHTML = '';

  //Store the clusters
  var cluster = event.feature.cluster;

  //Loop through the cluster features
  for(var i=0; i<cluster.length; i++){
    //Update the div with the info of the photos
    info_div.innerHTML += "<strong>"
      + cluster[i].attributes.name
      + "</strong><br />"
      + "<img src="" + cluster[i].style.externalGraphic + "" 
/>"
      + cluster[i].attributes.Snippet
      + "<br /><hr />";
```

```
}
}
function on_unselect_feature(event) {
    //Store a reference to the element
    var info_div = document.getElementById('photo_info_wrapper');
    
    //Clear out the div
    info_div.innerHTML = '';
}
```

10. 现在，我们只需要注册事件。

```
vector_layer.events.register('featureselected', this, on_select_feature);
vector_layer.events.register('featureunselected', this, on_unselect_feature);
```

11. 现在，当我们点击一个集群时，我们应该看到关于点的信息如下：

![](img/1b8cd151c9f278555f9cf255b20d244e_342_0.png)

## 发生了什么？

我们刚刚为地图添加了一些交互性。它并不太复杂，尽管乍一看代码可能令人望而生畏。不过我们实际上并没有做任何“新”的事情。我们注册了一些矢量图层事件，这些事件在选择要素时会触发，然后显示所选要素的信息。从这里开始，你可以轻松地对应用程序进行一些更改——例如更改显示的方式和内容。这个示例或多或少可作为指南，帮助你创建自己的应用程序。让我们进一步扩展这个示例；但首先，我们需要使用实时数据。

## 使用 ProxyHost 使用实时数据

我们加载的数据 (`flickr_data.kml`) 来自第三方来源，但它仅在我们下载时保持新鲜。让我们解决之前遇到的第二个问题：

*使用“实时”数据，而不是在我们想要新数据时自己下载 KML 文件*

我们需要做的是实时访问数据。为此，我们需要使用一个 **ProxyHost**。该术语在第 9 章中有更详细的讨论，但本质上它是一种通过调用某些服务器端脚本来为我们发出请求（充当代理）从而规避 AJAX 跨域请求限制的方法。

## 动手时间 – 获取动态数据

让我们指定一个 ProxyHost 并实时获取 Flickr 的数据。

- 1. 我们可以使用刚刚创建的示例，因为我们只会在其中添加两件事。
- 2. 首先，我们需要定义 proxyhost 的 URL。它可以是 CGI 脚本、python 脚本等。OpenLayers 在 `http://trac.osgeo.org/openlayers/wiki/FrequentlyAskedQuestions#ProxyHost` 处提供了一个 CGI 脚本。如果你使用该文件，请确保已安装 Python。默认情况下，使用 Xampp 在 Windows 上文件会放在 `c:/xampp/cgi-bin`，在 Linux 上为 `/usr/lib/cgi-bin` 或 `/var/www/cgi`（位置可能因发行版而异）。在你的代码中指定 ProxyHost，将 URL 更改为指向你的 ProxyHost：

```
OpenLayers.ProxyHost = '/cgi-bin/proxy.cgi?url=';
```

3. 接下来，我们需要在代理文件中的 allowed host 变量中添加 Flickr 域名。编辑代理文件并将 Flickr URL 添加到 allowedHosts 列表中（在 OpenLayers 提供的文件中靠近顶部）。在撰写本文时，列表中的最后一项是 'vmap0.tiles.osgeo.org'。在最后一项后添加一个逗号，然后将 'api.flickr.com' 添加到列表中：

```
allowedHosts = [ ..., 'vmap0.tiles.osgeo.org', 'api.flickr.com']
```

4. 最后，我们只需更改矢量图层指向的 URL。在矢量图层的 protocol 对象中，我们需要更改 url 值并添加一个 params 对象，该对象指定将附加到 URL 调用的额外 GET 变量。在这种情况下，我们希望指定 format 和 tags 参数：

```
...
protocol: new OpenLayers.Protocol.HTTP({
    url: 'http://api.flickr.com/services/feeds/geo/',
    params: {'format':'kml', 'tags':'bird'},
...
```

5. 就这样！现在，假设你已正确设置 ProxyHost，你应该会看到与之前几乎相同的内容。唯一的区别是数据可能不同，因为你现在正在实时接入数据流。

## 发生了什么？

我们刚刚使用了 ProxyHost 并将矢量图层指向了实际的 Flickr API URL。为什么我们一开始不这样做呢？同样是为了减少在开发应用程序时需要担心的事情。现在我们已经有了一些交互性和实时 URL，让我们更进一步。

## 收尾应用程序

显示来自世界各地的鸟类图片很不错——但如果让用户能够显示他们想要的任何标签的照片呢？我们接下来要做的就是这个。

## 回顾

到目前为止，我们已经创建了一个地图，让用户可以与 Flickr 数据交互。就我们而言，交互部分或多或少已经完成。现在我们将重点放在更改“数据源”部分。目前，我们只请求带有 bird 标签的照片，但我们希望允许该标签为任意标签。

应用程序开发中的一个重要概念是保持模块化。这基本上意味着我们尝试以这样的方式编写应用程序，以便我们可以取出和放入不同的部分，而不会大幅更改其余代码。在这种情况下，我们将保持代码的交互部分不变（至少到目前为止我们所做的工作），并主要关注检索数据的代码。

## 计划

需要发生什么？好吧，让我们思考一下。我们希望用户能够指定他们想要的任何标签。我们希望允许多个标签。这意味着我们需要更改矢量图层指向的 URL，但仅在他们指定标签之后。我们可以通过更改矢量图层协议对象的 params 属性来实现这一点。目前，tags 硬编码为 'bird'，但这将基于用户输入的变量。

## 更改 URL

因此，我们知道需要允许一个变量，该变量指定 URL 中的 tags 参数基于用户输入。我们需要创建一个输入框，让用户指定标签。我们还将有一个提交按钮，当点击时，它将调用一个函数，使用指定的参数更新矢量图层的 URL。

## 动手时间 – 为你的地图添加动态标签

现在让我们为地图添加更多交互。我们将添加一个输入框，根据用户的输入更改请求的 Flickr 数据。

- 1. 打开之前的示例，我们将在其基础上进行添加。首先，我们需要在 HTML 页面中添加一个输入框和按钮。我们将创建一个包含它们的 div 并将其放置在地图旁边。地图 div 之后，添加以下内容：
  
  ```html
  <div id='input_wrapper' style='position:absolute; left:610px; top:0;'>
      <input type='text' id='input_tags' value='bird' />
      <input type='button' id='input_submit' value='Show Data' />
  </div>
  ```
  
- 2. 接下来，我们需要创建一个函数，当点击 input_submit 按钮时会被调用。此函数执行三件事。首先，它将矢量图层的 tags 参数设置为 input_tags 输入框中的值。其次，它将调用矢量图层的 refresh() 方法，该方法更新矢量图层并根据新参数发出新的数据请求。最后，它将清除 photo_info_wrapper div 的 HTML。你可以在 init() 函数底部添加此内容。

```
<div id='input_wrapper' style='position:absolute; left:610px; top:0;'>
    <input type='text' id='input_tags' value='bird' />
    <input type='button' id='input_submit' value='Show Data' />
</div>
```
