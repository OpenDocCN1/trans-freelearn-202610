
## 摘要

在本章中，我们讨论了使用 BizTalk Dynamics AX 2009 适配器用于 BizTalk 集成应用程序的优势。我们介绍了 Dynamics AX 2009 中 Application Integration Framework 模块所需的配置设置。

在第一个示例中，我们使用了开箱即用的 AIF 服务 **LedgerExchangeRatesService** 来创建一个货币汇率应用程序，演示了 BizTalk 适配器的同步集成模式。在第二个示例中，我们创建了一个通用解决方案来从 Dynamics 检索传出消息，这演示了异步集成（包括批处理作业）的要求。

最后，我们提到了使用 **.NET Business Connector** 与 Dynamics AX 集成，并讨论了在不利用 AIF 模块编写自定义应用程序时需要考虑的影响。在下一章中，我们将讨论与 Salesforce CRM 的集成。一种流行的在 "云" 中运行的 CRM。

## 10 集成 BizTalk Server 和 Salesforce.com

Software as a Service (SaaS) 是企业软件中的一种趋势，不可忽视也不应忽视。合法的 SaaS 方案提供了一个高度可扩展、可靠且可配置的环境，该环境托管在组织之外。与传统的 Application Service Providers (ASP) 不同，SaaS 提供商利用多租户、元数据驱动的架构，通常为所有用户运行单一实例和软件版本。这意味着客户没有前期基础设施成本，可以在没有重大“升级项目”的情况下看到定期软件更新，并可确保应用程序性能的强服务水平协议。

虽然我们可能会为特定解决方案采用 Salesforce.com 等 SaaS 产品，但我们对现有的托管关键信息和算法的本地系统该怎么办？如何在 SaaS 环境和本地源之间共享数据？我们将在本章中回答这个问题。

在本章中，我们将介绍：
- Salesforce.com 是什么以及为什么我们可能希望将其与 BizTalk Server 连接
- 如何使用 BizTalk Server 从 Salesforce.com 拉取数据
- 从 Salesforce.com 向 BizTalk Server 发送数据和查询请求的工具和技术

### 什么是 Salesforce.com？

Salesforce.com 是一家云计算公司，销售企业软件解决方案的订阅。它是标准客户关系管理（CRM）重点领域（如销售、营销和客户服务）的可行选择。您会找到传统的 CRM 功能，如账户管理、客户跟踪、机会管理、销售历史、案例管理等等。Salesforce.com 还具有一些创新能力，例如 Chatter，这是一种类似 Facebook 的协作工具，用于在组织内共享信息。

Salesforce.com 本身就是一个出色的产品，但对于技术人员来说，真正的兴奋点应保留给其底层 Force.com 平台。Force.com 是 Salesforce.com 构建的 Platform as a Service (PaaS) 软件。更重要的是，Salesforce.com 已将 Force.com 平台开放给外部开发人员，以构建和扩展基于云的应用程序。

Force.com 面向的是（结构化）数据驱动的应用程序，这些应用程序可以从云托管的基础平台服务集中受益。开发人员无需花费时间和精力构建托管基础设施、用户界面框架、Web 服务层、安全框架和报表工具，而是可以使用 Force.com 的这些内置功能快速部署丰富、可靠且安全的系统。

当我们在 Force.com 应用程序中编写代码时，我们使用一种名为 Apex 的 Java/C# 类似语言。它是 Force.com 应用程序专用语言，但您会发现它相当容易上手和理解。新的 Web 页面使用 Visualforce 技术构建，该技术利用标准 HTML、JavaScript 和 Apex 特定标签。Force.com 应用程序中的每个对象都可以通过 SOAP 或 REST 服务接口访问。这为将 Salesforce.com 数据与存储在其他地方的数据集成打开了大量场景。

### 为什么要集成 Salesforce.com 和 BizTalk Server？

BizTalk Server 是一款世界级的集成服务器，擅长连接不同的应用程序、转换文档结构、桥接通信协议以及执行长时间运行的事务性工作流。超过一万家组织依靠 BizTalk Server 来分发数据并在系统之间协调交互。Salesforce.com 等 SaaS 解决方案只是组织企业架构的另一个组件，可被视为企业数据的第一类接收方和来源。

