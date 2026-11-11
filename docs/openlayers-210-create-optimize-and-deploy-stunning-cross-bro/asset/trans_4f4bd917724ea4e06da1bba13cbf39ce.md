
| 名称 | 类型 | 描述 | 默认值 |
|---|---|---|---|
| displayProjection | `{OpenLayers.Projection}` | 控制坐标将以何种投影显示。任何除 `EPSG:4326` 或 `EPSG:900913` 之外的投影都需要 Proj4js 库来正确地重新投影坐标。如果未设置此属性，则将使用地图的投影。 |  |
| div | `{HTML Element}` | 如果指定，则为放置此控件的 HTML 元素。 |  |

## OpenLayers.Control.Navigation

**Navigation** 控件是默认添加到地图的四个控件之一。此控件通过鼠标输入处理地图导航，例如拖动地图和滚动以放大。让我们看一下其中几个属性。

### Navigation 属性

| 名称 | 类型 | 描述 | 默认值 |
|---|---|---|---|
| documentDrag | `{Boolean}` | 确定当鼠标移出地图视口（即地图 HTML div 元素）时是否允许拖动地图。 | `false` |
| handleRightClicks | `{Boolean}` | 如果设置为 true，地图将拦截右键单击。双击右键时，地图将缩小。 | `false` |
| zoomWheel | `{Boolean}` | 指定鼠标滚轮是否会使地图缩放。 | `true` |
| autoActivate | `{Boolean}` | 确定控件在添加到地图时是否应被激活。在将此控件添加到面板时可能很有用。 | `true` |

## OpenLayers.Control.NavigationHistory

**NavigationHistory** 控件存储有关（正如你猜到的）地图导航的信息。它实际上是一个创建两个子控件的控件，分别称为 `next` 和 `previous`。你不需要直接访问这些子控件，通常只需要使用 **NavigationHistory** 控件对象本身的方法即可。通过调用 `nextTrigger()` 或 `previousTrigger()` 函数，你可以在导航历史中向前或向后（如果可能）移动。

### NavigationHistory 属性

| 名称 | 类型 | 描述 | 默认值 |
|---|---|---|---|
| limit | {Integer} | 指定要存储的历史状态限制。 | 50 |
| nextStack | {Array} | 包含下一个历史状态的数组。实例化对象时不使用。 |  |
| previousStack | {Array} | 包含上一个历史状态的数组。实例化对象时不使用。 |  |

### NavigationHistory 函数

有两种函数可以调用来访问导航历史记录。

- **nextTrigger()**：此函数将调用子 `next` 控件的 trigger() 函数（也可以通过 `nav_history_control.next.trigger()` 调用）。如果存在下一个位置，它将把地图移动到位置栈上的下一个位置。
- **previousTrigger()**：此函数将调用子 `previous` 控件的 trigger() 函数（也可以通过 `nav_history_control.previous.trigger()` 调用）。如果存在上一个位置，它将把地图移动到位置栈上的上一个位置。

### Time for Action – using the NavigationHistory control

让我们使用 **NavigationHistory** 控件的 `nextTrigger` 和 `previousTrigger` 方法来遍历我们的导航历史。

- 1. 打开之前的示例并启用 Firebug。我们将通过控制台创建一个 **NavigationHistory** 控件对象并将其添加到地图中。输入并运行以下代码：
```
var nav_history_control = new OpenLayers.Control.NavigationHistory();
map.addControl(nav_history_control);
```
- 2. 在地图上平移和缩放。每次更新地图时，**NavigationHistory** 控件都会存储有关你操作的信息。然后，在 Firebug 中发出以下命令：
- 3. 你应该会看到地图返回到之前的缩放级别和坐标。现在，让我们通过调用以下命令返回到之前的位置：
```
nav_history_control.previousTrigger();
```
```
nav_history_control.nextTrigger();
```

### What Just Happened?

我们刚刚创建并向地图添加了 **NavigationHistory** 控件，然后调用了 `previousTrigger()` 和 `nextTrigger()` 方法。

