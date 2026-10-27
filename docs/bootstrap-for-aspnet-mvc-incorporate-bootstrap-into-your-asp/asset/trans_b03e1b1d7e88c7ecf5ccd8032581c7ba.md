

```
parameters,
skipIfExists: false);
}
}
```

+   21. 在前面的代码中，我们将用户在表单中捕获的值添加到 `parameters` 集合中。我们还通过使用 `IEntityFrameworkService` 收集了所选对象的全部元数据。该服务期望其 `AddRequiredEntity` 方法的参数之一为上下文类型名称，因此我们将传入用户在表单上选择的 `DbContext` 对象的名称。
+   
+   22. 然后我们将检查用户是否将生成的项目添加到文件夹中，并做出一个小的假设，即所选文件夹的父文件夹将是 Views 文件夹。然后我们将调用 `AddFileFromTemplate` 方法。
+   
+   23. 在 UI 和视图模型就绪后，打开 `CustomTextTemplate.cs.t4` 文件。
+   
+   24. 模板文件的顶部部分指定了模板语言，在本例中为 C#，以及模板需要输出的扩展名。由于我们要生成一个视图，这应为 `.cshtml`。
+   
+   25. 我们还将在顶部声明将传递到模板的所有变量名。以下内容应添加到模板文件的顶部：

```
<#@ template language="C#" #>
<#@ output extension=".cshtml" #>
<#@ assembly name="System.Core" #>
<#@ assembly name="EnvDTE" #>
<#@ include file="Imports.include.t4" #>
<#@ parameter name="ModelType" type="EnvDTE.CodeType" #>
<#@ parameter name="Title" type="System.String" #>
<#@ parameter name="ControllerName" type="System.String" #>
<#@ parameter name="ViewName" type="System.String" #>
<#@ parameter name="ModelMetadata" type="Microsoft.AspNet.Scaffolding.Core.Metadata.ModelMetadata" #>
```

+   26. 模板其余部分的 T4 标记如下：

```
@model <#= ModelType.Namespace.FullName #>.<#= ModelType.Name #>

@{
    ViewBag.Title = "<#= Title #>";
}
```

## 创建 T4 模板以生成 Bootstrap 视图

```
Layout = "~/Views/Shared/_Layout.cshtml";
}

<div class="container">
    <div class="page-header">
        <h1><#= Title #></h1>
    </div>

    <ol class="breadcrumb">
        <li>@Html.ActionLink("Home", "Index", "Home")</li>
        <li class="active"><#= Title #></li>
    </ol>

    <div class="row clearfix">
        <div class="col-md-12">
            @using (Html.BeginForm("<#= ViewName #>", "<#= ControllerName #>", FormMethod.Post, new { role = "form" }))
            {
                @Html.AntiForgeryToken()
                @Html.ValidationSummary(true)

                <#
                    foreach (PropertyMetadata property in ModelMetadata.Properties)
                    {
                    #>
                        <div class="form-group">
                            @Html.LabelFor(model => model.<#= property.PropertyName #>)
                        </div>
                    <#
                    }
                #>
        </div>
    </div>
</div>
```

# 将 Bootstrap HTML 模板转换为可用的 ASP.NET MVC 项目

使用 Bootstrap 的主要好处之一是互联网上可用的资源种类繁多。Web 开发社区接受了 Bootstrap，你会发现大量有价值的模板、代码片段以及使用 Bootstrap 的建议。

通过结合预设计的 Bootstrap 模板和 ASP.NET MVC，你可以节省大量时间，而无需担心网站布局或设计。

在本章中，我们将涵盖以下主题：

+   - 为什么我们使用预建的 HTML 模板以及它们如何节省时间
+   - 构建主布局
+   - 添加特定的页面视图
+   - 在视图中包含图表

# 使用预建的 HTML 模板

众所周知，大多数开发人员不一定是优秀的设计师。我们更喜欢在后端工作，构建性能卓越且智能的软件，有时我们倾向于把用户界面视为事后才考虑的事情。

通过使用预设计的 HTML Bootstrap 模板，我们可以为用户提供直观且设计精良的用户界面，该界面由专业设计师设计。如果设计基于 Bootstrap，开发人员已经熟悉大部分 CSS 类名、组件和插件，无需重新学习任何东西。

