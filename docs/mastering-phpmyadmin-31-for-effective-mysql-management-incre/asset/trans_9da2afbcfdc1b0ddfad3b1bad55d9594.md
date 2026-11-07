
我们点击包含 author_id 索引的那一行上的编辑链接（小铅笔）；这会显示该索引的当前状态。界面有空间在此索引中添加另一个字段。如果我们需要添加多个字段，可以使用 Add to index 1 column(s) 功能。在字段选择器中，我们选择 language。这次我们不需要输入大小，因为整个字段将在索引中使用。为了更好地记录，我们更改索引名称（author_language 是合适的）。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_135_0.png)

我们保存此索引修改。在索引列表中，我们可以确认我们的索引修改。

| 操作 | 键名 | 类型 | 唯一 | 打包 | 字段 | 基数 | 校对规则 | 空值 | 注释 |
|---|---|---|---|---|---|---|---|---|---|
|  | PRIMARY | BTREE | Yes | No | isbn | 3 | A |  |  |
|  | by_title | BTREE | No | No | title (30) | 0 | A |  |  |
|  | author_language | BTREE | No | No | author_id | 0 | A |  |  |
|  |  |  |  |  | language | 0 | A |  |  |

## FULLTEXT 索引

这种特殊类型的索引允许进行全文搜索。它仅支持 MyISAM 表的 VARCHAR 和 TEXT 字段。我们可以在字段列表中使用 Fulltext 快速链接，或前往索引管理面板并在下拉菜单中选择 Fulltext。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_135_1.png)

我们在 description 字段上添加一个 FULLTEXT 索引，以便能够根据描述中出现的词语定位一本书。

## 表优化——解释查询

在本节中，我们想要获取有关 MySQL 为特定查询使用的索引的信息，以及未定义索引时的性能影响。

假设我们想要使用以下查询：

```
SELECT  *
FROM `book`
WHERE author_id = 2 AND language = 'es'
```

我们想要知道，作者 id 为 2 所写的书籍中，哪些是 es 语言——我们的西班牙语代码。

要输入此查询，我们使用数据库或表菜单中的 SQL 链接，或 SQL 查询窗口（参见第 12 章）。我们在查询框中输入此查询并点击 Go。查询是否找到结果，现在并不重要。

> 你可以通过遵循第 9 章中的解释来产生对 author_id 2 和 language es 的搜索，从而获得相同的查询。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_136_0.png)

让我们查看链接：[Edit] [Explain SQL] [Create PHP Code] [Refresh]

我们现在将使用 [Explain SQL] 链接来获取有关此查询使用了哪个索引（如果有）的信息：

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_136_1.png)

| id | 选择类型 | 表 | 类型 | 可能的键 | 键 | 键长度 | 参考 | 行数 | 额外信息 |
|---|---|---|---|---|---|---|---|---|---|
| 1 | SIMPLE | book | ref | author_language | author_language | 6 | const,const | 1 | Using where |

我们可以看到 EXPLAIN 命令已传递给 MySQL，告诉我们使用的 possible_keys 是 author_language。因此，我们知道此索引将用于此类查询。如果此索引不存在，结果会大不相同。

| id | 选择类型 | 表 | 类型 | 可能的键 | 键 | 键长度 | 参考 | 行数 | 额外信息 |
|---|---|---|---|---|---|---|---|---|---|
| 1 | SIMPLE | book | ALL | NULL | NULL | NULL | NULL | 3 | Using where |

这里，possible_keys（NULL）和 type（ALL）意味着不会使用索引，并且需要检查所有行才能找到所需数据。根据总行数，这可能会对性能产生严重影响。我们可以通过检查 phpMyAdmin 在每个结果页面上显示的查询时间（Query took x sec）并在有索引和无索引的情况下进行比较来确定确切的影响。然而，如果我们只有有限的测试数据，与生产环境中的真实表相比，时间差异可能很小。

## 索引问题的检测

为了帮助用户保持最佳索引策略，phpMyAdmin 会尝试检测一些常见的索引问题。例如，让我们访问 book 表并在 isbn 列上添加一个索引。当我们显示此表的结构时，会收到一个警告：

索引：
| 操作 | 键名 | 类型 | 唯一 | 打包 | 字段 | 基数 | 校对规则 | 空值 | 注释 |
|---|---|---|---|---|---|---|---|---|---|
|  | PRIMARY | BTREE | Yes | No | isbn | 3 | A |  |  |
|  | by_title | BTREE | No | No | title (30) | 0 | A |  |  |
|  | author_language | BTREE | No | No | author_id | 0 | A |  |  |
|  |  |  |  |  | language | 0 | A |  |  |
|  | isbn | BTREE | No | No | isbn | 0 | A |  |  |
|  | description | FULLTEXT | No | No | description | 0 |  |  |  |
警告 | 索引 PRIMARY 和 isbn 似乎相等，其中一个可能被移除。

