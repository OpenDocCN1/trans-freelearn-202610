
## 字体属性

Pentaho Reporting 在大多数字体操作中都使用 Java 内置的字体支持。`libfonts` 中存在一些额外的功能，为报表引擎提供 Java 标准 `Font` 和 `FontMetrics` API 中不可用的额外信息。

在 Pentaho Report Designer 中，字体可以通过多种方式进行修改。首先，所有基于文本的元素都包含字体属性。这些属性允许选择字体族、字体大小和字体样式。通过菜单 格式 | 字体... 编辑这些属性时，会出现以下屏幕：

![](img/9337ac94537bf7e092e1ea6531dce8fd_198_0.png)

除了修改属性外，Pentaho Report Designer 还提供了一个工具栏（如下截图所示），您可以在其中快速修改字体名称、大小、样式和对齐方式：

![](img/9337ac94537bf7e092e1ea6531dce8fd_198_1.png)

另一种配置字体的方法是使用元素属性，其中提供以下可用属性：

| Name | Type | Description |
|------|------|-------------|
| family | Selection | 表示字体类型的字符串，例如 `SansSerif`。 |
| font-size | Integer | 表示字体磅值的数字。 |
| bold | Boolean | 如果设置为 true，则以粗体显示字体。例如，如果公式设置为 true，字体将以粗体显示。 |
| italics | Boolean | 如果设置为 true，则以斜体显示字体。 |
| underline | Boolean | 如果设置为 true，则以下划线显示字体。 |
| strike through | Boolean | 如果设置为 true，则以删除线显示字体。 |
| smooth | Selection | 如果设置为 `auto`，则 8 磅以上的字体启用抗锯齿。如果设置为 `always`，则所有字体都启用抗锯齿。最后，如果设置为 `never`，则没有任何字体启用抗锯齿。 |
| embed | Boolean | 如果此样式等于 true，则在渲染 PDF 时嵌入报表中包含的字体。 |

如果报表在一个操作系统环境中设计并在另一个环境中发布，可能会出现额外的字体相关问题。例如，Windows 操作系统附带的字体标准集与 Solaris、Mac 和 Linux 系统不同。Pentaho Reporting 会尽力匹配正确的字体，但在发布后您应验证字体大小和样式是否仍按预期工作。

![](img/9337ac94537bf7e092e1ea6531dce8fd_200_0.png)

作为练习，从第 4 章《使用 Report Designer 创建报表》中开发的 `report_02.prpt` 开始，并将整个报表的字体族（即在 `Master Report` 中）修改为 `Purisa`。要查看最终结果，您可以检查 GitHub 仓库中 `Chapter 05 - Design and layout in Report Designer/my_reports` 文件夹中存储的 `report_07.prpt` 文件。

## 文本属性

文本属性对 `Master Report` 和频段是通用（且继承）的，除了 `h-align` 和 `v-align`。以下属性会影响报表元素中文本的外观：

| Name | Type | Description |
|---|---|---|
| `h-align` | Selection | 元素内文本的水平对齐方式。此属性也可以在主工具栏中编辑。 |
| `v-align` | Selection | 元素内文本的垂直对齐方式。此属性也可以在主工具栏中编辑。 |
| `v-align-in-band` | Selection | 指定频段中文本的垂直对齐方式。 |
| `text-wrap` | Selection | 指定文本是否应换行。适当的值包括 `none` 和 `wrap`。 |
| `word-break` | Boolean | 指示是否在单词内换行的标志。如果未定义，则默认为 true（仅在单词边界处换行）。 |
| `direction` | Selection | 文本方向（例如阿拉伯文本），指示是左到右（`ltr`）还是右到左（`rtl`）。 |
| `text-color` | String | 前景字体颜色。此属性也可以在主工具栏中编辑。 |
| bg-color | String | 文本元素的背景颜色。 |
| line-height | Decimal | 文本元素中字体的行高值。 |
| overflow-text | String | 如果给定文本无法完全放入元素边界内，则打印的文本引用。 |
| trim | Boolean | 指示是否删除前导和尾随空白的标志。 |
| trim-whitespace | Selection | 控制渲染器如何处理空白。 |
| encoding | String | 指定给定字段的目标文本编码，以防输出支持按字段编码。 |