像任何企业软件一样，Salesforce.com 将成为某些信息的孤岛。然而，其数据可能需要其他业务领域成功运营。对于实例，当医生致电服务中心询问产品问题时，他们可能拥有完全不同部门可能感兴趣的医学专业。该客户记录可以从 Salesforce.com 中提取并与组织中的其他系统共享。同样，公司可能拥有本地数据聚合服务，其中保存的信息对 Salesforce.com 应用程序有用。

当我们将组织的集成服务器暴露给 Salesforce.com 时，这意味着我们可以帮助防止形成新的数据孤岛，同时仍能利用创新且可扩展的云软件。

### 从 BizTalk Server 与 Salesforce.com 通信

BizTalk Server 如何向 Salesforce.com 或 Force.com 应用程序发出请求？我们有几个可行的选择：第三方适配器或调用原生 Web 服务 API。

供应商构建了许多适配器来加速与 Salesforce.com 的集成。通过使用适配器，开发人员无需学习底层 API，也可避免构建扩展函数来解决 API 缺陷。其中一个适配器是 TwoConnect BizTalk Adapter for Salesforce.com，它支持推送和拉取交互模式。此类适配器的有价值功能之一是它可以在 BizTalk Server 接收位置轮询 Salesforce.com。使用原生 BizTalk 适配器无法轻松可靠地轮询 Web 服务端点。

尽管适配器可以加快开发并在底层系统 API 上引入有用的包装功能，但企业系统上可用 Web 服务接口的日益普及意味着直接集成比以往任何时候都简单。消费 Web 服务是大多数开发人员工具箱的标准部分。本章剩余部分，我们将重点介绍在与 BizTalk Server 集成时使用原生的 Salesforce.com API。

BizTalk Server 获取 Salesforce.com 数据的最直接方法是使用查询策略。当 BizTalk 需要来自 Salesforce.com 的信息时，使用一套标准可用服务操作执行查询。本章后面我们将介绍一种从 Salesforce.com 向 BizTalk 推送数据的有效方法，但在许多情况下，轮询或查询是更理想的模式。当一个系统向另一个系统推送数据时，目标系统无法控制数据的传输时间或数量。如果源系统中发生重大数据更改，则下游系统将被迫处理大量意外的负载。相反，当我们使用轮询或查询策略时，我们可以控制请求信息的时间，并且在 Salesforce.com 的情况下，规定单次请求返回的消息数量。

正如您所期望的那样，BizTalk 可以对 Salesforce.com 数据执行丰富的操作。信息可以通过记录 ID 或时间戳检索，并使用 Salesforce Object Query Language (SOQL)，其看起来像 T-SQL 查询，或 Salesforce Object Search Language 进行查询，后者对实体中的字段进行文本搜索。Salesforce.com 服务 API 还公开了广泛的“写入”操作，允许我们创建记录、更新记录、upsert 记录（创建，或如果存在则更新它）、删除记录，甚至执行批量修改。

在某些情况下，开箱即用的 API 并不能完全满足您的需求。例如，标准 API 不支持多对象事务。还有 API 限制，在特别频繁的交互中可能会达到。幸运的是，我们可以定义自定义服务，这些服务可能会做诸如捆绑相关操作或返回整个实体的特定子集之类的事情。与标准 Salesforce.com 服务一样，自定义服务具有易于消费的 WSDL 定义。

Salesforce.com 中的 Enterprise WSDL 定义了您特定 Salesforce.com 账户中的所有标准、扩展和自定义对象。WSDL 始终是对象状态的最新视图，这意味着如果您向 Customer 对象添加新字段，该字段会立即在 WSDL 中可用。如果您定义自定义服务，则该特定服务可用唯一的 WSDL。

Salesforce.com 宇宙中最重要的服务操作之一称为 `login`。每次对 Salesforce.com 业务操作的调用都需要在 HTTP 头中包含会话令牌。为了获取令牌，调用方将其 Salesforce.com 用户凭据以及账户特定的安全令牌发送到 `login` 函数。如果凭据有效，则该函数返回会话令牌和服务 URL。会话令牌具有可配置的生命周期，但每次调用服务操作时超时计数器都会重置。作为最佳实践，您应创建一个专用于集成交互的 Salesforce.com 用户账户，并在为 Salesforce.com 服务调用获取会话令牌时使用该账户。
