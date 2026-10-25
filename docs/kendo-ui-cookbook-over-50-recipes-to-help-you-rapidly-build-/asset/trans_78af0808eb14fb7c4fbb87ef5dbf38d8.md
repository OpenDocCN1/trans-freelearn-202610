


date: new Date('2014/01/02')
},
.
.
. {
 billAmount: 80,
 date: new Date('2014/01/07')
}, {
 billAmount: 70,
 date: new Date('2014/01/07')
}
]

在前面的代码片段中提到的 DataSource 对象中，每个订单包含两个字段，即 billAmount 和 date。订单数据跨越 2014 年 1 月 1 日至 2014 年 1 月 7 日。这只是示例数据，您可以考虑跨越一个月或一年的数据。

我们希望在两个级别上显示分析：

- 一段时间内的数据
- 按日期分组并应用某些聚合函数（例如平均值、总计和计数）的数据

要显示一段时间内的数据，可以在 categoryAxis 字段中设置 baseUnit 值：

```
categoryAxis: {
 baseUnit: 'days'
}
```

这将按天对 DataSource 对象中的对象进行分组。其他可能的选项包括 weeks、months 和 years。aggregate 字段用于指定系列中要使用的聚合函数，如下面的代码片段所示：

```
series: [{
 type: 'area',
 aggregate: 'sum',
 field: 'billAmount',
 categoryField: 'date',
 labels: {
 visible: true,
 background: 'transparent'
 },
 color: '#0F0'
}]
```

在此处，在系列定义中，aggregate 字段指定 sum 作为要使用的聚合函数；这意味着图表将绘制餐厅在一段时间内的总营业额。可以分配给 aggregate 的其他可能值包括 avg、count、first、min 和 max。还有一个选项可以提供自定义聚合器函数，该函数将返回应用于投影数据的值。

series 选项指定系列的颜色，labels 选项用于在图表上显示数据值。当显示单个系列时，所有选项都可以在 series 选项中指定，而不是使用 seriesDefaults 选项。

## 工作原理...

执行页面时，订单数据将绘制在图表上，显示一段时间内的总营业额，如下面的屏幕截图所示：

![](img/2a66507445090790d34dc5588b56b24c_194_0.png)

同样，您可以创建多个图表，使用其他聚合函数（例如 avg 和 count），分别显示一段时间内的平均账单金额和订单总数。

## Kendo UI DataViz

您可以通过在页面上创建多个 `div` 元素（每个图表一个）、绘制按日期分组的数据以及使用聚合函数来构建显示这些统计信息的仪表板。

![](img/2a66507445090790d34dc5588b56b24c_195_0.png)

## 更多...

前面示例中构建的面积图通过直线连接数值。您可以通过指定要使用的线条样式来自定义此行为。可能的值为 step 和 smooth：

```
line: {
    style: 'smooth'
}
```

当您指定 `smooth` 时，图表中的数值将通过平滑线连接，即一条曲线，它连接两个点，使图表看起来像是一条连接所有点的曲线，如下所示：

![](img/2a66507445090790d34dc5588b56b24c_195_1.png)

当您将线条样式指定为 `step` 时，图表上的数值将通过直角线连接。

![](img/2a66507445090790d34dc5588b56b24c_196_0.png)

## 通过添加事件使图表具有交互性

Kendo UI Chart 库可以定义各种事件处理程序，当用户将鼠标悬停在系列中的数据项上或单击该数据项时，这些处理程序将被触发。在此示例中，当用户将鼠标悬停在图表中的数据项上时，我们将显示一个工具提示；在另一个示例中，当用户单击某个列时，我们将更改所选列的颜色。

## 操作步骤...

让我们考虑与我们用于展示 Chrome 浏览器多年来采用率的相同 `DataSource` 对象。在此示例中，让我们添加另一个字段 `feature`，我们将使用它来显示 `tooltip`：

