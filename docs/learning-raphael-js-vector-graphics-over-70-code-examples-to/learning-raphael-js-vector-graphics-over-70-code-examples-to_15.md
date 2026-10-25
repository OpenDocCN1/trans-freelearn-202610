## 使用现有 SVG

大多数设计师在其项目中使用栅格图形和矢量图形的混合体，因此熟悉 Adobe Illustrator 和 Inkscape 等矢量图形编辑器。这些工具的实用性近年来随着数字媒体的进步而大幅增长——特别是，设计师需要能够创建分辨率独立的图形，这些图形可以在不同的上下文中工作。

例如，移动和平板电脑市场鼓励设计师创建在非常低分辨率（约 320 x 240）和非常高分辨率（2,048 x 1,536）下都能适当缩放的图像。如今，桌面浏览器也大致相同，用户体验被定制以满足一系列视口尺寸。满足一系列视口尺寸通常被称为 **响应式设计**。

虽然 SVG 本身在旧版浏览器中支持有限，但我们仍然可以提供矢量图形解决方案。我们现在看到，矢量图形在网站和应用程序中被更广泛地利用。由此可见，我们正在见证设计从僵硬但更精美的艺术作品向灵活的基于矢量的设计转变。

话虽如此，现在更加重视能够使用图形编辑软件生成的矢量图形。虽然在 Raphaël 中使用现有 SVG 没有银弹式的方法，但只要有一定的决心，我们就可以在项目中利用现有的 SVG。

在本章中，我们将探讨以下内容：

- Inkscape 矢量图形编辑器
- 检查 SVG 路径的手动方法
- SVG 到 Raphaël 转换工具
- 现有 SVG 的示例：分级地图





## Inkscape

Inkscape 是一个开源的 SVG 编辑器，其功能集与 Adobe Illustrator 相似。许多设计师使用它，原因如下：

- 它是开源的，并且积极维护/开发
- 在线上有大量可用的教程
- 它拥有全面的功能集
- 它符合 W3C 标准 SVG

Inkscape 还具有跨平台的优势，可供下载的版本包括 Windows、Mac 和 Linux。

## 下载 Inkscape

Inkscape 可在 `http://inkscape.org/download/` 下载。建议下载最新的稳定版本，撰写本文时该版本为 0.48.4。

## 开始使用 Inkscape

虽然对 Inkscape 的深入介绍超出了本书的范围，但我们将使用以下步骤，通过 Inkscape 创建一些基本的 SVG：

1. 首先，打开 Inkscape，从菜单中选择 **文件 | 新建**。Inkscape 自带多种预定义的文档尺寸，您应选择最适合所创建图形的尺寸。目前，请选择 **默认**。

在左侧，您会看到 Inkscape 拥有多种形状绘制工具，用于绘制基本形状、文本、自由手绘和 Bézier 路径。

2. 选择圆形。
3. 在画布上任意位置单击并拖动。
4. 释放鼠标时，您的圆形将如下所示：

![](img/d8ec653db4cab13fec8b3f5718174af6_93_0.png)

注意圆上的锚点。最左侧的点用于控制圆的宽度，最上方的点控制高度，而最右侧的点用于更改终点。在最右侧断点上单击并拖动：在圆内拖动会从终点到起点绘制一条直线，而在圆外拖动则会创建一个线段。

## 处理现有 SVG

在圆外按几度角拖动的结果如下截图所示：

![](img/d8ec653db4cab13fec8b3f5718174af6_94_0.png)

对于基于 Web 的项目，文档尺寸通常由组成对象定义。单击 **文件 | 文档属性 | 页面**，然后展开 **调整页面大小以适应内容** 下拉菜单。在此处，您可以为文档定义自定义宽度和高度。单击 **将页面大小调整为绘图或选择** 将调整页面和所选元素的尺寸（如果未选中任何元素，则调整所有绘制元素的尺寸）。

这实际上是对当前图层上的所有元素应用平移，即先绘制路径，然后应用变换。如果我们希望所有路径相对于“新”原点绘制（通常是这种情况），则应执行以下步骤：

