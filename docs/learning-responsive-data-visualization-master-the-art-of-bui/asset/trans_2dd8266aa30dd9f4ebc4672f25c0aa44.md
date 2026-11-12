

一旦窗口宽度小于 992 px，div 元素的背景色就会变为蓝色（见下图）：

![](img/72559bd4713b2b89631dcca187d7e2db_23_0.png)

宽度小于 992px 的浏览器窗口

如下图所示，媒体查询受所有主流浏览器以及从版本 9 开始的 Internet Explorer 支持：

![](img/72559bd4713b2b89631dcca187d7e2db_23_1.png)

跨浏览器兼容性媒体查询来自 http://caniuse.com/#feat=css-mediaqueries

## 理解断点

在响应式设计中，当某个布局和媒体查询生效时，我们称之为一个 **状态**，而我们称从一个状态过渡到另一个状态为 **断点**。

让我们看一个示例。首先，我们为以下设备定义一些屏幕尺寸：

- 小于 768px 用于手机
- 768px-992px 用于平板电脑
- 992px-1200px 用于台式机
- 大于等于 1200px 用于大型设备和电视

## 为这些设备尺寸使用媒体查询创建 CSS 代码片段：

```
/* phones, default no query */
/* tablets */
@media (min-width: 768px) { ... }
/* desktops */
@media (min-width: 992px) { ...}
/* large devices */
@media (min-width: 1200px) { ... }
```

在上述代码中，我们定义了四个状态，这些状态之间有三个断点，每个状态对应一个典型的屏幕尺寸和设备类型。

我们还可以通过在查看响应式网页时缩小浏览器窗口来观察这些断点；我们会观察到设计在某个浏览器窗口宽度时如何“断裂”。在下图中，断点被可视化为虚线，显示了一个状态转变为另一个状态的时刻：

![](img/72559bd4713b2b89631dcca187d7e2db_24_0.png)

响应式设计中的断点

我们在查看下图中的 Google Plus 网页时也可以观察到同样的效果。在前两张图中，浏览器窗口大小在变化，但三列布局保持不变。在第三张图中，设计断裂为仅包含两列的新布局：

![](img/72559bd4713b2b89631dcca187d7e2db_24_1.png)

Google Plus 页面的断点

### 拥有正确的视口

跨越移动和桌面设备，存在大量不同的像素密度和屏幕分辨率。分辨率更高的显示器会将页面渲染得比分辨率较低的显示器小得多；这立即导致可读性问题。为防止这种效果并将更大的页面适配到更小的屏幕，引入了一个虚拟窗口——即所谓的视口（网页的可见区域）。这使得网站能够以正确的像素分辨率渲染并缩放到整个设备宽度以实现最大可访问性；但是，开发者无法控制这个特定于设备的设置。后来，在 HTML5 中，引入了视口 meta 标签，以便开发者能够完全控制这些视口设置。

今天，针对移动设备优化的应用程序通常在网页的 head 部分定义以下视口标签，以便正常渲染网站并将内容缩放到设备宽度：

```
<meta name="viewport" content="width=device-width, initial-scale=1">
```

> 有关所有有效视口选项的更多信息，请参阅 MDN 上的 https://developer.mozilla.org/en-US/docs/Mozilla/Mobile/Viewport_meta_tag。

### 相对单位 – %, REM, 和 EM

在处理响应式设计时，使用相对单位来根据父元素描述元素的尺寸始终是个好主意。因此，当父容器改变大小时，所有子元素都会相应调整其尺寸。

用于相对尺寸的最流行单位是 %，它基于下一个绝对定位的父元素的某个百分比来定义尺寸。

但是，还有更有用的单位来描述相对尺寸；例如，以下两个单位基于 font-size：

- em：相对于元素的字体大小
- rem：相对于根元素的字体大小

这些单位有一个很大的优点；它们会受到浏览器字体大小设置变化的影响。一个简单的例子是，当用户增大浏览器字体大小时，内边距的自适应调整。

> 关于所有不同 CSS 单位的简短总结和解释，请参阅 MDN 上的 https://developer.mozilla.org/de/docs/Web/CSS/length。

### 移动设备优先设计

响应式应用程序应该在大量设备（具有大量不同的屏幕尺寸和分辨率）上看起来和感觉都很棒（或者至少能正常工作）；它们通常既在移动设备上运行，也在大屏电视上运行。因此，在项目开始时，每个开发者都必须选择一种设计响应式应用的方法来掌握这种过渡；一般来说，我们区分以下方法：

