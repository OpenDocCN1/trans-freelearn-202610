
| 按职位标题的实际值 |  |  |
|---|---|---|
| District Manager | 682,625 | 617,250 |
| Senior Sales Rep | 497,223 | 484,820 |
| Sales Rep | 675,975 | 612,500 |
| Account Executive | 409,975 | 422,500 |

在继续自定义之前，通过点击铅笔图标（✏️）再次切换到设计模式。

## 添加明细标题并自定义明细

现在报告中的标题已正确开发，我们应该再次处理明细以提高最终结果的吸引力。为了达到该目标，让我们看看如何为明细添加标题以及如何添加交替行颜色。

默认情况下，报告带有隐藏的明细标题频段（正如您所见，明细标题频段在 Report Canvas 选项卡中不可见）。要显示明细标题，请在 Report explorer 的 Structure 选项卡中选择 Details Header 项（Details Header 是 Details Body 的子项）。之后，切换到 Element properties 中的 Attributes 选项卡并将 hide-on-canvas 属性更改为 false。立即，您会看到 Report canvas 中出现一个名为 details header 的新频段。

正如您所料，此频段可作为常规频段使用，如果您希望在报告的最终版本中显示有关明细标题的内容，则应进行填充。在我们的示例中，我们希望为每个字段显示一个标签作为标题。

在添加标签之前，再次点击 Element properties 的 Style 选项卡并将 text-color 属性的值更改为 #77b3d4。这将使标签具有不同的着色以获得更好的效果。

要添加标签，请从 Report Element 调色板中拖动标签图标（Ab）并将其放置到 Details Header 频段中。您应该重复此简单操作三次（每个明细字段一个）并将标签放置在 Details 频段中明细字段对应的位置。

对于每个标签，将字体格式设置为粗体（在字体工具栏中点击 B 按钮）并双击元素以将内容更改为 Position title（第一个）、Actuals（第二个）和 Budget（第三个）。对于 Actuals 标签和 Budget 标签，在字体工具栏中点击右对齐图标（≡）。

为了使报告的最终版本更具吸引力，请尝试在频段顶部到标签之间留出少量空间（10 到 15 像素）。这将使您的报告布局更整洁，在标题栏和明细标题之间增加一点距离。

![](img/9337ac94537bf7e092e1ea6531dce8fd_169_0.png)

要完成任务，请从 Details 频段中选择 `Actuals` 字段和 `Budget` 字段并在字体工具栏中点击右对齐图标（≡）。任务结束时，Report canvas 应如下图所示：

![](img/9337ac94537bf7e092e1ea6531dce8fd_169_1.png)

![](img/9337ac94537bf7e092e1ea6531dce8fd_169_2.png)

*使用默认设置，明细标题将在第一页显示。要在每页重复标题，请从 Report explorer 的 Structure 选项卡中选择 `Details Header` 项。在 Element properties 的 `Style` 选项卡中，您可以在 `page-behaviour` 组中找到 `repeat-header` 属性。将值更改为 true。使用眼睛图标（👁）预览结果。使用导航栏滚动页面，您会看到标题出现在每一页上。在继续自定义之前，通过点击铅笔图标（✏️）再次切换到设计模式。*

现在明细标题已开发完成，让我们继续自定义明细字段。特别是，我们将添加交替行颜色。要完成任务，请从 Report explorer 的 `Structure` 选项卡中选择 `Details` 项并在 `Style` 选项卡的 `text` 组中搜索 `bg-color` 属性。在此情况下，我们将使用动态表达式来评估该属性。要设置 **expression**，请点击属性旁边的绿色加号图标（⊕）。此时将出现一个配置窗口。在窗口右下方的 `Formula` 文本字段中，输入以下公式：=IF(ISEVEN(ROWCOUNT()); "white"; "#d2e8f4")

作为最终微调，将 `v-align` 属性更改为 `MIDDLE` 值。现在自定义已完成，让我们通过使用眼睛图标（👁）预览报告来查看报告的样子：

## 按职位标题的实际值

| 职位标题 | 实际 | 预算 |
|---|---|---|
| District Manager | 682,625 | 617,250 |
| Senior Sales Rep | 497,223 | 484,820 |
| Sales Rep | 675,975 | 612,500 |
| Account Executive | 409,975 | 422,500 |
| Pre-Sales | 649,375 | 593,500 |
| CEO | 549,625 | 522,250 |
| SVP WW Operations | 476,000 | 725,887 |

