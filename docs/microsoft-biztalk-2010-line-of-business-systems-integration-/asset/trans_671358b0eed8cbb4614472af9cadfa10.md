
![](img/23d1402649b6528b085cd6b900a2c9c3_119_0.png)

为了构建复合操作，我们需要有一个包含多个根节点的单一架构。在第一个根节点下，我们添加所有操作执行请求架构，在另一个根节点下我们添加所有操作响应架构。根节点的唯一命名约定规则是，操作响应根节点必须与操作根节点相同并后缀为 'Response'。对于此示例，我们创建了一个名为 **CompositeSchema.xsd** 的架构，并创建了两个根节点：**Request** 和 **RequestResponse**。

一旦我们有了根节点，我们就可以使用 BizTalk Server 架构的 Import 属性将两个更新架构导入到复合架构中：

![](img/23d1402649b6528b085cd6b900a2c9c3_120_0.png)

在导入架构后，我们需要确保所有父节点都指定了正确的 Data Structure Type。如以下图像所示，我们在 Request 根节点下有两个 Insert 节点。在这些 Insert 节点中的每一个中，我们为 SpecialOffer 请求和 SpecialOfferProduct 请求提供 Data Structure 类型。同样，我们对 RequestResponse 节点也做同样的操作：

![](img/23d1402649b6528b085cd6b900a2c9c3_120_1.png)

下面是我们的映射，它将演示如何从单个请求中获取数据并将其拆分到复合架构中的多个请求节点中。当此映射通过 BizTalk 运行后，我们将以单个事务的形式将数据插入到 **SpecialOffer** 和 **SpecialOfferProduct** 表中：

![](img/23d1402649b6528b085cd6b900a2c9c3_121_0.png)

在我们为复合操作创建的 solicit response 发送端口上，我们需要使用一个名为 **CompositeOperation** 的特殊 Action 名称：

![](img/23d1402649b6528b085cd6b900a2c9c3_122_0.png)

另一个需要注意的适配器属性位于 **Binding** 选项卡上，名为 **useAmbientTransaction**，应设置为 **True**。结合在 SOAP Action 头中提供 CompositeOperation，此属性指示适配器将执行多个操作，并且这些操作必须作为单个事务执行。

![](img/23d1402649b6528b085cd6b900a2c9c3_123_0.png)

通过对两个表更新使用复合操作，我们具有以下好处：

- 对两个操作使用相同的事务。这意味着如果出现异常，两者都会回滚。这对我们的解决方案很重要，因为我们希望两个更新一起执行，如果其中一个失败，两者都会回滚。
- 使用相同的数据库连接。适配器将为复合操作中的所有更新、插入和删除语句使用来自连接池的相同连接，从而最大限度地降低连接池耗尽连接的风险。

现在有了我们的复合架构和将传入请求转换为复合消息的映射，我们可以构建解决方案的其余部分：

![](img/23d1402649b6528b085cd6b900a2c9c3_124_0.png)

在部署应用程序后，我们可以通过复制 ...\Chapter02\Example4\SpecialOffer.xml 文件并将其粘贴到 Chapter02\Example4\FileDrop 文件夹来初始化此过程。

如果我们检查 SpecialOffer 和 SpecialOfferProduct 表，我们会发现我们在这两个表中都有 SpecialOfferID 为 182 的记录（在您的系统上可能会有所不同，因为 Special_Offer_To_Composite 映射中的第一个脚本化函数化为 SpecialOfferID 生成一个随机数），并且时间戳相似。这些记录作为单个事务写入此数据库。预期复合操作运行没有问题，因此 SQL Server 响应中没有传递回数据。

![](img/23d1402649b6528b085cd6b900a2c9c3_125_0.png)

## 摘要

在本章中，我们学习了如何使用 ASDK SQL 适配器将 SQL Server 和 BizTalk Server 2010 集成。

BizTalk Server 解决方案经常集成到 SQL Server，本章中的最佳实践和示例应有助于 BizTalk 开发人员在构建集成解决方案时。

在下一章中，我们将研究 BizTalk Server 和 Microsoft Dynamics CRM 的集成。

## 3

## 将 BizTalk Server 与 Microsoft Dynamics CRM 集成

Microsoft Dynamics CRM 2011 是来自 Microsoft 的最新最强大的关系管理软件。此产品版本引入了对用户界面的广泛改进、新的 WCF 编程模型、基于角色的表单、数据分析仪表板、与 SharePoint 2010 的紧密集成、Windows Azure 集成等等。任何评估小型或大型 CRM 项目的人都应考虑 Dynamics CRM 2011。但我们如何使 Dynamics CRM 成为企业环境中集成的一部分？本章重点关注这个问题。

在本章中，我们将介绍：

- 什么是 Microsoft Dynamics CRM 以及它解决了什么问题？
- 为什么要将 Dynamics CRM 与 BizTalk Server 集成
- BizTalk Server 如何向 Dynamics CRM 发出请求
- Dynamics CRM 如何向 BizTalk Server 发送数据

### 什么是 Microsoft Dynamics CRM？

客户关系管理是几乎每个企业的关键部分。Dynamics CRM 2011 为 CRM 的三个传统领域提供了解决方案：销售、营销和客户服务。

对于有兴趣管理销售团队的客户，Dynamics CRM 2011 具有强大的功能集。这包括将团队组织到区域、定义价格表、管理机会、维护组织结构、跟踪销售渠道、启用移动访问等等。

如果您使用 Dynamics CRM 2011 进行营销工作，那么您有能力从多个来源导入数据、计划活动并设置目标列表、创建大规模通信、跟踪活动响应、与销售团队共享线索并分析营销计划的成功情况。

Dynamics CRM 2011 还可作为客户服务场景的强大中心。功能包括丰富的账户管理、案例路由和管理、内置知识库、呼叫中心资源调度、称为 Dialogs 的脚本化问答工作流、合同管理等。

除了这三个领域之外，Microsoft 将 Dynamics CRM 作为通用应用程序平台 xRM 进行推广，其中“X”代表任何类型的关系管理。Dynamics CRM 具有强大的底层框架，用于屏幕设计、安全角色、数据审计、实体定义、工作流和移动性等。无需将这些基础方面构建到每个应用程序中，我们可以在 Dynamics CRM 中构建我们的数据驱动应用程序。

Microsoft 在 Dynamics CRM 2011 的此版本中做出了进入云的大举措。公司历史上首次，产品在线发布（Dynamics CRM Online）早于本地软件。应用程序的主机版本运行与本地版本相同的代码库，这意味着为本地实例构建的代码在云中也能正常工作。除了在 CRM 主机上的大举措外，Microsoft 还将 Windows Azure 集成融入了 Dynamics CRM 2011。具体来说，我们现在有能力配置对 Azure AppFabric Service Bus 端点的调用。为此，下游服务必须实现特定的 WCF 接口，并且在 CRM 中，Azure AppFabric 插件配置为通过 Azure AppFabric Service Bus 中继服务调用该下游服务。为了使 BizTalk Server 适应这种模式，我们希望构建一个代理服务，该服务实现所需的 Dynamics CRM 2011 接口并将请求转发到 BizTalk Server 端点。本章不会演示此场景，因为重点仅放在与本地实例的集成上。

### 为什么集成 Dynamics CRM 和 BizTalk Server？

将这两种技术结合在一起有许多原因。请记住，BizTalk Server 是一个企业集成总线，连接不同的应用程序。可能存在将数据囤积在特定应用程序中的自然倾向，但如果我们采用实时消息交换，我们实际上可以拥有一个更敏捷的企业。

