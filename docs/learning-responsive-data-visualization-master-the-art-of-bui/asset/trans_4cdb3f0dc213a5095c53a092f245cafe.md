

| IE | Edge | Firefox | Chrome | Safari | Opera | iOS Safari | Opera Mini | Android Browser | Chrome for Android |
|---|---|---|---|---|---|---|---|---|---|
| | | | | | | | | | |

SVG 的跨浏览器兼容性（来自 http://caniuse.com/#feat=svg）

矢量图形是图像，其中关于图像的所有信息都被编码为图像元素的几何形状（如矩形、圆形、椭圆、路径和文本）的数值表达式。浏览器始终使用这些信息以尽可能最佳的分辨率绘制和渲染图形。在下图中，我们可以看到一个高分辨率的圆形矢量图，而右侧是图像的缩放视图。可以清楚地看到，缩放后的图像显示边缘完美清晰；这种无损缩放效果源于浏览器的分辨率相关渲染。

> 您可以在 W3 网站上找到有关 SVG 规范的详细信息：http://www.w3.org/Graphics/SVG/.

![](img/72559bd4713b2b89631dcca187d7e2db_44_0.png)

矢量图，左：高分辨率，右：缩放

更多优势源于这样一个事实：可视化的元素只是简单地嵌入到 HTML 页面中。现在，我们可以轻松地在图形的每个组件上附加原生 JavaScript 事件监听器，例如 `mouseover` 或 `click` 事件，并使用 CSS 对其进行样式设置，与应用程序的其余部分一起。

另一个巨大的好处是，我们可以直接查看 Web 浏览器的开发者控制台，随意操作并调试可视化。我们可以单击图像的每个元素以查看其附加的事件处理程序，并可以实时修改 CSS 样式。任何处理过 OpenGL 或 WebGL 调试的人都会同意，在开发 Web 可视化时，这是一种你不能错过的便利。

现在，我们已经有很多理由支持使用 SVG 进行响应式跨浏览器可视化：

- 调试简单，因为图形的源代码只是简单地嵌入到 HTML 文档中
- 所有屏幕分辨率下渲染清晰，因为浏览器根据客户端的屏幕分辨率渲染图形
- 所有主流浏览器均具备跨浏览器兼容性
- 使用 CSS 对可视化进行样式设置
- 原生 JavaScript 事件处理程序可以附加到图像的所有元素
- 在开发者工具中调试和实时编辑可视化

## 使用 D3.js 创建图形

D3.js（Data-Driven Documents 的缩写）是由 Mike Bostock 和斯坦福可视化小组的其他成员构建的 JavaScript 库，用于 DOM 操作、数据转换和图形创建。它使用类似 jQuery 的数据驱动方法，并利用现代浏览器和基础 Web 标准（如 HTML、CSS 和 SVG）的所有功能。

D3.js 是开源的，并托管在 GitHub（https://github.com/mbostock/d3）上，采用略作修改的 BSD 3 条款许可证；因此，它可以在开源和商业产品中使用，无需发布任何修改。GitHub 本身使用 D3.js 来可视化存储库的贡献历史。

在本书中，我们将使用稳定版本 3.5.6，但大多数示例也应适用于不同的次要版本。

## 引入 D3.js

将 D3.js 引入项目有多种方式：

- 使用名为 bower 的包管理器（推荐），通过在终端中运行 bower install d3#3.5.6 命令获取库。然后，在网页的 head 部分引用 bower_components/ 目录中的压缩 CSS 文件：<link rel="stylesheet" href="bower_components/d3/d3.min.css">。

- 使用 CDN，例如 cdnjs（开发期间需要活跃的互联网连接），在网页的 head 部分引用 CDN 中的压缩 CSS 文件：<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/d3/3.5.6/d3.min.js">。

- 从 GitHub（https://github.com/mbostock/d3/releases）手动下载 3.5.6 版本并解压到 libs/ 目录。然后，在网页的 head 部分引用 libs/ 目录中的压缩 CSS 文件 <link rel="stylesheet" href="libs/d3/d3.min.css">。

我强烈建议使用名为 bower 的包管理器来管理前端依赖项。但是，在本书的示例中，我们将出于简单性直接从 cdnjs 链接 D3.js；因此，本书源中的所有示例都需要活跃的互联网连接。

## 查找其他资源

D3.js 的终极资源是 GitHub 项目的 WIKI 页面，可在 https://github.com/mbostock/d3/wiki 找到。在这里，您还将找到示例和 API 的详细描述。

另一个绝佳的示例资源是 Mike Bostock 的页面 http://bl.ocks.org/mbostock 和他的博客 http://bost.ocks.org/mike/。

有关更多详细信息和示例，我建议阅读 Pablo Navarro Castillo 编写的 Mastering D3.js 一书。

## 总结

在本章中，我们简要介绍了响应式设计、媒体查询和视口设置。您了解了相对单位以及用于响应式应用程序的移动优先设计方法。

随后，我们介绍了 Bootstrap 并亲自实现了一个简单的栅格系统。这帮助我们了解了 Bootstrap 中响应式流式栅格的工作原理。我们看到了内置的响应式实用程序以及 LESS 中的媒体查询。

在本书的最后一部分，我们通过比较 Canvas、WebGL 和 SVG 在响应式设计中的优势，讨论了位图和矢量图形之间的区别。我们认识到 SVG 非常适合实现响应式可视化。最后，我们简要介绍了 D3.js。

在下一章中，我们将开始使用 D3.js，并使用数据连接、动态属性和简单变换实现一个简单的图形。

## 下载示例代码

您可以从 http://www.packtpub.com 的帐户中下载本书的示例代码文件。如果您在其他地方购买了本书，可以访问 http://www.packtpub.com/support 并注册，以便文件直接通过电子邮件发送给您。

您可以按照以下步骤下载代码文件：

![](img/72559bd4713b2b89631dcca187d7e2db_47_0.png)

- 使用您的电子邮件地址和密码登录或注册我们的网站。
- 将鼠标指针悬停在顶部的 SUPPORT 选项卡上。
- 单击 Code Downloads & Errata。
- 在搜索框中输入书名。
- 选择要下载代码文件的图书。
- 从下拉菜单中选择购买本书的渠道。
- 单击 Code Download。

文件下载完成后，请确保使用最新版本的以下工具解压或提取文件夹：

- 适用于 Windows 的 WinRAR / 7-Zip
- 适用于 Mac 的 Zipeg / iZip / UnRarX
- 适用于 Linux 的 7-Zip / PeaZip

## 使用 D3.js 和 SVG 创建柱状图

在本章中，您将学习如何创建柱状图，包括合适的轴和比例尺——这需要一些 D3 的基础知识，我们也将在此介绍。您将学习以下内容：

- 如何使用 D3 选择和转换 DOM 元素
- 如何将 D3 与 SVG 元素一起使用
- 如何使用数据驱动方法创建可视化
- 如何使用数据绑定和数据连接
- 如何使用动态属性
- 如何绘制线条、区域和弧
- 如何使用比例尺
- 如何绘制轴
- 如何创建柱状图

首先，我们将从 HTML 元素的一些简单 DOM 选择和转换开始，并继续使用 D3 创建 SVG 元素。我们还将看到如何将数据绑定到 DOM 以及使用数据连接和动态属性应用数据驱动转换。

在第二部分中，我们将看到 D3 中一个内置的 SVG 抽象——所谓的生成器函数——尤其是线条、区域和弧生成器。与手动修改 SVG 路径元素的 d 属性相比，这些抽象极大地简化了形状的处理。