Web 提供了各种免费和付费的 Bootstrap 模板。ThemeForest（www.themeforest.net）提供了令人眼花缭乱的各种付费网站样式和设计。

在本章的示例中，我们将使用 Start Bootstrap（www.startbootstrap.com）设计的免费 SB Admin 2 模板。SB Admin 2 模板是一个使用 Bootstrap 3 的管理主题，非常适合后端管理或更复杂风格的 Web 应用程序。该主题如下图所示：

![](img/371b9a67300a03b7b335eb8a6fe55d94_142_0.png)

在我们使用该模板构建 ASP.NET MVC 网站之前，我们需要通过完成以下步骤下载源文件：

+   1. 导航到 `http://startbootstrap.com/sb-admin-v2` 并单击 **Download** 按钮，以下载包含所有必要 HTML、CSS 和 JavaScript 文件的 zip 归档。
+   2. 将文件提取到本地硬盘上的文件夹；你会注意到归档包含熟悉的 Bootstrap 文件夹：
+      - `css`
+      - `font-awesome`
+      - `fonts`
+      - `js`
+   3. 归档还包含许多 HTML 文件，展示了模板的各种页面和组件布局，如下图所示：

![](img/371b9a67300a03b7b335eb8a6fe55d94_143_0.png)

# 创建 ASP.NET MVC 项目

要创建新的 ASP.NET MVC 项目，请执行以下步骤：

+   1. 在 Visual Studio 中，创建新的 **ASP.NET Web Application** 项目，如下图所示：

![](img/371b9a67300a03b7b335eb8a6fe55d94_144_0.png)

+   2. 在 **New ASP.NET Project** 对话框中，选择 **Empty** 模板，勾选 **MVC** 复选框，然后单击 **OK** 按钮。

![](img/371b9a67300a03b7b335eb8a6fe55d94_144_1.png)

+   3. Visual Studio 将创建一个默认的空 MVC 项目。右键单击项目名称并导航到 Add | New Folder。创建以下四个文件夹：
+       - css
+       - font-awesome
+       - fonts
+       - js
+   4. 将 SB Admin 源 css 文件夹中的以下 .css 文件添加到项目中的 css 文件夹：
+       - bootstrap.css
+       - sb-admin.css

5. 将 SB Admin 源 font-awesome\css 文件夹中的 font-awesome.css 文件添加到项目中的 css 文件夹。

6. 将 SB Admin 源 fonts 和 font-awesome\fonts 文件夹中的所有文件添加到项目中的 fonts 文件夹。

+   7. 将 SB Admin 源 js 文件夹中的以下文件添加到项目中的 js 文件夹：
+       - bootstrap.js
+       - sb-admin.js

# 创建主布局

我们已添加创建项目主布局文件所需的 CSS、JavaScript 和字体文件。接下来，我们需要创建一个 home 控制器以及一个主布局文件。为此，完成以下步骤：

+   1. 右键单击 Controllers 文件夹并导航到 Add | Controller....
+   2. 从 Add Scaffold 对话框窗口中选择 MVC 5 Controller - Empty，然后单击 Add。

3. 在 Add Controller 对话框窗口的 Controller name 文本框中输入 HomeController，然后单击 Add。

![](img/371b9a67300a03b7b335eb8a6fe55d94_146_0.png)

4. 接下来，右键单击项目中的 Views 文件夹并导航到 Add | New Folder。将文件夹命名为 Shared。

5. 右键单击新创建的 Shared 文件夹并导航到 Add | MVC 5 Layout Page (Razor)。

6. 在 Specify Name for Item 对话框中，在 Item name 文本框中输入 _Layout，然后单击 OK。

![](img/371b9a67300a03b7b335eb8a6fe55d94_146_1.png)

7. 打开 SB Admin 源文件中的 blank.html 文件并将其内容复制到 _Layout.cshtml 文件。

8. 将 <head> 标签更改为以下代码：

```
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Start Bootstrap - SB Admin Version 2.0 :: @ViewBag.Title</title>
    <link href="@Url.Content("~/css/bootstrap.css")" rel="stylesheet">
    <link href="@Url.Content("~/css/font-awesome.css")" rel="stylesheet">
    <link href="@Url.Content("~/css/sb-admin.css")" rel="stylesheet">
</head>
```

