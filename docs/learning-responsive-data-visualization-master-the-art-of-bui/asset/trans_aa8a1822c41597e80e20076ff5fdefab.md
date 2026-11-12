

<script src="some_script.js"></script>
</head>
<body>
</body>
</html>

在前面的示例中，我们可以看到 `<!--[if lte IE 9]>...<![endif]-->` 表示一个有效的 HTML 注释，它在 Internet Explorer 中会转换为条件 HTML 节点。

该语法仅允许 `<!--[if ... ]>` 和 `<![endif]-->` 关键字以及少量表达式、条件和运算符。以下运算符可用于识别浏览器版本：

- ! 非运算符，例如，[if !IE]
- lt 小于运算符，例如，[if lt IE 5.5]
- lte 小于或等于运算符，例如，[if lte IE 6]
- gt 大于运算符，例如，[if gt IE 5]
- gte 大于或等于运算符，例如，[if gte IE 7]
- ( ) 子表达式运算符，例如，[if !(IE 7)]
- & 和运算符，例如，[(gt IE 5)&(lt IE 7)]
- | 或运算符，例如，[(IE 6)|(IE 7)]

![](img/72559bd4713b2b89631dcca187d7e2db_224_0.png)

## 使用 Modernizr 检测原生功能

设计响应式应用程序的关键挑战是将应用程序正确地适配到用户设备的外形因素（屏幕尺寸和分辨率）。了解外形因素并利用原生工具（例如媒体查询）有助于创建非常响应式的应用程序。只要原生实现可用，我们就希望使用它们；只要可以使用 CSS 而不是 JavaScript 来实现某个功能，我们就希望这样做以获得重要的性能提升。因此，如果我们希望不仅支持最新的浏览器，就需要能够检测原生功能是否可用。

有人可能会问，如何在用户的浏览器上检测某个特定功能。一种简单的方法是检查 API 是否可用，这与以下示例类似。我们检查名为 Array.prototype.every 的原生函数是否可用：

```
if ('every' in Array.prototype) {
    console.log('Array.prototype.every is available');
}
```

前面的代码可以轻松地推广到针对不同对象和 API 函数进行测试。但是，如果我们想知道浏览器中是否可用 in 关键字呢？或者是否支持箭头函数呢？这的工作方式略有不同；我们必须在 try-catch 块中计算表达式。以下是测试当前是否可用箭头函数的示例：

```
function hasArrow() {
    try {
        eval('() =>{}');
    } catch (err) {
        return false;
    }
    return true;
}
```

使用此技术，我们可以将其扩展到大量不同的功能和 API，并检查这些功能是否可用。但是，我们非常懒惰，不想全部实现它们。幸运的是，有一个名为 Modernizr 的优秀库可以为我们完成所有这些工作，并额外提供一种易于使用的基于类的方法来在 CSS 中进行功能检测。

首先，我们将安装 Modernizr；然而，这并非一个简单的过程。没有全局的 Modernizr.js 文件可让您检查目标浏览器中可用的每个功能。此外，您必须自己构建（精简版）Modernizr。听起来复杂，但实际并非如此。首先，使用 npm 通过以下命令全局安装 Modernizr（您可能需要 sudo 权限）：

```
npm install -g modernizr
```

现在，我们可以创建 modernizr-config.json 文件，用于存储项目的 Modernizr 配置。我们为 SVG 功能设置了配置文件；不过，随时可以向配置中添加更多功能（通过链接 https://modernizr.com/download 查看可用选项，并单击 BUILD 创建配置文件。

![](img/72559bd4713b2b89631dcca187d7e2db_226_0.png)

Modernizr 网站

我们为基本的 Canvas 和 SVG 支持以及其他 SVG 功能创建配置文件：

```
{
    "minify": true,
    "options": [
        "setClasses"
    ],
    "feature-detects": [
        "test/canvas",
        "test/svg",
        "test/svg/asimg",
        "test/svg/clippaths",
        "test/svg/filters",
        "test/svg/foreignobject",
        "test/svg/inline",
        "test/svg/smil"
    ]
}
```

最后，我们可以使用以下命令构建自定义的 Modernizr 功能检测脚本：

```
modernizr -c modernizr-config.json
```

您也可以使用 -d scripts/modernizr.min.js 标志将 Modernizr 脚本的安装目标更改到 scripts 文件夹。让我们创建一个包含此脚本的 HTML 文档：

