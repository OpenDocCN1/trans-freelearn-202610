

当你指定 create 属性时，图像浏览器屏幕上会添加一个按钮，如下面的屏幕截图所示：

![](img/2a66507445090790d34dc5588b56b24c_86_0.png)

现在，当你单击 **Create** 按钮时，图像浏览器模态窗口会显示一个已创建的目录，并允许用户指定目录名称。

![](img/2a66507445090790d34dc5588b56b24c_86_1.png)

现在，用户可以指定目录名称并按 *Enter* 键。这将向 `create` 服务发送一个 `POST` 请求，并在请求正文（表单数据）中提及目录名称：

```
{
    name: 'test',
    size: '',
    type: 'd',
    path: ''
}
```

这里，type 属性是 `'d'`，因为我们正在服务器上创建一个目录。现在，用户可以选择该目录并上传文件到其中。这里还需要注意的是，当你浏览目录时，会向作为 `read` 属性值指定的服务发送一个 `POST` 请求。表单数据包含路径详细信息，也就是说，它将目录名称指定为 `{ 'path': 'test' }`，其中 `test` 是要读取的目录。

## Kendo UI PanelBar

在本章中，我们将介绍以下配方：

- 创建 PanelBar
- 使用 Ajax 加载 PanelBar 的内容
- 将 PanelBar 绑定到 DataSource 对象
- 自定义 PanelBar

### 简介

Kendo UI PanelBar 小部件类似于包含分组在不同选项卡或窗格下的项目列表的选项卡式面板。在 PanelBar 中，这些项目列表是垂直堆叠的，每个列表项都可以展开以显示内容。当数据可以分组为多个部分且可用于显示这些数据的空间受到限制时，此小部件特别有用。例如，你的所有联系人都可以按某个类别（如 **Favourites**、**Family**、**Friends** 和 **Office Contacts**）分组；当你点击其中一个类别时，面板将展开以显示属于该类别的联系人列表。

### 创建 PanelBar

PanelBar 可以通过在 HTML 标记中指定其结构，或通过使用 API 进行配置来创建。在本配方中，我们将介绍前一种方法。

### 如何操作...

PanelBar 小部件包含垂直堆叠的项目列表。要创建 PanelBar，我们需要一个项目列表，因此使用 HTML 的有序列表或无序列表。在此示例中，我们将列出旅游目的地、航空公司和要参观的重要景点作为三个面板：

```
`<ul id="panelBar" style="width: 400px">
	<li>
	Destinations
	</li>
	<li>
	Flights
	</li>
	<li>
	Important Sights
	</li>
</ul>`
```

这里，列表包含三个项目，这些是我们希望每个面板的标题。此外，width 属性设置为 400px，这将设置 PanelBar 组件的宽度。如果未指定 width 属性，则 PanelBar 将占据整个屏幕宽度，这并不理想。

前面的面板尚未列出任何内容。让我们在 HTML 标记中为每个面板指定内容：

```
`<ul id="panelBar" style="width: 400px">
	<li>
	Destinations
	<div class="destinationData">
	<h2>India</h2>
	<img src="./images/India-Flag-icon.png" />
	</div>
	<div class="destinationData">
	<h2>Australia</h2>
	<img src="./images/Australia-Flag-icon.png" />
	</div>
	<div class="destinationData">
	<h2>Indonesia</h2>
	<img src="./images/Indonesia-Flag-icon.png" />
	</div>
	</li>
	<li>
	Flights
	<div class="destinationData">
	British Airways
	</div>
	<div class="destinationData">
	Air France
	</div>
	<div class="destinationData">
	Lufthansa Airlines
	</div>
	<div class="destinationData">
	Singapore Airlines
	</div>
	<div class="destinationData">
	Qatar Airways
	</div>
	</li>
	<li>
	Important Sights
	<div class="destinationData">
	<h2>Taj Mahal</h2>
	<img src="./images/TajMahal.jpg" />
	</div>
	<div class="destinationData">
	<h2>Sydney Opera House</h2>
	<img src="./images/OperaHouse.jpg" />
	</div>
	<div class="destinationData">
	<h2>Tanah Lot Temple</h2>
	<img src="./images/TanahLot.jpg" />
	</div>
	</li>
