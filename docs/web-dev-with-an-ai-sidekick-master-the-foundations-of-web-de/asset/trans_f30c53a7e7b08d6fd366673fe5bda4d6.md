
# SQL 基础：复习题

1. SQL 作为一种声明式语言意味着什么？
2. 表、行和列有什么区别？
3. 为什么主键很重要？
4. 什么是外键，它解决了什么问题？
5. 为什么规范化在数据库设计中很重要？
6. `WHERE` 子句在 `SELECT` 查询中的作用是什么？
7. `INNER JOIN` 和 `LEFT JOIN` 有什么区别？
8. `WHERE` 和 `HAVING` 有什么区别？
9. 为什么在没有 `WHERE` 子句的情况下运行 `UPDATE` 或 `DELETE` 很危险？
10. 什么是事务，为什么它很重要？

## 链接

SQL 是标准化的，但每个数据库都有扩展和边缘情况。有疑问时，请始终查阅你实际使用的数据库的官方文档。首先信任供应商文档，其次是博客，最后才是复制粘贴的代码片段。

以下是可靠的官方 SQL 参考指南和文档站点。它们由数据库供应商或核心项目团队维护，反映实际行为，而不是像 LinkedIn 等网站上流传的迷思。

### 跨数据库和标准导向的参考资料

这些资源适合想要获得更广泛或更中立的 SQL 视角的读者。

- ISO SQL 标准概览：<https://www.iso.org/standard/76583.html>
- W3Schools SQL 参考（非官方，但易上手且可搜索）：<https://www.w3schools.com/sql/>
- MDN SQL 概览：<https://developer.mozilla.org/en-US/docs/Glossary/SQL>

### SQLite

SQLite 文档极其完善，且异常易读。

- SQLite 官方文档首页：<https://www.sqlite.org/docs.html>
- SQL 语言参考（适合了解 SQLite 特有行为和扩展）：<https://www.sqlite.org/lang.html>
- `SELECT` 语句参考：<https://www.sqlite.org/lang_select.html>
- 数据类型和类型亲和性（理解 SQLite 灵活性的关键）：<https://www.sqlite.org/datatype3.html>
- SQLite 命令行工具文档：<https://www.sqlite.org/cli.html>

### PostgreSQL

PostgreSQL 拥有目前最全面、最严谨的 SQL 文档之一。它冗长但极其精确。

- PostgreSQL 官方文档首页：<https://www.postgresql.org/docs/>
- PostgreSQL SQL 命令参考（按字母顺序）：<https://www.postgresql.org/docs/current/sql-commands.html>
- 索引和性能概念：<https://www.postgresql.org/docs/current/indexes.html>

### SQL Server (T-SQL)

Microsoft SQL Server 使用 Transact-SQL (T-SQL)，它用过程化特性扩展了标准 SQL。

- SQL Server 文档首页：<https://learn.microsoft.com/sql/sql-server/>
- T-SQL 语言参考：<https://learn.microsoft.com/sql/t-sql/language-reference>
- 事务和锁定：<https://learn.microsoft.com/sql/t-sql/language-elements/transactions-transact-sql>

## 总结

在本章中，你了解到 SQL 不主要关注巧妙的语法。它关乎描述你想要的数据，并依赖数据库引擎安全地计算出如何检索或修改数据。这就是为什么关系型数据库如此重视模式设计、键、约束和事务。良好的 SQL 始于良好的模型。

你梳理了使关系系统可用的核心要素：表、行、列、主键、外键、规范化、`NULL` 和约束。随后你在 SQLite 中将这些概念付诸实践，编写了能够筛选、排序、连接、分组和汇总数据的查询。你还看到了如何安全地修改数据，以及为什么一旦系统超越玩具规模，事务和索引就变得至关重要。

当模型错误时，数据库是无情的。混乱的模式会导致笨拙的查询、脆弱的连接和基于猜测的性能工作。合理的模式不会让每个查询都变简单，但它能消除大量不必要的困难。从数据模型入手，再编写适配它的查询。

具体来说，你学习了：

- SQL 是什么和不是什么，包括 SQL 是声明式和基于集合的，而不是逐步执行的这一概念
- 表、行和列如何建模现实世界的概念，以及为什么重复数据会导致错误和不一致
- 主键如何赋予行标识，外键如何定义和强制表之间的关系
- 诸如 `NOT NULL`、`UNIQUE`、`CHECK` 和外键等约束如何防止无效数据进入数据库
- 如何使用 SQLiteStudio、VS Code 和 SQLite 命令行工具等跨平台运行 SQL 查询
- 如何编写 `SELECT` 查询以检索数据、筛选行、排序结果并可预测地限制输出
- 如何使用不同类型的 `JOIN` 组合相关数据，以及如何判断哪些行出现在结果中
- 如何使用 `GROUP BY`、聚合函数（如 `COUNT` 和 `AVG`）以及 `HAVING` 筛选来汇总数据
- 如何使用 `INSERT`、`UPDATE` 和 `DELETE` 安全地插入新数据、更新现有数据和删除行
- 为什么事务很重要，以及它们如何防止数据库陷入“半完成”状态
- 索引是什么，它们如何加速读取同时减慢写入，以及查询计划如何解释数据库行为

在下一章中，你将学习 Python，这是一种以可读语法和庞大标准库著称的高级通用编程语言，广泛用于 Web 开发、数据分析、自动化和脚本编写。这是一个很好的过渡，因为 Python 为你提供了一种构建应用逻辑的通用语言，而本章专注于该逻辑所依赖的数据层。

## 获取本书的 PDF 版本和更多内容

扫描二维码（或访问 packtpub.com/unlock）。按名称搜索本书，确认版本，然后按照页面上的步骤操作。

![](Images/Unlock.png) ![](Images/Unlock_Now.png)

> **注意：** 请保留好发票。直接从 Packt 购买不需要发票。

# 第 8 章：Python – 后端编程

Python 是本书后端部分将使用的语言，因此本章旨在让你熟悉实际工作中最重要的 Python 部分。本章不试图涵盖 Python 的每个角落。它提供足够的流利度，让你能够阅读 Python 代码、运行它、修改它，并理解它在做什么。

你将从安装 Python 开始，确保它能在命令行和 VS Code 中正常运行。然后你将学习基础知识：变量、值、条件、循环、函数，以及 Python 高度依赖的内置数据结构。之后，你将了解字符串、调试、异常、模块、包、虚拟环境，以及文件输入和输出 (I/O)。本章以一个小型的命令行任务追踪器结束，它将这些概念整合成一个可工作的程序。
