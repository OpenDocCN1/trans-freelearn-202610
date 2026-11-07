
## 使验证器可用

一些参数必须在 config.inc.php 中进行配置。将 $cfg['SQLQuery']['Validate'] 设置为 TRUE 可启用 Validate SQL 链接。

我们还应启用 Validator 本身（因为在未来的 phpMyAdmin 版本中可能提供其他验证器）。这可通过将 $cfg['SQLValidator']['use'] 设置为 TRUE 来完成。

Validator 默认使用匿名 Validator 账户访问，如以下命令配置：

```
$cfg['SQLValidator']['username'] = '';
$cfg['SQLValidator']['password'] = '';
```

相反，如果公司已为我们提供了一个账户，我们可以在此处使用该账户信息。

## 验证器结果

Validator 返回两种报告——一种是在查询符合标准时，另一种是在查询不符合标准时。

## 符合标准的查询

我们将尝试一个简单的查询：SELECT COUNT(*) FROM book。像往常一样，我们在查询框中输入此查询并发送。在结果页面上，我们现在看到一个额外的链接——Validate SQL。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_227_0.png)

点击 Validate SQL 会产生以下报告：

```
Conforms to Core SQL-99
SQL queries stored anonymously for statistical purposes.
```

我们可以选择点击 Skip Validate SQL 来查看原始查询。

## 不符合标准的查询

让我们尝试另一个在 MySQL 中能正确工作的查询：SELECT * FROM book WHERE language = 'en'。将其发送到 Validator 会产生以下报告：

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_228_0.png)

每次 Validator 发现问题时，它都会在错误位置添加类似 {error: 1} 的消息，并在报告中添加脚注。这次，language 列名是非标准的。因此，Validator 告诉我们它在此处期望一个标识符。关于 LIMIT 子句的使用还报告了另一个非标准错误，该子句是由 phpMyAdmin 添加到查询中的。

另一个例子是反引号。如果我们只是为 book 表点击 **Browse**，phpMyAdmin 会生成 SELECT * FROM `book`，用反引号将表名括起来。这是 MySQL 保护标识符的方式，标识符可能包含特殊字符，例如空格、国际字符或保留字。然而，将此查询发送到 Validator 会显示反引号不符合标准 SQL。我们甚至可能收到两个错误，每个反引号一个。

## 摘要

本章帮助我们了解查询框的目的并展示它们的位置。它还概述了 Query 窗口选项、多语句查询、如何使用字段选择器、如何使用 SQL Validator，以及最后如何获取已输入命令的历史记录。下一章将展示如何借助 phpMyAdmin 的查询生成器在不输入的情况下生成多表查询。

## 13 多表查询生成器

数据库视图或表视图中的 **Search** 页面用于单表查找。本章介绍数据库视图中可用的多表 **Query by example (QBE)** 功能。

许多 phpMyAdmin 用户在表视图中逐表工作，因此往往会忽略多表查询生成器，而这是一个用于微调查询的绝佳功能。查询生成器不仅在多表情况下有用，在单表情况下也很有用。它使我们能够为一列指定多个条件，而表视图中的 **Search** 页面不具备此功能。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_231_0.png)

> 本章中的示例假定已完成链接表基础设施的多用户安装（参见第 11 章），并且在第 10 章练习期间创建的 book-copy 表仍存在于 marc_book 数据库中。

要打开此功能页面，我们进入特定数据库的数据库视图（查询生成器一次仅支持在一个数据库上工作）并点击 **Query**。

对面页面的截图显示了初始 QBE 页面。它包含以下元素：

- 条件列
- 添加条件行的界面
- 添加条件列的界面
- 表选择器

## 多表查询生成器

- 查询区域
- 更新或执行查询的按钮

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_232_0.png)

## 选择表

初始选择包含所有表。在此示例中，我们假设链接表基础设施已安装在 marc_book 数据库中。（参见第 11 章中的“Single-user installation”部分。）因此，字段选择器包含大量字段。对于我们的示例，我们将仅使用 author 和 book 表。因此，我们仅从 Use Tables 选择器中选择这些表。

然后我们点击 Update Query。这会刷新屏幕并减少字段选择器中可用的字段数量。我们始终可以使用浏览器在下拉菜单中进行多项选择的机制（通常是 control-click）稍后更改所选表。

## 列条件

默认情况下提供三个条件列。本节讨论编辑其条件的选项。这些包括选择字段、排序单个列、为单个列输入条件等选项。

## 字段选择器：单列或所有列

字段选择器包含所选表的各个列，以及每个表以星号 (*) 结尾的特殊选择，这意味着所有字段都被选中。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_233_0.png)

要显示 author 表中的所有字段，我们将选择 `author`.* 并勾选 Show 复选框，而不在 Sort 和 Criteria 框中输入任何内容。在我们的例子中，我们选择 `author`.`name`，因为我们想为作者姓名输入一些条件。

## 排序

对于每个选定的单个列，我们可以指定排序（升序或降序），或让该行保持不变（不排序）。如果我们选择多个排序列，排序将从左到右进行。

> 当我们要求对列进行排序时，我们通常会勾选 Show 复选框。但这不是必要的，因为我们可能只想执行排序操作而不显示该列。

## 显示列

我们勾选 Show 复选框以便在结果中看到该列。有时，我们可能只想对列应用条件，而不将其包含在结果页面中。在这里，我们添加 phone 列，要求对其进行排序，并选择同时显示 name 和 phone number。我们还要求按升序对 name 进行排序。排序将首先按 name 进行，如果姓名相同，则按 phone number 进行。这是因为 name 在 phone 列左侧的条件列中，因此优先级更高。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_234_0.png)

## 更新查询

在任何时候，我们都可以点击 Update Query 按钮查看生成的查询进度。我们必须在执行查询之前至少点击一次。现在，让我们点击它并查看查询区域中生成的查询。在以下示例中，我们将在每次修改后点击 Update Query。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_234_1.png)

我们已选择了两个表，但尚未从 book 表中选择任何列。因此，该表未在生成的查询中提及。

## 条件

在 Criteria 框中，我们可以为每个对应列输入条件（遵守 SQL WHERE 子句的语法）。默认情况下，我们有两个条件行。要查找姓名中包含 Smith 的所有作者，我们使用 LIKE 条件（LIKE '%SMITH%'）并点击 Update Query。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_235_0.png)

我们还有另一行可用于输入额外条件。假设我们还想找到作者 **Maria Sunshine**。这次我们使用 = 条件。两个条件行将由 OR 运算符连接，该运算符默认从界面左侧选择。

![](img/cb8c75b631b4e1f7afe5a0d39becfda9_235_1.png)
