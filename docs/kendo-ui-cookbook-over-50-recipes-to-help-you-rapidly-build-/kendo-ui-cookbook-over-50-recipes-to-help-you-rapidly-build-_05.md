# Kendo UI TreeView

在本章中，我们将介绍以下示例：

- 使用 TreeView 小部件显示目录结构
- 使用拖放功能在 TreeView 中移动元素
- 使用复选框在 TreeView 中选择节点
- 在 TreeView 中将图标与标签关联

### 简介

Kendo UI TreeView 小部件允许你以树状结构显示层次数据。此小部件允许用户展开或折叠层次数据。它还允许他们执行拖放操作，允许列表项从一个节点移动到另一个节点。

### 使用 TreeView 小部件显示目录结构

可以通过在 HTML 标记中指定层次数据，或将其绑定到 `DataSource` 对象来创建 TreeView 小部件。

#### 操作方法...

要创建 TreeView，让我们在标记中指定数据，如下面的代码片段所示：

```html
<ul id="treeView">
  <li>
    Directory1
  </li>
</ul>
```

```html
<ul>
    <li>File1.txt</li>
    <li>File2.txt</li>
</ul>
</li>
<li>
    Directory2
    <ul>
        <li>File3.txt</li>
        <li>File4.txt</li>
    </ul>
</li>
<li>
    Directory3
    <ul>
        <li>File5.txt</li>
        <li>File6.txt</li>
    </ul>
</li>
</ul>
```

下一步是通过在 DOM 节点上调用 `kendoTreeView` 函数来初始化 TreeView 小部件，使用以下代码行：

`$('#treeView').kendoTreeView();`

#### 工作原理...

TreeView 小部件以树状结构显示层次数据，如下面的截图所示：

![](img/2a66507445090790d34dc5588b56b24c_61_0.png)

请注意，TreeView 仅显示顶级元素。它在元素前显示一个右箭头图标。点击该图标时，顶级元素会展开以显示层次结构中的下一级。

![](img/2a66507445090790d34dc5588b56b24c_62_0.png)

默认情况下，层次结构中的元素在初始时都不会展开。要在渲染时展开元素，请将 expanded 数据属性指定为 true：

```
.
.
<li data-expanded="true">
Directory2
<ul>
<li>File3.txt</li>
<li>File4.txt</li>
</ul>
</li>
.
.
```

通过将 data-expanded 属性指定为 true，该元素将展开以显示层次结构中的下一级元素。

### 更多内容...

TreeView 小部件可以通过在 DataSource 对象中指定层次结构来创建。然后将此 DataSource 对象绑定到视图元素。让我们使用一个 div 元素，它将绑定到 DataSource 对象：

```
<div id="treeView">
</div>
```

现在，让我们在初始化 TreeView 小部件时将此视图元素绑定到 DataSource 对象：

```
$('#treeView').kendoTreeView({
dataSource: [{
text: "Directory1",
items: [
```





