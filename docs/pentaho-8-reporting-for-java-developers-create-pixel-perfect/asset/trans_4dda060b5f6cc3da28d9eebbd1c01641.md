
## 气泡图

气泡图允许您查看数据的三个维度。前两个维度是传统的 x 和 y 维度，也称为域和范围。第三个维度通过渲染的各个气泡的大小来表示。气泡图使用之前定义过的通用属性，包括 XYZ 系列数据集属性。以下描述列出了在可用属性方面的所有差异：

- 属性的标题集提供了一个额外的属性：
- 选项属性集提供了之前介绍的一些属性：`horizontal`、`series-color` 和 `label-font`。此外，还有 `max-bubble-size` 属性（十进制），定义首选最大气泡大小，作为所代表气泡值的百分比。可能的值为 1 到 100 之间的整数。
- 通用集具有与饼图描述相同的属性（通用通用集，以及 `shadow-paint`、`shadow-x-offset`、`shadow-y-offset`、`url-formula` 和 `tooltip-formula`）。
- X 轴和 y 轴集在此也提供，与条形图描述的属性相同。
- 值集具有条形图描述的属性的子集（特别是：`show-item-labels`、`item-label-font`、`url-formula` 和 `tooltip-formula`）。
- 图例集具有与饼图描述相同的属性（图例通用集以及 `legend-label-format`）。

| 名称 | 类型 | 描述 |
|------|------|-------------|
| `pos-title` | Selection | 设置显示图表标题的位置。可能的值为：north, west, south, east, top, left, bottom 和 right |

### 气泡图示例

例如，让我们重用为条形图描述的相同数据源。从空白报表开始，将方向更改为横向，并按前面的示例所述设置图表元素。

通过双击图表元素，将显示图表编辑窗口。从上部面板选择气泡图图标，并在数据集面板中直接将 `x-value-columns` 属性设置为 `BUYPRICE`，将 `y-value-columns` 和 `z-value-columns` 属性设置为 `QUANTITYINSTOCK`，并将 `series-by-field` 属性设置为 `PRODUCTLINE`。在属性面板中，将 `max-bubble-size` 属性（在 `Options` 组中）设置为 `100`，然后按 OK 按钮并通过按眼睛（👁️）图标预览生成的报表。以下屏幕截图显示了您第一个气泡图的预览效果：

![](img/9337ac94537bf7e092e1ea6531dce8fd_388_0.png)

## 散点图

散点图将系列中的所有项目作为点渲染在图表中。此图表类型使用之前定义的通用属性，默认包括 XY 系列数据集属性。作为替代数据集，您也可以选择使用 XYZ 系列数据集或时间序列数据集。您可以使用 Primary DataSource 选项卡上部下拉菜单更改数据集。关于可用属性，以下描述列出了与本章开头介绍的通用列表的所有差异：

- 属性的标题集提供了与气泡图描述相同的属性（标题通用集以及 `pos-title`）。
- 选项属性集提供了之前介绍的一些属性：`stacked`、`horizontal` 和 `series-color`。此外还提供了 `dot-height` 和 `dot-width` 属性（整数），它们定义表示系列点的标记的高度/宽度（以像素为单位）。
- 通用集具有与通用集描述相同的属性，此外还具有 `show-plot-border` 和 `label-font`（请参阅条形图了解其定义）。
- X 轴和 y 轴集在此也提供，与条形图描述的属性相同。
- 值集具有条形图描述的属性的子集（更详细地说：`show-item-labels`、`item-label-font`、`url-formula` 和 `tooltip-formula`）。

### 散点图示例

例如，即使对于这种情况并不完美，让我们重用为条形图介绍的相同数据源。从空白报表开始，将方向更改为横向，并按前面的示例所述设置图表元素。

通过双击图表元素，将显示图表编辑窗口。从上部面板选择散点图图标，并在数据集面板中直接将 `x-value-columns` 属性设置为 `BUYPRICE`，将 `y-value-columns` 属性设置为 `QUANTITYINSTOCK`，并将 `series-by-field` 属性设置为 `PRODUCTLINE`。然后按 OK 按钮并通过按眼睛（👁）图标预览生成的报表。以下屏幕截图显示了您第一个散点图的预览效果：

