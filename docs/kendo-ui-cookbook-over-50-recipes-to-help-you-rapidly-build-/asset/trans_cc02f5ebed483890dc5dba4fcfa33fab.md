

## 设置应用程序初始化时的布局和视图

在处理复杂的移动应用程序时，页面中会有多个布局和视图。在这种情况下，无法确定在所选布局中渲染的初始视图。但是，可以将选项传递给 kendo.mobile.application 函数，以设置应用程序初始化时使用的布局和初始视图。

## 操作方法...

假设页面中有两个布局和两个视图。布局之间的区别在于 tabstrip 中的元素数量。在 layout1 中，让我们考虑标题文本，它读取为 Layout1 header，并且其 tabstrip 中有两个元素：

```
<div
data-role="layout"
data-id="layout1">
<!- - header here - ->
<footer data-role="footer">
<div data-role="tabstrip">
<a data-icon="about">About</a>
<a data-icon="settings">Settings</a>
</div>
</footer>
</div>
```

在同一页面中，还会有另一个布局，其 `data-id` 属性设置为 `layout2`。该布局的标题文本将读取为 `Layout2 header`，页脚将包含 tabstrip 中的四个元素：

```
<div
data-role="layout"
data-id="layout2">
<!- - header here - ->
<footer data-role="footer">
<div data-role="tabstrip">
<a data-icon="about">About</a>
<a data-icon="settings">Settings</a>
<a data-icon="share">Share</a>
<a data-icon="refresh">Refresh</a>
</div>
</footer>
</div>
```

下一步是向页面添加视图。让我们添加两个视图并为它们指定 id 属性：

```
<div
 data-role="view"
 id="view1">
 From View1
</div>

<div
 data-role="view"
 id="view2">
 From View2
</div>
```

请注意，此页面中的视图没有 data-layout 属性。当页面渲染时，您会看到页面中的第一个视图（id='view1'）在没有任何布局的情况下渲染。要更改此行为，您可以在初始化应用程序时指定要使用的布局和初始视图：

```
<script>
 var app = new kendo.mobile.Application(document.body, {
 layout: 'layout2',
 initial: 'view2'
 });
</script>
```

在这里，kendo.mobile.Application 函数的第一个参数是 document.body。此函数的第一个参数是目标元素。它通常是各种小部件的容器（带有 data-role 属性的元素）。第二个参数是一个对象，包含一组用于初始化应用程序的选项。在这里，我们将指定在渲染页面时要使用的布局和初始视图。

## 工作原理...

当您渲染页面时，您会看到在渲染 `view2` 时使用了 `layout2`。

![](img/2a66507445090790d34dc5588b56b24c_131_0.png)

如果您为视图元素指定了 `data-layout` 属性，则该视图将使用该布局进行渲染，而不是在初始化应用程序时提到的布局。

## 更多内容...

在之前的配方中，提到 `data-platform` 属性用于指定构建布局的平台。在许多场景中，您可能希望在所有平台上都出现相同的外观和感觉。例如，如果您使用 iOS 设备并希望强制使用 Android 的外观和感觉，那么您可以在初始化应用程序时指定 platform 属性：

```html
<script>
var app = new kendo.mobile.Application(document.body, {
    layout: 'layout2',
    initial: 'view2',
    platform: 'android'
});
</script>
```

现在，当您在任何设备上渲染页面时，您应该能够在所有平台上看到相同的外观和感觉。

## 使用 Application 对象导航到各种视图

在构建移动应用程序时，您将拥有代表应用程序中各种状态的多个视图，也就是说，每个状态都由应用程序中的 View 组件表示。例如，当您单击按钮时，当前视图将被销毁，新视图应显示。Application 对象用于在应用程序中导航到各种视图。这些视图可以定义在同一 HTML 文档中，也可以作为单独的文档（部分）存在。

## 操作方法...

