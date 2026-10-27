

将参数设置为 `PagedListRenderOptions.TwitterBootstrapPagerAligned` 将显示两个按钮，标题为 **Older** 和 **Newer**。它们与容器的边缘对齐，如以下屏幕截图所示：

![](img/371b9a67300a03b7b335eb8a6fe55d94_75_0.png)

将参数设置为 `PagedListRenderOptions.TwitterBootstrapPager` 将在表格下方居中添加 **Previous** 和 **Next** 按钮：

![](img/371b9a67300a03b7b335eb8a6fe55d94_75_1.png)

`PagedList` 库开箱即支持默认的 Bootstrap 样式；这意味着您无需执行任何特定操作即可启用正确的样式。

## 输入组

输入组是向用户提供有关您期望他们在特定表单元素中输入的数据的额外信息的一种方法。Bootstrap 提供类可在输入元素之前或之后添加部分。这些部分可以包含文本或 Bootstrap 中包含的 200 个 Glyphicons 中的任何一个。

> Bootstrap 预装了 200 个 Glyphicon Halfling 图标。通常，设置的 Glyphicon Halfling 图标不是免费的，但其创建者将其免费提供给 Bootstrap。您可以在 http://getbootstrap.com/components/ 上阅读有关它们的更多信息。

要创建文本输入元素以提示用户需要在字段中输入电话号码，我们将使用以下标记：

```html
<div class="form-group">
    @Html.LabelFor(model => model.Phone, new { @class = "control-label col-md-2" })
    <div class="col-sm-4 input-group">
        <span class="input-group-addon">
            <span class="glyphicon glyphicon-phone-alt"></span>
        </span>
        @Html.EditorFor(model => model.Phone)
        @Html.ValidationMessageFor(model => model.Phone)
    </div>
</div>
```

此标记的结果将是左侧带有灰色区域的文本输入元素，其中包含电话图像，如以下屏幕截图所示：

![](img/371b9a67300a03b7b335eb8a6fe55d94_76_0.png)

我们还可以创建在文本输入右侧对齐并包含文本而不是图像的输入组。例如，以下标记创建了一个文本输入字段，提示用户我们只需要电子邮件地址的第一部分，最后部分将自动附加：

```html
<div class="form-group">
    @Html.LabelFor(model => model.Email, new { @class = "control-label col-md-2" })
    <div class="col-sm-4 input-group">
        @Html.EditorFor(model => model.Email)
        <span class="input-group-addon">@@northwind.com</span>
    </div>
</div>
```

前述标记的结果将在浏览器中看起来像以下屏幕截图：

![](img/371b9a67300a03b7b335eb8a6fe55d94_77_0.png)

您还可以使用以下代码创建两侧都有灰色区域的文本输入：

```html
<div class="form-group">
    @Html.LabelFor(model => model.UnitPrice, new { @class = "col-sm-2 control-label" })
    <div class="col-sm-2 input-group">
        <span class="input-group-addon">$</span>
        @Html.TextBoxFor(model => model.UnitPrice, new { @class = "form-control" })
        <span class="input-group-addon">.00</span>
        @Html.ValidationMessageFor(model => model.UnitPrice)
    </div>
</div>
```

结果将在浏览器中看起来像以下屏幕截图：

![](img/371b9a67300a03b7b335eb8a6fe55d94_79_0.png)

## 警告

警报组件用于向用户提供视觉反馈。这可用于向用户提供记录已保存的确认消息、发生错误时的警告消息，或基于系统事件的信息消息。

Bootstrap 提供四种不同样式的警报。例如，以下标记生成绿色、蓝色、橙色和红色警报框：

```html
<div class="alert alert-success"><strong>Success. You have successfully saved the file.</strong></div>
<div class="alert alert-info"><strong>Info. Something has just happened.</strong></div>
<div class="alert alert-warning"><strong>Warning! The file size is too big.</strong></div>
<div class="alert alert-danger"><strong>Danger! The file could not be saved.</strong></div>
```

警报框在浏览器中应与以下屏幕截图类似：

