# 高效 MySQL 管理的 phpMyAdmin 3.1 精通指南

## 鸣谢

| **作者** | **生产编辑经理** |
|---|---|
| Marc Delisle | Abhijeet Deobhakta |
| **审阅者** | **编辑团队负责人** |
| Kai "Oswald" Seidler | Akshara Aware |
| **策划编辑** | **项目团队负责人** |
| Shilpa Dube | Lata Basantani |
| **开发编辑** | **项目协调员** |
| Shilpa Dube | Rajashree Hamine |
| **技术编辑** | **审校** |
| Gaurav Datar | Mark Reardon |
|  | Joel T. Johnson |
| **校对编辑** | **生产协调员** |
| Sumathi Sridhar | Rajni R. Thorat |
| **索引员** | **封面工作** |
| Rekha Nair | Rajni R. Thorat |

## 关于作者

Marc Delisle 由于其对 phpMyAdmin 的参与，是汇聚社区开发者的 MySQL Developers Guild 的成员。他于 1998 年 12 月开始为这个流行的 MySQL Web 界面做出贡献，当时他制作了其第一个多语言版本。作为一名开发人员和项目管理员，他自 2001 年以来一直积极参与该软件项目。

自 1980 年以来，Marc 在加拿大魁北克谢布鲁克的 Cegep de Sherbrooke 工作，担任应用程序程序员和网络管理员。他还教授网络、安全以及 PHP/MySQL 应用程序开发。在他的其中一门课程中，他很高兴遇到了来自阿根廷的 phpMyAdmin 用户。Marc 与妻子住在谢布鲁克，他们喜欢与四个孩子共度时光。

本书是 Marc 于 2004 年由 Packt Publishing 出版的第一本关于 phpMyAdmin 的书籍的更新，随后还有 Packt Publishing 出版的“Creating your MySQL Database: Practical Design Tips and Techniques”。

> 我由衷感谢 Louay Fatoohi，他为这个图书项目找到了我，并感谢 Packt 团队在制作过程中提出的宝贵意见。我的感谢也献给 Garvin Hicking、Alexander Marcus Turek 和 Kai 'Oswald' Seidler——本书历次版本的审阅者。他们的敏锐目光帮助使本书更清晰、更完整。
> 
> 最后，我想感谢所有为 phpMyAdmin 的源代码、翻译和文档做出贡献的人。他们为这个项目付出的时间仍然激励着我，并不断推动我前进。

## 关于审阅者

**Kai "Oswald" Seidler** 于 1970 年出生于汉堡。他毕业于柏林工业大学，获得 Diplom-Informatiker 学位（相当于计算机科学硕士学位）。在 90 年代，他创建并管理了德国最大的 IRCnet 服务器——irc.fu-berlin.de，并共同管理了世界上最大的匿名 FTP 服务器之一——ftp.cs.tu-berlin.de。

他在 1993 年建立了第一个专业公共 Web 服务器。1993 年至 1998 年，他是 Projektgruppe Kulturraum Internet 的成员——一个关于网络文化和网络组织的科研项目。2002 年，他共同创立了 Apache Friends 并创建了跨平台 Apache Web 服务器捆绑包 XAMPP。约在 2005 年，XAMPP 成为全球最受欢迎的 Apache 堆栈。

2006 年，他的第三本书“Das XAMPP-Handbuch”由 Addison Wesley 出版。

致 Carole、André、Corinne、Annie 和 Guillaume，满怀爱意。

## 前言

为 MySQL 提供强大的图形化管理界面，phpMyAdmin 是最受欢迎的开源应用程序之一。虽然大多数开发人员每天都在使用 phpMyAdmin 的常规功能，但很少有人意识到其高级功能所具有的强大功能和潜力。本书建立了对 phpMyAdmin 核心功能的扎实理解，然后带您了解这一传奇工具的方方面面。

被数百万开发人员使用，MySQL 是最受欢迎的开源数据库。它支持众多大型、动态的网站和应用程序。MySQL 凭借其开源特性、性能、可靠性、健壮性以及对多平台的支持而获得了广泛的流行。然而，这种流行也得益于 phpMyAdmin 的存在——该行业标准管理工具使数据库管理对经验丰富的开发人员和初学者都变得简单。

为 MySQL 带来 Web 界面，使 phpMyAdmin 成为 MySQL 和 Web 开发人员的必备工具，无论他们是专业人士还是在课堂上。每位用户都可以通过释放这一强大应用程序的全部潜力而从 phpMyAdmin 中受益。

## 本书涵盖内容

第 1 章介绍了我们为什么应使用 phpMyAdmin 作为开发 Web 应用程序的手段。然后它涵盖了 phpMyAdmin 的下载和安装程序。

第 2 章提供了 MySQL 中使用的各种身份验证类型的概述。然后它涵盖了与 phpMyAdmin 安装相关的安全问题。

第 3 章介绍了 phpMyAdmin 界面的概述。它包括登录面板、左侧和右侧面板（包括 Light 和 Full 模式）以及查询窗口。

第 4 章全部关于数据库创建。它教我们如何创建表，如何手动插入数据，以及如何排序数据。

第 5 章涵盖了 phpMyAdmin 中数据编辑的方面。它教我们处理 NULL 值、多行编辑。最后，它涵盖了任何数据库工具的一个重要方面——数据删除。

第 6 章探讨了更改表结构的问题。其主要重点是编辑字段属性和索引管理。

第 7 章涉及备份和导出。它列出了触发导出的各种方式、可用导出格式、与导出格式相关的选项以及导出文件可能发送到的各个位置。