构建代码的一个好方法是创建多个页面并分离视图。在同一页面中指定所有视图将增加响应的大小，从而影响性能。让我们创建一个包含两个按钮（即 Login 和 Register）的页面：

```
<div
data-role="layout"
data-id="layout1">
<header data-role="header">
<div data-role="navbar">
<span data-role="view-title"></span>
</div>
</header>
</div>
<div
data-role="view"
data-layout="layout1"
data-title="Home">

<a
data-role="button"
id="loginBtn"
class="button">
Login
</a>
<a
data-role="button"
id="registerBtn"
class="button">
Register
</a>
</div>
```

让我们称这个页面为 **Home** 页面。它包含一个布局小部件和一个视图小部件。视图包含两个按钮。请注意，布局的 navbar 小部件包含一个 data-role 属性设置为 view-title 的 span 元素。这允许各种视图提供将在 navbar 小部件中显示的标题。视图小部件指定了 data-title 属性，其值为 Home。这将把布局的 navbar 文本设置为相同的值。

视图包含两个按钮：**Login** 和 **Register**。下一步是定义单击其中一个按钮时将显示的视图；以下代码片段向您展示了 Login View 的描述：

```
<div
data-role="layout"
data-id="login-layout">
<header data-role="header">

<div data-role="navbar">
<a
class="nav-button"
data-role="backbutton"
data-align="left">
Back
</a>
<span data-role="view-title"></span>
</div>
</header>
</div>
<div
data-role="view"
data-layout="login-layout"
data-title="Login">
Login Page
</div>
```

前面的代码片段放在 login.html 文件中。在这里，布局与在 Home 页面中创建的布局类似，只是它现在包含一个返回按钮（data-role=backbutton）。backbutton 小部件用作导航小部件，用于导航回上一页。创建了一个类似的页面 register.html，包含相同的布局和视图。

下一步是将 Home 页面中的按钮绑定到这些页面。Application 对象用于在各种页面之间导航。让我们为页面上的 login 和 register 按钮添加事件处理程序：

```
<script>
var app = new kendo.mobile.Application();
$('#loginBtn').on('click', function () {
app.navigate('login.html');
});
$('#registerBtn').on('click', function () {
app.navigate('register.html');
});
</script>
```

navigate 方法用于导航到不同的页面。当用户单击其中任何按钮时，Application 对象上的 navigate 方法将被调用以导航到相应的页面。

## 工作原理...

当您渲染页面（**Home 页面**）时，有两个按钮，**Login** 和 **Register**。

![](img/2a66507445090790d34dc5588b56b24c_135_0.png)

单击 **Login** 按钮时，用户将被导航到 `login.html` 页面，当您单击 **Register** 按钮时，您将被导航到 `register.html` 页面。**Login** 页面的屏幕截图如下所示：

![](img/2a66507445090790d34dc5588b56b24c_135_1.png)

navbar 标题已更新，并且它有一个 **Back** 按钮。单击 **Back** 按钮时，用户将被导航到 **Home** 页面。在 **Home** 页面上，当您单击 **Register** 按钮时，将显示注册页面，即 `register.html` 页面，如下所示：

![](img/2a66507445090790d34dc5588b56b24c_136_0.png)

**Register** 视图也包含 **Back** 按钮，允许用户导航到 **Home** 页面。

在从一个视图导航到另一个视图时，您可以指定可以应用的过渡效果。

## 指定视图过渡

在从一个视图过渡到另一个视图时，您可以使用 `data-transition` 属性指定需要应用的过渡效果。以下值可以设置为 `data-transition` 属性的值：

- `slide`: 此过渡效果将旧视图内容向左滑动，并将新视图内容滑动到其位置。您可以使用 slide (direction) 指定方向。默认情况下，方向为左；另一个选项是右。
- `zoom`: 在这里，新视图内容从旧视图的中心缩放。旧视图内容将淡出，新视图内容将显示。