```
var chromeBrowser = [{
'year'   : 2008,
'percentage': 0,
'feature'   : 'First stable release'
}, {
'year'   : 2009,
'percentage': 4,
'feature'   : 'HTML5 Video and Audio tag support'
}, {
'year'   : 2010,
'percentage': 15,
'feature'   : 'Support for WebM videos'
}, {
'year'   : 2011,
'percentage': 26,
'feature'   : 'Hardware accelerated 3D CSS'
}, {
'year'
: 2012,
'percentage' : 40,
'feature' : 'New Extensions API'
}, {
'year'
: 2013,
'percentage' : 52,
'feature' : 'Web Speech API'
}, {
'year'
: 2014,
'percentage' : 56,
'feature' : 'Different look for Win8 metro mode'
}];
```

下一步是指定 tooltip 选项，该选项包含将在用户将鼠标悬停在系列中的数据项上时用于显示工具提示的字段，使用以下代码片段：

```
tooltip: {
visible: true,
template: '#= dataItem.feature #',
background: '#0F0'
}
```

在此处，template 字段包含 dataItem.feature 值。dataItem 字段指用于在图表中绘制列的对象。其他可能的值包括 category，它指 categoryAxis 中的类别，在此情况下将是 year 字段；value，它指用于绘制列的 percentage 值；以及 series，它指整个系列对象。

要处理单击事件，请指定 seriesClick 选项：

```
seriesClick: function (e) {
//iterate over series data and set the color for each series
for (var i = 0, j = e.series.data.length; i < j; i++) {
e.series.data[i].color = '#00F';
}
//change the color of the selected series
e.dataItem.color = '#F00';
//redraw the graph - required for the changes to get reflected
this.redraw();
}
```

前面函数中的第一条语句遍历系列中的所有对象并重置颜色；然后，系列中的选定项被分配不同的颜色。图表需要刷新或重新绘制才能使这些更改生效；因此，最后一行在此对象上调用 redraw 函数。this 对象指上下文；在此情况下，this 是系列对象。

订阅事件的另一种方法是在图表对象上使用 bind 方法添加监听器：

```
var chart = $('#chart').data('kendoChart');
chart.bind('seriesClick', function (e) {
    for (var i = 0, j = e.series.data.length; i < j; i++) {
        e.series.data[i].color = '#00F';
    }
    e.dataItem.color = '#F00';
    this.redraw();
});
```

前面代码片段中的第一行获取对图表对象的引用。然后使用图表对象订阅 seriesClick 事件。现在，当用户单击系列中的特定列时，将调用回调函数。

> 刷新或重新绘制图表时，在绘制图表时会显示动画。要禁用动画，请将 transitions 属性指定为 false。

## 工作原理...

当您将鼠标悬停在绘制区域上时，您将看到数据项中的 feature 值显示为工具提示，如下面的屏幕截图所示：

![](img/2a66507445090790d34dc5588b56b24c_198_0.png)

## Kendo UI DataViz

现在将系列类型更改为 `column` 并刷新页面；您将看到图表中所有列的颜色相同。当您单击图表中的任何列时，所选列的颜色将更改并重新绘制。

![](img/2a66507445090790d34dc5588b56b24c_199_0.png)

在前面的屏幕截图中，所选列的颜色已更改并在图表中突出显示。

## 动态更改图表类型

在此示例中，我们将了解如何在运行时更改图表的类型和皮肤。例如，如果您已投影一个列图表并希望将其更改为，例如，折线图，则可以对系列进行更改，图表将更新。当您希望以多种形式直观地表示数据但由于空间限制无法做到时，这很有用。在此示例中，我们将使用 `data` 属性而不是使用 `kendoChart` 函数来构建图表。这是构建图表的另一种方式。此外，我们将使用模型-视图-视图模型模式，当用户更改图表类型或皮肤时，对图表进行更改。此模式消除了添加事件处理程序的需求，而是依赖于监听模型更改以动态更新图表。

## 操作步骤...

让我们通过指定各种选项作为 data 属性来创建图表：

```
<div id="chartArea" data-role="chart" data-title="{text: 'Browser adoption', position: 'bottom'}" data-category-axis="{field: 'year'}" data-series-defaults="{type: 'area'}" data-theme="blueopal" data-legend="{position: 'bottom'}" data-series="[


