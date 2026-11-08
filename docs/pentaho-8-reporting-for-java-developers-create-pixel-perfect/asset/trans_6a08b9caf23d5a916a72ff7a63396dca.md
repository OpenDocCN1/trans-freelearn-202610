
| 名称 | 描述 |
|------|-------------|
| `report-header` | `report-header` 元素指定报表头的布局。该元素包含一个名为 `root-level-content` 的子元素，其中包含对所有子元素的引用。请注意，`report-header` 是一个报表元素。因此，它可能包含通用元素属性和子元素，这些在本章的 *Report elements* 部分后面定义。 |
| `group` | `group` 元素定义报表中定义的组和明细频带的整个层次结构。该元素层次结构在本章的 *Group and detail band hierarchy* 部分有更详细的定义。 |
| `report-footer` | `report-footer` 元素指定报表脚部的布局。该元素包含一个名为 `root-level-content` 的子元素，其中包含对所有子元素的引用。请注意，`report-footer` 是一个报表元素。因此，它可能包含通用元素属性和子元素，这些在本章的 *Report elements* 部分后面定义。 |

还请注意，报表本身表示为一个报表元素。因此，除了之前定义的属性和子元素外，报表还可能包含报表元素中定义的属性和元素（这些内容在本章的 *Report elements* 部分后面介绍）。

## 组和明细频带层次结构

一个组可能包含 `group-body` 或 `data-body` 子 XML 元素。`group-body` 包含 XML 元素 `group`，用于表示分组的层次结构。除了包含 `group-body` 或 `data-body` 外，`group` XML 元素还可能包含 `group-header` 和 `group-footer` XML 元素。`group-header` 和 `group-footer` XML 元素可能包含一个 `root-level-content` XML 元素，其中包含子布局元素。

`data-body` XML 元素包含与报表中行级数据相关的频带。该 XML 元素可能包含 `detail-header`、`details`、`no-data` 和 `details-footer` 子 XML 元素。这些元素中的每一个都可能包含一个 `root-level-content` XML 元素，其中包含子布局元素。

## 报表元素

前几章中定义的每个报表元素都在 XML 中表示，定义其属性以及其在频带内的位置。最常见的元素在此突出显示。

所有元素都定义 `style:element-style` 子 XML 元素，其中包含单个元素的样式信息。最常见的样式元素是 `style:spatial-styles`，它定义元素在频带中的位置以及应以何种大小渲染：

```
<style:element-style>
  <style:spatial-styles x="20" y="240" min-width="100" min-height="20"/>
</style:element-style>
```

以下是一些主要报表元素的列表，以及创建它们的特定详细信息：

