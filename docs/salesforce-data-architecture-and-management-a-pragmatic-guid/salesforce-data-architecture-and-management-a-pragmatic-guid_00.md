# Salesforce 数据架构与管理：有志成为 Salesforce 架构师和开发人员的实用数据管理、治理与安全指南







## Contributors

### About the author

Ahsan Zafar is Salesforce Technical Architect. He is 15x Salesforce certified and is a Salesforce Certified Technical Architect (CTA) candidate. He has spent around two decades working with different aspects of data, including data architecture, master data management, and data migrations. Ahsan has also had stints as a Business Analyst, Project Manager, and Salesforce consultant on various projects that gave him a unique understanding of how data is perceived by different roles. He has also worked as a developer and as a technical lead, spending almost a decade implementing Oracle ERP systems that typically involved migrating significant volumes of data from legacy systems.

### About the reviewers

**Ali Najafi** is an enterprise architect with 18 years of experience, of which 13 years has been in Salesforce. He is a Salesforce CTA with a track record for delivering complex digital transformation projects on the Salesforce platform.

Experienced in leading Salesforce architecture and design authority teams, he was responsible for ensuring all Salesforce delivery teams adhered to Salesforce's best architecture and design principles. He has a significant interest in cybersecurity, identity and access management, digital data management, MDM, analytics, and AI, where he has worked on several enterprise clients both directing the architectural domain and delivering solutions that have proven to be secure and scalable for millions of customers and thousands of enterprise users.

**Ram Iyer** is a certified Salesforce application architect and system architect with 20 years of overall experience in enterprise architecture, solution design, data architecture, business analysis, and project management. He has over 12 years of architecture, design, and development expertise in Salesforce.com, Lightning, and Einstein Analytics. Ram has been a guest speaker at Dreamforce (2016, 2017) and is an evangelist for enabling continuous integration and delivery capabilities in large organizations. Passionate about technology, Ram has worked extensively on API design, application integration, business intelligence, DevSecOps, Agile, and Waterfall models of software development, with proven leadership abilities in managing large, complex projects.


## 您可能还喜欢的其他书籍

## 索引

## 前言

数据对组织来说变得越来越有价值，因为它们意识到了数据的真正潜力以及数据如何帮助其实现业务目标。之所以这样，是因为数据可以转化为可操作的情报，组织可以利用这些情报让内部和外部客户满意，同时保持竞争优势。

关键在于从数据中得出可操作的情报，否则它只会占用存储空间并成为组织的一项开支。Salesforce 平台有助于将无法操作的数据转化为可操作的情报，但要使此持续发生，数据架构必须得到合理设计并支持可扩展性。数据建模的失误包括在账户记录上创建汇总字段，其中年份被硬编码以显示账户收入，并且需要每年更新汇总字段过滤器。失误还包括更糟糕的情况：原本可能为应用程序使用查找关系，却创建了父子关系，导致紧密耦合的关系。选择这种关系类型的唯一原因在于它提供了汇总字段功能。

在本书中，我们将从理解数据架构师的职责以及如何成为一名成功的 Salesforce 数据架构师的基础知识入手。接着，你将学习数据建模和数据管理。掌握基础后，我们将深入探讨主数据管理、数据治理，以及如何确保应用程序的性能。

数据正以快速的速度增长，了解如何有效设计涉及大量数据的解决方案是一项关键技能。因此，我们将全面介绍大数据量（LDVs），以及作为架构师，我们可以在考虑可扩展性和 Salesforce 限制的前提下，如何有效管理它们。

我在每章末尾都包含了问题，以确保你已清晰理解章节中讨论的概念。此外，各章中还包含大量示例和图表，以确保建立对概念的深入理解。

读完本书后，你将掌握可以立即应用于 Salesforce 环境中的数据架构技能和最佳实践的扎实理解。本书涵盖的内容也与数据架构与管理设计师考试，以及 Salesforce 认证技术架构师（CTA）考试中测试的数据架构领域高度相关（https://trailhead.salesforce.com/credentials/dataarchitectureandmanagementdesigner）。

## 本书适合人群

本书适合渴望成为架构师的初学者和有经验的架构师、Salesforce 管理员以及开发人员，他们希望在数据管理与架构的背景下了解更多关于 Salesforce 平台核心架构的内容。无论你是刚以管理员或开发人员身份步入 Salesforce 生态系统，还是拥有多年经验的资深架构师，本书都能让每个人受益。

要充分利用本书，你应具有在大型复杂技术环境（由多种异构系统组成）中进行数据迁移和集成的经验。你还需要处理过涉及隐私法规以及主数据管理等主题的数据，以便在你的技术知识环境中进一步管理数据。拥有数百万条记录的大型 Salesforce 组织并担心性能问题的专业人士也将从本书中受益，因为在撰写本书时，可扩展性和性能一直是重中之重。

尽管本书涵盖了与多个 Salesforce 认证考试重叠的众多主题，但专门准备数据架构与管理设计师考试或 Salesforce CTA 评审委员会的 Salesforce 专业人士会发现，本书对这些考试的准备有所帮助，可作为辅助材料。

## 本书涵盖的内容

第 1 章“数据架构师：角色与职责”描述了数据架构师的角色及其所需的核心技能和经验。它还将详细介绍在该角色中取得成功所需的软技能。你还将了解数据架构师的一天。

第 2 章“理解 Salesforce 对象与数据建模”将带你了解 Salesforce 平台的独特架构，以及它为何针对读取访问（而非关系型数据库中传统的写操作）进行了优化。本章将讨论数据建模概念、它们在 Salesforce 中的应用方式、什么是反规范化，以及为何花时间正确设计数据模型非常重要。章节末尾将介绍 Salesforce 对象及其创建方式、对象上的不同字段类型及其用例。

