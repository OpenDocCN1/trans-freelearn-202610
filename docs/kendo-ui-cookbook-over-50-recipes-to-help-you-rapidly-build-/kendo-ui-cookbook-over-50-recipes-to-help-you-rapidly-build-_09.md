### Kendo UI Window

在本章中，我们将介绍以下示例：

- 通过将其配置为模态窗口来显示弹出窗口并阻止用户交互
- 自定义窗口的外观并包含操作按钮
- 使用 Window API 对窗口对象执行操作

#### 简介

Kendo UI Window 小部件用于在弹出窗口或模态窗口中显示任何 HTML 内容。窗口内容显示在页面上的其他内容之上，并带有带关闭按钮的标题栏。内容可以是静态 HTML，也可以使用 Ajax 动态加载。页面中的任何 DOM 节点都可以初始化为窗口，也就是说，DOM 中任何元素的内容都可以包装在窗口内。此外，窗口可以调整大小、移动和关闭。

Kendo UI Window 小部件提供了多个配置选项，可用于自定义可以应用的操作以及小部件的外观。在本示例中，我们将首先了解如何创建弹出窗口并将其配置为模态。然后，我们将自定义窗口的外观并使用 API 对窗口对象调用操作。

### 通过将其配置为模态窗口来显示弹出窗口并阻止用户交互

在本示例中，我们将了解如何为页面中的 DOM 节点创建 Kendo UI 窗口。随后，我们还将了解如何通过指定页面的 URL 来异步加载窗口的内容。

#### 操作方法

让我们创建一个包含一些文本的 div 元素，这些文本将显示在窗口中：

```html
<div id="window">
 Window content goes here...
</div>
```

下一步是在前面的 DOM 节点上调用 kendoWindow 函数：

```javascript
$("#window").kendoWindow();
```

使用 kendoWindow 函数初始化 DOM 节点后，您会注意到窗口中包含 div 元素中提到的文本并正在显示。请注意，这不会添加背景遮罩层。要添加背景遮罩层，应将窗口配置为模态。

窗口和模态之间的区别在于，在模态的情况下，会显示隐藏正文内容的背景遮罩层。要创建模态窗口，请将 modal 属性指定为 true：

```javascript
$("#window").kendoWindow({
 modal: true
});
```

默认情况下，modal 属性的值为 false，因此不会显示背景遮罩层。

#### 工作原理

当您初始化任何 DOM 节点时，其内容将被包装在窗口中，如下面的屏幕截图所示：

![](img/2a66507445090790d34dc5588b56b24c_113_0.png)

窗口包含 **Window content goes here...** 文本，标题栏上有一个叉号 (x) 图标。如果将其初始化为模态窗口，则会向页面添加背景遮罩层，如下面的屏幕截图所示：

![](img/2a66507445090790d34dc5588b56b24c_114_0.png)

请注意，背景遮罩层是半透明的；它会显示页面中存在的文本，并突出显示窗口。这将阻止您与页面的其余部分进行交互。通常，只有在用户需要在继续下一步之前完成某个操作时，才会使用模态窗口。例如，在同一用户查看页面内容之前，可能会显示包含登录表单的模态窗口。

#### 更多内容

模态窗口可以配置为在用户触发操作时打开。例如，单击按钮时可以显示模态窗口。此外，由于模态的内容是在用户在页面上触发操作时加载的，因此模态的内容可以在运行时使用 Ajax 加载。

#### 在单击按钮时显示模态窗口

在前面提到的场景中，窗口或模态窗口在页面加载时显示。在大多数场景中，您可能希望仅在用户发起操作时才显示模态窗口。让我们假设当用户单击按钮时应显示模态窗口。

让我们按如下方式在页面上放置一个按钮：

```html
<button id="btn1">Click</button>
```

要隐藏模态窗口，请将 `visible` 属性设置为 `false`，如下所示：

```javascript
$('#window').kendoWindow({
    visible: false,
    modal: true
});
```





### Kendo UI Window

现在，当页面加载时，模态窗口将被隐藏。为按钮添加一个 `click` 事件处理程序以打开模态窗口：

```javascript
$('#btn1').on('click', function() {
    var modalObject = $('#window').data('kendoWindow');
    modalObject.open();
});
```