## OpenLayers.Control.NavToolbar

OpenLayers 的 **NavToolbar** 控件创建了一个导航控件按钮工具栏。默认情况下它包含两个按钮——一个用于平移，一个用于缩放。平移模式允许你像在 **Navigation** 控件中一样在地图上平移；而缩放控件让你可以选择要缩放的范围。如果希望为地图提供额外的自定义功能，此控件尤其方便。

## OpenLayers.Control.OverviewMap

**OverviewMap** 控件在地图右下角创建一个可切换的窗口，显示一个较小的、缩小的地图，其中包含一个可拖动的矩形，表示你在地图上的当前位置（即概览地图）。当你放大地图本身时，概览地图会相应更新（尽管，你可以通过在 `mapOptions` 属性中设置 `numZoomLevels` 属性来关闭此行为）。

![](img/1b8cd151c9f278555f9cf255b20d244e_165_0.png)

你可能熟悉此工具，许多人期望它出现在网络地图上，因为 Google、Bing 等都有类似的控件——因此根据你的应用情况，包含它可能是一个好主意。我们应该介绍一些配置选项。

### OverviewMap 属性

| 名称 | 类型 | 描述 | 默认值 |
|---|---|---|---|
| size | {OpenLayers.Size} | 设置概览地图的大小，以像素为单位，格式为宽度、高度。示例调用为：`size: new OpenLayers.Size(300,300)`。这将增加概览地图本身的大小，但概览地图所在实际 div 元素的大小由 CSS 指定。 | `new OpenLayers.Size(180, 90)` |
| layers | {Array {OpenLayers.Layer}} | 指定从地图中用于概览地图的图层数组。如果此处未添加任何图层，默认情况下将使用底图作为概览地图。 | Uses the map base layer by default. |
| minRectSize | {Integer} | 此属性确定范围矩形的最小宽度或高度（以像素为单位），在此之前它将被 `minRectDisplayClass` 的值替换。数字越大，范围矩形在被替换前越大。此属性可与 `minRatio` 和 `maxRatio` 属性一起使用，以提供概览地图控件的更自定义行为。 | 15 |
| minRectDisplayClass | {String} | 这指定当达到 minRectSize 值时范围矩形将使用的类名。我们将在下一章关于样式化地图控件的部分更详细地讨论控件样式。 | 'RectReplacement' |
| minRatio | {Float} | 此属性有助于确定何时缩小概览地图。此比率通过将概览地图的分辨率除以底图的分辨率来计算。默认值是 8。这意味着如果概览地图的分辨率小于主地图分辨率的 8 倍，概览地图将缩小。 | 8 |
| maxRatio | {Float} | 此属性有助于确定何时放大概览地图。原则与上面相同。默认值是 32，意味着如果概览地图的分辨率大于主地图分辨率的 32 倍，概览地图将放大。 | 32 |
| mapOptions | `{Object}` | 这是一个对象，就像地图对象的 options 属性一样。它是一个匿名对象，可以包含地图可以包含的相同选项。设置其中一些选项非常有用，尤其是 `numZoomLevels` 属性。例如，`mapOptions: {numZoomLevels: 1}` 将导致概览地图永远不会放大或缩小，保持在最大范围。 | `{}` |
| autoPan | `{Boolean}` | 此属性确定是否自动平移概览地图以使范围矩形保持在中间。将此值设置为 `true` 将模仿 Google 地图中的概览地图行为。 | `false` |
| div | `{HTML Element}` | 如果指定，则为放置此控件的 HTML 元素。如果未指定，**OverviewMap** 控件位于地图右下角。 |  |

### OverviewMap 函数

- **minimizeControl()**：调用时，它将最小化图层切换控件。如果希望以编程方式隐藏图层切换控件，这尤其有用。
- **maximizeControl()**：它显示图层切换控件，本质上与 `minimizeControl()` 相反。

## OpenLayers.Control.PanPanel

此控件在地图左上角添加一个平移面板。它包含四张图片：上、左、右、下箭头。单击箭头将按箭头方向平移地图。
