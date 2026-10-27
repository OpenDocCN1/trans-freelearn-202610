

![](img/371b9a67300a03b7b335eb8a6fe55d94_51_0.png)

## Bootstrap 按钮

Bootstrap 提供了种类繁多的按钮，具有多种颜色和尺寸。核心按钮提供五种颜色和四种尺寸的选择。按钮的颜色和尺寸通过其 class 属性应用。用于设置按钮尺寸的类列表如下：

- `btn btn-default btn-xs`
- `btn btn-default btn-sm`
- `btn btn-default`
- `btn btn-default btn-lg`

## 使用 Bootstrap CSS 和 HTML 元素

要创建从特小到大的四个白色/默认按钮，你需要实现以下 HTML 标记：

```html
<div class="row">
    <!-- 标准按钮 -->
    <button type="button" class="btn btn-default btn-xs">Default Extra Small</button>
    <button type="button" class="btn btn-default btn-sm">Default Small</button>
    <button type="button" class="btn btn-default">Default</button>
    <button type="button" class="btn btn-default btn-lg">Default Large</button>
</div>
```

按钮颜色同样通过类名指定。以下是可用的颜色类名列表：

- `btn-default`
- `btn-primary`
- `btn-success`
- `btn-info`
- `btn-warning`

按钮的范围如下图所示：

![](img/371b9a67300a03b7b335eb8a6fe55d94_52_0.png)

在之前的截图中，按钮的边缘是方形的，而不是默认 Bootstrap 捆绑包中的圆角。这是因为示例站点使用了自定义的 Bootstrap 样式。你可以从 http://bootswatch.com/ 下载各种不同的 Bootstrap 样式。

## 表单布局与元素

表单构成了大多数业务线应用程序的很大一部分；因此，为 Web 应用程序中的所有表单应用统一样式，不仅在视觉上令人愉悦，还能为用户提供更友好的界面。Bootstrap 提供了一系列 CSS 样式，使你能够创建视觉上吸引人的表单。

### 水平表单

用于 ASP.NET MVC 脚手架生成的默认创建和编辑模板会生成水平样式的 Bootstrap 表单。这是通过自动将 `form-horizontal` 类名添加到 `<form>` 元素来实现的。在以下代码示例中，使用 `Html.BeginForm` 辅助方法创建了一个水平登录表单；该表单包含两个文本框和一个提交按钮：

```
@using (Html.BeginForm("Login", "Account", FormMethod.Post, new {
@class = "form-horizontal", role = "form" }))
{
    <div class="form-group">
        @Html.LabelFor(m => m.UserName, new { @class = "col-md-2 control-label" })
        <div class="col-md-10">
            @Html.TextBoxFor(m => m.UserName, new { @class = "form-control" })
            @Html.ValidationMessageFor(m => m.UserName)
        </div>
    </div>
    <div class="form-group">
        @Html.LabelFor(m => m.Password, new { @class = "col-md-2 control-label" })
        <div class="col-md-10">
            @Html.PasswordFor(m => m.Password, new { @class = "form-control" })
            @Html.ValidationMessageFor(m => m.Password)
        </div>
    </div>
}
```

```
@Html.LabelFor(m => m.UserName, new { @class = "sr-only" })
@Html.TextBoxFor(m => m.UserName, new { @class = "form-control", placeholder = "Enter your username" })
@Html.ValidationMessageFor(m => m.UserName)
</div>
<div class="form-group">
    @Html.LabelFor(m => m.Password, new { @class = "sr-only" })
    @Html.PasswordFor(m => m.Password, new { @class = "form-control", placeholder = "Enter your username" })
    @Html.ValidationMessageFor(m => m.Password)
</div>
<div class="form-group">
    <input type="submit" value="Log in" class="btn btn-default" />
</div>
}
```

这将在浏览器中渲染以下表单：

![](img/371b9a67300a03b7b335eb8a6fe55d94_56_0.png)

### Bootstrap 验证样式

默认的 ASP.NET MVC 项目模板支持不显眼的验证，并自动将所需的 JavaScript 库添加到项目中。但是，你会注意到默认验证不使用 Bootstrap 特定的 CSS 样式。当输入元素验证失败时，jQuery 验证插件会将其类名更改为 `input-validation-error`。该类名在 `Site.css` 文件中声明，并且仅在无效输入元素周围绘制红色边框，如下面的截图所示：

![](img/371b9a67300a03b7b335eb8a6fe55d94_57_0.png)

然而，Bootstrap 的验证样式会将无效输入元素关联的标签字体粗细设置为粗体，并在元素周围绘制红色边框。jQuery 验证插件无法与 Bootstrap 验证样式配合工作的原因是，Bootstrap 并不将验证样式应用于输入元素，而是将 `has-error` 类名添加到父元素上：