第 8 章告诉我们如何恢复为备份和传输目的而创建的导出数据。它提供了 phpMyAdmin 中导入数据的各种选项的概述，以及导入 SQL 和 CSV 文件所涉及的不同机制。最后，它涵盖了导入文件时可能遇到的限制以及克服这些限制的方法。

第 9 章介绍了用于有效搜索数据的机制。

第 10 章涵盖了执行某些操作的方法，这些操作会影响整个表或数据库并可在其上执行。最后，它处理表修复和优化的表维护操作。

第 11 章是我们开始介绍 phpMyAdmin 高级功能的地方。本章解释了如何定义表间关系。它还解释了如何安装链接表基础设施——高级功能的前提条件。

第 12 章帮助我们输入自己的 SQL 命令。本章还提供了查询窗口的概述——用于编辑 SQL 查询的窗口。最后，它还帮助我们获取已输入命令的历史记录。

第 13 章涵盖了多表查询生成器，它允许我们在不实际输入的情况下生成多表查询。

第 14 章涵盖了书签——链接表基础设施的功能之一。它涵盖了如何记录书签以及如何操作它们。最后，它涵盖了向书签传递参数，以及直接从 pma_bookmark 表执行书签。

第 15 章概述了如何生成文档，该文档使用 phpMyAdmin 提供的工具解释数据库的结构。

第 16 章解释了如何对数据应用转换，以便在查看时自定义其格式。

第 17 章涵盖了 phpMyAdmin 对 MySQL 5.0 和 5.1 版本中新增功能的支持。

第 18 章关于 MySQL 服务器的管理，重点是用户账户和权限的管理。本章讨论系统管理员如何使用 phpMyAdmin 的服务器管理功能进行日常用户账户维护、服务器验证和服务器保护。

附录 A 提供了 phpMyAdmin 项目的历史，从 1998 年的起源到 2001 年的项目重新启动，以及随后的演进。

附录 B 解释了如何通过检查其一些错误消息并提出适当的解决方案来排除 phpMyAdmin 的故障。它还解释了如何与开发团队互动以获得支持、错误报告和贡献。

## 您需要具备的条件

您需要能够访问安装了以下软件的服务器或工作站：

- MySQL 5.0 或更新版本
- PHP 5.0 或更新版本
- 支持 PHP 的 Web 服务器软件（本书解释了 phpMyAdmin 在 Apache 和 IIS 上的安装）

## 本书适合谁

phpMyAdmin 是每个需要有效管理 MySQL 数据库和表的人必备的工具。本书是每个希望充分发挥这一出色应用程序潜力的严肃 phpMyAdmin 用户必读之作。

## 约定

在本书中，您会发现多种文本样式，用于区分不同类型的信息。以下是这些样式的一些示例及其含义说明。

文本中的代码词显示如下：“如果我们使用 config 身份验证类型，则不会显示登录面板。”



代码块将如下所示设置：

```
$cfg['MySQLManualBase'] = 'http://www.mysql.com/doc/en';
$cfg['MySQLManualType'] = 'searchable';
```

当我们希望引起您对代码块中某一特定部分的注意时，相关行或项目将以粗体显示：

```
$cfg['Servers'][$i]['AllowDeny']['order'] = 'allow,deny';
```

任何命令行输入或输出均如下所示书写：

```
tar -xzvf phpMyAdmin-3.1.1-all-languages.tar.gz
```

新术语和重要词语以粗体显示。例如，您在屏幕上看到的、菜单或对话框中的词语，在我们的文本中会这样出现："The Home link from the left panel is used to display this page"。

警告或重要提示会出现在如下的框中。

技巧和窍门会这样出现。

## 读者反馈

来自我们读者的反馈总是受欢迎的。请告诉我们您对本书的看法——您喜欢或可能不喜欢的部分。读者反馈对我们开发您真正能充分利用的书名非常重要。

如需发送一般反馈，只需发送电子邮件至 feedback@packtpub.com，并在邮件主题中提及书名即可。

如果您需要一本书并希望我们出版，请在 www.packtpub.com 上的 SUGGEST A TITLE 表单中给我们留言，或发送电子邮件至 suggest@packtpub.com。

如果您在某个主题方面有专长，并且有兴趣撰写或为一本书做出贡献，请参阅 www.packtpub.com/authors 上的作者指南。

## 客户支持

现在您已成为 Packt 书籍的自豪拥有者，我们提供多项帮助，以助您充分利用您的购买。

## 勘误

尽管我们已尽一切努力确保内容的准确性，但错误仍会发生。如果您在我们的某本书中发现错误——可能是文本或代码中的错误——我们将不胜感激您向我们报告。这样做可以让其他读者免于沮丧，并帮助我们改进本书的后续版本。如果您发现任何勘误，请访问 http://www.packtpub.com/support，选择您的图书，点击 let us know 链接，并输入勘误的详细信息。一旦您的勘误得到验证，您的提交将被接受，并将勘误添加到现有勘误列表中。任何现有勘误都可以通过从 http://www.packtpub.com/support 选择您的书名来查看。

## 盗版

互联网上版权材料的盗版是所有媒体中持续存在的问题。在 Packt，我们非常重视对版权和许可的保护。如果您在互联网上以任何形式遇到我们作品的非法复制品，请立即提供位置地址或网站名称，以便我们采取补救措施。

请通过 copyright@packtpub.com 联系我们，并附上疑似盗版材料的链接。

我们感谢您帮助保护我们的作者，以及我们为您提供有价值内容的能力。

## 问题

如果您在本书的任何方面遇到问题，可以联系 questions@packtpub.com，我们将尽力解决。