第 3 章“理解数据管理”将解释什么是数据管理以及它为何重要。还将讨论管理数据的不同方面，包括数据生命周期。由于 Salesforce 已停止数据恢复服务，数据备份和归档已成为重中之重，因此我们也将详细讨论这一点。最后，将回顾一些可用于有效管理数据的工具。

第 4 章“理解主数据管理”将讨论主数据的关键属性、什么是金色记录（Golden Record）以及它对组织为何如此重要。我们将探讨如何将 MDM 与 CRM 策略相协调，并讨论 Salesforce 的 Customer 360 及其关键组件。MDM 是一种与平台无关的概念，也可用于非 Salesforce 环境。本章将以对通用信息模型（CIM）的简要讨论结束。

第 5 章“实施数据治理”涵盖企业数据治理的重要性、数据治理与数据管理之间的关系，以及如何评估当前的数据治理状态。还将详细介绍两项主要隐私保护法：GDPR 和 CCPA。为了总结并巩固对内容的理解，一个示例案例研究将描述一个假设场景及其解决方法。

第 6 章“管理性能”将探讨 Force.com 平台上的性能基础知识，如何使用 Query Plan 工具确定影响性能的查询，以及使用索引与全表扫描时的查询成本。本章还涵盖可用于监控平台性能和审计更改的各种工具。将使用多个代码块来说明如何确定和优化性能。性能测试在处理大量数据时至关重要，因此将全面讨论性能测试的各个方面，随后讨论监控 Salesforce 组织的性能。

第 7 章“处理大数据量”将介绍关系型和非关系型数据库的概念。LDVs 随着组织生成或消耗大量数据，在 Salesforce 生态系统中变得越来越重要，本章将广泛讨论 LDV，从识别 LDV 场景到管理 LDV 组织，再到将数据集成到这些组织类型中。




我们将探讨在大量数据不一定需要导入 Salesforce，但数据仍可供用户使用的情况下的一些选项。我们将以 Big Objects 作为讨论的收尾，这是处理超大数据量的另一种方式。

_第 8 章，通用数据迁移最佳实践_ 将向您介绍数据迁移——如何评估、规划和执行数据迁移。还将讨论与数据迁移相关的注意事项和最佳实践。接近结尾时，我们将讨论可用于数据迁移的一些常用工具。我们将通过讨论数据迁移背景下 Salesforce 中可用的不同 API 来结束我们的讨论。

## 充分利用本书

本书各章中包含多个 SOQL 查询代码块，强烈建议您跟随学习并在 Salesforce 开发者版中尝试这些查询。对于可能看起来偏理论的章节，例如关于数据治理的章节，我建议您思考自己当前或过去的组织，并尝试应用章节中学到的原则，包括这些主题的实际方面，例如数据质量的指标或 KPI。

本书假设读者具备基本的 Salesforce 功能知识，因此在缺乏所需知识的情况下，某些高级主题可能难以理解。

| 在本书中涵盖的软件/硬件 | OS 要求 |
|---|---|
| Salesforce Developer Edition (https://developer.salesforce.com/signup) | Windows、macOS 或 Linux（任意） |
| Workbench (https://workbench.developerforce.com/) | Windows、macOS 或 Linux（任意） |

## 下载彩色图片

我们还提供一个包含本书中使用的截图和图表彩色图片的 PDF 文件。您可以在此处下载：https://static.packt-cdn.com/downloads/9781801073240_ColorImages.pdf。

## 使用的约定

本书中使用了多种文本约定。

正文中的代码：表示文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号。

示例如下：“将条件更改为 LastModifiedDate 将修复问题。”

代码块设置如下：

```
SELECT Id, Name, Description, LastRunDate, LastModifiedBy.Name
FROM Report
WHERE LastRunDate < 2019-06-01T00:00:00Z
ORDER BY LastRunDate DESC
LIMIT 50
```

当我们希望引起您对代码块特定部分的注意时，相关行或项目将以粗体显示：

```
SELECT Id, Name, Description, LastRunDate, LastModifiedBy.Name
FROM Report
WHERE LastRunDate < 2019-06-01T00:00:00Z
ORDER BY LastRunDate DESC
LIMIT 50
```

任何命令行输入或输出均按以下方式编写：

```
ping -f -n 25 -l 1200 na111.salesforce.com
ping -f -n 25 -l 1300 na111.salesforce.com
ping -n 25 -l 1400 na111.salesforce.com
```

**粗体：** 表示新术语、重要单词或屏幕上看到的单词。例如，菜单或对话框中的单词以 **粗体** 显示。示例如下：“在 **开发者控制台** 中，单击 **日志** 选项卡。”

> 提示或重要说明
> 显示如下。

## 联系我们

读者反馈始终受欢迎。

**一般反馈：** 如果您对本书的任何方面有疑问，请发送电子邮件至 customercare@packtpub.com，并在消息主题中注明书名。

**勘误：** 尽管我们已尽力确保内容的准确性，但错误难免发生。如果您在本书中发现错误，我们将不胜感激如果您能向我们报告。请访问 www.packtpub.com/support/errata 并填写表格。

**盗版：** 如果您在互联网上以任何形式发现我们作品的非法副本，我们将不胜感激如果您能向我们提供位置地址或网站名称。请通过 copyright@packt.com 联系我们，并附上材料链接。

**如果您有兴趣成为作者：** 如果您有擅长的主题并有兴趣撰写或参与编写书籍，请访问 authors.packtpub.com。

## 分享您的想法

读完《Salesforce 数据架构与管理》后，我们非常乐意听取您的想法！请 https://packt.link/r/1801073244 针对本书并分享您的反馈。

您的评论对我们和技术社区非常重要，并将帮助我们确保提供优质内容。