```
<!DOCTYPE HTML>
<html>
<head>
    <title>Feature Detection using Modernizr</title>
    <script src="scripts/modernizr.min.js"></script>
</head>
<body>

<script>
/* here we go */</script>

</body>
</html>
```

前面的代码展示了如何将 Modernizr 脚本包含到页面中。请注意，除了使用 Modernizr 的命令行工具和配置文件外，您还可以为喜欢的任务运行器（例如 Gulp 或 Grunt）使用插件，或者直接从 Modernizr 网站下载自定义构建版本。

## 在 JavaScript 中使用 Modernizr

让我们尝试 Modernizr 的 JavaScript API；它将所有可用功能直接作为属性暴露在全局 Modernizr 对象上。检查 SMIL 支持就像以下代码片段一样简单：

```
if (Modernizr.smil) {
    console.log('SMIL is available');
}
```

太棒了；我们现在可以轻松地在 JavaScript 中检查所有功能是否可用，并轻松地将应用程序和功能适配到用户的目标浏览器。

> 可用功能的完整列表在 Modernizr 网站上提供，网址为 https://modernizr.com/docs/#features。

## 在 CSS 中使用 Modernizr

在 JavaScript 中使用 Modernizr 非常适合根据目标浏览器的功能调整函数和脚本。但是，当我们处理的是 HTML 元素而非 JavaScript 时，我们可能希望使用 CSS 类根据浏览器的可用功能来显示和隐藏 DOM 元素。

在前面的示例中，我们向配置文件添加了一个名为 setClasses 的选项。该选项会为每个受支持和不受支持的功能向 HTML 根节点添加类。我们可以通过查看以下图中显示的演示页面的 DOM 树来观察这一点：

```
<html class="canvas svg svgasimg svgclippaths svgfilters svgforeignobject inlinesvg smil">
<head></head>
<body>
<script></script>
</body>
</html>
```

使用 Modernizr 的网站的 DOM 树

在前面的示例中，我们只看到了根 HTML 节点上命名为受支持功能的类。不受支持的功能会导致带有名为 no- 前缀的类，例如 no-canvas、no-svg 等等。现在，我们可以向可视化添加 CSS 样式，以显示和使用可用功能或隐藏不受支持的功能：

```
.no-svg .chart {
  display: none;
}

.no-svg .sorry-for-not-using-a-modern-browser {
  display: block;
}
```

在前面的片段中，我们从页面中删除图表，并向用户显示一条错误消息，要求其更新到现代浏览器。但是，通常我们并不想简单地为不兼容浏览器的用户禁用某个功能。此外，我们希望将缺失的功能添加到用户的浏览器中，以便他/她可以获得类似于使用最新浏览器的用户体验。请在以下部分中了解更多相关信息。

我们将更频繁地使用 Modernizr 基于类的功能检测来为应用程序设置样式，而不仅仅是检测 SVG 或 Canvas 支持。想想这样一个场景：您想使用 CSS3 动画为可视化的某些部分添加动画。优势显而易见：您将拥有 CSS 中原生动画的功能，而不是在 JavaScript 中使用手动计时器和 DOM 转换。

因此，我们可以在配置文件中包含 CSS3 特定的动画选项，例如 `cssanimations`、`csstransforms`、`csstransforms3d`、`preserve3d` 等，并检查这些功能的可用性。现在，我们可以在 CSS 中创建原生动画，并在不支持 CSS 过渡时创建回退解决方案：

```
@keyframes fadeIn {
    from {opacity: 0.0;}
    to {opacity: 1.0;}
}
```

```
.cssanimations .chart {
    animation-name: fadeIn;
    animation-duration: 4s;
}
```

作为 JavaScript 回退解决方案，我们添加以下脚本：

```
<script>
d3.selectAll('.no-cssanimations .chart')
    .transition()
    .duration(4000)
    .style('opacity', 1.0);
</script>
```

当然，我们也可以使用略显更明确的 Modernizr JavaScript 语法：

```
<script>
if (!Modernizr.csstransitions) {
    d3.selectAll('.chart')
    .transition()
    .duration(4000)
    .style('opacity', 1.0);
}
</script>
```

现在，我们可以通过适配目标浏览器的功能，为可视化在用户体验、兼容性和性能之间提供一种折中方案。请注意，这种折中方案是有代价的——应用程序中存在多个冗余代码片段，需要进行测试和维护。

## 原生功能的自定义实现

如果目标平台或设备上不可用原生功能，开发者有三种选择：

- 删除使用原生功能的那段代码