1. 将文档大小调整为所需尺寸。
2. 转到 **图层 | 图层...**
3. 通过单击图层窗格上的 **+** 符号创建新图层。
4. 选择上一图层中的所有对象。
5. 选择 **图层 | 将选择移动到上方图层**。
6. 删除上一图层。

结果如下：

![](img/d8ec653db4cab13fec8b3f5718174af6_95_0.png)

我们可用的最佳工具或许是钢笔工具，它用于绘制 Bézier 曲线路径：

1. 选择钢笔工具（*Shift + F6*）。
2. 在文档中任意位置单击一次。
3. 在文档中的另一点单击并拖动。

## 处理现有 SVG

您会注意到，每次单击都定义一个锚点，该锚点决定曲线的弯曲程度，如下截图所示：

![](img/d8ec653db4cab13fec8b3f5718174af6_96_0.png)

每次后续单击都定义一个新的锚点，路径通过将其连接到起点锚点来闭合：

![](img/d8ec653db4cab13fec8b3f5718174af6_96_1.png)

## 检查路径

由于我们熟悉向 Raphaël 的 `path` 方法提供原始路径字符串数据，不言而喻，我们可以从 SVG 复制路径字符东并以这种方式绘制路径。对于复杂图形，SVG 的原始 XML 不总是易于处理，操作多个路径可能会很麻烦，但这种处理现有 SVG 的方法确实让我们对正在操作的图形拥有完全控制权。

## Inkscape 的 XML 编辑器

Inkscape 的一个出色功能是能够直接查看和编辑 SVG 的 XML 标记。考虑上一节中的圆形示例。如果单击 **编辑 | XML 编辑器**，您将看到 SVG 的文档结构。单击左侧您创建的元素——`svg:path` 元素——将列出对象的所有属性，包括 `d` 属性，该属性是在应用变换之前元素的路径。单击 `d` 属性将显示元素的路径：

![](img/d8ec653db4cab13fec8b3f5718174af6_97_0.png)

## 处理现有 SVG

如果我们获取此路径并在 Raphaël 中按以下方式使用：

```javascript
var initialPath = 'M 197,94.46 '
 + 'A 98.5,94.5 0 1 1 167.39,26.92 L 98.5,94.46 z';
var pac = paper.path(initialPath);
pac.attr({fill: '#09c', 'stroke-width': 0});
```

然后它将绘制到我们的画布上，如下所示：

![](img/d8ec653db4cab13fec8b3f5718174af6_98_0.png)

返回 Inkscape，移动弧形锚点以关闭我们创建的线段的“嘴巴”：

![](img/d8ec653db4cab13fec8b3f5718174af6_98_1.png)

与之前一样，我们使用 XML 编辑器获取此元素的路径。

我们可以使用路径动画，以相对较少的努力实现一个有趣的效果，从之前的“张嘴”路径动画到之后的“闭嘴”路径。首先，我们定义一个名为 move 的函数，该函数根据旋转（初始化为零）确定 pac 元素的 x 和 y 坐标：

```javascript
var rotation = 0;
var move = function() {
    var x = Math.cos(Raphael.rad(rotation)) * 800;
    var y = Math.sin(Raphael.rad(rotation)) * 800;
}
```

接下来，我们将元素动画化到新路径或“闭嘴”路径：

```javascript
pac.animate({
    path: 'M 197,94.46 ',
    + 'a 98.5,94.5 0 1 1 -2.1e-4,-0.194727, L 98.5,94.46 z',
    transform: ['...T', x, y]
}, 500, function() {
    // Rotate our element and reset the path
    rotation += 90;
    pac.attr({
        path: initialPath
    }).transform(['...R', 90]);
    setTimeout(move);
});
```

我们根据元素的 x 和 y 坐标在半秒内应用平移。动画完成后，我们旋转元素。请注意，变换前缀带有省略号，这确保任何平移在考虑旋转后都会被计入；90 度旋转后，平移将沿 y 轴进行。通过对 move 函数调用 setTimeout，此过程无限重复。

