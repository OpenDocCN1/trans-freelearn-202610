
## Microsoft Word 2000

此导出格式直接生成适用于所有理解 Word 2000 格式的软件的 .doc 文件。我们发现的选项与 Microsoft Excel 2000 导出中的选项相似，还有一些更多。我们可以独立导出表的结构和数据。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_154_0.png)

请注意，对于此格式和 Excel 格式，我们可以为一次导出选择多个表。但是，如果其中一个表包含非文本数据，就会产生不愉快的结果。以下是 author 表的结果：

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_154_1.png)

## LaTeX

LaTeX 是一种排版语言。phpMyAdmin 可以生成一个 .tex 文件，以横向表格格式表示表的结构和/或数据。请注意，此文件无法直接查看，必须进一步处理或转换才能用于预期的最终媒体。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_155_0.png)

可用的选项是：

- **包含表格标题**：在表格输出中显示标题。
- **结构和数据**：熟悉的选择，用于请求结构、数据或两者。
- **表格标题**：放在第一页的标题。
- **续表标题**：放在每一页上的标题。
- **关系、注释、MIME 类型**：我们希望作为输出的其他结构信息。如果关系基础设施已就位，则可以使用这些选项（详见第 11 章）。

## XML

这种格式在当今数据交换中非常流行。在 **导出** 界面中选择 **XML** 不会提供选项选择。下面是 **author** 表的输出。

```
<?xml version="1.0" encoding="utf-8" ?>
<!--
-
- phpMyAdmin XML Dump
- version 3.1.1
- http://www.phpmyadmin.net
-
- Host: localhost
- Generation Time: Dec 21, 2007 at 01:13 PM
- Server version: 5.0.67
- PHP Version: 5.2.6
-->

<!--
- Database: `marc_book`
-->
<marc_book>
    <!-- Table author -->
    <author>
        <id>1</id>
        <name>John Smith</name>
        <phone>+01 445-789-1234</phone>
    </author>
    <author>
        <id>2</id>
        <name>Maria Sunshine</name>
        <phone>333-3333</phone>
    </author>
</marc_book>
```

## Native MS Excel (pre-Excel 2000)

phpMyAdmin 提供了一个实验性模块，可直接导出为 **.xls** 格式——MS Excel 和 OpenOffice Calc 理解的原生电子表格格式。当此支持被激活时（稍后会详细介绍），我们会看到一个新的导出选项 **Native MS Excel**，以及与 Microsoft Excel 2000 相同的选项。

我们可以选择将字段名称放在电子表格的第一行，使用 **将字段名称放在第一行**。

此功能依赖于 PEAR 模块 **Spreadsheet_Excel_Writer**，其当前版本为 0.9.1，并生成 Excel 5.0 格式文件。该模块记录在 http://pear.php.net/package/Spreadsheet_Excel_Writer，但 phpMyAdmin 环境下的完整安装记录如下：

- 1. 确保 PHP 服务器具有 PEAR 支持。（如果没有 PEAR 支持，pear 命令将失败。）PEAR 本身记录在 http://pear.php.net。
- 2. 如果我们以安全模式运行 PHP，必须确保允许包含 PEAR 模块。假设模块位于 /usr/local/share/pear 下，我们应在 php.ini 中有 safe_mode_include_dir = /usr/local/share/pear 这一行。
- 3. 然后我们使用 pear -d preferred_state=beta install -a Spreadsheet_Excel_Writer 安装该模块（因为该模块目前处于 beta 状态）。此命令通过互联网获取必要的模块并将其安装到我们的 PEAR 基础设施中。
- 4. 我们需要在 phpMyAdmin 主目录下为 .xls 生成准备一个临时目录。在 Linux 系统上，可以使用 mkdir tmp ; chmod o+rwx tmp 创建。
- 5. 我们将 config.inc.php 中的 $cfg['TempDir'] 参数设置为 './tmp'。

我们现在应该能够看到新的 **Native MS Excel data** 导出选项。

## Open document spreadsheet

此电子表格格式是开放文档（http://en.wikipedia.org/wiki/OpenDocument）的一个子集，该格式因 OpenOffice.org 办公套件而流行。我们需要只选择一个表进行导出，以便获得连贯的电子表格。以下是我们的 author 表，导出为名为 author.ods 的文件，随后从 OpenOffice 查看：

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_157_0.png)

## Open document text

这是开放文档标准的另一个子集，这次面向文本处理。可用选项如以下屏幕截图所示：

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_158_0.png)

我们的 author 表现已导出并从 OpenOffice 查看。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_158_1.png)

## YAML

YAML 的全称是 YAML Ain't Markup Language。YAML 是一种人类可读的数据序列化格式；其官方网站是 http://www.yaml.org。此格式在 phpMyAdmin 内部没有可供选择的选项。以下是 author 表的 YAML 导出：

```
1:
  id: 1
  name: John Smith
  phone: +01 445-789-1234

2:
  id: 2
  name: Maria Sunshine
  phone: 333-3333
```

## CodeGen

此选项将来可能支持与代码开发相关的许多格式。目前，它可以导出为 NHibernate 对象关系映射（ORM）格式。更多详情请参阅 http://en.wikipedia.org/wiki/Nhibernate。

## Texy! text

Texy! 是一种格式化工具（http://texy.info/en/），具有自己的简化语法。以下是此格式的导出示例：

```
===Database marc_book

== Table structure for table author
|------|
|Field|Type|Null|Default|
|------|
|/**id**/|int(11)|Yes|NULL
|name|varchar(30)|Yes|NULL
|phone|varchar(30)|Yes|NULL

== Dumping data for table author
|1|John Smith|+01 445 789-1234
|2|Maria Sunshine|333-3333
```

## Table exports

表视图中的导出链接会打开特定表的导出子面板。它与数据库导出面板类似，但没有表选择器。然而，在“另存为文件”子面板之前，还有一个用于拆分导出的额外部分。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_160_0.png)

## Split-file exports

“转储从记录 # 0 开始的 2 行”对话框使我们能够将文件拆分为块。根据确切的行大小，我们可以尝试不同的行数值，以确定在 Web 服务器达到内存或执行时间限制之前，单个导出文件可以容纳多少行。然后我们可以使用类似 book00.sql 和 book01.sql 的名称为导出文件命名。

## Selective exports

在 phpMyAdmin 界面的各个位置，我们可以导出所看到的结果，或选择要导出的行。

## Exporting partial query results

当 phpMyAdmin 显示结果时（此处为查询 author_id 为 2 的书籍的结果），页面底部会出现一个导出链接。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_161_0.png)

点击此链接会打开一个特殊的导出面板，顶部包含查询以及其他表导出选项。通过此面板生成的导出将仅包含此结果集的数据。

> 单表查询的结果可以用所有可用格式导出，而多表查询的结果只能以 CSV、XML 和 LaTeX 格式导出。

## Exporting and checkboxes

无论何时我们看到结果（例如浏览或搜索时），都可以勾选想要的行旁边的复选框，并使用“已选择：导出”图标生成仅包含这些行的部分导出文件。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_162_0.png)

## Multi-database exports

任何用户都可以在一次操作中导出其有权访问的数据库。

在主页上，导出链接会将我们带到下一页所示的屏幕。它与其它导出页面的结构相同，只是数据库列表不同。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_162_1.png)

> 导出大型数据库可能成功也可能不成功。这取决于其大小、所选选项以及 Web 服务器的 PHP 组件设置（尤其是内存大小和执行时间）。