</ul>`
```

### Kendo Ui PanelBar

在前面的 HTML 标记中，每个列表项包含标题和内容，当用户单击 PanelBar 中的某个项目时，需要显示这些内容。

> 前面代码片段中使用的 destinationData 类仅用于样式设置。

最后一步是初始化 PanelBar 小部件。要初始化，请在列表上调用 kendoPanelBar 函数，如下所示：

```
$("#panelBar").kendoPanelBar();
```

### 工作原理...

最初，当列表仅包含标题时，你将看到三个垂直堆叠的窗格，如下面的屏幕截图所示：

![](img/2a66507445090790d34dc5588b56b24c_91_0.png)

当你向列表中的每个项目添加内容时，你将看到列表中每个标题旁有一个向下箭头，如下面的屏幕截图所示：

![](img/2a66507445090790d34dc5588b56b24c_91_1.png)

现在，当你单击其中一个标题时，列表项将展开以显示其内容，如下面的屏幕截图所示：

![](img/2a66507445090790d34dc5588b56b24c_92_0.png)

### 更多...

在前面的示例中，列表元素遵循单层级层次结构。PanelBar 可以自定义以处理多层级层次结构数据。前面的示例现在可以包含每个旅游目的地的重要城市。在这种情况下，国家名称将作为嵌套面板元素：

```
`<ul id="panelBar" style="width: 400px">
<li>
Destinations
<ul>
<li>
<span>India</span>
<div class="destinationData">
<span>Bangalore</span>
</div>
<div class="destinationData">
<span>New Delhi</span>
</div>
<div class="destinationData">
<span>Mumbai</span>
</div>
</li>
</ul>
</li>
</ul>
<ul>
<li>
<span>Australia</span>
<div class="destinationData">
<span>Sydney</span>
</div>
<div class="destinationData">
<span>Melbourne</span>
</div>
<div class="destinationData">
<span>Cairns</span>
</div>
</li>
</ul>
</li>
<li>
Flights
</li>
<li>
Important Sights
</li>
</ul>`
```

在前面的代码片段中，第一个列表项包含嵌套列表元素，每个嵌套列表元素包含一些内容，如下面的屏幕截图所示：

![](img/2a66507445090790d34dc5588b56b24c_93_0.png)

与它们的父列表元素一样，每个子列表元素都有一个标题，用户可以单击该标题以展开内容。

### 使用 Ajax 加载 PanelBar 的内容

在前面的配方中，我们了解了如何使用页面中存在的标记创建 PanelBar 小部件。在本配方中，我们将了解如何通过从服务器使用 Ajax 获取内容来加载 PanelBar 中每个列表项的内容。

如前面的配方中所观察到的，每个列表项的内容都包裹在 div 元素内。这样做会增加页面大小，从而可能影响应用程序的性能。解决此问题的方法是在使用 Ajax 请求时加载每个列表项的内容。

#### 如何操作...

让我们考虑相同的示例；在这里，列表标题在标记中指定，div 元素用作占位符。这些占位符将在运行时包含从服务器获取的数据：

```
<ul id="panelBar" style="width: 400px">
  <li>
    Destinations
    <div></div>
  </li>
  <li>
    Flights
    <div></div>
  </li>
  <li>
    Important Sights
    <div></div>
  </li>
</ul>
```

这里，使用占位符 div 元素，以便在初始化 PanelBar 时，列表标题将显示向下箭头，指示它是可展开的。

下一步是创建包含列表中每个标题内容的 HTML 文件。例如，要填充前面列表中的 Destinations 列表，请创建 PanelBar_Destinations.html 文件：

```
<div class="destinationData">
  <h2>India</h2>
  <img src="./images/India-Flag-icon.png" />
```

