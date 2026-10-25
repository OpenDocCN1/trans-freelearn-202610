## 前言

Kendo UI 是一个基于 HTML 和 jQuery 的客户端框架，可让你为 Web 和移动平台构建 Web 应用程序。该库包含大量小部件，可轻松配置以快速构建应用程序。除了 Web 和移动平台的小部件之外，该库还提供众多数据可视化组件，可让你构建复杂的图表、图表、交互式地图以及条形码和二维码生成器。

## 本书涵盖的内容

_第 1 章，Kendo UI 应用程序框架_，向你介绍框架中的核心组件，例如客户端模板、使用模型 - 视图 - 视图模型 (MVVM) 模式以及使用内置验证器验证表单字段。

_第 2 章，Kendo UI Grid_，向你介绍用于以表格格式显示数据的 Grid 组件，并包含各种功能，例如基于选定列的排序、筛选符合特定条件的数据以及更新数据。

_第 3 章，Kendo UI TreeView_，向你介绍用于显示层次结构数据（例如目录结构）的 TreeView 小部件。该小部件可以通过在 TreeView 中的组件前加上复选框或使用带前缀的图像来表示它们来自定义。

_第 4 章，Kendo UI Editor_，向你介绍允许你创建所见即所得 (WYSIWYG) 界面的 Editor 小部件。此界面包含可添加到工具栏的多个工具。

_第 5 章，Kendo UI PanelBar_，向你介绍用于构建类似手风琴布局的 PanelBar 组件。此布局允许你对数据进行分组并将其垂直堆叠。

_第 6 章，Kendo UI 文件上传器_，向你介绍允许你将多个文件上传到服务器并跟踪其进度的 Upload 小部件。它为所有浏览器提供一致的界面。

_第 7 章，Kendo UI 窗口_，向你介绍用于创建弹出窗口或模态窗口以显示警报消息的 Window 小部件。

第 8 章，Kendo UI 移动框架，向你介绍移动框架，其中包括用于构建移动应用程序的核心移动组件。这还包括创建布局和视图、在视图之间导航以及添加触摸事件。

第 9 章，Kendo UI 移动小部件，向你介绍 Kendo UI 移动库中的移动小部件。这些小部件可让你快速构建移动应用程序。这包括使用 ListView、ActionSheet、TabStrip 和 SplitView 等小部件用于平板电脑应用程序。

第 10 章，Kendo UI DataViz，向你介绍允许你轻松创建交互式图表的基本数据可视化组件。这些组件还可以自定义以匹配所需的外观和感觉。

第 11 章，Kendo UI DataViz – 高级图表，向你介绍允许你轻松构建类似仪表板的应用程序的高级数据可视化组件。这包括创建股票图表、创建径向和线性仪表、生成条形码和二维码、创建类似流程图的图表、创建表示层次结构的图表以及创建显示地理空间数据的地图。

## 本书所需内容

本书包含大量使用 Web、移动和数据可视化组件构建应用程序的配方。你需要从 www.kendoui.com 下载 Kendo UI 框架。该库依赖于 jQuery 框架，因此应在包含 Kendo UI 库之前加载它。

需要具备使用 HTML、CSS 和 JavaScript 构建 Web 应用程序的基本知识。如果你在项目中使用过 jQuery，将会有所帮助。

## 本书适用人群

本书将对了解 HTML 和 JavaScript 并希望提升使用 Kendo UI 库构建应用程序技能的 Web 开发者大有裨益。

该库提供 Web、移动和数据可视化小部件，可轻松配置以匹配应用程序的需求。

## 约定

在本书中，你会发现许多文本样式，用于区分不同类型的信息。以下是这些样式的一些示例及其含义说明。

文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号显示如下："The Kendo UI library exports an object, kendo, which is a namespace for various other objects and functions."





一段代码设置如下：

var template = kendo.template("Full Name: " +
"<span> #= lastName # </span>," +
"<span> #= firstName # </span>");

当我们希望将您的注意力引向代码块中的特定部分时，相关的行或项目会以粗体显示：

<div
data-role="view"
data-layout="defaultLayout">
<div
id="touchSurface"
data-role="touch"
data-enable-swipe="1"
data-touchstart="touchstart"
**data-swipe="swipe"**
data-tap="tap"
data-doubletap="doubletap"
data-hold="hold"
style="height: 200px;">
Touch Surface
</div>
</div>

任何命令行输入或输出均按如下形式编写：

Directory1 is checked: no

**新术语**和**重要词汇**以粗体显示。您在屏幕、菜单或对话框中看到的词语，例如，会在文本中以如下形式出现：“例如，如果未指定名字，则消息将显示为 **First Name is required.**”

![](img/2a66507445090790d34dc5588b56b24c_14_0.png)

![](img/2a66507445090790d34dc5588b56b24c_14_1.png)

## 读者反馈

我们始终欢迎读者的反馈。请告诉我们您对本书的看法——您喜欢或不喜欢的地方。读者反馈对我们开发您能从中获得最大收益的书籍至关重要。

如需向我们发送一般性反馈，只需发送电子邮件至 <feedback@packtpub.com>，并在邮件主题中注明书名。

如果您在某个主题方面有专长，并有兴趣撰写或参与编写书籍，请参阅我们在 www.packtpub.com/authors 上的作者指南。

## 客户支持

既然您是 Packt 图书的自豪拥有者，我们有许多资源可以帮助您充分利用您的购买。

## 下载示例代码

您可以从 http://www.packtpub.com 的帐户中下载已购买的所有 Packt 图书的示例代码文件。如果您在其他地方购买了本书，可以访问 http://www.packtpub.com/support 并注册，以便文件直接发送到您的电子邮件。

## 勘误

尽管我们已尽一切努力确保内容的准确性，但错误难免会发生。如果您在我们的某本书中发现错误——也许是文本或代码中的错误——我们将不胜感激如果您能将其报告给我们。这样做，您可以帮助其他读者避免沮丧，并帮助我们改进本书的后续版本。如果您发现任何勘误，请访问 http://www.packtpub.com/submit-errata，选择您的书籍，单击 **errata submission form** 链接，并输入您的勘误详细信息。一旦您的勘误得到验证，您的提交将被接受，勘误将上传到我们的网站，或添加到该书名下勘误部分的任何现有勘误列表中。任何现有勘误都可以通过从 http://www.packtpub.com/support 选择您的书名来查看。

### 盗版

互联网上受版权保护材料的盗版是跨所有媒体的持续问题。在 Packt，我们非常重视对我们的版权和许可的保护。如果您在互联网上以任何形式发现我们作品的非法副本，请立即向我们提供位置地址或网站名称，以便我们可以采取补救措施。

请通过 <copyright@packtpub.com> 联系我们，并提供涉嫌盗版材料的链接。

我们感谢您帮助保护我们的作者，以及我们为您提供有价值内容的能力。

### 问题

如果您在本书的任何方面遇到问题，可以通过 <questions@packtpub.com> 联系我们，我们将竭尽全力予以解决。