这里的意图是警告我们在考虑整个表时索引结构效率低下。我们不需要在同一列上拥有两个索引。

## 摘要

在本章中，我们概述了：

- 如何添加字段，包括特殊字段类型，如 TEXT、BLOB、ENUM 和 SET
- 如何为 DATE、DATETIME 和 TIMESTAMP 字段使用日历弹出窗口
- 如何将二进制数据上传到 BLOB 字段
- 如何管理索引（多字段和全文），并从 MySQL 获取有关特定查询中使用了哪些索引的反馈

在第 7 章中，我们将学习如何导出表的结构和数据以备备份之用，或用作通往另一个应用程序的网关。

## 7

## 导出结构和数据（备份）

保持良好的备份对项目至关重要。备份包括最新的备份以及在开发和生产阶段拍摄的中间快照。phpMyAdmin 的导出功能可以生成备份，也可以用于将数据发送到其他应用程序。

## 转储、备份和导出

让我们先澄清一些词汇。在 MySQL 文档中，你会遇到术语 dump，而在其他应用程序中，术语为 backup 或 export。在 phpMyAdmin 上下文中，这些术语含义相同。

MySQL 包含 mysqldump——一种可用于生成导出文件的命令行实用程序。但并非每个主机提供商都提供命令行实用程序所需的 shell 访问权限。此外，从 Web 界面访问导出功能更为方便。这就是为什么 phpMyAdmin（自 1.2.0 版本起）提供导出功能，其导出格式比 mysqldump 更多。本章将重点介绍 phpMyAdmin 的导出功能。

在开始导出之前，我们必须对导出的预期目标有清晰的认识。以下问题可能有所帮助：

- 我们需要完整的数据库还是仅某些表？
- 我们只需要结构、只需要数据，还是两者都需要？
- 将使用哪种实用程序将数据导入回来？（并非每个导出格式都可由 phpMyAdmin 导入。）
- 我们是否只想要数据的一个子集？
- 预期导出的大小是多少，以及我们与服务器之间的链路速度是多少？

## 导出的范围

当我们从 phpMyAdmin 点击 Export 链接时，我们可能处于以下其中一种视图或上下文中——Database view、Table view 或 Server view（更多内容将在第 18 章介绍）。根据当前上下文，结果导出的范围将是完整的数据库、单个表，甚至像 Server view 情况那样的多数据库导出。我们将首先解释数据库导出和所有相关的导出类型。然后我们将继续介绍表和多数据库导出，强调这些导出模式的差异。

## 数据库导出

在 Database view 中，点击 Export 链接。默认导出面板如下所示：

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_140_0.png)

这里选择的默认值取决于 config.inc.php，更具体地说是 $cfg['Export'] 参数数组。例如，$cfg['Export']['format'] 参数设置为 'sql'，因此默认选择 SQL 导出模式。

导出面板有三个子面板。顶部面板 **Export** 和底部面板 **Save as file** 始终存在。然而，第三个面板会变化（使用动态菜单技术），以显示所选导出模式的选项（此处为 SQL）。

## 导出子面板

此子面板包含一个表选择器，我们可以在其中选择所需的表和格式。SQL 格式对我们的需求很有用，因为它创建可在任何 SQL 服务器上工作的标准 SQL 命令。其他可能的格式包括 LaTeX、PDF、Microsoft Excel 2000、Microsoft Word 2000、**逗号分隔值（CSV）**、YAML、CodeGen、Texy! 文本、Open Document Spreadsheet、Open Document Text 和 XML。另一种格式 Native MS Excel 在进一步的软件安装和配置后可用。（参见本章中关于 *Native MS Excel* 的部分。）

我们将现在讨论可以使用 **Export** 子面板选择的格式（以及在选择后可用的选项）。

> 即使我们可以将 phpMyAdmin 导出为所有这些格式，只有 SQL 和 CSV 格式可以使用当前 phpMyAdmin 版本重新导入。仅将这两种格式用于备份。

## SQL

我们将首先点击 **Select All**；我们想要所有表。我们知道表很小，因此屏幕上的导出不会太大。暂时，让我们取消选择 **Extended inserts** 复选框。然后我们点击 **Go**，产生以下输出：

```
-- phpMyAdmin SQL Dump
-- version 3.1.1
-- http://www.phpmyadmin.net
--
-- Host: localhost
-- Generation Time: Dec 21, 2008 at 12:55 PM
-- Server version: 5.0.67
-- PHP Version: 5.2.6
SET SQL_MODE="NO_AUTO_VALUE_ON_ZERO";
```