```html
<div class="form-group has-error">
    <label class="control-label" for="name">Name</label>
    <input type="text" class="form-control" id="name">
</div>
```

在此代码中，父 `<div>` 元素的类被更改为 `form-group has-error`。

为了使我们的表单在不直接修改 jQuery 验证插件的情况下使用 Bootstrap 验证类，我们首先需要向 Scripts 文件夹添加一个名为 `jquery.validate.bootstrap.js` 的新 JavaScript 文件。

以下代码将通过调用 `setDefaults` 函数修改 jQuery 验证插件的默认设置。然后我们使用 `highlight` 函数指定无效元素应如何突出显示。在此函数中，我们告诉验证插件应找到具有 `.form-group` 类的最接近的元素，并将 `has-error` 类添加给它。在 `unhighlight` 函数中，我们移除 `has-error` 类，如下所示：

```javascript
$.validator.setDefaults({
    highlight: function (element) {
        $(element).closest('.form-group').addClass('has-error');
    },
    unhighlight: function (element) {
        $(element).closest('.form-group').removeClass('has-error');
    }
});
```

![](img/371b9a67300a03b7b335eb8a6fe55d94_58_0.png)

> 你可以从其项目站点 `http://jqueryvalidation.org/` 了解更多关于 jQuery 验证插件及其各种设置的信息。

接下来，我们需要将新的 JavaScript 文件添加到 `jqueryval` 捆绑包中，该捆绑包在 `BundleConfig.cs` 文件中指定。当前的 `jqueryval` 捆绑包使用通配符将所有以 `jquery.validate` 开头的文件添加进来。我们需要更改这一点，以明确指定每个文件，因为通配符捆绑是按字母顺序排列的，而 `jquery.validate.bootstrap.js` 文件应在 jQuery 验证库之后加载。以下代码将把所有必需的验证库和文件添加到我们的捆绑包中：

```csharp
bundles.Add(new ScriptBundle("~/bundles/jqueryval").Include(
    "~/Scripts/jquery.validate.js",
    "~/Scripts/jquery.validate.unobtrusive.js",
    "~/Scripts/jquery.validate.bootstrap.js"));
```

在构建和运行项目时，你现在应该会看到 Bootstrap 验证样式应用于无效的表单元素，如下面的截图所示：

## 为原始类型创建编辑器模板

编辑器模板用于根据对象属性的数据类型自动生成表单输入元素。ASP.NET MVC 包含许多标准编辑器模板，但开发人员也可以创建自己的模板。编辑器模板类似于分部视图，但分部视图是按名称渲染的，而编辑器模板是按类型渲染的。

当使用 Visual Studio 的默认脚手架功能为模型创建编辑视图时，它会使用 `EditorFor` HTML 辅助方法生成文本输入元素，如下所示：

```
@Html.EditorFor(model => model.CategoryName)
```

`EditorFor` 辅助方法根据模型属性的数据类型渲染其 HTML。对于字符串值，生成的标记将类似于以下内容：

```
<input type="text" name="CategoryName" id="CategoryName" data-val-required="The Category Name field is required." data-val="true" class="text-box single-line">
```

在此生成的标记中，输入元素的类为 `text-box single-line`。为了使文本元素使用 Bootstrap 样式正确渲染，输入元素的类应设置为 `form-control`。

为了使 `EditorFor` 辅助方法生成具有正确类的输入元素，我们需要通过完成以下步骤来创建编辑器模板：

1. 首先，在 Shared 文件夹内创建一个名为 EditorTemplates 的新文件夹。
2. 接下来，向 EditorTemplates 文件夹添加一个名为 string.cshtml 的新文件，并向其中添加以下代码：

```
@model string
@Html.TextBox("", ViewData.TemplateInfo.FormattedModelValue, new {
@class = "form-control"})
```

在前面的代码中，我们创建了一个文本框，并向其传递了一个空字符串作为文本框的名称。这将使 ASP.NET MVC 使用模型的属性名称作为文本框的名称。通过使用 `TemplateInfo` 的 `FormattedModelValue` 属性来指定文本框中显示的值。最后，我们指定元素的 HTML 属性；在这种情况下，我们将其 class 属性设置为 `form-control`。

现在，当使用 `EditorFor` 辅助方法渲染文本框时，生成的 HTML 标记将如下所示：

```
<input type="text" name="CategoryName" id="CategoryName" data-val-required="The Category Name field is required." data-val="true" class="form-control">
```

## 为非原始类型创建编辑器模板

ASP.NET MVC 使开发人员能够为自定义数据类型创建编辑器模板。例如，在下面的截图中，我们有一个包含一个文本框和一个多行文本框的表单：

