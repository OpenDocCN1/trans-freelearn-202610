

## 创建 ASP.NET MVC Bootstrap 助手

ASP.NET MVC 允许开发者通过创建静态或扩展方法来创建自己的 HTML 助手方法。本质上，HTML 助手只是一个返回 HTML 字符串的方法。

HTML 助手使你能够在多个页面上使用相同的常用标记块，并使页面中的代码和标记更易于阅读和维护。这促进了可重用代码的使用，开发者也可以对助手方法进行单元测试。

在本章中，我们将涵盖以下主题：

- 内置 ASP.NET MVC HTML 助手的概述
- 使用 @helper 语法创建助手
- 使用静态方法创建 HTML 助手
- 创建 HTML 助手扩展方法
- 创建流畅 HTML 助手
- 创建自关闭助手

## 内置 HTML 助手

HtmlHelper 是一个在视图中渲染 HTML 内容的方法。其目的是允许开发者在多个页面中重用常见的 HTML 标记块。

ASP.NET MVC 提供了多种标准开箱即用的 HTML 助手。例如，要生成以下带有 ID 和 name 属性为 CustomerName 的文本框的 HTML，请使用以下代码：

```
<input type="text" name="CustomerName" id="CustomerName">
```

你应该使用如下所示的 TextBox 助手：

```
@Html.TextBox("CustomerName")
```

大多数内置 HTML 助手都提供了多个重载版本。例如，要创建文本框并显式设置其 name 和 value 属性，你应该使用以下重载的 TextBox 助手方法：

```
@Html.TextBox("CustomerName", "Northwind Traders")
```

大多数内置助手还提供了指定由元素生成的 HTML 属性的选项，通过传递匿名类型。在下面的示例中，我们将创建一个文本框并使用其中一个重载方法设置其 style 属性：

```
@Html.TextBox("CustomerName","Northwind Traders", new {
style="background-color:Blue;" })
```

> 你可以在 http://bit.ly/MVCFormHelpers 阅读更多关于 ASP.NET MVC 中可用的标准 HTML 助手的信息。

## 创建自定义助手

由于 Bootstrap 提供了各种不同的组件，因此非常适合使用助手。ASP.NET MVC 中最简单的助手形式是你可以使用 @helper 语法创建的助手。自定义助手可以包含任何 HTML 甚至 Razor 标记。要创建生成 Bootstrap 按钮的简单助手方法，请完成以下步骤：

- 1. 在网站根文件夹内创建一个名为 App_Code 的新文件夹。
- 2. 在新创建的 App_Code 文件夹内创建一个名为 BootstrapHelpers.cshtml 的新文件。
- 3. 向 BootstrapHelper.cshtml 文件添加以下代码：

```
@helper PrimaryButtonSmall(string id,string caption)
{
    <button id="@id" type="button" class="btn btn-primary btn-sm">@caption</button>
}
```

此代码使用 @helper 语法创建一个名为 PrimaryButtonSmall 的新助手。助手接受两个参数以指定按钮元素的 id 属性以及按钮的 caption 元素。我们已向按钮元素添加了适当的 Bootstrap 类以渲染蓝色主按钮。任何自定义助手必须位于 App_Code 文件夹内，以便 ASP.NET MVC 在你的视图中识别它。

## 在视图中使用助手

让我们在前一节中创建的助手中使用它。为此，打开站点中的一个视图，并将以下代码添加到页面中：

```
@BootstrapHelpers.PrimaryButtonSmall("myButton","My New Button")
```

当你运行站点并查看使用助手页面的源代码时，你会看到助手生成了正确的 HTML 标记以创建以下主 Bootstrap 按钮：

```
<button class="btn btn-primary btn-sm" type="button" id="myButton">My New Button</button>
```

你会注意到，尽管这种方法有效，但它并不实用，因为这意味着我们必须为每种样式和尺寸的按钮创建 HTML 助手。解决此问题的可能方案是更改我们的助手代码，以便接受一个参数，开发者可以用它指定按钮应具有的样式和尺寸。要实现此目的，请执行以下步骤：

- 1. 打开位于 App_Code 文件夹中的 BootstrapHelpers.cshtml 文件。
- 2. 用以下代码替换文件中的代码：
- ```csharp
@helper Button(string style, string size, string caption, string id)
{
    <button id="@id" type="button" class="btn btn-@style btn-@size">@caption</button>
}
```
- 3. 打开使用助手的视图并添加以下标记：
- ```csharp
@BootstrapHelpers.Button("danger","lg","A Large button","myLargeButton")
```

在前一步中，我们已更新助手以接受两个字符串参数来指定 Bootstrap 按钮的大小和样式。此方法的问题在于，开发者仍然需要知道样式和大小的名称才能渲染正确的按钮。

## 使用静态方法创建助手

创建助手使用我们之前提到的 @helper 语法最简单的替代方法是创建仅返回包含 HTML 标记的字符串的静态方法。

为此，我们需要完成以下步骤：

- 1. 在项目根文件夹内创建一个名为 Helpers 的新文件夹。
- 2. 向此文件夹添加一个名为 Enums.cs 的新类。此文件将包含我们项目的枚举器。
- 3. 向 Enums.cs 文件添加以下代码：
- 4. 在 Helpers 文件夹中创建一个名为 ButtonHelper.cs 的新类。
- 5. 向 ButtonHelper 类添加一个名为 Button 的方法，并向其中添加以下代码：

```
public class Enums
{
    public enum ButtonStyle
    {
        Default,
        Primary,
        Success,
        Info,
        Warning,
        Danger,
        Link
    }

    public enum ButtonSize
    {
        Large,
        Small,
        ExtraSmall,
        Normal
    }
}
```

```
public static MvcHtmlString Button(string caption, Enums.ButtonStyle style, Enums.ButtonSize size)
{
    if (size != Enums.ButtonSize.Normal)
    {
        return new MvcHtmlString(string.Format("<button type=\"button\" class=\"btn btn-{0} btn-{1}\">{2}</button>", style.ToString().ToLower(), ToBootstrapSize(size), caption));
    }
```

```
}
return new MvcHtmlString(string.Format("<button type=\"button\" class=\"btn btn-{0}\">{1}</button>", style.ToString().ToLower(), caption));
}
```

- 6. 最后，添加另一个名为 ToBootstrapSize 的方法：

```
private static string ToBootstrapSize(Enums.ButtonSize size)
{
    string bootstrapSize = string.Empty;
    switch (size)
    {
        case Enums.ButtonSize.Large:
            bootstrapSize = "lg";
            break;

        case Enums.ButtonSize.Small:
            bootstrapSize = "sm";
            break;

        case Enums.ButtonSize.ExtraSmall:
            bootstrapSize = "xs";
            break;
    }
    return bootstrapSize;
}
```

我们之前创建的 Button 方法接受三个参数以设置按钮的 caption、大小和样式。我们使用在 Enums.cs 文件中声明的枚举值来列出按钮可用的大小和样式——这使开发者无需记住每个 Bootstrap 类名。

Button 方法返回一个 MvcHtmlString 对象，该对象在 .NET 4 中引入。MvcHtmlString 对象表示一个 HTML 编码的字符串，无需再次编码。如果我们仅返回一个普通字符串对象，实际的 HTML 将在视图中呈现，而不是按钮。

ToBootstrapSize 方法基本上将 ButtonSize 值转换为表示按钮大小的有效 Bootstrap 类名。结果在浏览器中看起来类似于以下屏幕截图：

![](img/371b9a67300a03b7b335eb8a6fe55d94_109_0.png)

