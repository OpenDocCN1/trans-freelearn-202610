
+   - 区域管理：
+     - 我们将了解区域管理，以及它如何与 Salesforce Lightning 平台上的数据模型及其使用设计相关联。

最后，本书有一节专门讨论考试成功，重点关注以下内容：

+   - 练习题和答案
+   - 速查表
+   - 介绍本书中提到的某些概念和主题的进一步阅读资料
+   - Webassessor 概述，包括如何注册和参加考试

## 总结

在本章中，我们了解了 Salesforce 认证数据架构师认证为何存在、考试形式以及 Salesforce 数据架构师的概况，从而真正理解该考试背后的概念为何对您的日常工作有用。最后，我们介绍了本书的学习路径，为本书其余部分定下基调。

在*第2章“数据建模与数据库设计”*中，我们将开始学习之旅，重新审视 Salesforce 数据模型、不同对象如何影响它，并涉及数据倾斜和所有权倾斜等概念。我们将介绍如何在适当考虑共享、记录数和性能的情况下有效描述 Salesforce 数据模型。

## 练习题

回答以下问题以测试您对本章的掌握情况：

+   1. 什么是 Salesforce 认证数据架构师认证？
+   2. 为什么该认证存在？
+   3. Salesforce 数据架构师的典型概况是什么？

## 进一步阅读

*阅读 Salesforce 官方考试指南：* https://trailhead.salesforce.com/help?article=Salesforce-Certified-Data-Architecture-and-Management-Designer-Exam-Guide.

## 数据建模与数据库设计

在本章中，我们将开始探索认证大纲背后的理论，从数据建模和数据库设计开始。您将了解 Salesforce 数据模型是如何架构的、其背后的设计决策，以及您作为 Salesforce 数据架构师在工作中创建的数据模型如何影响共享和安全性。我们将介绍 Salesforce 共享模型，以便您在考虑数据访问选项时对可用内容有一个概览。

接下来，我们将研究不同类型的对象（例如标准对象、自定义对象、外部对象和大对象），以及它们如何融入数据模型设计策略。同时，我们还将介绍不同的关系类型，以便您了解何时应用它们。

转向性能方面，我们将介绍数据倾斜，让您理解 Salesforce 中的这一概念。然后，我们将介绍图表绘制技术，以便您能够开始有效地设计和表示 Salesforce 数据模型。

在本章中，我们将涵盖以下主题：

+   - Salesforce 数据模型
+   - 理解 Salesforce 共享与安全
+   - 探索标准对象、自定义对象、外部对象和大对象
+   - 克服数据倾斜
+   - 通过数据建模将所有内容整合在一起

## Salesforce 数据模型

Salesforce 通过为其给定客户组织中的对象提供一种伪 RDBMS 视图，抽象化了其数据库中的底层数据结构。例如，查看 Sales Cloud ERM 时看到的图表，位于 https://architect.salesforce.com/diagrams/template-gallery/sales-cloud-overview-data-model：

![](img/7cca8452e237cf62a2d0e00a080d3fe9_32_0.png)

图 2.1 – Salesforce Sales Cloud 数据模型

正如我们所见，联系人记录将有一个 Account Id，以查找关系表示（在 RDBMS 术语中相当于外键）。以 Schema Builder 中一个新 Developer Edition 组织的 Account 和 Contact 为例：

![](img/7cca8452e237cf62a2d0e00a080d3fe9_33_0.png)

图 2.2 – 全新 Developer Edition 组织中 Schema Builder 中表示的 Account 和 Contact

### 自己动手尝试

您可以随时在 https://developer.salesforce.com/signup 注册（完全免费的）Developer Edition Salesforce 组织，以便跟随本书中的示例进行操作，包括探索 Schema Builder。

Salesforce 会让您认为 Account 和 Contact 作为两个独立的数据库表存在（实际上，模式中的每个标准或自定义对象都有一个单独的表）。事实上，作为 Salesforce 数据架构师，在日常工作中以这种方式思考对象是完全正常的。

