
## 外键的下拉列表

回到书表，在插入模式（或编辑模式）中，我们现在会看到每个具有定义关系的字段的可能键的下拉列表。该列表以两种顺序包含键和描述（显示字段）——键到显示字段以及显示字段到键。这使我们能够使用键盘，并输入键或显示字段的首字母。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_213_0.png)

只有键（本例中为 1）会被存储在书表中。显示字段仅用于辅助我们。

默认情况下，如果外表中的行数最多为 100 行，则会显示此下拉列表。这由以下参数控制：

```
$cfg['ForeignKeyMaxLimit'] = 100;
```

对于更大的外表，会出现一个不同的窗口——外表窗口（见下一节），可以浏览。

我们可能希望在下拉列表中以不同的方式查看信息。这里，John Smith 是内容，1 是 ID。默认显示由以下控制

```
$cfg['ForeignKeyDropdownOrder'] = array( 'content-id', 'id-content');
```

我们可以在定义数组中使用一个或两个字符串——content-id 和 id-content——并按我们偏好的顺序排列。因此，将 $cfg['ForeignKeyDropdownOrder'] 定义为 array('id-content') 将只产生以下选择：

- 1 – John Smith
- 2 – Maria Sunshine
- 3 – André Smith

## 可浏览的外表窗口

我们当前的作者表条目很少。因此，为了说明此机制，我们将把 $cfg['ForeignKeyMaxLimit'] 设置为人为的低数值 1。现在在书表的插入模式中，我们会看到 author_id 的一个小表形图标。此图标会打开另一个窗口，该窗口将显示 author 表的值以及一个搜索输入字段。左侧的值按键值排序（这里是 author_id 列），右侧按描述排序。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_214_0.png)

选择其中一个值（通过点击键值或描述）会关闭此窗口并将值带回 author_id 列。

## 引用完整性检查

我们在第 10 章讨论了操作子页面及其表维护部分。对于此练习，我们假设书表和作者表都不受 InnoDB 或 PBXT 存储引擎控制。如果我们为作者表定义了内部关系，则书表会出现一个新的选择 – 检查引用完整性。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_214_1.png)

对于每个定义的关联都会出现一个链接（author_id -> author.id），点击它会开始验证。对于每一行，都会验证外表中对应键的存在性，并报告错误（如有）。如果结果页面报告零行，则是好消息！

此操作存在的原因是，对于不原生支持外键的存储引擎下的表，MySQL 和 phpMyAdmin 都不会强制执行引用完整性。例如，完全可能将无效的 **author_id** 值导入书表。

## 元数据的自动更新

phpMyAdmin 会将内部关系的元数据与通过 phpMyAdmin 对表所做的每一次更改保持同步。例如，重命名关系的一部分列会使 phpMyAdmin 在关系的元数据中重命名此列。这保证了内部关系即使在列名更改后仍能继续 functioning。删除列或表时也会发生同样的情况。

> 如果结构更改是在 phpMyAdmin 之外进行的，则应手动维护元数据。

## 列注释

在 MySQL 4.1 之前，MySQL 结构本身不支持为列添加注释。尽管如此，得益于 phpMyAdmin 的元数据——我们可以对列进行注释。然而，自 MySQL 4.1 以来，原生列注释已被支持。好消息是，对于任何 MySQL 版本，phpMyAdmin 中的列注释始终通过 **Structure** 页面访问，通过编辑每个字段的结构。在以下示例中，我们需要注释三个列。因此，我们选择它们并点击 **With selected** 选择附近的铅笔图标。

|   | Field | Type |
|---|---|---|
| ☑ | isbn | varchar(25) |
| ☐ | title | varchar(100) |
| ☑ | page_count | int(11) |
| ☑ | author_id | int(11) |

为了获得此处所示的下一个面板，我们通过将 $cfg['DefaultPropDisplay'] 设置为 'vertical' 以垂直模式工作。我们输入以下注释：

- isbn: book number
- page_count: approximate
- author_id: cf author table

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_216_0.png)

然后我们点击保存。

这些注释会出现在各个地方——例如，在导出文件（见第 7 章）中，在 PDF 关系模式（见第 15 章）中，以及在浏览模式中。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_216_1.png)

如果我们不希望注释出现在浏览模式中，可以将 $cfg['ShowBrowseComments'] 设置为 FALSE。（默认值为 TRUE。）

列注释也会作为工具提示出现在 Structure 页面中，并且列名会用破折号下划线标出。要停用此行为，可以将 $cfg['ShowPropertyComments'] 设置为 FALSE。（该项默认也为 TRUE。）

## 自动迁移

每当 phpMyAdmin 检测到列注释已存储在其元数据中，并且我们使用的是 MySQL 4.1.2 或更高版本时，它会自动将这些列注释迁移到原生的 MySQL 列注释。

## 摘要

本章介绍了如何安装保持特殊元数据（关于表的数据）所需的必要基础设施，以及如何在 InnoDB 和非 InnoDB 表之间定义关系。它还检查了 phpMyAdmin 在存在关系时的修改行为以及外键。最后，它涵盖了 Designer 功能、列注释以及如何从表中获取信息。第 12 章将介绍输入 SQL 命令的手段，当 phpMyAdmin 的界面不足以完成我们所需的操作时，这些命令很有用。

## 12

## 输入 SQL 命令

本章解释了我们如何将自己的 SQL 命令（查询）输入到 phpMyAdmin 中，以及如何保留这些查询的历史记录。

## SQL 查询框

phpMyAdmin 允许我们通过其图形界面完成许多数据库操作。然而，在某些时候，我们必须依赖 SQL 查询输入来实现复杂操作。以下是一些复杂查询的示例：

```
SELECT department, AVG(salary) FROM employees GROUP BY department
HAVING years_experience > 10;
SELECT FROM_DAYS(TO_DAYS(CURDATE()) +30);
```

要输入此类查询，查询框可在 phpMyAdmin 内的多个位置获得。

## 数据库视图

我们遇到的第一个查询框是在数据库视图中进入 SQL 菜单时。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_219_0.png)

此框很简单——我们输入一些有效的（希望是）MySQL 语句并点击 Go。在查询文本区域下方，有与书签相关的选择（第 14 章后面会解释）。通常，我们不必更改标准的 SQL 分隔符，即分号。然而，如果需要，有分隔符对话框（见第 17 章）。

要使默认查询出现在此框中，我们可以使用 $cfg['DefaultQueryDatabase'] 配置指令来设置，默认情况下它是空的。我们可以在此指令中放置一个类似 SHOW TABLES FROM %d 的查询。此查询中的 %d 参数将被当前数据库名称替换，从而在查询框中得到 SHOW TABLES FROM `marc_book`。

## 表视图

在表视图的 SQL 菜单中，有一个略有不同的框。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_220_0.png)

右侧有一个字段选择器和一个插入按钮（<<）。该框已具有如前一屏幕截图所示的默认查询。

此默认查询由 $cfg['DefaultQueryTable'] 配置指令生成，其中包含 SELECT * FROM %t WHERE 1。这里，%t 被当前表名替换。$cfg['DefaultQueryTable'] 中还提供了一个占位符 %f。此占位符将被此表的完整字段列表替换，从而生成以下查询：

```
SELECT `isbn`, `title`, `page_count`, `author_id`, `language`, `description`, `cover_photo`, `genre`, `date_published`, `stamp`, `some_bits` FROM `book` WHERE 1.
```
