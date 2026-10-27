

```
closeButton.Attributes.Add("data-dismiss", "alert");
closeButton.InnerHtml = "&times;";
return closeButton;
}
```

14. AddCloseButton 方法还使用 TagBuilder 对象来构建必要的 HTML，以便在 Bootstrap Alert 类内部渲染一个关闭按钮。

## 在视图中使用流畅的 HTML 帮助器

为了在视图中使用流畅的 HTML 帮助器，我们需要向 Alerts 文件夹中添加最后一个名为 AlertHelper.cs 的文件。这个类将向 HtmlHelper 对象添加一个扩展方法，以便渲染 Bootstrap Alert 类。向类中添加一个名为 Alert 的新方法。此方法应返回一个 Alert 对象，并且它将接受两个参数。第一个参数必须以 this 关键字开头，用于指示扩展将扩展哪个类。第二个参数将是在 Bootstrap Alert 类内部显示的实际文本。

Alert 类的代码如下：

```
public static class AlertHelper
{
    public static Alert Alert(this HtmlHelper html,string message)
    {
        return new Alert(message);
    }
}
```

打开一个视图，并添加以下标记，以渲染一个可关闭的 Bootstrap 警告框，该警告框将显示危险样式的消息：

```
@Html.Alert("Core meltdown imminent").Danger().Dismissible()
```

该警告框在浏览器中的显示效果类似于以下截图：

![](img/371b9a67300a03b7b335eb8a6fe55d94_115_0.png)

## 创建自闭合帮助器

自闭合帮助器是可以包含 HTML 和 Razor 标记的帮助器。内置的 @Html.BeginForm() 帮助器就是这类帮助器的一个示例。

为了创建此类 HTML 帮助器，我们需要创建一个实现 IDisposable 接口的帮助器类。利用 IDisposable 接口，我们可以在对象被释放时编写元素的结束标签。

Bootstrap Panel 组件是此类帮助器的绝佳候选。要创建该帮助器，我们需要完成以下步骤：

- 1\. 在项目的 Helpers 文件夹中创建一个名为 Panels 的新子文件夹。
- 2\. 打开 Enums.cs 文件，并添加一个名为 PanelStyle 的新项：
- 3\. 向 Panels 文件夹添加一个名为 Panel.cs 的新类文件。
- 4\. 向类中添加一个名为 _writer 的私有只读字段。
- 5\. 为 Panel 类创建一个构造函数，该构造函数接受三个参数。第一个参数是对 HtmlHelper 对象的引用，第二个参数指定面板的标题，第三个参数指示面板的样式。
- 6\. 向 Panel 类的构造函数添加以下代码：

```
public enum PanelStyle
{
    Default,
    Primary,
    Success,
    Info,
    Warning,
    Danger
}
```

```
public Panel(HtmlHelper helper, string title, Enums.PanelStyle style = Enums.PanelStyle.Default)
{
    _writer = helper.ViewContext.Writer;

    var panelDiv = new TagBuilder("div");
    panelDiv.AddCssClass("panel-" + style.ToString().ToLower());
    panelDiv.AddCssClass("panel");

    var panelHeadingDiv = new TagBuilder("div");
    panelHeadingDiv.AddCssClass("panel-heading");
```

```
var heading3Div = new TagBuilder("h3");
heading3Div.AddCssClass("panel-title");
heading3Div.SetInnerText(title);

var panelBodyDiv = new TagBuilder("div");
panelBodyDiv.AddCssClass("panel-body");

panelHeadingDiv.InnerHtml = heading3Div.ToString();

string html = string.Format("{0}{1}{2}", panelDiv.ToString(TagRenderMode.StartTag), panelHeadingDiv, panelBodyDiv.ToString(TagRenderMode.StartTag));
_writer.Write(html);
}
```

在这段代码中，我们将 _writer 字段设置为 HtmlHelper 对象的 ViewContext 属性的 Writer 属性。通过使用此属性，我们将能够向当前视图写入 HTML。

然后，我们使用 TagBuilder 对象构建面板元素的 HTML 标记，最后将结果发送到 _writer 对象以进行输出。请注意，我们使用了 TagBuilder 的重载 ToString 方法，以指定它只应生成 `<div>` 元素的开始标签。