然而，事实是 Salesforce 有几个大型的底层数据库表（一个用于对象，一个用于字段，一个用于映射到这些对象和字段的数据，透视表等等），它们为每个组织提供了在运行时物化的虚拟数据结构。一个虚拟存储区域包含标准对象和标准字段，另一个包含标准对象和自定义字段，另一个包含自定义对象及其自定义字段。这在 https://developer.salesforce.com/wiki/multi_tenant_architecture 中有更详细的解释。本质上，标准对象/标准字段存储区域在创建新组织时填充，而自定义存储区域则在添加新自定义对象或自定义字段时添加（无论该自定义字段是添加到标准对象还是自定义对象）。因此，可以如下表示此结构，假设向标准 Account 和 Contact 对象各添加一个自定义字段：

![](img/7cca8452e237cf62a2d0e00a080d3fe9_34_0.png)

图 2.3 – Salesforce 数据库的表示（仅对象数据），其中向 Account 和 Contact 标准对象添加了自定义字段

现在让我们添加一个自定义对象，看看虚拟存储如何受影响，以及该更改在 Schema Builder 中如何显示：

![](img/7cca8452e237cf62a2d0e00a080d3fe9_35_0.png)

图 2.4 – Salesforce 数据库的表示（仅对象数据），其中向组织添加了一个自定义对象，并向该新自定义对象添加了自定义字段

*图 2.5* 在模式中包含了自定义对象和自定义字段：

![](img/7cca8452e237cf62a2d0e00a080d3fe9_35_1.png)

图 2.5 – Schema Builder，其中显示了新添加的自定义对象和自定义字段

了解了底层 Salesforce 数据模型的表示方式（这对性能调优等很重要，将在本书后面讨论）后，我们现在可以将注意力转向 Salesforce 中可用的标准对象。这些对象是为了解决常见用例而创建的（例如用 Account 表示公司，用 Contact 表示人），因此应尽可能使用。这不仅减少了重复造轮子，而且 Salesforce 还扩展了标准功能以应对特定用例，例如客户团队。客户团队扩展了 Account 的共享模型，允许在协作处理某个 Account 的团队之间进行定制共享。类似的功能也可用于 Case 和 Opportunity（分别称为**案例团队**和**机会团队**）。此团队功能是 Salesforce 数据模型支持的标准功能的一个示例，该功能并未扩展到自定义对象。

Salesforce 中可用对象及其字段的完整详尽列表由 API 文档提供（可在 https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_objects_list.htm 获取），但一些最常用的对象如下：

+   - **Account** – 代表您可能与之开展业务的公司或机构。
+   - **Contact** – 代表一个人（通常与一个或有时多个 Account 关联）。
+   - **Lead** – 代表一个潜在客户或未合格销售线索。潜在客户通常用于售前场景，目标是将潜在客户转换为 Account、Contact，以及可选的 Opportunity。
+   - **Person Account** – 一个 *分组* 的 Account/Contact 对象（主要允许将 Opportunity 关联到个人买家而不是 Account/公司）。这本质上是将 Account/Contact 关系从查找关系改为 *由父级控制*。这对共享和数据可见性有影响。
+   - **Case** – 代表需要关注的跟踪问题或工作项。通常由支持人员使用。
+   - **Opportunity** – 代表一笔业务交易。
+   - **Opportunity Line Item** – 构成 Opportunity 的一部分并用于驱动其价值的项目。

> **标准对象数据模型图**
> Salesforce 标准数据模型可在 https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/data_model.htm 获取（请记住，它们显示的是对象的逻辑表示及其关系，而不是物理数据库结构）。

现在我们已经了解了标准对象，以及支撑标准对象/标准字段、标准对象/自定义字段、自定义对象/自定义字段的三张表，我们可以开始关注数据查询如何工作，并关注这些表之间的性能。