第一行获取模态窗口的实例。然后，在下一行中，对窗口实例调用 open 方法以打开模态窗口。

`open` 方法是可用于对窗口对象执行操作的多种方法之一。我们将在下一个示例中查看其他方法。

### 使用 Ajax 加载模态窗口的内容

在此示例中，模态窗口的内容是在 HTML 标记中指定的。模态窗口的内容可以通过指定返回 HTML 标记的 URL 来加载。当页面加载时，此内容将异步获取。

要加载窗口的内容，请在使用 `kendoWindow` 初始化窗口时指定 content 属性：

```javascript
$('#window').kendoWindow({
    visible: false,
    modal: true,
    content: './jsbooks.html'
});
```

在此，`./jsbooks.html` URL 在 content 属性中指定。这可以是任何返回 HTML 标记的 URL。例如，它可以是生成 HTML 标记的 PHP 页面。当你加载页面时，会向 `./jsbooks.html` 发送异步请求，生成的标记将插入到窗口中。

### 自定义窗口的外观并包含操作按钮

用户可以使用各种选项来自定义窗口对象。在此示例中，我们将查看这些选项。

#### 操作步骤...

窗口的一些自定义选项具有默认值。例如，弹出窗口可以调整大小并且可以拖动。另外，由于 visible 属性设置为 true，窗口会显示。让我们来看看其他一些属性：

```javascript
$('#window').kendoWindow({
    visible: false,
    modal: true,
    title: "Example Modal Window",
    width: '400px',
    height: '400px',
    maxWidth: '600px',
    maxHeight: '600px',
    minWidth: '200px',
    minHeight: '200px',
    position: {
        top: '100px',
        left: '100px'
    }
});
```

在此，title 属性指定窗口的标题。当模态窗口呈现时，其宽度和高度将为 400px。此模态窗口可以调整大小；但是，它可以调整到最大宽度（maxWidth）和高度（maxHeight）为 600px，以及最小宽度（minWidth）和高度（minHeight）为 200px。position 属性指定 top 和 left 位置为 100px，窗口将相应定位。如果未指定相同内容，则窗口默认相对于文档定位。

窗口的标题栏可以自定义以添加更多操作按钮。默认情况下，仅显示关闭（x）按钮。可以添加到标题栏的操作按钮包括关闭（默认添加）、最小化、最大化、刷新和固定。这些选项可以作为 actions 属性的值指定：

```javascript
$('#window').kendoWindow({
    visible: false,
    modal: true,
    title: "Example Modal Window",
    actions: ['Pin', 'Refresh', 'Minimize', 'Maximize', 'Close'],
    width: '400px',
    height: '300px',
    content: './jsbooks.html'
});
```

#### 工作原理...

当你执行前面的代码片段时，你将看到窗口具有标题栏和各种操作按钮，如下面的屏幕截图所示：

![](img/2a66507445090790d34dc5588b56b24c_117_0.png)

操作按钮按指定的相同顺序添加。当你单击刷新按钮时，将向 content 属性中指定的 URL 发送请求。如果窗口的内容是内联指定的，即在同一页面中，则不需要刷新按钮。

当你单击最小化或最大化按钮时，窗口将被调整，使其仅显示标题或分别扩展以占据整个屏幕。在这两种情况下，这些按钮将被还原按钮替换。单击此按钮时，窗口将恢复到其原始状态。

固定按钮允许你固定窗口的位置，即当用户滚动时，窗口将继续保持在相同的固定位置。另外，如果窗口被固定，标题栏中的图标将被取消固定按钮替换，以便用户可以将窗口恢复到其原始状态。

#### 更多内容...

也可以在窗口实例的标题栏中添加自定义操作按钮。另外，单击自定义按钮时要执行的操作也可以通过添加 click 事件处理程序来定义。

要向标题栏添加自定义操作按钮，首先在 actions 数组中指定 Custom：

```javascript
$('#window').kendoWindow({
    title: "Example Modal Window",
    actions: ['Custom', 'Pin', 'Refresh',
        'Minimize', 'Maximize', 'Close'],
    width: '400px',
    height: '300px',
    content: './jsbooks.html'
});
```

下一步是为添加到标题栏的自定义按钮定义事件处理程序：

