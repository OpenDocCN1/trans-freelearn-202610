
## 页眉和页脚属性

`Page Header` 和 `Page Footer` 频段出现在每个页面的开头和结尾，由特定的输出格式决定。

这些频段与之前定义的通用属性略有不同。`pagebreak-before` 和 `pagebreak-after` 属性不适用于这些频段。此外，除了默认值外，还提供以下属性：`display-on-firstpage`、`display-on-lastpage` 和 `sticky`。

除通用属性外，还提供了 `table` 集合，包含以下属性：

| 名称 | 类型 | 描述 |
|------|------|-------------|
| `col-span` | Integer | HTML 渲染时的列跨度 |
| `row-span` | Integer | HTML 渲染时的行跨度 |

在 `excel` 集合中，`page-header-left`、`page-header-center` 和 `page-header-right` 以十进制值提供。

## 报表页眉和报表页脚属性

`Report Header` 和 `Report Footer` 出现在报表的开头和结尾，通常用于显示标题和汇总信息，如图表、目录等。`Report Header` 和 `Report Footer` 在通用集合之外还具有 `table` 集合作为附加属性。

## 组页眉和组页脚属性

`Group Header` 和 `Group Footer` 频段可能为报表中配置的每个已定义组出现。报表中定义的分组是一组在选定数据列中具有相同值的数据。 当定义分组列中的值发生变化时，会触发新的组。

![](img/9337ac94537bf7e092e1ea6531dce8fd_236_0.png)

> *将作为分组定义的列在传递到 Pentaho Reporting 之前进行适当排序至关重要，否则在渲染的报表中可能会出现重复的组。*

`Group Header` 和 `Footer` 与之前定义的通用属性略有不同，因为除了默认值外，还提供以下属性：`repeat-header`、`sticky`、`page-header-left`、`page-header-center` 和 `page-header-right`。`Group Header` 和 `Footer` 在通用集合之外还具有 `table` 集合作为附加属性。

## 详细信息主体属性

`Details Body` 由四个不同的频段组成。`Details Header` 和 `Details Footer` 频段在明细行分组之前和之后渲染。`Details` 频段为每行数据渲染一次，而在报表没有可用数据时渲染 `No Data` 频段。`Details Header`、`Details Footer` 和 `No Data` 频段在 Report Designer 中默认隐藏。

`Detail Header` 和 `Detail Footer` 频段与组频段共享相同的附加属性 `sticky`、`repeat-header`、`page-header-left`、`page-header-center` 和 `page-header-right`。此外，`table` 集合作为通用集合之外的附加属性提供。`table` 属性集合也提供给 `No Data` 频段。其余明细频段未定义超出通用属性集合的附加属性。

## 水印属性

`watermark` 频段出现在所有其他频段之后，用于报表的背景图像和样式。`watermark` 频段在通用集合之外还具有 `table` 集合作为附加属性。

## 报表元素

所有可用的 **报表元素** 都会出现在调色板中，可以拖放到报表画布中。报表元素构成了报表的內容。它们范围从标签和文本元素到图形、图表和子报表元素。

与频段类似，其他报表元素也会显示在报表资源管理器结构选项卡中，属性列表可在报表元素属性中获得。与上一节描述的元素类似，报表元素也会继承其父元素的现有属性，并将通用属性传播到子元素。

在本节中，您将了解调色板中每种类型元素的相关属性。正如我们在上一节关于频段中所看到的，您将从通用属性开始，然后查看每个元素的详细描述。

![](img/9337ac94537bf7e092e1ea6531dce8fd_239_0.png)

> *在本例中也适用本书前面分享的建议：尽可能多地使用元素和 Report Designer 进行操作以获得直接体验。如果尝试向示例报表添加元素，可以在面板中查看属性并尝试更改值，记住其行为。再次，直接体验是最好的老师。*

## 通用属性

与频段和 `Master Report` 类似，报表元素也有按组组织的通用属性，以便更好地理解。幸运的是，报表元素的绝大多数通用属性与您为频段所见的通用属性相同。

考虑到这一点，这很有道理，因为报表元素的属性嵌套在频段中，因此自然会发现频段的一些（或全部）通用属性被继承到子元素中。

考虑到报表元素非常具体，针对一个目的（标签、图表、来自数据源的字段、形状等），频段的一些属性会被继承到某些报表元素中，而不会被继承到其他元素中。无论如何，您将在专门针对每种报表元素类型的章节中找到详细的差异。

回到通用属性；以下是属性集合的详细描述。

## 大小和位置属性

此属性集合与您为频段和 `Master Report` 所见的相同，在有意义的地方。`Layout` 用于所有可以根据报表拉伸或修改宽度/长度的报表元素。

## 字体属性

此属性集合与您为频段和 `Master Report` 所见的相同，在有意义的地方。对于某些报表元素，如形状和线条，字体属性不可用，因为不使用文本。

## 文本和文本间距属性

这两个属性集合与您为频段和 `Master Report` 所见的相同，在有意义的地方。对于某些报表元素，如形状、`simple-barcodes`、`survey-scale` 和线条，`text` 和 `text-spacing` 属性仅限于以下属性：`h-align`、`v-align`、`text-color` 和 `bg-color`。

## 旋转属性

此集合在此首次引入，仅适用于基于文本的元素，如标签等：

| 名称 | 类型 | 描述 |
|------|------|-------------|
| `rotation` | Selection | 旋转，值为 90 或 -90 |

## 对象属性

此属性集合与您为频段和 `Master Report` 所见的相同。对于某些报表元素，如形状、`simple-barcodes`、`survey-scale`、图像和线条，`object` 属性扩展了该集合所有可用的属性。

## 链接属性

此属性集合与您为频段和 `Master Report` 所见的相同。对于某些报表元素，如形状、`simple-barcodes`、`survey-scale`、图像和线条，不提供 `pdf-bookmark` 属性。

## Excel 属性

此属性集合与您为频段和 `Master Report` 所见的相同，在有意义的地方。对于某些报表元素，如形状、`survey-scale`、`simple-barcodes` 和线条，`excel` 属性不可用，因为它们不是必需的。在某些情况下，例如图像，不提供 `sheet-name` 和 `format-override` 属性。

## 页面行为属性

此属性集合由您为频段和 `Master Report` 所见的属性的子集组成，特别是 `avoid-page-break`、`orphan`、`widows` 和 `widow-orphan-opt-out`。在某些情况下，例如频段和子报表元素，还提供 `pagebreak-before` 和 `pagebreak-after` 属性。

## 内边距和边框属性

这些属性集合与您为频段和 `Master Report` 所见的完全相同。
