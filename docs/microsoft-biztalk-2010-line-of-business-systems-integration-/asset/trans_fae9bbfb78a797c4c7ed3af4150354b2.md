
在本章的首次演示中，BizTalk Server 将接收一条消息，然后附加从 Salesforce.com 检索到的一些额外数据元素。在此场景中，我们的一位客户正在申请之前购买商品的报销，我们希望查找该客户的联系人详情，而这些详情恰好存储在 Salesforce.com 中。

## 配置基础 BizTalk 组件

我们的解决方案除了包含需要访问 Salesforce.com 数据的业务流程的中央编排之外，还需要一些核心架构：

- 1. 打开 `Chapter10-SFDC.sln` 解决方案并找到四个预建项目。第一个是将在后续练习中使用的 WCF 消息检查器行为。第二个项目是一个保存对 Salesforce.com 会话令牌引用的组件。下一个项目是一个简单的测试应用程序，用于验证我们的 Salesforce.com 凭据。最后一个是一个功能完整的 BizTalk 项目，将在最后一次练习中使用。
- 2. 创建一个名为 `Chapter10-SFDC.AccountProcessing.ReimbApproval` 的新 BizTalk Server 项目。设置项目的强名称密钥，并将部署应用程序值设置为 **Chapter10**。
- 3. 添加一个名为 `ReimbursementRequest_XML.xsd` 的新架构，并将目标命名空间设置为 **http://Chapter10-SFDC**。
- 4. 按以下屏幕截图所示构建架构。此架构包含有关报销请求和客户联系信息的详细信息。

![](img/23d1402649b6528b085cd6b900a2c9c3_481_0.png)

- 5. 向项目添加名为 `ApprovalProcess.odx` 的编排。此编排驱动报销的长时间运行的审批流程。
- 6. 定义两个编排消息，表示初始请求和 Salesforce.com 增强的审查消息。第一个消息名为 **ReimburseInput**，第二个为 **ReimburseOutput**，两者类型均为 **Chapter10-SFDC.ReimbApproval.ReimbursementRequest_XML**。
- 7. 草拟一个编排流程，首先接收一条消息，然后为与 Salesforce.com 的待处理交互设置一个 **Group** 占位符，发送增强的响应消息以供审批，最后分发结果。

![](img/23d1402649b6528b085cd6b900a2c9c3_482_0.png)

## 自定义 Salesforce.com 数据实体

Salesforce.com 中的对象可以自定义，我们需要在 Account 实体上添加一些新字段。

