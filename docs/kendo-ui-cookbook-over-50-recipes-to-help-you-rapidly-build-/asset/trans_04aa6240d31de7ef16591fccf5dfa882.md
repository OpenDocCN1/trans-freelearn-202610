

在前面的代码片段中，所有选项均作为数据属性指定。数据属性 role，其值设置为 chart，将 div 元素初始化为图表小部件。它还具有 data-bind 属性，该属性指定其源为 dataset。data-bind 属性用于指定模型绑定值。在此示例中，指定了源绑定，即此图表绑定到的 DataSource 对象。我们即将构建的 ViewModel 对象将指定 dataset 属性。

下一步是创建两个下拉列表，一个列出图表类型，另一个列出应应用的各种主题或皮肤。我们还添加一个复选框，表示图表是否应堆叠，如下面的代码片段所示：

```html
<select id="chartTypeDropdown" data-bind="value: chartType,
events: {change: onChangeHandler}">
<option value="area">Area</option>
<option value="line">Line</option>
<option value="column">Column</option>
</select>
<select id="chartSkin" data-bind="value: chartSkin,
events: {change: onChangeHandler}">
<option value="default">Default</option>
<option value="blueopal">BlueOpal</option>
<option value="bootstrap">Bootstrap</option>
<option value="highcontrast">HighContrast</option>
<option value="metro">Metro</option>
<option value="metroblack">MetroBlack</option>
<option value="silver">Silver</option>
<option value="uniform">Uniform</option>
</select>
<input type="checkbox" data-bind="checked: isStacked,
events: {change: onChangeHandler}" />Stacked
```

在此，select 元素和 input 元素均指定了 data-bind 属性。该属性分别指定 value 和 checked 的模型绑定，并指定 change 事件的事件绑定。所有这些都应在 ViewModel 中指定。

下一步是指定将用于绘制图表的数据源。让我们使用之前示例中使用的同一数据源：

```javascript
var browserAdoption = [{
'year': 2011,
'chrome': 40,
'firefox': 30,
'ie': 15,
'safari': 15
}, {
'year': 2012,
'chrome': 45,
'firefox': 25,
'ie': 12,
'safari': 18
}, {
'year': 2013,
'chrome': 50,
'firefox': 22,
'ie': 8,
'safari': 20
}];
```

最后一步是创建一个 ViewModel 对象，该对象包含先前提到的元素将使用的所有模型属性。它还应指定 onChangeHandler 函数，当用户更改下拉列表中的值或选择堆叠复选框时将调用该函数，如下面的代码片段所示：

```javascript
var viewModel = kendo.observable({
chartType: 'area',
chartSkin: 'blueopal',
isStacked: false,
onChangeHandler: function (e) {
var chart = $('#chartArea').data('kendoChart');
chart.setOptions({
seriesDefaults: {
type: this.chartType,
stack: this.isStacked
},
theme: this.chartSkin
});
chart.refresh();
},
dataSet: new kendo.data.DataSource({
data: browserAdoption
})
});
```

在此，指定了 chartType、chartSkin、isStacked、dataset 和 onChangeHandler 等各种模型属性的绑定。onChangeHandler 函数是一个事件处理程序，用于处理来自下拉列表和输入复选框的 change 事件。当其中任何值更改时，图表将被更新。setOptions 函数用于在 seriesDefaults 字段中指定类型和 stack 值，以及要用于图表的主题。然后调用 refresh 函数来更新图表。dataSet 属性指定绘制图表时要使用的 DataSource 对象。

# 工作原理...

当您呈现页面时，您将看到应用了 **BlueOpal** 皮肤的 **area** 图表，如下面的屏幕截图所示：

![](img/2a66507445090790d34dc5588b56b24c_202_0.png)

现在，让我们将图表类型更改为 **Line**，主题更改为 **Silver**；这将触发 ViewModel 对象中的 onChangeHandler 函数，图表将使用所选选项进行更新。