| 名称 | 描述 |
|------|-------------|
| label | `label` XML 元素渲染一个标签。该元素包含一个 `core:value` 子元素，其中使用 XML 文本作为标签值。 |
| text-field | `text-field` XML 元素渲染一个文本字段。该元素包含一个 `core:field` 属性，用于定义要渲染的字段。 |
| number-field | `number-field` XML 元素渲染一个数字字段。该元素包含一个 `core:field` 属性，用于定义要渲染的字段，以及一个 `core:format-string` 属性，用于定义要渲染的数字格式字符串。 |
| date-field | `date-field` XML 元素渲染一个日期字段。该元素包含一个 `core:field` 属性，用于定义要渲染的字段，以及一个 `core:format-string` 属性，用于定义要渲染的日期格式字符串。 |
| message | `message` XML 元素渲染一条消息。该元素包含一个 `core:value` 子元素。 |
| resource-label | `resource-label` XML 元素渲染一个资源标签。该元素包含一个引用消息包的 `core:resource-identifier` 属性，以及一个 `core:value` 属性，用于指定包中使用的键。 |
| resource-field | `resource-field` XML 元素渲染一个资源字段。该元素包含一个引用消息包的 `core:resource-identifier` 属性，以及一个 `core:field` 属性，用于指定用于确定要解析的键的字段。 |
| resource-message | `resource-message` XML 元素渲染一个资源消息。该元素包含一个引用消息包的 `core:resource-identifier` 属性，以及一个 `core:value` 属性，用于指定消息。 |
| rectangle | `rectangle` XML 元素渲染一个矩形。矩形的大小由前面定义的 `style:spatial-sizes` 元素确定。可以定义一个 `style:content-styles` XML 元素，用于指定诸如布尔属性 `draw-shape` 和 `fill-shape` 之类的属性，以及诸如 `fill-color` 之类的颜色属性。颜色以 RGB 十六进制表示法表示，例如 `#ff3333`。 |
| horizontal-line | `horizontal-line` XML 元素渲染一条水平线。线的长度由前面定义的 `style:spatial-sizes` XML 元素确定。定义一个 `style:content-styles` XML 元素以指定线条样式和颜色。 |
| vertical-line | `vertical-line` XML 元素渲染一条垂直线。线的长度由前面定义的 `style:spatial-sizes` XML 元素确定。定义一个 `style:content-styles` XML 元素以指定线条样式和颜色。 |
| sub-report | `sub-report` XML 元素渲染一个子报表。属性 `href` 指定子报表在 `.prpt` 包中的位置。该 XML 元素还可以通过 `style:element-style` 子 XML 元素包含有关其位置和样式的信息，以及通过 `input-parameter` 和 `output-parameter` XML 元素进行输入和输出参数映射。这些映射使用 `master-fieldname` 和 `detail-fieldname` 属性进行配置。 |
| content | `content` XML 元素渲染一张图片。图片二进制的位置由 `core:value` XML 元素确定。`content` 元素的缩放和纵横比属性可以通过指定带有 `scale` 和 `keep-aspect-ratio` 属性的 `style:content-styles` 元素来自定义。 |
| content-field | `content-field` XML 元素基于字段渲染动态图片。要渲染的动态内容的位置由 `core:field` XML 元素确定。`content-field` 元素的缩放和纵横比属性可以通过指定带有 `scale` 和 `keep-aspect-ratio` 属性的 `style:content-styles` 元素来自定义。 |

在 Pentaho Reporting 中定义了其他报表元素类型。表中列出的元素是最常见的元素，以及它们的一些主要属性和样式。

## settings.xml 文件

`settings.xml` 文件包含报表的全局配置属性，以及未来运行时信息的占位符。

配置设置的一个示例与公式表达式的执行相关。如果将属性 `org.pentaho.reporting.engine.classic.core.function.LogFormulaFailureCause` 设置为 true，并且将 Log4j 日志记录设置为 debug，当公式失败时，您将收到堆栈跟踪以及错误消息。

还有其他配置属性，主要用于 Pentaho Report Designer 等设计工具。在生成报表时，很少需要定义配置属性。

## styles.xml 文件

`styles.xml` 文件定义报表应以何种页面格式呈现、规定报表应如何呈现的样式规则、全局布局处理器、报表水印，以及最后报表的页面页眉和页脚部分。

`styles.xml` 文件包含 `style` 根 XML 元素，并包括与渲染样式相关的命名空间，包括前面定义的 `core` 和 `layout` 命名空间，以及作为默认命名空间的 `style` 命名空间，其位置为 http://reporting.pentaho.org/namespaces/engine/classic/bundle/style/1.0。可以在 `style` XML 元素中指定以下子元素：

| 名称 | 描述 |
|------|-------------|
| `page-definition` | `page-definition` XML 元素包含定义报表页面属性的属性。属性 `pageformat` 指定页面格式类型。属性 `horizontal-span` 和 `vertical-span` 定义报表应跨越多少页。`width` 和 `height` 属性定义纸张的宽度和高度。`orientation` 属性定义页面应以纵向、横向或反向横向布局。属性 `margin-top`、`margin-left`、`margin-bottom` 和 `margin-right` 定义报表的边距值。 |
| `style-rule` | 报表中可以定义多个 `style-rules`。样式规则具有 `name` 和 `parent` 属性，用于定义一组样式的继承树。在 `style-rule` 内，定义了诸如 `text-styles` 和 `spatial-styles` 之类的样式元素，其中包含特定的样式信息。 |
| `layout:layout-processors` | 某些操作整个报表布局的函数可能出现在样式文档的 `layout-processors` 部分。 |
| `watermark` | `watermark` XML 元素在背景中渲染，位于后面 |