关于 `text-color` 和 `bg-color` 属性，您应该知道 Report Designer 具有从视觉编辑器中选取颜色的有用工具。以下图像显示了视觉编辑器的外观。

![](img/9337ac94537bf7e092e1ea6531dce8fd_202_0.png)

> *作为练习，从第 4 章《使用 Report Designer 创建报表》中开发的 `report_02.prpt` 开始，并将整个报表的行高（即在 `Master Report` 中）修改为 30 像素。要查看最终结果，您可以检查 GitHub 仓库中 `Chapter 05 - Design and layout in Report Designer/my_reports` 文件夹中存储的 `report_08.prpt` 文件。*

组合

调色板

新建

旧

H: 0 °

S: 0 %

B: 0 %

R: 0

G: 0

B: 0

#

示例文本 示例文本

示例文本 示例文本

示例文本 示例文本

确定

取消

![](img/9337ac94537bf7e092e1ea6531dce8fd_203_0.png)

## 文本间距属性

文本间距属性让您可以控制文本元素中字母和单词之间的间距。

| Name | Type | Description |
|---|---|---|
| character | Integer | 指定两个字母之间最小字符间距 |
| word | Integer | 指定渲染时单词之间的额外填充 |
| preferred-character | Integer | 指定两个字母之间首选字符间距 |
| max-character | Integer | 指定两个字母之间的最大间距 |

![](img/9337ac94537bf7e092e1ea6531dce8fd_204_0.png)

*作为练习，从通常的 `report_02.prpt` 开始，将字符距离修改为 5.0 像素。要查看最终结果，您可以检查 GitHub 仓库中 `Chapter 05 - Design and layout in Report Designer/my_reports` 文件夹中存储的 `report_09.prpt` 文件。*

## 对象属性

`object` 属性控制元素的整体外观，如下表所述：

| Name | Type | Description |
|---|---|---|
| anti-alias | Boolean | 指示在绘制元素期间是否启用抗锯齿的标志。 |
| keep-aspect-ratio | Boolean | 指示缩放是否应保持纵横比的标志。 |
| fill | Boolean | 指示元素是否应被填充的标志。 |
| fill-color | Selection | 定义填充颜色。如果未定义，则使用前景色。 |
| draw-outline | Boolean | 定义是否应绘制元素轮廓。 |
| stroke | Selection | 定义用于渲染元素的笔类型和宽度。 |
| scale | Boolean | 指示元素中打印的内容是否应缩放以适应元素边界的标志。 |

## 链接属性

`Links` 属性管理从报表元素创建的 HTML 链接：

| Name | Type | Description |
|---|---|---|
| `pdf-bookmark` | String | 在 PDF 输出中添加书签 |
| `html-anchor` | String | 嵌入 HTML 输出中的锚点（链接目标）的名称 |
| `url` | String | 链接的目标 URL |
| `url-tooltip` | String | 链接的标题，以工具提示显示 |
| `url-window-target` | String | 链接应打开的目标窗口 |

## Excel 属性

这是一组与 Microsoft Excel 相关的属性：

| Name | Type | Description |
|---|---|---|
| sheet-name | String | 渲染频段的工作表名称。 |
| format-override | String | Excel 单元格数据格式字符串。 |
| formula-override | String | Excel 单元格数据公式字符串。 |
| wrap-text-override | Boolean | 如果为 true，则在 Excel 单元格内换行基于文本的报表元素文本。此值由频段内的基于文本的报表元素继承。 |
| indentation-text | Integer | 缩进的字符数。 |