运行此代码时，“嘴巴”在一次动画过程中从张开变为闭合，然后在应用旋转的情况下重复。旋转的效果是使我们的元素沿容器边缘移动（此演示可在本书附带的源代码和网站中找到）。

## 从现有 SVG 图像中提取路径

由于 SVG 图像只是 XML 文件，您可以在文本编辑器中打开它们以获取路径信息。通常，只需从 XML 中的单个 `path` 元素复制并粘贴路径信息即可。如前所述，虽然这种方法很麻烦，但还有其他问题。主要问题是元素或元素组是否应用了变换。如果应用了变换，您需要将相同的变换应用到您创建的 `path` 元素上。

## SVG 到 Raphaël 转换工具

有许多优秀的 SVG 到 Raphaël JS 转换工具，可以自动且轻松地将 SVG 图像转换为 Raphaël。

### Ready Set Raphaël

Ready Set Raphaël (http://www.readysetraphael.com) 是一个基于 Web 的 SVG 转换器，允许您上传图像并获取生成的 Raphaël 代码。它支持 SVG 组、椭圆、多边形、文本、圆形、路径和矩形，并将所有相关样式信息解析为元素属性：

![](img/d8ec653db4cab13fec8b3f5718174af6_100_0.png)

使用它，您只需上传现有 SVG 图像并单击 **复制代码** 按钮。

要测试此功能，请从 http://www.readysetraphael.com 页面底部下载示例文件之一，并将生成的代码复制并粘贴到 HTML 文档的 `<script></script>` 标签之间。请注意，您需要创建一个 ID 为 `rsr` 的 HTML 元素：





## 其他转换器

值得注意的是，网上有许多其他优秀的 SVG 转 Raphaël 转换器可供使用。你可能会发现，每个转换器在转换原生 SVG 时的结果各不相同，因此你应该进行实验，找出哪个最适合你。

Charles Thomas 的 SVG 转 HTML 转换器 (http://www.irunmywebsite.com/raphael/SVGTOHTML_LIVE.php) 将所有基本形状转换为路径，并确保所有路径都使用绝对坐标，而 http://toki-woki.net/p/SVG2RaphaelJS/ 上的转换器则速度快且轻量。

![](img/d8ec653db4cab13fec8b3f5718174af6_101_0.png)

示例企鹅文件的渲染结果如下所示：

| 下载示例文件 |
|-----------------------|
| 试用这些 SVG 文件。(168kb 压缩包) |
| 查看我们的交互式地图示例 |

## 栅格地图

**栅格地图** 是显示某个特定区域或地区及其关联测量值或统计数据的地图。它们通常用于制图学中描绘人口密度和类似指标，并常用于可视化具有地理意义的数据。下图使用 https://commons.wikimedia.org/wiki/File:World_population_density_map.PNG 和 https://en.wikipedia.org/wiki/File:ElectoralCollege2012.svg 的 Wikimedia Commons 图片编译而成，展示了制图学（左）和数据可视化（右）的应用：

![](img/d8ec653db4cab13fec8b3f5718174af6_102_0.png)

## 创建栅格地图

通常，当你需要将数据与某个地理区域或地区关联时，最佳方法是利用现有的 SVG 格式地图，然后根据你的需求进行定制（手动修改原始 XML 或使用 Inkscape 等图形编辑器）。为了演示，我们将使用来自 http://en.wikipedia.org/wiki/File:England_Regions_within_UK.svg 的英国现有地图。

如果你下载此地图并在 XML 编辑器中打开，你会发现英格兰的区被定义为带有 ID 的单个 `<path>` 元素（例如，`<path id="London">` 代表伦敦地区，而 `<path id="north-west">` 描绘整个西北英格兰地区）。Ready Set Raphael 处理此问题非常出色，因为它为每个带有 ID 的路径定义了变量：

```javascript
var london = rsr.path(...);
var northwest = rsr.path(...);
```

你可以使用 Ready Set Raphael 转换器转换上述图像，并将生成的 JavaScript 导入 HTML 文档。运行此代码时，你会发现转换器在保持路径及其关联属性方面做得非常好。

英格兰共有九个官方区域（行政区划），它们都绘制在生成的地图上。这些地区的人口如下，表中显示了 Ready Set Raphael 创建的相应变量名：

| 区域 | 变量名 | 人口 |
|---|---|---|
| 西北英格兰 | northwest | 7,052,000 |
| 东北英格兰 | northeast | 2,597,000 |
| 约克郡和亨伯 | yorkshireandhumber | 5,284,000 |
| 西密德兰兹 | westmidlands | 5,602,000 |
| 东密德兰兹 | eastmidlands | 4,533,000 |
| 英格兰东部 | eastengland | 5,847,000 |
| 伦敦 | london | 8,174,000 |
| 东南英格兰 | southeast | 8,635,000 |
| 西南英格兰 | southwest | 5,289,000 |

所有人口总和为 53,013,000。

在 Ready Set Raphael 创建的代码末尾，我们定义了一个对象数组，将 `path` 属性与表中列出的路径变量及其关联人口相关联。这种合理化很有用，因为它允许我们将地区人口与定义的路径关联起来：

```javascript
var data = [
 {
   path: northwest,
   population: 7052000
 },
 {
   path: northeast,
   population: 2597000
 },
 // ... and so on ...
];
```

## 使用现有 SVG

现在，为了可视化不同的人口，我们定义了一个名为 shading 的自定义属性，该属性基于 0 色相和 0 饱和度返回填充，其中只有亮度变化。这将根据人口大小对国家进行着色：

```javascript
rsr.customAttributes.shading = function(population, min, max, total) {
    var brightness = 1 - ((population - min) / (max - min));
    return {
        fill: 'hsb(0, 0, ' + brightness + ')'
    };
};
```

亮度定义如下：

brightness = 1 - \(\frac{population - smallest population}{largest population - smallest population}\)

该方程根据最大和最小人口大小对亮度进行归一化。当人口大小等于最大人口时，亮度将为零（黑色）；当人口大小等于最小人口时，亮度将等于 1（白色）。人口的所有其他值都返回 0 到 1 之间的值（灰色阴影）。

我们通过遍历定义绘制路径及其关联人口的数据数组来利用此属性：

```javascript
var totalPopulations = 530130000;
var minPopulation = 25970000;
var maxPopulation = 86350000;
for(var i = 0, ii = data.length; i < ii; i+=1) {
    var path = data[i].path;
    var population = data[i].population;
    path.attr({
        shading: [population, minPopulation, maxPopulation,
        totalPopulations]
    });
}
```

每个地区的人口，以及总人口、最大人口和最小人口，都会传递给我们的自定义属性，进而相应地填充区域。此操作的结果如下图所示：

![](img/d8ec653db4cab13fec8b3f5718174af6_105_0.png)

你可以看到，东北地区（人口最少）被涂成白色，而东南地区（人口最多）被涂成黑色。

伦敦的人口规模几乎与该国东南地区的人口规模一样大，这很不幸，因为这使得无法区分伦敦地区和东南地区。在这种情况下，可以通过在遍历数据数组时为每个区域应用较浅颜色的描边来解决：

```javascript
path.attr({
    shading: [population, minPopulation, maxPopulation,
    totalPopulations],
    'stroke': '#bbb'
});
```

结果如下所示：

![](img/d8ec653db4cab13fec8b3f5718174af6_105_1.png)

## 开源 SVG

Open Clip Art Library (http://openclipart.org) 上有许多可用的 SVG。该网站拥有一个出色的 SVG 编辑器，允许你即时编辑托管的 SVG 并查看其底层源代码或下载它们。

## 摘要

你现在应该能够在 Raphaël 驱动的项目中熟练使用现有 SVG。你应该对 Inkscape 有基本了解，并能够使用它创建基本 SVG 并检查图形的 XML 结构。正如我们对栅格地图的介绍所显示的，在线 SVG 转 Raphaël 转换器非常宝贵，你现在应该能够处理更复杂的图形，其特性为我们向最终用户提供的内容提供了额外的可能性。
