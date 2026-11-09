
考虑这样一种场景：客户的完整“联系历史”分布在多个系统中。Dynamics CRM 2011 联系中心可能仅服务于特定受众，而公司内部的其他系统则保存着有关公司客户的额外详细信息。一种设计选择可能是按计划间隔将这些信息批量加载到 Dynamics CRM 2011 中。然而，更有效的方法可能是调用 BizTalk Server 服务，该服务汇总跨系统的數據并返回客户与公司历史的综合视图。

类似地，思考一下系统之间如何共享信息。公司的公共网站可能包含一个注册页面，访客可以在此注册以获取更多信息和更深入的内容访问权限。该注册事件与公司内部的多个系统相关。我们可以将初始注册消息发送到 BizTalk Server，然后将该消息广播给希望了解该客户的多个系统。营销应用程序可能希望通过个性化电子邮件回应，欢迎该人访问网站。销售团队可能会决定跟进该人，如果他们表示对购买产品感兴趣。我们的 Dynamics CRM 2011 客户服务中心可以选择自动添加注册事件，以便在该客户来电时随时准备就绪。在这种情况下，BizTalk Server 充当数据的中央路由器，并调用已公开的 Dynamics CRM 服务来创建客户和交易。

在本章剩余部分，我们将了解如何查找驻留在 Dynamics CRM 中的数据、如何将新数据推送到 Dynamics CRM，以及如何将数据从 Dynamics CRM 推送出来。

## Communicating from BizTalk Server to Dynamics CRM

从 BizTalk Server 向 Dynamics CRM 2011 发送请求的方式在本版本中发生了显著变化。在之前的 Dynamics CRM 版本中，可用于与平台通信的 BizTalk“发送”适配器是可用的。Dynamics CRM 2011 不再随附适配器，建议开发人员使用产品公开的 WCF 端点。

Dynamics CRM 具有 WCF REST 和 SOAP 端点。REST 端点只能在 CRM 应用程序内部使用。例如，您可以构建所谓的 Web 资源，该资源嵌入在 Dynamics CRM 页面中。该资源可以是 Microsoft Silverlight 或 HTML 页面，从三个不同的 Dynamics CRM 实体中查找数据并在页面上汇总它们。此 Web 资源可以与 Dynamics CRM REST API 通信，后者对 JavaScript 客户端友好。不幸的是，您无法从 Dynamics CRM 环境外部使用 REST 端点，但由于 BizTalk 无法与 REST 服务通信，这对 BizTalk 集成影响不大。

与 ASMX Web 服务前身不同，Dynamics CRM SOAP API 是静态的，并使用通用的 `Entity` 数据结构运行。不是拥有一个动态 WSDL 来公开系统中所有标准和自定义实体的类型定义，Dynamics CRM 2011 SOAP API 拥有一组操作（例如 Create、Retrieve），它们使用单一对象类型运行。`Entity` 对象具有一个属性，用于标识其表示的具体对象（例如 Account 或 Contract），以及一个名称/值对集合，表示其表示的对象中的列和值。例如，一个 Entity 的 `LogicalName` 可能设置为 "Account"，并且具有 "telephone1"、"emailaddress" 和 "websiteurl" 的列。

本质上，这意味着从 BizTalk Server 与 Dynamics CRM 2011 交互时，我们有两个选择。我们的第一个选项是直接使用和调用未类型化的 SOAP API。执行此操作涉及从规范模式创建到无类型 `Entity` 模式的映射。在执行 Retrieve 操作的情况下，我们可能还必须将无类型 Entity 消息映射回结构化消息以进行进一步处理。下面，我们将通过一个示例来说明这一点。

![](img/23d1402649b6528b085cd6b900a2c9c3_130_0.png)

第二个选项涉及为 BizTalk Server 调用创建类型化代理服务。Dynamics CRM 拥有功能丰富的解决方案开发工具包 (SDK)，允许我们创建类型化对象并将其发送到 Dynamics CRM SOAP 端点。该代理服务随后将向 BizTalk 公开类型化接口，该接口使用强类型模式按预期运行。即将进行的练习演示了此场景。