- 1. 登录到您的 Salesforce.com 账户 (https://login.salesforce.com)，或创建一个新的免费账户。请注意，Salesforce.com 每季度发布一次，因此本章中所示内容不可避免地会出现细微的用户界面或功能差异。
- 2. 我们的大部分时间将花费在配置 Salesforce.com 上，这需要我们访问强大的设置屏幕。您可以通过在屏幕右上角姓名下方找到下拉菜单来访问该网站部分。
- 3. 在 App Setup 部分，我们的一个选项是扩展 Salesforce.com 原生提供的标准对象。在我们的案例中，我们希望为 Account 实体添加一些额外字段。展开 Customize 菜单，然后选择 Accounts 以找到 Fields 选项。

![](img/23d1402649b6528b085cd6b900a2c9c3_483_0.png)

![](img/23d1402649b6528b085cd6b900a2c9c3_483_1.png)

![](img/23d1402649b6528b085cd6b900a2c9c3_483_2.png)

- 4. 在下一个屏幕上，我们可以看到所选实体的标准字段和自定义字段。我们的报销查找流程需要在 Account 实体上有一个“主要联系人”字段和一个“主要联系人电话号码”字段。页面底部有一个 **Account Custom Fields and Relationships** 部分。单击 **New** 按钮开始字段创建向导。
- 5. 将此新字段的数据类型设置为 **Text** 值。我们也可以在这里考虑使用查找值，并指定一个特定的联系记录作为账户的主要联系人。
- 6. 将此新字段的名称设置为 **PrimaryContactName**，并将其长度定义为 100 个字符。

![](img/23d1402649b6528b085cd6b900a2c9c3_484_0.png)

- 7. 在随后的两个向导页面上保留默认的安全性和页面布局选项，并在最后一页单击 **Save & New**。这将创建新字段并重新启动向导。
- 8. 对于此新字段的数据类型，选择 **Phone**，并将字段命名为 **PrimaryContactPhone**。
- 9. 将剩余选项保留为默认选择并保存新字段定义。
- 10. 如果一切成功，您应该在实体定义上看到自定义字段（及其底层 API 名称）。请注意，Salesforce.com 上的所有自定义字段都有“__c”后缀，这使得它们易于识别。

![](img/23d1402649b6528b085cd6b900a2c9c3_485_0.png)

- 11. 回想一下，Salesforce.com 提供了 Enterprise WSDL 定义，描述了我们可以从本地软件系统交互的操作和实体。请注意，WSDL 中同时提供自定义对象以及修改后的标准对象。WSDL 是动态生成的，因此它始终反映我的 Salesforce.com 对象模型的当前状态。
- 12. 导航到 App Setup 部分，然后是 Develop，然后是 API 以找到指向 Enterprise WSDL 的链接。

![](img/23d1402649b6528b085cd6b900a2c9c3_485_1.png)

- 13. 单击 Generate Enterprise WSDL 链接并将生成的 WSDL 文件以名称 SFDC_EnterpriseWSDL.wsdl 保存到您的机器上。
- 14. 在 Visual Studio 2010 中打开 WSDL 文件，找到 Account 对象并注意我们的两个自定义字段存在于该对象的定义中。

```
<element name="PartnersFrom" nillable="true" ...>
<element name="PartnersTo" nillable="true" minOccur ...>
<element name="Phone" nillable="true" minOccur ...>
<element name="PrimaryContactName__c" nillable ...>
<element name="PrimaryContactPhone__c" nillable ...>
<element name="ProcessInstances" nillable="tr ...>
<element name="ProcessSteps" nillable="true" ...>
```

## 在 BizTalk Server 中使用 Salesforce.com WSDL

Salesforce.com Enterprise WSDL 包含我们需要提取 Account 数据的所有消息和函数定义。在此步骤中，我们将这些定义导入 BizTalk Server：

- 1. 在撰写本文时下载的 WSDL 文件大小为 455KB，包含所有 Salesforce.com 对象和操作定义。如果您只需要单个对象或操作，您可以选择手动从 WSDL 中提取所需的架构并手工构建 BizTalk 消息端点。对于此场景，我们将引用整个 WSDL，并让 BizTalk 为我们生成架构、编排端口和绑定文件。如果您可能在多个项目中重用这些 Salesforce.com 工件，那么创建单独的 BizTalk 项目来包含它们是有意义的。话虽如此，我们将继续在主要的 BizTalk Server 项目中添加对 Salesforce.com WSDL 的引用。
- 2. 右键单击我们的 BizTalk 项目并选择 **Add Generated Items**，然后选择 **Consume WCF Service**。
- 3. 一旦向导弹出，选择服务元数据的来源为 **Metadata files**，因为我们有一个物理 WSDL 文件需要使用。

![](img/23d1402649b6528b085cd6b900a2c9c3_486_0.png)

- 4. 当提示添加元数据文件时，浏览到您机器上名为 `SFDC_EnterpriseWSDL.wsdl` 的 WSDL。
- 5. 保留建议的命名空间并完成向导。
- 6. 向导会创建一系列文件：
  - `SforceService.BindingInfo.xml` 文件包含创建 **WCF-BasicHttp** 发送端口所需的信息，该端口使用 Salesforce.com 服务。
  - `SforceService.odx` 文件是一个编排，其中包含一个名为 **Soap** 的单个 **Port Type** 定义，其中包含您可以在 Salesforce.com WSDL 上执行的所有操作。此编排还为每个与端口类型操作对应的输入和输出提供了多部分消息定义。
  - `SforceService_Custom.BindingInfo.xml` 文件是 **WCF-Custom** 适配器绑定，为我们提供了进一步自定义对 Salesforce.com Web 服务调用的机会。
  - `SforceService_enterprise_soap_sforce_com.xsd` 架构描述了 Salesforce.com 操作使用的每个根消息类型（例如 `login`、`sendEmail`、`update`、`queryAll`）。底层消息类型位于不同的架构中。
  - `SforceService_fault_enterprise_soap_sforce_com.xsd` 架构定义了 Salesforce.com 服务返回的所有可能故障类型。
  - `SforceService_sobject_enterprise_soap_sforce_com.xsd` 架构保存了 Salesforce.com 域中可用的所有业务实体（例如 **Account**、**Campaign**、**Case History**）。

![](img/23d1402649b6528b085cd6b900a2c9c3_487_0.png)