在继续自定义之前，通过点击铅笔图标（✏️）再次切换到设计模式。

## 自定义页脚

自定义页眉和页脚与我们为报告标题所见的内容并无不同。Report canvas 中的频段非常有帮助，它们将使您能够按此任务的要求开发所有内容。我们现在的目标是在页脚中添加执行报告的用户和页码。

为了准备页脚，我们将通过从 Report Explorer 的 Structure 选项卡中选择 Page Footer 项来更改默认文本颜色。使用 Element properties 的 Style 选项卡更改颜色非常简单。特别是，识别 `text-color` 属性并将其设置为 `#77b3d4`。

要将用户添加到报告中，让我们使用一个名为 `env:username` 的捆绑 **parameter**。要在报告中使用该参数，请在 Report explorer 中选择 `Data` 选项卡并向下滚动直到找到 Environment 组。像往常一样，拖动 `env:username` 参数并将其放置到 Page footer 频段的左侧：

![](img/9337ac94537bf7e092e1ea6531dce8fd_171_0.png)

要添加页码，您需要使用公式创建一个新的 **function**。要创建函数，请右键单击 Report Explorer 中 Data 选项卡的 `functions` 组。点击后，一个简单的菜单将显示 Add function .. 项。

![](img/9337ac94537bf7e092e1ea6531dce8fd_172_0.png)

点击它以访问窗口：

展开 Common 并选择 Page of Pages，然后点击 OK 按钮将新公式添加到 functions 组。现在 Page of Pages 函数可用后，将其从 functions 组拖出并放置在 Page footer 频段右侧附近。要完成任务，请在字体工具栏中点击右对齐图标（≡）：

![](img/9337ac94537bf7e092e1ea6531dce8fd_172_1.png)

现在自定义已完成，让我们通过使用眼睛图标（👁）预览它来查看报告的样子：

|  |  |  |
|---|---|---|
| EOE | 530,207 | 538,380 |
| CMO | 827,861 | 760,990 |
| Product Marketing Mgr | 693,531 | 665,040 |
| Analyst Relations | 383,375 | 443,500 |
| Press Relations | 497,296 | 524,872 |
| Graphics | 782,375 | 728,500 |
| Writer | 405,985 | 459,650 |
| CTO | 405,985 | 459,650 |

如您所见，页脚会为每一页重复，并且页码会相应更改。在继续自定义之前，通过点击铅笔图标（✏️）再次切换到设计模式。

## 分组明细

此时，Pentaho 报告对最终用户来说更具吸引力，但我们还不满意，我们现在想讨论的是关于分组明细的问题。在报告中分组明细是一个常见请求，我们想使用 `Actuals` 中的 `DEPARTMENT` 字段举例说明。

为了准备任务，我们需要修改数据源，以确保所有明细都按 `DEPARTMENT` 字段排序。

![](img/9337ac94537bf7e092e1ea6531dce8fd_174_0.png)

> 如果您将在报告中使用分组，请确保正确按分组字段对行进行排序。如果您忘记了这个提示，您将遇到一些意外的结果，而没有任何异常或错误消息。这种行为的原因很容易理解；Pentaho Reporting Engine 通过将分组字段的当前值与前一个值进行比较来更改组，因此如果未排序，报告可能会显示错误的结果。

要更改数据源，请在 Report explorer 中选择 Data 选项卡并右键单击 `Actuals` 查询。选择 `Edit query` 项并将查询替换为接下来描述的内容：

```
SELECT *
FROM "QUADRANT_ACTUALS"
ORDER BY "DEPARTMENT" ASC
```

正如您所见，唯一的区别是 `ORDER BY` 关键字。按 Preview 并在预览显示数据集后稍后按 OK 按钮。

一旦数据源准备就绪，下一个任务是定义布局。查看 Report Explorer 的 Structure 选项卡，您可以找到 `Group` 元素。点击它并从 Element properties 中选择 Attributes 选项卡。在属性列表中，您可以在 `common` 组中找到 `group` 属性。使用 ... 按钮将值更改为 `DEPARTMENT`。完成后，布局实际上没有变化，但报告中的分组已正确设置。

要更改布局，请在 Report explorer 中点击 Group Header 并更改