![](img/23d1402649b6528b085cd6b900a2c9c3_131_0.png)

哪个选择最好？对于简单的解决方案，直接与 Dynamics CRM 2011 SOAP API 交互可能是可以的。如果您正在更新实体上的几个字段，或检索一对数据值，则无类型模式的混乱程度值得采用直接的解决方案。然而，如果您对实体进行大规模更改，或获取整个实体并发布到 BizTalk 总线以供更多订阅者接收，那么严格使用类型化代理服务是最好的选择。然而，我们将在下面查看两种场景，您可以自行做出选择。

## Integrating Directly with the Dynamics CRM 2011 SOAP API

在以下一系列步骤中，我们将了解如何在 BizTalk Server 中使用本地 Dynamics CRM SOAP 接口。我们首先将了解如何查询 Dynamics CRM 以返回 Entity。之后，我们将了解在 Dynamics CRM 中创建新 Entity 的步骤。

### Querying Dynamics CRM from BizTalk Server

在此场景中，BizTalk Server 将请求特定 Dynamics CRM “contact” 记录的详细信息，并将查询结果发送到另一个系统。

### Building the BizTalk components

在这第一组步骤中，我们定义规范 BizTalk 模式，添加 Dynamics CRM 模式，创建格式之间的映射，并构建将所有部分连接起来的编排：

- 1. 转到本书的代码包并导航到 `C:\LOBIntegration\Chapter3-DynamicsCrm`，并打开名为 `Chapter3-DynamicsCrm.sln` 的 Visual Studio 2010 解决方案文件。
- 2. 在 Visual Studio 2010 中，创建一个名为 **Chapter3-DynamicsCRM.AccountInt** 的新 BizTalk 项目。此项目将保存此解决方案的所有 BizTalk 工件。右键单击项目，选择 **Properties**，并设置强名称密钥，并将部署应用程序设置为 **Chapter3**。
- 3. 右键单击 BizTalk 项目并选择添加新项。选择 (XML) 模式项类型并将模式命名为 `Customer_XML.xsd`。
- 4. 此模式反映了我们组织对客户的内部定义。对 Dynamics CRM 的查询结果应映射到此结构。我们的客户由标识符、名称、地址集、电话号码集和电子邮件地址定义。以下图像反映了模式的结构：

![](img/23d1402649b6528b085cd6b900a2c9c3_132_0.png)

- 5. 添加对 Dynamics CRM SOAP 端点的引用，以便生成我们需要用于使用服务的工件。Dynamics CRM 2011 实例的 WCF SOAP 端点形式为：`http://<server name>/<instance name>/XRMServices/2011/Organization.svc`
- 6. 右键单击 BizTalk 项目并选择 **Add** 然后 **Add Generated Items**。选择 **Consume WCF Service**。在 BizTalk WCF 服务使用向导的 **Metadata source** 页面上，选择元数据的来源为 **Metadata Exchange (MEX) endpoint**。在向导的 **Metadata Endpoint** 页面上，输入您的 Dynamics CRM SOAP 端点的 URL。单击 **Metadata Address (URL)** 旁边的 **Get** 按钮后，移动到向导的下一页并单击 **Import**。
- 7. 向导向 BizTalk 项目添加大量文件。这包括两个绑定文件、一个编排（包括消息和编排端口定义），以及七个表示此服务使用的数据类型的模式。
- 8. 不幸的是，为此端点生成的模式被认为无效。它们彼此之间没有正确的链接，并且在打开其中一个时会出现错误。您可以手动更正每个模式，或者，更好的是，利用可以从 Microsoft 网站下载的 Dynamics CRM SDK 中包含的有效 BizTalk 模式。
- 9. 删除服务引用添加的七个 XSD 模式。
- 10. 右键单击 BizTalk 项目并选择 **Add** 然后 **Existing Item**。导航到您已解压 Dynamics CRM SDK 包的位置，找到 `sdk\schemas\crmbiztalkintegration` 文件夹并选择七个 "organizationservice" 模式，然后单击 **Add**。

![](img/23d1402649b6528b085cd6b900a2c9c3_133_0.png)