{ text: "File1.txt" },
{ text: "File2.txt" }
],
expanded: true
}, {
text: "目录2",
items: [
{ text: "File1.txt" },
{ text: "File2.txt" }
]},
{
text: "目录3",
items: [
{ text: "File1.txt" },
{ text: "File2.txt" }
]
}
]
});
```

这里，DataSource 对象是一个对象数组，每个对象指定 TreeView 小部件中节点的属性。每个对象都有一个 text 属性，用于指定 TreeView 小部件中节点的标签。它还有一个 items 集合，用于指定 TreeView 中节点的子项。

DataSource 集合中的第一个对象将 expanded 属性设置为 true，以指示该节点将展开以显示子项。

### 从远程 DataSource 对象构建 TreeView 小部件

也可以通过从远程服务获取数据来构建 TreeView 小部件。DataSource 对象可以指定远程服务的 URL，用于获取层次结构数据：

```javascript
$('#treeView').kendoTreeView({
dataSource: {
transport: {
read: '/services/music'
}
}
});
```

这里，远程服务 URL /services/music 应返回包含 TreeView 小部件中第一级元素的数据。假设该服务返回以下数据：

```json
[
    {
        "categoryId": 1,
        "categoryName": "Genre",
        "hasSubCategories": true
    },
    {
        "categoryId": 2,
        "categoryName": "Artists",
        "hasSubCategories": true
    },
    {
        "categoryId": 3,
        "categoryName": "Songs",
        "hasSubCategories": true
    },
    {
        "categoryId": 4,
        "categoryName": "Playlists",
        "hasSubCategories": true
    }
]
```

服务返回的前面 JSON 响应包含四条记录，这些记录是 TreeView 小部件中的顶级节点。响应中的每个对象包含三个字段；在初始化 TreeView 小部件时，应指定用于显示文本的字段：

```javascript
$('#treeView').kendoTreeView({
    dataSource: {
        transport: {
            read: '/services/music'
        }
    },
    dataTextField: 'categoryName'
});
```

### Kendo UI TreeView

现在，当你执行前面的代码时，你会看到类别名称在 TreeView 小部件中显示。这里，`dataTextField` 属性用于指定在 TreeView 小部件中显示标签时应使用的字段名。

要填充 TreeView 中的下一级节点，应指示小部件当前节点是否包含子项。请注意，响应包含 `categoryId` 和 `hasSubCategories` 字段；`categoryId` 字段用作响应中每个对象的唯一 ID，而 `hasSubCategories` 字段指示该对象是否包含子项。这应在 `DataSource` 对象中标识，因此应为其指定一个架构：

```javascript
$('#treeView').kendoTreeView({
    dataSource: {
        transport: {
            read: 'TreeView.json'
        },
        schema: {
            model: {
                id: 'categoryId',
                hasChildren: 'hasSubCategories'
            }
        }
    },
    dataTextField: "categoryName",
});
```

这里，架构模型中的属性 `id` 和 `hasChildren` 映射到前面提到的响应中的属性。现在，TreeView 显示顶级节点列表，并指示树中每个节点都有子项。当你单击 TreeView 中的任何节点时，会向远程服务发送一个 `GET` 请求，并将 `categoryId` 作为查询参数指定。例如，当你单击树中的第一个节点时，会发送一个 `GET` 请求，并将 `categoryId` 查询参数设置为 1：

```
/services/music?categoryId=1
```

预期该服务将返回类似的 JSON 响应，即一组对象，每个对象包含 `categoryId` 和 `hasSubCategories` 属性。

### 使用拖放功能在 TreeView 中移动元素

在 TreeView 小部件中，树中的元素可以拖放到各个级别。这不仅适用于子元素，也适用于树中的顶级节点。

#### 操作方法...

要启用拖放功能，请在初始化 TreeView 小部件时将 dragAndDrop 属性指定为 true，如以下代码片段所示：

```javascript
$('#treeView').kendoTreeView({
dragAndDrop: true,

dataSource: [{
text: "目录1",
items: [
{ text: "File1.txt" },
{ text: "File2.txt" }
],
expanded: true
}, {
text: "目录2",
items: [
{ text: "File3.txt" },
{ text: "File4.txt" }
] }, {
text: "目录3",
items: [
{ text: "File5.txt" },
{ text: "File6.txt" }
]
}
]
});
```

这里，当 dragAndDrop 属性设置为 true 时，TreeView 中的元素可以在树的各个级别之间移动。

### Kendo UI TreeView

当用户拖动节点时，会生成各种事件。应用程序可以监听这些事件并自定义其行为。drag、dragstart、dragend 和 drop 事件是用户可以为其指定事件监听器的一组事件。

让我们考虑 drag 事件。生成的事件包含元信息，并包含一个设置工具提示的函数，即当用户执行拖动操作时会显示一个工具提示。该事件包含 sourceNode 等属性，指代正在拖动的元素；dropTarget，指代 sourceNode 放置其上的元素；pageX 和 pageY，指代鼠标指针在屏幕上的 x 和 y 坐标；以及 statusClass，指示拖动操作的状态，即节点被添加、插入，或者如果操作不可接受则被拒绝。

要设置工具提示，请在事件对象上调用 setStatusClass 函数。该函数接受一个字符串作为参数，该字符串应为 k-insert-top、k-insert-middle、k-insert-bottom、k-add 或 k-denied 之一。

可以在元素拖动过程中使用 statusClass 属性在 setStatusClass 函数中设置适当的工具提示：

```javascript
$('#treeView').kendoTreeView({
    dragAndDrop: true,
    drag: function (e) {
        e.setStatusClass('k-' + e.statusClass);
    }
});
```

现在，当元素被拖动时，工具提示是通过调用 setStatusClass 函数来设置的。拖动操作的状态可在事件对象的 statusClass 属性中获取。

#### 工作原理...

将 dragAndDrop 属性设置为 true 后，TreeView 中的元素可以拖放到树中的任何级别。

![](img/2a66507445090790d34dc5588b56b24c_67_0.png)

前面的屏幕截图显示 File1.txt 元素正在被拖动，工具提示指示该元素将插入到底部，即它将是父节点 Directory1 下的最后一个元素。当元素被拖动时，statusClass 属性设置为 insert-bottom，工具提示设置为 k-insert-bottom。

同样，当元素移动到不同节点时，你会注意到工具提示会发生变化，指示拖动的元素（sourceNode）将被添加到 TreeView 中的一个节点。

![](img/2a66507445090790d34dc5588b56b24c_68_0.png)

请注意前面屏幕截图中的工具提示；它指示该元素正在被添加到 Directory3。放下元素后，父节点将展开以显示其内容。

![](img/2a66507445090790d34dc5588b56b24c_68_1.png)

#### 更多...

在许多场景中，你可能希望树中的某个节点不接受来自任何其他元素的放置。在这种情况下，可以使用 drop 事件监听器来防止来自树中任何节点的放置：

```javascript
$('#treeView').kendoTreeView({
 dragAndDrop: true,
 drop: function(e) {
 if ($(e.dropTarget)[0].innerText == 'Directory3') {
 e.setValid(false);
 }
 }
});
```

这里，检查 dropTarget 节点以查看其是否包含 Directory3 文本。

如果此条件为真，则在事件对象上调用 setValid 函数，并将 false 作为参数传递。这将禁用将任何 sourceNode 放置在包含 Directory3 文本的节点上的操作。

在另一个场景中，如果你想禁用拖动树中的顶级节点，即树的根节点，则可以为 dragstart 事件添加一个事件监听器：

```javascript
$('#treeView').kendoTreeView({
dragAndDrop: true,

dragstart: function (e) {
if($(e.sourceNode).parentsUntil('.k-treeview', '.k-item').length == 0) {
e.preventDefault();
}
}
});
```

这里，检查 sourceNode 是否有父级节点。如果未找到，即 sourceNode 本身就是父节点，那么通过在事件对象上调用 preventDefault 函数，禁用拖动元素的操作。

### 使用复选框在 TreeView 中选择节点

在 TreeView 中，每个节点可以关联一个复选框。通过使用复选框，你可以选择树中的多个节点并执行操作。

#### 操作方法...

要在 TreeView 中节点旁添加复选框，请在初始化 TreeView 小部件时将 checkboxes 属性指定为 true：

```javascript
$('#treeView').kendoTreeView({
checkboxes: true
});
```

如果你希望在选择父节点时也选择子节点，请指定 checkChildren 为 true：

```javascript
$('#treeView').kendoTreeView({
checkboxes: {




### Kendo UI TreeView

checkChildren: true
}
});
```

Now, when you select the parent node, all child nodes within the parent node would be selected. It is also possible to specify nodes as checked when they are rendered. In the DataSource configuration, specify checked as true:

```
$('#treeView').kendoTreeView({
 checkboxes: {
 checkChildren: true
 },
 dataSource: [{
 text: "Directory1",
 items: [
 {text: "File1.txt"},
 {text: "File2.txt"}
 ],
 checked: true
 }
 .
 .
 ]
});
```

### 工作原理...

当你将 checkboxes 属性设置为 true 时，你会在 TreeView 中每个节点前面看到一个复选框。

![](img/2a66507445090790d34dc5588b56b24c_70_0.png)

当你检查任何父节点（例如，`Directory1`）时，子节点不会被选中。当你将配置选项 `checkChildren` 设置为 `true` 时，子级节点将被选中。

![](img/2a66507445090790d34dc5588b56b24c_71_0.png)

另外，请注意，当你选择任何子节点时，父节点将被部分选中。

![](img/2a66507445090790d34dc5588b56b24c_71_1.png)

在这里，`File1.txt` 子节点被选中，根节点 `Directory1` 被部分选中。当你选择所有子节点时，父节点将被标记为选中。

### 更多...

当你在树中选择任何节点时，相应元素的 checked 属性将被设置为 true。这将触发与 TreeView 关联的 DataSource 对象上的 change 事件。可以使用 change 事件监听器来检查树中哪些节点被选中：

```
var treeViewWidget = $("#treeView").data("kendoTreeView");
treeViewWidget.dataSource.bind("change", function() {
 var treeView = $("#treeView").data("kendoTreeView"),
 nodes = treeView.dataSource.view();
 (function(nodes) {
 for (var i = 0; i < nodes.length; i++) {
 console.log(nodes[i].text + ' is checked: ' +
 (nodes[i].checked ? 'yes': 'no'));
 if(nodes[i].hasChildren) {
 arguments.callee.call(this, nodes[i].children.view());
 }
 }
 })(nodes);
});
```

在这里，代码检查树中的节点是否被选中；然后递归遍历其子节点并检查子节点是否被选中。当你检查树中的第一个节点时，你会在控制台中看到以下消息：

```
Directory1 is checked: no
File1.txt is checked: no
File2.txt is checked: no
Directory2 is checked: yes
File3.txt is checked: yes
File4.txt is checked: yes
Directory3 is checked: no
File5.txt is checked: no
File6.txt is checked: no
```

### 在 TreeView 中将图标与标签关联

类似于将复选框与 TreeView 小部件中的每个节点关联，图像或图标可以与树中的节点关联。此图标将显示在节点名称之前。

### 如何操作...

让我们首先引用精灵图表并向页面添加 CSS 类：

```
<style type="text/css">
#treeView .k-sprite {
    background-image:
        url('../../examples/content/web/treeview/coloricons-
sprite.png');
}
.folder {
    background-position: 0 0
}
.pdf {
    background-position: 0 -32px;
}
.image {
    background-position: 0 -64px;
}
.html {
    background-position: 0 -48px;
}
</style>
```

下一步是将树中的节点与前面提到的 CSS 类关联。这可以通过在 DataSource 集合中的每个对象上添加 spriteCssClass 属性来完成：

```
$('#treeView').kendoTreeView({
    dataSource: [{
        text: "Directory1",
        spriteCssClass: "folder",
        items: [
            { text: "File1.png", spriteCssClass: "image" },
            { text: "File2.html", spriteCssClass: "html" }
        ],
    }, {
    text: "Directory2",
    spriteCssClass: "folder",
    items: [
        { text: "File3.pdf", spriteCssClass: "pdf" },
        { text: "File4.html", spriteCssClass: "html" }
    ]
    }]
});
```

在这里，根级节点的 spriteCssClass 属性设置为 folder，items 数组中的节点设置为相应的文件类型。

### 工作原理...

当你关联 spriteCssClass 属性时，文本节点的容器（span 元素）的 class 将设置为 k-sprite，你指定的类名将是 spriteCssClass 的值。例如，根节点 Directory1 的容器元素的 class 将设置为 k-sprite folder。

当你执行页面时，你会看到图标显示在树中的节点之前。

![](img/2a66507445090790d34dc5588b56b24c_74_0.png)