![](img/9337ac94537bf7e092e1ea6531dce8fd_390_0.png)

## XY 条形图、XY 折线图和 XY 面积图

XY 条形图、XY 折线图和 XY 面积图将 XY 系列数据集绘制为条形图、折线图或面积图。这些图表类型使用之前定义的通用属性，默认包括 XY 系列数据集属性。作为替代数据集，您也可以选择使用 XYZ 系列数据集或时间序列数据集。您可以使用 Primary DataSource 选项卡上部下拉菜单更改数据集。

关于 XY 条形图的可用属性，以下描述列出了与本章开头介绍的通用列表的所有差异：

- 标题集具有与气泡图描述相同的属性（标题通用集以及 `pos-title`）
- 选项集具有与条形图描述相同的属性，但不包括 `max-bar-width`
- 通用集具有与条形图描述相同的属性，但不包括 `gridlines`
- 阴影集具有与条形图描述相同的属性，但不包括 `shadow-color`
- X 轴集由不同的属性组成，如下所示：

| 名称 | 类型 | 描述 |
|---|---|---|
| `x-axis-title` | String | 设置域轴的标题（如果存在）。在典型的二维水平图表中，这是分类轴或 x 轴。 |
| `x-font` | String | 设置范围轴标题的字体。 |
|  |  |  |

| 属性 | 类型 | 描述 |
|---|---|---|
| `x-sticky-0` | Boolean | 如果范围包含轴中的零，使其粘性将强制轴在设置为 true 时截断到零。此属性的默认值是 true。 |
| `x-incl-0` | Boolean | 如果设置为 true，范围轴将强制包含零在轴中。 |
| `x-auto-range` | Boolean | 如果设置为 true，范围轴将根据内容自动调整。 |
| `x-min` | Decimal | 在范围轴中渲染的最小值。 |
| `x-max` | Decimal | 在范围轴中渲染的最大值。 |
| `x-tick-interval` | Decimal | 在图表中分隔范围刻度的数值间隔。 |
| `x-vtick-label` | Boolean | 设置域轴上刻度标签的方向；垂直为 true，水平为 false。默认值为 false。 |
| `x-tick-font` | String | 渲染范围刻度值的字体。 |
| `x-tick-ftm-str` | String | 渲染数值范围刻度值的 `DecimalFormat` 字符串。 |
| `x-tick-period` | Selection | 如果 x（范围）数据集返回日期或时间数据，则指定 x 轴刻度间隔。 |

- Y 轴集具有与条形图描述相同的属性，但不包括 `y-scale-factor`
- 值集是条形图描述的属性的子集，仅限于 `show-item-labels`、`item-label-font`、`url-formula` 和 `tooltip-formula`

关于 XY 折线图的可用属性，以下描述列出了与本章开头介绍的通用列表的所有差异：

- 标题集具有与气泡图描述相同的属性（标题通用集以及 `pos-title`）
- 选项集具有与折线图描述相同的属性，但不包括 `show-markers` 并包括 `stacked`（在条形图的选项组中引入）
- 通用集具有与条形图描述相同的属性，但不包括 `gridlines`
- X 轴集具有与 XY 条形图描述相同的属性
- Y 轴集具有与条形图描述相同的属性，但不包括 `y-scale-factor`
- 值集是条形图描述的属性的子集，仅限于 `show-item-labels`、`item-label-font`、`url-formula` 和 `tooltip-formula`

关于 XY 面积图的可用属性，以下描述列出了与本章开头介绍的通用列表的所有差异：

- 标题集具有与气泡图描述相同的属性（标题通用集以及 `pos-title`）
- 选项集具有与条形图描述相同的属性，但不包括 `stacked-percent` 并包括 `stacked`（在条形图的选项组中引入）、`line-style`、`line-size` 和 `show-markers`（在折线图的选项组中引入）
- 通用集具有与条形图描述相同的属性，但不包括 `gridlines`
- X 轴集具有与 XY 条形图描述相同的属性
- Y 轴集具有与条形图描述相同的属性，但不包括 `y-scale-factor`
- 值集是条形图描述的属性的子集，仅限于 `show-item-labels`、`item-label-font`、`url-formula` 和 `tooltip-formula`

