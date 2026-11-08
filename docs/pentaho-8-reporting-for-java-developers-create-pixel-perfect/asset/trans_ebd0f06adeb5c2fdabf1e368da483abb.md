
*该属性将被传播到所有后代（即整个报表）直到其中一个显式重写该值。*

你可以很容易地通过元素属性面板中对应项的标志 ☑ 是否被勾选来识别某个属性值是从父级继承的。可以想象，你可以打破可继承性，通过更改属性值来实现。这样做时，标志会自动设置为 *未勾选*。从那时起，所有后代元素都将继承新的属性值，直到另一次显式更改被设置。再次点击标志（☑）图标，你可以指示 Report Designer 再次从父元素继承属性值，就像默认情况下一样。

每个选项卡中的属性都按子集分组，以便在开发过程中更好地选择和搜索。这些子集使用有意义的描述命名，例如：`size & position`、`font`、`text`、`text-spacing`、`object`、`link`、`excel`、`padding`、`border`、`query`、`common`、`query-metadata`、`parameter`、`wizard`、`html`、`html-event`、`pdf-event`、`swing-event`、`advanced`、`pentaho`，还有一些。

在报表中选择一个元素（使用报表画布、使用报表资源管理器，或使用 Report Designer 的任何其他可能方式），元素属性面板将显示其所有可用属性。每次属性值的更改都会立即反映在报表画布中的报表预览中。

## Master report and report bands properties

现在元素和属性的概念已经介绍过，让我们开始讨论报表的根（`Master Report` 元素）以及树结构中的第一层级，即频段。具体来说，我们将描述与每种元素类型相关的属性，从通用属性开始，然后逐个元素描述特定属性。

你创建的每个 Pentaho 报表都有一个 `Master Report` 元素作为树层次结构的根。`Master Report` 元素在预览或报表画布中没有明确显示，但它包含一组通用属性。在以下专门介绍 `Master Report` 元素的部分中，你将了解有关此根元素的更多特定属性。

初次创建报表时，在你的画布和报表资源管理器结构树中，你会看到：`Page Header`、`Report Header`、`Details`、`Report Footer` 和 `Page Footer` 频段。这些频段以及其他频段，包括 `Group`、`Watermark` 和 `No Data`，构成了整个可视报表。所有频段都可能包含调色板中的元素并充当渲染这些元素的容器。每个频段都有不同的生命周期，并根据其上下文进行渲染。

为了更好地解释属性的重要性及其在报表开发中的作用，你可以在 https://github.com/fcorti/pentaho-8-reporting-for-java-developers 仓库中找到一些 Pentaho 报表的引用示例；更准确地说，在 `Chapter 05 - Design and layout in Report Designer/my_reports` 路径中。每个报表都展示了相关属性（或一组属性）的使用作为示例，可以作为你理解和学习的指南。

## Common properties

如 *The report element properties* 部分所述，嵌套元素之间属性值的继承是 Pentaho Reporting 的一个重要特性。当然，只有在父元素和子元素都使用相同属性时，继承才有效。这一概念引入了这样一个原则：大量通用属性在各种元素之间共享（并被继承）。在本节中，你将深入了解 `Master Report` 元素与各种类型频段之间的通用属性。

## Size and position properties

这组属性定义频段的大小和位置。这些属性可以通过在结构树中选择频段然后点击 Format | Size & Border... 菜单项来一起编辑，或在

![](img/9337ac94537bf7e092e1ea6531dce8fd_193_0.png)

report element Style tab 中单独编辑：

下表列出了你在报表元素资源管理器中 Style 选项卡的 Size & Border... 组中可以看到的大小和位置属性：

| 名称 | 类型 | 描述 |
|------|------|------|
| layout | Selection | 这仅适用于频段，并定义频段内元素如何渲染。- 默认布局为 `canvas`，允许你指定频段中每个报表元素应渲染的确切位置。- `block` 布局将频段中的所有项目从上到下堆叠。- `inline` 布局将所有项目从左到右堆叠，并自动换行。- `row` 布局将所有报表元素显示在单行中。- `auto` 布局自动显示报表元素以避免重叠。- 与 `table` 相关的布局（包括特定的 `table-body`、`table-header`、`table-footer`、`table-row`、`table-cell`、`table-col` 和 `table-col-group`）在将报表导出为 HTML 时，将所有项目堆叠在 HTML 表格结构中。 |
| x | Integer | 该元素在其父容器内的 x 位置。 |
| width | Decimal | 该元素的宽度。-100 到 0 之间的数字表示父容器宽度的百分比。 |
| y | Integer | 该元素在其父容器内的 y 位置。 |
| height | Decimal | 该元素的高度。-100 到 0 之间的数字表示父容器高度的百分比。 |
| invisible-consumes-space | Boolean | 如果设置为 true，则该频段中不可见的子元素仍会在报表中占用空间。 |
| visible | Boolean | 如果设置为 false，则元素不被渲染。 |
| dynamic-height | Boolean | 如果设置为 true，则通知报表引擎该元素具有动态高度。 |
| preferred-width | Decimal | 该元素的首选宽度。 |
| preferred-height | Decimal | 该元素的首选高度。 |
| min-width | Decimal | 该元素的最小宽度。 |
| min-height | Decimal | 该元素的最小高度。 |
| max-width | Decimal | 该元素的最大宽度。 |
| max-height | Decimal | 该元素的最大高度。 |
| x-overflow | Boolean | 如果设置为 true，文本可能会水平溢出元素之外。 |
| y-overflow | Boolean | 如果设置为 true，文本可能会垂直溢出此元素之外。 |
| fixed-position | Integer | 如果指定，则设置该频段在报表内的固定垂直位置。 |
| box-sizing | Selection | 这可以设置为 `content-box` 或 `border-box`。如果设置为 `content-box`，则尺寸样式不包括边框；如果设置为 `border-box`，则尺寸样式包括边框框。此样式的默认值是 `content-box`。 |
| table-layout | Selection | 高级功能，用于指示 Pentaho 报表引擎以 `fixed` 或 `automatic` 大小和位置渲染表格。 |

如你在环境中看到的，点击报表元素资源管理器并在元素属性选项卡中检查，只有这些属性的一小部分在 `Master Report` 元素中可用，但在所有类型的频段中大部分都是可用的。

![](img/9337ac94537bf7e092e1ea6531dce8fd_196_0.png)

作为练习，从 *Chapter 4, Creating a Report with Report Designer* 中开发的 `report_02.prpt` 开始，并修改图表使其覆盖页面宽度的 50% 并将行高扩展到 40 像素。要查看最终结果，你可以检查存储在 GitHub 仓库的 `chapter 05 - Design and layout in Report Designer/my_reports` 文件夹中的 `report_06.prpt` 文件。

## Font, text, and text-spacing properties

`Font`、`text` 和 `text-spacing` 是三个基本属性集，完全致力于报表中的文本布局。与之前属性集不同，大多数属性是从 `Master Report` 和所有类型的频段共享（并继承）的。让我们在以下各节中详细查看每个属性。