![](img/371b9a67300a03b7b335eb8a6fe55d94_80_0.png)

可关闭警报是用户可以通过单击其右上角的小 X 图标关闭的警报。为了创建可关闭警报，您可以使用 alert-dismissible 类名，如下所示：

```html
<div class="alert alert-warning alert-dismissible" role="alert">
<button type="button" class="close" data-dismiss="alert"><span aria-hidden="true">&times;</span><span class="sr-only">Close</span></button>
<strong>Alert!</strong> This is a dismissible alert.
</div>
```

> 您可参考本书附带的示例项目，了解如何直接从 ASP.NET MVC 控制器显示特定样式的警报。这些警报改编自 Matt Honeycutt 在 GitHub 上的示例：https://github.com/MattHoneycutt/Fail-Tracker。

## 进度条

进度条是与传统桌面以及 Web 开发一起使用的隐喻，用于向用户提供任务或操作的进度视觉反馈。Bootstrap 提供多种不同样式的进度条。

### 基础进度条

基础 Bootstrap 进度条显示一个纯蓝色进度条。添加带有 `sr-only` 类名的 `<span>` 元素是良好做法，以便屏幕阅读器可以读取进度百分比。以下标记生成带有标题的基础进度条：

```html
<h4>Basic Progressbar</h4>
<div class="progress">
    <div class="progress-bar" role="progressbar" aria-valuenow="80" aria-valuemin="0" aria-valuemax="100" style="width: 80%;">
        <span class="sr-only">80% Complete</span>
    </div>
</div>
```

此标记的结果如以下屏幕截图所示：

![](img/371b9a67300a03b7b335eb8a6fe55d94_81_0.png)

您还可以通过在其 `<div>` 元素内添加文本来为进度条显示内联标签：

```html
<h4>Basic Progressbar with label</h4>
<div class="progress">
    <div id="progressbarTitle" class="progress-bar" role="progressbar" aria-valuenow="60" aria-valuemin="0" aria-valuemax="100" style="width: 60%;">
        60%
    </div>
</div>
```

此代码的结果如以下屏幕截图所示：

![](img/371b9a67300a03b7b335eb8a6fe55d94_82_0.png)

### 上下文进度条

您可以使用相同的按钮和警报样式类生成不同颜色的进度条。这是通过将进度条的类名设置为以下之一来实现的：

- `progress-bar-success`
- `progress-bar-info`
- `progress-bar-warning`
- `progress-bar-danger`

结果如以下屏幕截图所示：

![](img/371b9a67300a03b7b335eb8a6fe55d94_82_1.png)

### 条纹和动画进度条

要生成具有渐变条纹效果的进度条，请向进度条的父 `<div>` 元素添加类名 `progress-striped`：

```html
<h4>Striped Progressbar (Danger)</h4>
<div class="progress progress-striped">
    <div class="progress-bar progress-bar-danger" role="progressbar" aria-valuenow="80" aria-valuemin="0" aria-valuemax="100" style="width: 80%">
        <span class="sr-only">80% Complete (danger)</span>
    </div>
</div>
```

此结果如以下屏幕截图所示：

![](img/371b9a67300a03b7b335eb8a6fe55d94_83_0.png)

要添加动画效果，使进度条上的条纹看起来在移动，只需向其父 `<div>` 元素添加 `active` 类即可：

```html
<div class="progress progress-striped active">
    <div class="progress-bar progress-bar-danger" role="progressbar" aria-valuenow="80" aria-valuemin="0" aria-valuemax="100" style="width: 80%">
        <span class="sr-only">80% Complete (success)</span>
    </div>
</div>
```

### 动态更新进度条的百分比

进度条只有在实际提供特定任务进度的反馈时才有价值。要使用 ASP.NET MVC 动态更新进度条，请执行以下步骤：

1. 通过在 Visual Studio 程序包管理器控制台中运行以下命令，将 SignalR NuGet 包添加到我们的项目中：`Install-Package Microsoft.AspNet.SignalR`

