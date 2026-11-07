
![](img/cb8c75b631b4e1f7afe5a0d39becfda9_181_0.png)

> 完整的搜索表达式是通过组合搜索条件、逻辑 AND 以及在查询示例行中输入的其他条件生成的。

我们可以有一个更复杂的搜索条件列表，这些条件将输入到同一个文本框中，可能包含括号以及诸如 AND 或 OR 之类的运算符。

文档链接指向 MySQL 手册，在那里我们可以看到大量可用的函数。 （每个函数都适用于特定字段类型。）

## Obtaining distinct results

有时，我们可能希望避免重复获取相同的结果。 例如，如果我们想知道客户在哪些城市，只显示每个城市名称一次就足够了。 这里，我们想知道我们的书籍是用哪些语言编写的。 在“选择字段”对话框中，我们只选择语言字段，并勾选 DISTINCT，如以下屏幕截图所示：

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_181_1.png)

点击 Go 后会产生以下图像：

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_181_2.png)

> 使用 **DISTINCT**，我们只能看到每种语言一次。 如果没有此选项，包含 **en** 的行会出现三次。

## Complete database search

在之前的示例中，搜索仅限于一张表。 这假设知道必要信息可能存储的确切表（和列）。

当数据隐藏在数据库的某个地方，或者当相同的数据可以在各种列中呈现时（例如 **title** 列或 **description** 列），使用数据库搜索方法会更容易。

我们进入 `marc_book` 数据库的数据库视图中的 **Search** 页面：

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_182_0.png)

在 **Word(s) or value(s)** 部分，我们输入想要查找的内容。 这里 % 通配符可能很有用。 我们输入 **souvenirs**。

在 **Find** 部分，我们指定如何处理输入的值。 我们可能需要查找输入的 **at least one of the words**、**all words**（无特定顺序）或 **the exact phrase**（按相同顺序，出现在列中的某处）。 另一个选择是使用 **regular expression**，这是一种更复杂的模式匹配方式。 我们将保留默认值 — **at least one of the words**。

我们可以选择要限制搜索的表或选择所有表。 由于我们只有两个（小型）表，因此我们同时选择两者。

> 由于搜索将在所选每个表的每一行上执行，如果行数或表数过大，我们可能会遇到一些时间限制。 因此，可以通过将 $cfg['UseDbSearch'] 设置为 FALSE 来停用此功能。 （默认设置为 TRUE。）

点击 Go 会为我们找到以下内容：

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_183_0.png)

这是匹配数量和相关表的概览。 我们可能会在不感兴趣的表中得到一些匹配。 然而，对于看起来有希望的匹配，我们可以浏览结果页面，或删除不需要的行。

## Restricting search to a column

有时，某个特定列名是属于一张（或多张）表的一部分，而我们只想在此列内部进行搜索。 例如，假设我们在查找 "Marc"，但这个名字也可能是书名的一部分。 因此，我们希望将搜索限制为所选表中仅 "name" 列。 这可以通过在 Inside 字段选择中输入 "name" 来实现。

## Summary

在本章中，我们概述了使用“query by example criteria”以及附加条件规范的单表搜索——选择显示的值和排序结果。 我们还查看了通配符搜索和完整数据库搜索。 下一章将解释如何对表执行操作——例如更改表的属性，如其存储引擎。 表的修复和优化主题也在本章中有所涉及。

## 10

## Table and Database Operations

在前几章中，我们主要处理表字段。 在本章中，我们将学习如何执行影响整个表或数据库的一些操作。 我们将介绍表属性及其修改方法，并讨论多表操作。

各种启用表操作的链接已放在表视图的 Operations 子页面上。 以下是此子页面的概览：

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_185_0.png)

## Table maintenance

在表的生命周期中，它会反复被修改；因此，它会不断增长和缩小。 服务器可能会出现故障，导致某些表处于损坏状态。

使用 Operations 子页面，我们可以执行各种操作，接下来列出。 然而，并非每个操作都适用于每种表类型。

