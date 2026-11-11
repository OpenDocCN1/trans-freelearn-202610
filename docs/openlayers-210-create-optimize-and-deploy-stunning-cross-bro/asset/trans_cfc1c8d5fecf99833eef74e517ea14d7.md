
让我们回到代码。

**Line [17]:** `{ }`

第四个参数是一个可选的 **options** 对象，是我们在前面讨论过的格式的匿名对象。这些属性通常由每个 OpenLayers Layer 类共享。例如，无论 Layer 类型如何（例如 WMS 或 Google Layer），你都可以传入一个不透明度设置（例如 `{opacity: .8}` 表示 80% 不透明度）。因此，无论你使用的是 WMS 还是 Vector 层，此不透明度属性都可应用于任一层。

![](img/1b8cd151c9f278555f9cf255b20d244e_40_0.png)

> 因为这是传递给 Layer 对象创建调用的最后一项，请确保没有前导或尾随逗号。尾随逗号是常见错误，且调试起来往往很费时。

此 **options** 对象是可选的，但我们经常会使用它，因此养成代码保持一致的习惯并提供一个空对象（使用 `{}`），即使我们暂时没有向其中传递任何内容，也是个好习惯。

**Line [18]:** `);`

这只是完成了对象创建调用。

**Line [20]:** `map.addLayer(wms);`

现在我们已经创建了 `wms_layer` 对象，需要将其添加到地图对象中。注意我们正在调用地图对象的一个函数。实际上，添加图层到地图对象有几种方法。我们可以使用上面的代码（通过调用 `map.addLayer`），其中我们传入单个图层，或者也可以使用 `map.addLayers`：

```js
map.addLayers( [layer1, layer2, ...] );
```

这里，我们传入一个图层数组。两种方法都有效，但在你有多个图层时，传入数组可能更容易。

你也可以在创建地图对象之前先创建图层对象，并在创建地图时将图层对象传入，例如：

```js
map = new OpenLayers.Map('map_element', {layers: [layer1, layer2,
...]});
```

所有方法都有效，但本书中我们通常会使用 `addLayer` 或 `addLayers`。

**Line [21] - [23]:**

```js
if (!map.getCenter()) {
  map.zoomToMaxExtent();
}
```

最后，我们必须指定地图的可视区域。这里实际移动地图的代码是 `map.zoomToMaxExtent()`，它会将地图缩放到地图的最大范围。它位于一个 `if` 语句中。此 `if` 语句检查地图是否已存在中心点。

我们添加此检查的原因是，默认情况下，你的地图可以接受一种特殊格式的 URL，其中可包含范围和要打开/关闭的图层。这在更常见的术语中被称为 `permalink`。如果我们不检查中心是否已设置，permalink 将无法工作。

![](img/1b8cd151c9f278555f9cf255b20d244e_41_0.png)

> 默认情况下，你的地图会添加一个 **argParser** 控制，它会尝试从 permalink 中提取信息。我们将在第 6 章介绍它，但要立即看到其效果，你只需在 URL 中添加以下内容，即可将地图缩放到相同的坐标和缩放级别：`?zoom=4&lat=56&lon=-116`
> 
> 因此，你的 URL 可能看起来像 `c:/code/index.html?zoom=4&lat=56&lon=-116`

设置地图范围有几种方法。如果你知道想要显示所有内容，`map.zoomToMaxExtent()` 函数是一个快速且良好的方法。还有其他方法，例如

```
map.zoomToExtent(new OpenLayers.Bounds([minx,miny,maxx,maxy]));
```

还有更多方法。如果你知道地图起始的具体位置，这就是另一种方法：

```
map.setCenter(new OpenLayers.LonLat(x,y));
map.zoomTo(5);
```

其中 x、y 是经纬度值，5 是你希望缩放到的缩放级别。默认情况下，你的地图有 16 个缩放级别，可通过在创建地图对象时设置 `numZoomLevels` 属性来配置。

还有更多方法，但这些是最常见的策略。基本思路是你需要指定一个中心位置和缩放级别——设置范围可以实现这一点，显式设置中心和缩放级别也可以。

**Line [24]:** `}`

这只是完成了 `init()` 函数。