![](img/2a66507445090790d34dc5588b56b24c_203_0.png)

现在，让我们更改所有三个选项，即类型更改为 **Column**，主题更改为 **Bootstrap**，并选择 **Stacked** 选项。当您选择复选框时，stack 选项设置为 true，系列数据现在将堆叠，而不是为每个浏览器显示四个不同的列。

![](img/2a66507445090790d34dc5588b56b24c_203_1.png)

在前面的屏幕截图中，各系列的值相互堆叠。

# 11 Kendo UI DataViz – 高级图表

在本章中，我们将介绍以下示例：

- 创建显示股票历史的图表
- 使用径向仪表小部件
- 使用线性仪表小部件
- 使用各种编码方法生成条形码图像
- 生成表示 URL、电子邮件、电话和地理位置的二维码图像
- 使用 Kendo Diagram 创建流程图
- 使用 `layouts` 创建层次结构图
- 使用 OpenStreet 地图创建显示地理空间信息的地图
- 通过将其绑定到 GeoJSON 数据创建地图

## 简介

Kendo UI 库提供了多种高级图表小部件，可用于可视化数据。这些包括显示特定股票股票价格历史的 Stock Chart 小部件、在地图上可视化数据的 Map 小部件、创建包含仪表图表的仪表板的 Gauge 小部件、允许您创建可用于识别产品的代码的 Barcode 和 QRCode 小部件，以及创建显示各种实体之间嵌套关系的图表的 Diagram 小部件。这些是除了前一章中提到的基本图表之外的一些高级图表。

# 创建显示股票历史的图表

Kendo UI 库提供了一个专用的图表小部件，可用于显示特定股票在一段时间内的股票价格数据。在此示例中，我们将了解如何创建 Stock 图表并对其进行自定义。

## 开始使用

在 head 部分中包含 CSS 文件 kendo.dataviz.min.css 和 kendo.dataviz.default.min.css。这些文件用于样式化股票历史图表的某些部分。

## 操作方法...

Stock 图表由两个图表组成：一个显示股票历史的窗格，以及一个用于通过更改日期范围来浏览图表的另一个窗格。

特定股票在某一天的股票价格可由以下五个属性表示：

- Open：显示交易开始当天的股票价值
- Close：显示交易结束当天的股票价值
- High：显示股票当天能够达到的最高值
- Low：显示股票当天达到的最低值
- Volume：显示当天交易的该股票的总股数

假设服务以以下格式返回此数据：

```json
[
    {
        "Date" : "2013/01/01",
        "Open" : 40.11,
        "Close" : 42.34,
        "High" : 42.5,
        "Low" : 39.5,
        "Volume": 10000
    }
    .
    .
    .
]
```

我们将使用前面的数据创建 Stock 图表。kendoStockChart 函数用于创建 Stock 图表，并使用与区域图或柱形图类似的一组选项进行配置，如前一章中所述。除了系列数据外，您还可以指定 navigator 选项，以在图表下方显示包含完整股票历史的导航窗格：

```javascript
$("#chart").kendoStockChart({
    title: {
        text: 'Stock history'
    },

    dataSource: {
        transport: {
            read: '/services/stock?q=ADBE'
        }
    },

    dateField: "Date",

    series: [{
        type: "candlestick",
        openField: "Open",
        closeField: "Close",
        highField: "High",
        lowField: "Low"
    }],

    navigator: {
        series: {
            type: 'area',
            field: 'Volume'
        }
    }
});
```

在前面的代码片段中，DataSource 对象指的是将返回一组天数股票数据的远程服务。series 选项指定系列类型为 candlestick；此处使用烛台图来指示特定日期的股票价格。指定了 openField、closeField、highField 和 lowField 的映射；它们将用于绘制图表，并在用户将鼠标悬停在其上时显示提示。navigator 选项指定用于创建区域图，该区域图使用 volume 数据来绘制图表。dateField 选项用于指定图表中的日期字段与响应中的日期字段之间的映射。