- **Check table:** 扫描所有行以验证已删除的链接是否正确。 同时，还会计算校验和以验证键的完整性；如果一切正常，我们应该会收到 OK 消息。
- **Analyze table:** 分析并存储键分布；这将在后续的 JOIN 操作中使用，以确定表应连接的顺序。
- **Repair table:** 修复 MyISAM 和 ARCHIVE 引擎中表的任何损坏数据。 请注意，表可能损坏到我们甚至无法进入其表视图！ 在这种情况下，请参阅 *Multi-Table Operations* 部分以获取修复程序。
- **Optimize table:** 当表包含开销时很有用。 在大量删除行或 VARCHAR 字段长度更改后，表中会残留丢失的字节。 phpMyAdmin 会在各个地方（例如在 Structure 视图中）警告我们，如果它认为表应该被优化。 此操作是对表的某种碎片整理。 在 MySQL 4.x 中，此操作仅适用于 MyISAM、Berkeley (DB) 和 InnoDB 引擎中的表。 在 MySQL 5.x 的情况下，相关表位于 MyISAM、InnoDB 和 ARCHIVE 引擎中。
- **Flush table:** 当出现许多连接错误且 MySQL 服务器阻止进一步连接时，必须执行此操作。 刷新将清除一些内部缓存并允许正常操作恢复。
- **Defragment table:** InnoDB 表中的随机插入或删除会使其索引碎片化。 表应定期进行碎片整理以加快数据检索。

> 操作基于可用的底层 MySQL 查询——phpMyAdmin 仅调用这些查询。

## Changing table attributes

表属性是表的各种属性。 本节讨论其中一些的设置。

## Table storage engine

我们可以更改的第一个属性称为 Storage Engine。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_187_0.png)

这控制着表的整体行为——其位置（磁盘上或内存中）、索引结构，以及是否支持事务和外键。 下拉列表根据我们 MySQL 服务器支持的存储引擎而变化。

> 如果行数很大，更改表的存储引擎可能是一项耗时操作。

## Table comments

这允许我们为表输入注释。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_187_1.png)

这些注释将在适当位置显示——例如，在导航面板中，在表视图中表名称旁边，以及在导出文件中。 当 $cfg['ShowTooltip'] 参数设置为其默认值 TRUE 时，导航面板如下所示：

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_187_2.png)

$cfg['ShowTooltipAliasDB'] 和 $cfg['ShowTooltipAliasTB'] 的默认值（FALSE）会产生我们之前看到的行为——真正的数据库和表名称显示在左侧面板中，并在 Database 视图的 Structure 子页面中显示。 当鼠标指针移到表名称上时，会出现注释。 如果其中一个参数设置为 TRUE，则相应的项（DB 的数据库名称和 TB 的表名称）将作为工具提示显示，而不是名称。 这次，鼠标悬停会显示该项的真实名称。 当真实的表名没有意义时，这很方便。

$cfg['ShowTooltipAliasTB'] 还有另一种可能性——'nested' 值。 如果我们使用此功能，会发生以下情况：

- 真正的表名显示在左侧面板中
- 表注释（例如 project__）被解释为项目名称并按原样显示（参见第 3 章中的 Nested display of tables within a database 部分）

## Table order

当我们浏览表，或执行诸如 SELECT * from book 之类的语句而不指定排序顺序时，MySQL 使用行物理存储的顺序。 此表顺序可以使用 Alter table order by 对话框进行更改。 我们可以选择任何字段，表将按该字段重新排序一次。 我们在示例中选择 author_id，并在点击 Go 后，表按该字段排序。

如果我们知道大部分时间将按此顺序检索行，则重新排序很方便。 此外，如果我们稍后使用 ORDER BY 子句，并且表已在该字段上物理排序，则性能应该更高。

只要表中没有更改（没有插入、删除或更新），这种默认排序就会持续。 这就是为什么 phpMyAdmin 显示（单个）警告的原因。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_188_0.png)