**Lines [26], [27]:**

这些行关闭了 script 标签和 head 标签。

**Line [29]:** `<body onload='init();'>`

这开始了 body 标签。当页面加载完成时，通过 `body` 标签中的 `onload='init();'` 属性，它将调用 JavaScript `init()` 函数。我们必须等到页面加载后才能执行此操作，因为在页面加载之前我们无法使用地图 div（或任何 HTML 元素）。另一种方法是将 `init()` 调用放在页面底部的 JavaScript 标签中（这也不会在页面加载前被调用），但两种方法都能达到相同的效果。

浏览器加载页面时是从上到下加载的。要在 JavaScript 中使用任何 **DOM (Document Object Model)** 元素（页面上的任何 HTML 元素），它们首先必须由浏览器加载。因此，在浏览器看到该元素之前，你不能用 JavaScript 引用 HTML。这类似于尝试访问尚未创建的变量。即使我们有在页面顶部引用 `map_element` div 的 JavaScript 代码，它实际上要等到页面加载后才会执行（因此需要 `onload` 和 `init()` 函数调用）。

**Line [30] and [31]:** `<div id='map_element' style='width: 500px; height: 500px'></div>`

要制作 OpenLayers 地图，我们需要一个 HTML 元素来显示地图。几乎总是这个元素是一个 `div`。你可以给它任何你想要的 ID，并且此 HTML 元素的 ID 会传入创建地图对象的调用中。你可以随意设置 div 的样式——例如将宽度和高度设置为 100%，如果你想要全页地图的话。最好使用 CSS 来设置元素的样式，但像这样内联设置 div 也能工作。

**Lines [32] and [33]:** 这些行通过关闭剩余的标签来完成页面。

## 背后原理——面向对象编程（OOP）

现在，让我们从更理论和技术层面谈谈这些东西是如何工作的。OpenLayers 采用 **Object Oriented Programming (OOP)** 技术，这意味着要使用该库，我们需要从 OpenLayers 提供的内置 **classes** 创建 **objects**。

这是什么意思？你已经熟悉类和对象是什么，只是可能不知道而已。把 **class** 和 **object** 的概念想象成词性部分。例如，思考名词的抽象概念——人、地点、事物或思想。名词本身（抽象概念）就是一个 **class**。但作为名词的具体、实际的词就是 **objects**。

### 与对象的交互

Ironman 和 Batman 是两个不同的词（或在 OOP 术语中是两个对象），但它们属于同一个名词类。类主要用于生成对象；我们与对象交互。考虑以下两个句子：

> "Noun was in a fight with noun."

> "Ironman was in a fight with Batman."

现在，第二句话更像是一句实际的句子。我们在句子中使用 Ironman 和 Batman 这两个词（它们是对象；它们是名词类的实例）。我们不会在句子中使用 noun（一个类），因为像这样与类交互是没有意义的。这是 OOP 的重要概念之一——我们与对象交互，并通过类生成对象。

### MadLibs

Madlibs，对于不熟悉的人来说，是一种游戏，你会得到一段文字，其中缺少一些词。目的是想出填入空白的词，但每个空白只能包含特定类型的词（名词、形容词、动词等）。词的类型是一个类，而你插入的实际词是一个对象。

### 动手实践 – 玩 MadLibs

通过用 verb 和 noun 'objects' 替换 verb 和 noun，完成这个句子：

I verb up to the noun. It's about 7 or 8 o'clock. I looked at my noun. I was there, to verb on my throne as prince of noun(place).

### 发生了什么？

你刚刚做了一个 Madlibs，演示了 OOP 的一些概念。正如你所见，把句子读成 "I verb up to the noun" 是没有意义的。因为 verb 和 noun 是类，我们不会在句子中使用实际的术语 verb 或 noun。

因此，想法是生成单独的对象，一个是 noun 类的对象，一个是 verb 类的对象。例如，前面的句子可以完成为："I pulled up to the house"。Pulled 和 house 是 verb 和 noun 类的实例，分别是对象。

我们使用属于这些类的词，它们是对象。另一种指代对象的术语是实例，它用于指定对象派生的类，例如，Frodo（一个人/霍比特人）是名词的一个实例。