```javascript
var modalObject = $('#window').data('kendoWindow');
modalObject.wrapper.find('.k-i-custom').on('click', function() {
    alert(modalObject.options.title);
});
```

前面的代码首先获取窗口的实例，然后为自定义按钮添加 click 处理程序。此处，使用对自定义图标的引用，即 k-i-custom 类来添加 click 处理程序。

当你执行页面时，你将看到自定义图标显示在标题栏中。

![](img/2a66507445090790d34dc5588b56b24c_118_0.png)

在前面的屏幕截图中，最左侧的第一个图标是自定义图标。当你单击自定义图标时，将显示一条显示窗口标题的警报消息。对于给定的窗口，你只能添加一个自定义图标。

### 使用 Window API 对窗口对象执行操作

在此示例中，我们将查看可用于对窗口对象执行操作的多种方法。

#### 操作步骤...

有各种可用的方法可帮助你对现有窗口对象执行操作。这些方法包括以下方法：

- **center**：这将使窗口居中。
- **open**：这将打开窗口。
- **close**：这将关闭窗口。
- **refresh**：这将刷新窗口的内容。可选地，可以向此函数传递一些参数，这些参数将覆盖默认选项。
- **maximize**：这将使窗口最大化到整个视图区域。
- **minimize**：这将使窗口最小化到其标题栏。
- **restore**：这将从最小化或最大化状态恢复窗口。
- **pin**：这将固定窗口。它将固定窗口的位置，因此，当你滚动时，窗口将出现在相同的位置。
- **unpin**：这将取消固定窗口并删除固定位置。
- **toggleMaximization**：这在最大化和还原状态之间切换窗口。

让我们通过创建一个窗口并添加按钮来测试这些函数，当单击这些按钮时将执行相应的操作：

以下是包含按钮的 HTML 标记：

```html
<button id="btnOpen">Open</button><br>
<button id="btnClose">Close</button><br>
<button id="btnRefresh">Refresh</button><br>
<button id="btnMaximize">Maximize</button><br>
<button id="btnToggleMaximization">Toggle Maximization</button><br>
<button id="btnMinimize">Minimize</button><br>
<button id="btnRestore">Restore</button><br>
<button id="btnPin">Pin</button><br>
<button id="btnUnpin">Unpin</button><br>
```

现在，按如下方式初始化窗口对象：

```javascript
$('#window').kendoWindow({
    visible: true,
    title: 'Example Modal Window',
    width: '400px',
    height: '300px',
    content: './jsbooks.html',
    position: {
        top: '200px',
        left: '500px'
    }
});
```

初始化窗口后，按如下方式添加 onclick 事件监听器：

```javascript
var modalObject = $('#window').data('kendoWindow');

$('#btnOpen').on('click', function() {
    modalObject.open();
});

$('#btnClose').on('click', function() {
    modalObject.close();
});

$('#btnRefresh').on('click', function() {
    modalObject.refresh();
});

$('#btnMaximize').on('click', function() {
    modalObject.maximize();
});

$('#btnToggleMaximization').on('click', function() {
    modalObject.toggleMaximization();

    //restore to original state after 2 seconds
    setTimeout(function () {
        modalObject.toggleMaximization()
    }, 2000);
});

$('#btnMinimize').on('click', function() {
    modalObject.minimize();
});

$('#btnRestore').on('click', function() {





### Kendo UI Window

```javascript
modalObject.restore();
});

$('#btnPin').on('click', function() {
    modalObject.pin();
});

$('#btnUnpin').on('click', function() {
    modalObject.unpin();
});
```

#### 工作原理...

初始化窗口时，您会发现它位于视口顶部 200 像素和左侧 500 像素处。窗口的内容是异步加载的，因为在初始化小部件时指定了引用 URL 的 `content` 属性。

初始化小部件后，对窗口对象的引用存储在 `modalObject` 中。`modalObject` 随后在所有事件监听器中被引用。

![](img/2a66507445090790d34dc5588b56b24c_121_0.png)

单击 **刷新** 按钮时，您会注意到向 `content` 属性中指定的同一 URL 发送了异步请求。请注意，窗口本身在标题栏中没有操作按钮。单击任何按钮时，都会调用相应的操作。
