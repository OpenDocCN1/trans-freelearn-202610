
每个需求的行项目，其中包含*功能考虑*和*技术考虑*列，用于记录此类项目。

## Application Architect 领域中的其他 Salesforce 认证

如您所知，每个 CTA 领域都有多项 Salesforce 认证。具体到 Application Architect 领域，其包含以下认证：

![](img/7cca8452e237cf62a2d0e00a080d3fe9_25_0.png)

图 1.2 – Application Architect 领域

您可能已经拥有 Application Architect 领域中的部分或全部其他认证。对于那些可能还没有的人，我强烈建议您在成为 Salesforce Data Architect 的旅程中，获取 Platform App Builder 和 Platform Developer 1 认证。这两项考试都包含与平台数据相关的基础问题，可以成为获得 Salesforce Certified Data Architect 认证的另一个*推动因素*。

Sharing and Visibility Designer 认证与 Data Architect 认证会有一些重叠，因为 Salesforce 平台上的数据和共享相对紧密相连。完成此认证后，我建议您继续学习 Sharing and Visibility Designer 认证，因为本书中将介绍的一些概念与该认证相关。您可以看到 Application Architect 认证之间的关系，以及如果您尚未获得 Platform Developer 1 和 Platform App Builder 认证中介绍的基础知识，那么获得这些基础知识是有用的（尽管这些并非参加 Data Architect 考试的严格先决条件）。根据上图，完成相应考试后会自动获得 Application Architect 领域认证。

既然我们了解了认证和考试的形式（包括一些有助于加强学习的技巧和习惯），让我们来分析一下 Salesforce Data Architect 的工作职责。

## Salesforce Data Architect 的简介

根据**Salesforce Certified Data Architect 考试指南**（位于 https://trailhead.salesforce.com/help?article=Salesforce-Certified-Data-Architecture-and-Management-Designer-Exam-Guide），Salesforce Data Architect 是指具备以下能力的人：

> ...评估架构环境和需求，并在 Customer 360 平台上设计健全、可扩展且高性能的解决方案，这些解决方案与企业数据管理相关。候选人熟悉信息架构框架，涵盖主要构建模块，如数据源、集成/移动、持久化、主数据管理、元数据管理和语义协调、数据治理、安全和交付。

> 候选人还拥有评估客户在数据质量方面的需求并创建解决方案以确保高质量数据（例如，无重复、数据正确）的经验，并且可以推荐组织变革以确保适当的数据管理。候选人拥有向业务利益相关者传达解决方案和设计权衡的经验。

让我们稍微分解一下这个定义。

该定义的第一段包含很多术语，但最终，它描述了一个能够理解需求并在制定解决方案或设计时应用数据架构原则和经验的人。对我来说，这是我们日常工作的一部分：理解需求和设计解决方案。其中还提到了性能、数据管理、安全性、可扩展性和治理。第二段侧重于数据质量和管理。所有这些方面都将在本书中涵盖。

当我们关注 Data Architect 的职责时，可以发现许多与谦逊的 Salesforce 管理员、顾问或架构师的相似之处——我们倾听利益相关者的需求，并设计满足这些要求的解决方案，同时始终注意在此过程中可能出现的考虑因素和权衡取舍。

Data Architect 运用其对 Salesforce Customer 360 平台数据架构的知识，包括其功能及其在设计或解决方案时对系统的影响，例如以下方面：

- 基于对象之间关系选择的共享和安全影响
- 大数据量的考虑和缓解措施
- 知道在何处使用哪种类型的对象（标准、自定义、大对象或外部对象）
- 索引
- 性能
- 数据管理和治理
- 从平台加载和提取数据，以及可用于影响这些操作性能的方法

在稍微分解了 Salesforce Data Architect 的职责定义之后，我们可以将注意力转向本书将要介绍的学习旅程。

## 学习旅程简介

在本书中，认证学习旅程将分为考试的理论部分和设计者部分。理论部分将与考试指南中描述的课程紧密对应，而*设计者*部分则专门学习理论中引用的实际概念——例如，了解 Person Accounts 及其如何影响数据模型和共享。

学习旅程从考试背后的理论概念开始：

- 数据建模和数据库设计：
  - 理解 Salesforce 数据模型的架构方式、其背后的设计决策，以及该模型如何影响共享和安全性。
  - 理解不同类型的对象，以及它们如何融入数据模型设计策略。
  - 理解数据倾斜，这将帮助您理解 Salesforce 中的这一概念。
- 主数据管理：
  - 理解如何在 Salesforce 平台上有效设计和实施**主数据管理（MDM）**策略。
  - 能够阐述*黄金记录*的概念，跨多个数据源保持数据可追溯性，并理解这如何影响业务规则运行的上下文。
- Salesforce 数据管理：
  - Salesforce 许可证类型如何影响数据模型和可用的共享选项。理解这些以及如何治理平台上的数据，对于设计可扩展、高性能的解决方案（这些方案组合来自一个或多个 Salesforce 实例的数据）至关重要。
- **数据治理：**
  - 能够保护 Salesforce 平台上的数据是 Salesforce Data Architect 角色的关键部分。本章涵盖构建合规解决方案背后的概念，以及 Salesforce 平台如何支撑企业数据治理策略。
- **大数据量：**
  - 虽然 Salesforce 平台可以处理大量数据，但对于更大/海量的数据（称为*大数据量*）以及平台性能如何受到影响，需要考虑一些因素。
  - 本章涵盖 LDV 的考虑因素和缓解措施，以及可扩展的数据模型设计和数据归档策略。
- **数据迁移：**
  - 如何将高质量数据导入 Salesforce 平台？
  - 如何确保大量数据的加载顺畅、可靠、可重复且及时？
  - 如何有效地从 Salesforce 平台导出数据？

接下来，我们将了解设计者概念，这些概念应用实践知识和实践来完善理论：

- **账户和联系人：**
  - 我们将介绍 Account/Contact 模型，包括 Person Accounts、Contacts to Multiple Accounts、账户倾斜，以及这对查询和报告性能的影响。
- **数据 API 和 Apex：**
  - 我们将了解各种 Lightning Platform API 和 Apex 操作（包括数据库、批处理和异步操作），以及如何在 Salesforce Customer 360 平台上设计数据操作时有效部署它们。
- **性能调优：**
  - 我们将了解在 Salesforce Customer 360 平台上设计 Salesforce 应用时可用的各种性能调优方法。
- **备份和恢复：**
  - 我们将了解可用于从 Salesforce 平台导出数据和将数据导回的方法，重点在于数据备份和恢复。当您在设计作为整体 Salesforce IT 环境一部分的 ETL 和其他解决方案时，这非常有用。