- 移动设备优先
- 桌面设备优先

在桌面设备优先方法中，我们首先为桌面设备和鼠标输入进行设计，然后调整并缩小设计以适应移动设备和触控交互。这通常会导致 Web 应用程序在移动设备上的功能受限，因为屏幕尺寸小得多且缺乏触控支持。

在移动设备优先方法中，我们首先为移动设备和触控交互进行设计，然后调整并放大设计以适应更大的设备和鼠标交互。这种方法确保移动设备支持完整的功能集，甚至可以在桌面客户端上使用。

这两种方法如今都同样正确、被接受且流行。唯一的例外是，由于近年来访问 Web 的移动设备数量增加，移动设备优先成为一个热门词汇。在整本书中，我们将选择移动设备优先的方法，并针对移动设备和触控支持进行设计。但是，我们当然会扩展可视化效果以支持鼠标输入和更大的屏幕。这就是响应式设计的全部内容。

### Bootstrap – 一个流行的移动设备优先 CSS 框架

如果我们想要创建响应式移动设备优先 Web 应用程序，通常不希望从零开始并重新实现常见的模式和组件；我们更愿意重用一些经过良好测试的现有组件。

Bootstrap 是一个由前 Twitter 员工构建的非常流行的 HTML 和 CSS 框架，用于轻松、快速和现代的前端开发；它提供了大量的响应式 CSS 和 LESS 布局，以及庞大的 HTML 和 JavaScript 组件集合。得益于其 MIT 许可证，它可以用于开源和商业软件。

当前稳定版本 3.3.5 包含具有以下功能的 LESS 样式：

- 容器和栅格（静态和流式）
- 响应式辅助类（在不同断点上隐藏和显示元素）
- LESS mixin 和变量声明
- 常见组件（如表单、输入字段和表格）的样式
- 图标、HTML 和 JavaScript 组件

> 功能完整列表可在 Bootstrap 网站 http://getbootstrap.com/ 上找到。

全新的 4 版本（目前处于 alpha 阶段）将附带 SASS 布局以及许多其他出色的功能和改进：

> GitHub 上还有一个针对 SASS 预编译器的官方分支，地址为 https://github.com/twbs/bootstrap-sass。

- 所有单位都使用 rem 和 em
- 添加了新的 xl 栅格层级
- 可选的 flexbox 支持
- 提供了 Reboot 模块以重置默认浏览器样式

> 新功能完整列表可在 Bootstrap 博客 http://blog.getbootstrap.com/2015/08/19/bootstrap-4-alpha/ 上找到。

在整本书中，除非另有说明，我们将对所有示例和解释使用 Bootstrap 3.3.5 版本（带 LESS）。在适当的时候，我们还会提供关于最新版本的一些提示。

### 引入 Bootstrap

确保已安装 Bower 包管理器；可以通过在终端中运行以下命令来完成：

```
npm install -g bower
```

关于 Bower 包管理器的更多信息可以在其网站 http://bower.io/ 上找到。

在整本书中，我们将仅使用 Bootstrap 的 CSS 组件，因此只需要将 bootstrap.css 文件引入项目。有多种方法可以做到这一点：

- 使用名为 bower 的包管理器（推荐）——首先，通过在终端中运行以下命令获取库：bower install bootstrap#3.3.5。然后，在网页的 head 部分从 bower_components/ 目录链接压缩的 CSS 文件：<link rel="stylesheet" href="bower_components/bootstrap/dist/css/bootstrap.min.css">。

- 使用内容分发网络（CDN），例如 MaxCDN（这在开发期间需要活动的 Internet 连接）。只需在网页的 head 部分包含来自 CDN 的压缩 CSS 文件：<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css">。

- 手动，从 GitHub (https://github.com/twbs/bootstrap/releases) 下载 3.3.5 版本并将其解压到 libs/ 目录。然后，在网页的 head 部分从 libs/ 目录链接压缩的 CSS 文件：<link rel="stylesheet" href="libs/bootstrap/dist/css/bootstrap.min.css">。

我强烈建议使用 bower 包管理器来管理您的前端依赖项（或类似的工具，例如 npm 或 component）。但是，在本书的示例中，出于简单性考虑，我们将直接从 MaxCDN 链接 Bootstrap；因此，本书源代码中的所有示例都需要活动的 Internet 连接。

### 查找其他资源

Bootstrap 的终极资源是可在 http://getbootstrap.com/getting-started/ 上找到的文档。在此，您还可以找到使用 Bootstrap 框架的示例和模板。