两个类名为 panel 和 panel-body 的 `<div>` 元素将在 Panel 类的 Dispose 方法中关闭，该方法的实现方式如下：

```
public void Dispose()
{
    _writer.Write("</div></div>");
}
```

## 在视图中使用自闭合帮助器

为了在我们创建的视图中使用该帮助器，我们将使用 C# 的 using 关键字。using 关键字限制了对象的作用域，因此与 IDisposable 接口配合得很好。实际上，一旦帮助器完成其 HTML 输出的渲染，Dispose 方法将被自动调用，从而关闭最后两个 `<div>` 元素标签。

要在视图中使用该帮助器，请使用以下标记：

```
@using (Html.Panel("Title", Enums.PanelStyle.Success))
{
    <p>Hello World</p>
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit.</p>
}
```

帮助器将生成所需的 HTML，以在浏览器中显示以下面板：

![](img/371b9a67300a03b7b335eb8a6fe55d94_118_0.png)

## 总结

在本章中，我们探讨了如何使用 HTML 帮助器减少视图中的标记数量。我们还了解了如何编写帮助器，使不熟悉 Bootstrap 框架的开发人员能够使用帮助器为其视图添加样式化组件。

在下一章中，我们将深入探讨如何使用 Bootstrap 样式和布局生成正确样式的脚手架视图。

## 创建 T4 模板以生成 Bootstrap 视图脚手架

Visual Studio 2013 已经包含用于 ASP.NET MVC 和 Web API 项目的代码生成器，使用这些内置的脚手架模板可以大幅减少你在创建标准视图和控制器操作上花费的时间。在 Visual Studio 2013 中向 ASP.NET MVC 项目添加标准控制器时，你已经在使用脚手架，这一点可以通过用于添加新控制器的对话框标题为 **添加脚手架** 来体现：

![](img/371b9a67300a03b7b335eb8a6fe55d94_120_0.png)

## T4 模板

T4 代表 Text Template Transformation Toolkit（文本模板转换工具包），是微软基于模板的文本生成框架。T4 是控制逻辑和文本块的组合，可以生成任何类型的文本文件，包括 C# 或 VB 源文件。

T4 代码生成器内置于 Visual Studio 中，用于生成内置的 ASP.NET MVC 模板，包括标准的 MVC 和 Web API 控制器模板。

> Oleg Sych 撰写了许多关于如何使用 T4 模板以及如何在您自己的开发项目中使用它获得最佳结果的非常有价值的博客文章。要了解更多信息，您可以访问他的博客 `http://bit.ly/OlegSychT4`。

不幸的是，Visual Studio 在编辑 T4 模板时不提供语法高亮或 IntelliSense 支持，但有可用的免费工具可以将这些功能添加到 Visual Studio 中。

## T4 工具

由于 Visual Studio 默认不提供 T4 IntelliSense，我们需要安装第三方扩展来为我们启用此功能。幸运的是，有两个免费的替代方案可用。如下所示：

- 用于 Visual Studio 的 Devart T4 编辑器（`http://bit.ly/DevArtT4`）
- tangible T4 编辑器（`http://bit.ly/TangibleT4`）

这两个扩展都提供类似的功能，但对于本书中的 T4 示例，我们将使用 tangible T4 编辑器。

另一个对于简化 T4 模板创建至关重要的免费扩展是用于 Visual Studio 的 **SideWaffle** 扩展。SideWaffle 将向 Visual Studio 添加许多极其有用的项模板，我们将使用这些模板来构建自己的 ASP.NET MVC 脚手架视图。您可以从 `http://www.sidewaffle.com/` 下载 SideWaffle。

> SideWaffle 不仅为 T4 提供项模板，还提供一系列项目和项模板，以及许多非常有用的代码片段。它是 GitHub 上一个积极维护的开源项目，对于任何 .NET 开发人员来说都是不可或缺的工具。

## T4 语法

T4 模板有一种非常特殊的语法，乍一看可能令人生畏，但一旦习惯了，它就相对容易理解和使用。T4 使用三个不同的标签来表示代码块。

