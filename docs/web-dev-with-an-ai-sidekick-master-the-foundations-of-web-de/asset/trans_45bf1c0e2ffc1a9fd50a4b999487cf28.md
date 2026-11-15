
# 复习问题

1. 什么是结构化类型，它与名义类型有何不同？
2. 为什么在现代 TypeScript 中，字面量联合类型通常比枚举更受青睐？
3. `tsconfig.json` 中的 `target` 选项控制什么，为什么应避免依赖默认目标？
4. 使用第三方库时，声明文件 (`.d.ts`) 为什么很重要？

# 官方 TypeScript 资源

以下是您应该了解的官方权威 TypeScript 资源。这些资源由 Microsoft 和 TypeScript 团队维护，因此请先从这些资源入手，然后再参考博客文章或论坛解答。

## TypeScript 手册

这是主要的学习资源。它结构清晰、循序渐进，由 TypeScript 团队编写。手册不仅解释语法，还阐述功能存在的原因及其预期用法。对大多数读者而言，在阅读完本章后，这里应是首选去处。

[https://www.typescriptlang.org/docs/handbook/intro.html](https://www.typescriptlang.org/docs/handbook/intro.html)

## TypeScript 文档

这是所有官方 TypeScript 文档的入口。从此处可访问手册、配置选项、编译器详情以及不同环境的指南。

[https://www.typescriptlang.org/docs/](https://www.typescriptlang.org/docs/)

## TypeScript Playground（在线练习场）

这是一个交互式的基于浏览器的编辑器，让您无需安装任何东西即可试验 TypeScript。左侧显示 TypeScript 代码，右侧显示生成的 JavaScript，这非常有助于强化“类型在运行时消失”的概念。

[https://www.typescriptlang.org/play](https://www.typescriptlang.org/play)

## TypeScript 编译器选项

此页面记录了 `tsconfig.json` 中的每一个编译器选项。您可能不立即需要它，但一旦您想在实际项目中收紧或放宽规则，它就变得极其宝贵。

[https://www.typescriptlang.org/tsconfig](https://www.typescriptlang.org/tsconfig)

## TypeScript GitHub 仓库

这是语言本身的事实来源。问题、发行说明、设计讨论和破坏性变更均首发于此。高级读者若想了解设计决策或跟踪即将推出的功能，最终都会来到这里。

[https://github.com/microsoft/TypeScript](https://github.com/microsoft/TypeScript)

## 发行说明（按 TypeScript 版本）

这有助于理解语言的演进。TypeScript 更新迅速，此页面帮助您查看变更内容及原因。

[https://www.typescriptlang.org/docs/handbook/release-notes/overview.html](https://www.typescriptlang.org/docs/handbook/release-notes/overview.html)

# 总结

在本章中，您学习了以下内容：

- TypeScript 是什么，以及它如何基于 JavaScript 构建而非取代它
- 浏览器从不直接运行 TypeScript，只会执行生成的 JavaScript
- 如何使用 Node.js 和 TypeScript 编译器设置基本的 TypeScript 环境
- TypeScript 编译器如何在开发时检查代码，然后移除所有类型
- 类型推断如何工作，以及何时依赖它与何时显式标注
- 接口如何描述对象的形状并在代码中充当契约
- 可选属性如何工作，以及它们如何强制更安全地处理缺失数据
- 类型化函数和数组如何减少整类运行时错误

在下一章中，您将从类型和工具链转向 SQL——关系型数据库的语言。这是一个很好的下一步，因为 TypeScript 部分旨在使数据形状显式化，而 SQL 则旨在干净地存储和查询结构化数据。

# 获取本书的 PDF 版本

扫描二维码（或访问 packtpub.com/unlock）。按名称搜索本书，确认版本，然后按照页面上的步骤操作。

![](Images/Unlock.png)![](Images/Unlock_Now.png)

> **注意：** 请保留好发票。直接从 Packt 购买的不需要发票。

# 第 2 部分：后端 Web 开发

在本书的第二部分中，您将深入幕后，了解处理数据、逻辑和通信的应用程序部分。您将使用 SQL、Python 和 Django 框架来理解请求如何被处理、数据如何被存储和检索，以及系统不同部分如何交互。

这部分侧重于浏览器请求到达服务器后发生的事情。您将看到数据建模和应用程序结构中的微小决策如何影响构建在其之上的一切。您将学习请求、数据和代码如何组合在一起，而不是将后端视为抽象事物，这使得调试和扩展应用程序变得更加容易。

本书的这一部分包含以下章节：

- [第 7 章](#07.xhtml#_idTextAnchor374)，*SQL – 管理数据*
- [第 8 章](#08.xhtml#_idTextAnchor494)，*Python – 后端编程*
- [第 9 章](#09.xhtml#_idTextAnchor598)，*Django – 后端 Web 开发*
- [第 10 章](#10.xhtml#_idTextAnchor689)，*Bash/Shell – 开发者命令行技能*

# 第 7 章：SQL – 管理数据

网页可以容忍一点丑陋。数据模型则不那么宽容。一旦数据重复、不一致或关联不当，整个应用程序就会开始为此买单。

本章使用 SQLite 和一个小型示例数据库介绍 SQL 和关系型数据库。目标不是将您培养成数据库管理员，而是帮助您理解结构化数据如何存储、关联、查询和修改。

您将从关系型数据库的基本概念入手：表、行、列、键、关系、`NULL` 和约束。然后您将设置 SQLite，检查示例数据库，并使用 `SELECT`、`WHERE`、`ORDER BY`、`JOIN`、`GROUP BY` 和 `HAVING` 编写真实查询。之后，您将使用 `INSERT`、`UPDATE` 和 `DELETE` 修改数据，了解事务，最后通过索引和性能的实用介绍结束本章。

读完本章后，您应该理解关系型数据库如何存储和关联相关事实。学习 SQL 语法很重要，但更大的技能是学会对数据提出精确的问题：什么存在、各部分如何关联、什么必须始终为真，以及应用程序需要什么答案。

在本章中，我们将涵盖以下主题：

- SQL 和数据库简介
- 设置 SQLite 和示例数据库
- 编写查询和连接表
- 修改数据
- 使用索引提升性能

本章建议的聊天机器人提示词及示例响应可在以下链接找到：[https://github.com/markjprice/web-dev-ai/blob/main/prompts/ch07-sql/readme.md](https://github.com/markjprice/web-dev-ai/blob/main/prompts/ch07-sql/readme.md)

## SQL 和数据库简介

我们应该首先回答这个显而易见的问题。

### 什么是数据库？

SQLite、PostgreSQL、MySQL 和 SQL Server 是数据库引擎。SQL 是您用来与它们交互的通用语言。每个引擎都有自己的扩展和怪癖，但核心 SQL 概念是共享的。

数据库是一个用于存储数据的有组织系统，以便数据可以被检索、更新并随时间保持一致。在关系型数据库中，数据存储在表中。每个表代表一种事物。例如，电影、电视剧、演员或剧集。

