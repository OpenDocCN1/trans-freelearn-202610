
这是根据同一本书表 PDF 页面定义生成的另一个示例。这次网格显示了，但没有颜色。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_260_0.png)

## A note about fonts used

所有在 PDF 模式中看到的文本都是使用特定字体绘制的。phpMyAdmin 3.1.x 使用 DejaVuSans 字体 (http://dejavu.sourceforge.net)，它涵盖了广泛的字符。

对于实际的 PDF 生成，phpMyAdmin 依赖于 tcpdf 库 (http://tcpdf.sourceforge.net)，它本身是对 fpdf 库 (http://www.fpdf.org) 的改编。这些库使用字体有两种方式——嵌入和不嵌入。嵌入字体会产生更大的 PDF 文件，因为整个字体包含在 PDF 中。这是 phpMyAdmin 选择的默认选项，因为该库不依赖于客户端操作系统中特定 TrueType 字体是否存在。

字体位于主 phpMyAdmin 目录下的 libraries/tcpdf/font 中。

要使用不同的字体文件，我们必须先将其添加到库中（原始 tcpdf 工具包中包含工具，且在 http://www.fpdf.org 网站上有教程），然后修改 phpMyAdmin 的 pdf_schema.php 源代码。

## Using the Designer for PDF layout

设计器功能（在数据库视图中可用）提供了一种更精细的方式在屏幕上移动表，因为列链接会跟随表的移动。因此，设计器保存的表的坐标与 PDF 模式的坐标之间存在接口。让我们进入设计器并点击小 PDF 标志。

这会带我们进入一个面板，在其中我们可以选择（已存在的）PDF 模式名称以及要执行的操作——在我们的例子中，是将设计器坐标导出到 PDF 模式。我们也可以使用“创建新页面”对话框，输入 PDF 页面名称并点击 Go 来创建空白页面。从这里，我们随后可以导出从设计器工作区保存的坐标。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_261_0.png)

> 设计器管理的表范围与编辑 PDF 功能管理的表范围存在差异。设计器操作数据库中的每个表，而编辑 PDF 面板提供表选择，使我们能够在表很多的情况下表示关系的一个子集。

## Summary

本章涵盖了 phpMyAdmin 提供的文档功能——数据库或表的打印视图以及完整的列列表的数据字典。本章还涵盖了 PDF 关系模式。特别是，它重点介绍了如何创建和修改 PDF 模式页面，以及如何使用可视编辑器（草稿板）。下一章将解释如何在查看时应用转换来定制数据的格式。

## 16 MIME-Based Transformations

在本章中，我们将介绍 phpMyAdmin 的一个强大功能——在表 **Browse** 期间根据称为转换的特定规则转换列内容的能力。通常，浏览表只显示其中存储的原始数据。然而，基于 MIME 的转换允许更改显示格式。

## Display behavior in browse mode

通常，每个行的确切内容都会显示，除了：

- **TEXT** 和 **CHARACTER** 字段可能会根据 $cfg['LimitChars'] 被截断，具体取决于我们是否选择查看 **Full Texts**
- **BLOB** 字段可能会被诸如 [**BLOB - 1.5 KB**] 的消息替换

我们将使用术语 **cell** 来表示特定行的特定列。包含“Future souvenirs”书封面照片的单元格（一个 **BLOB** 列）目前显示为诸如 %oPNG\r\nZ\n\0\0\0\rIHDR\0 之类的神秘数据，或显示 **BLOB** 字段大小的消息。能够直接在 phpMyAdmin 中看到图片的缩略图（在 *Clickable thumbnail* 部分下的第二张截图中显示）以及可能的全尺寸图片本身会很有趣。这将通过适当的转换实现。

## Display options

在 Browse 模式下，Options 链接会显示一个滑块，其中包含其他选项，如隐藏浏览器转换复选框。我们可以在想要在单元格的真实数据与其转换版本之间切换时使用它。

## Enabling transformations

我们将转换定义为一种机制，通过该机制，使用为该列定义的元数据，在浏览时转换与列相关的所有单元格。只有结果页面上可见的单元格会被转换。转换逻辑本身是用 PHP 脚本编写的，存储在 libraries/transformations 中，并使用插件架构调用。

要启用此功能，我们必须在 config.inc.php 中将 $cfg['BrowseMIME'] 设置为 TRUE。必须建立链接表基础设施（参见第 11 章），因为转换所需的元数据在官方 MySQL 表结构中不可用。这是专门为 phpMyAdmin 做的补充。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_264_0.png)

> 在 phpMyAdmin 主页的文档部分，有一个链接指向更多信息，供希望学习插件内部结构以编写自己的转换的开发人员参考。

## The MIME column's settings

如果我们进入书表的结构页面的表视图并点击 cover_photo 的 Change 链接，我们会看到字段的三个额外属性（前提是转换功能已启用）：

- MIME 类型
- 浏览器转换
- 转换选项

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_265_0.png)

对于特定字段，只能指定一种转换类型。这里，字段是 BLOB 字段。因此，它可以保存任何类型的数据。为了使 phpMyAdmin 正确解释和操作数据，转换系统必须被告知数据格式和预期结果。因此，我们必须确保上传的数据始终遵循相同的格式。

我们将首先了解这些属性的用途，然后在转换示例部分尝试一些可能性。

## MIME types

MIME 规范 (http://en.wikipedia.org/wiki/MIME) 被选为元数据属性，用于对列保存的数据类型进行分类。多用途互联网邮件扩展 (MIME) 最初设计用于扩展邮件，现在也用于描述其他协议的内容类型。在 phpMyAdmin 的上下文中，当前可能的值为：

- image/jpeg
- image/png
- text/plain
- application/octetstream

text/plain 类型可以为包含任何类型文本的列选择（例如 XHTML 或 XML 文本）。在转换示例部分，您将看到要实现特定效果需要选择哪种 MIME 类型。

## Browser transformations

这里设置要执行的确切转换。每个 MIME 类型可能支持多个转换。例如，对于 **image/jpeg** MIME 类型，我们有两个可用转换——**image/jpeg: inline** 用于图片的可点击缩略图，以及 **image/jpeg: link** 用于仅显示链接。

如以下图片所示，将鼠标移到下拉菜单中的每个选项上会提供相应转换的简短说明。更完整的转换说明和可能选项可在点击 **transformation descriptions** 链接后获得。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_266_0.png)

## Transformation options

在 *Examples of transformations* 部分，我们将看到某些转换接受选项。例如，生成图片的转换需要以像素为单位的宽度和高度。选项列表中的值用逗号分隔，一些选项可能需要在引号中括起来。

一些选项有默认值，我们必须注意遵守选项的文档顺序。例如，如果有两个选项，而我们只想为第二个选项指定值，可以使用空引号作为第一个选项的占位符，让系统使用其默认值。

## Requirements for image generation

正常的缩略图生成要求 Web 服务器上存在某些组件，并且 config.inc.php 中的参数配置正确。
