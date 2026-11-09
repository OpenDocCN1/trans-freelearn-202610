
![](img/23d1402649b6528b085cd6b900a2c9c3_81_0.png)

首先，我们需要在“Select contract type”控件中选择合约类型。然后，在“Available categories and operations”面板中，我们可以选择所需的操作并单击“Add”，将操作添加到“Added categories and operations”列表中。收集完所有所需操作后，我们可以单击“OK”关闭向导。此操作将返回到 Visual Studio，您将在其中看到一个或多个 xsd 架构和一个绑定文件已添加到项目中。如果需要为另一种合约类型生成架构，则必须再次进入 Consume Adapter Service Wizard 并重复该过程。

## BizTalk 管理控制台中的 ASDK SQL 适配器

部署应用程序后，并使用 BizTalk Admin Console 导入由 Consume Adapter Service Wizard 为我们生成的绑定文件后，我们可能需要为特定环境调整适配器配置。

### 入站操作

要访问入站场景的适配器属性，我们必须导航到适当的接收位置并双击它以打开 **Receive Location Properties** 窗口。然后单击 **OK** 打开 **WCF-Custom Transport Properties** 多标签对话框。

**General** 选项卡为我们提供了 Address（URI）文本控件以设置或修改连接字符串：

![](img/23d1402649b6528b085cd6b900a2c9c3_82_0.png)

**Bindings** 选项卡顾名思义，是可以访问绑定属性的地方：

![](img/23d1402649b6528b085cd6b900a2c9c3_83_0.png)

名为 Other 的选项卡用于指定访问目标数据库的凭据：

![](img/23d1402649b6528b085cd6b900a2c9c3_83_1.png)

我们可以采用以下选项来指定凭据：

- **None** – 适配器将使用 Windows 身份验证，并使用 Receive Handler 主机实例运行的帐户的身份
- **User account** – 此选项允许指定用户名和密码以使用 SQL Server 身份验证模式连接到目标数据库
- **Get credentials from affiliate application** – 允许指定 Single Sign-On 关联应用程序以获取凭据

在大多数情况下，您无需处理 **Behavior**、**Messages** 和 **Import/Export** 选项卡提供的配置选项。如果仍有必要，请参阅 WCF-Custom 适配器文档。

### 出站操作

出站场景的配置过程与入站基本相同。您必须找到适当的发送端口，双击它以打开 **Send Port Properties** 窗口，然后单击 **Configure** 按钮以打开 **WCF-Custom Transport Properties** 窗口。**General** 选项卡为您提供对 URI 连接字符串以及 SOAP 操作的访问。

![](img/23d1402649b6528b085cd6b900a2c9c3_84_0.png)

**Credentials** 选项卡如您所猜测，允许您提供访问数据库的凭据：

![](img/23d1402649b6528b085cd6b900a2c9c3_85_0.png)

可用选项如下：

- **Do Not Use Single Sign On** – 如果 User name 和 Password 字段为空，适配器将使用 Windows 身份验证和 Send Handler 帐户身份连接到数据库。否则适配器将使用 User name 和 Password 控件中指定的凭据使用 SQL Server 身份验证。
- **Use Single Sign On** 允许选择 Single Sign On 关联应用程序来提供凭据。

与接收位置一样，**General, Bindings, and Credentials** 选项卡上显示的属性涵盖了大多数配置场景。对于复杂情况，请参阅 WCF-Custom 适配器文档。

## WCF-SQL 与 WCF-Custom 适配器

除了 WCF-Custom，代表 ASDK SQL 适配器的 sqlBinding 还可以通过随 BizTalk Server 2010 Adapter Pack 安装的专用 WCF-SQL BizTalk 适配器访问。这些适配器之间的区别在于 WCF-SQL 适配器提供了一个针对 sqlBinding 量身定制的更直观的 UI。例如，通过单击专用 WCF-SQL 适配器 General 选项卡上的 Configure，我们可以填充一个表来构建连接字符串，而使用通用的 WCF-Custom 则必须直接输入整个字符串，正如您在上一节中看到的那样。

![](img/23d1402649b6528b085cd6b900a2c9c3_86_0.png)

要在 BizTalk Admin Console 中创建 WCF-SQL 适配器，我们需要执行以下步骤：

- 1. 右键单击 Adapters，然后选择 New - Adapter.

![](img/23d1402649b6528b085cd6b900a2c9c3_86_1.png)
- 2. 单击 **Adapter** 下拉列表并选择 **WCF-SQL**。在 **Name** 文本框中输入 **WCF-SQL** 并单击 **OK** 按钮：

![](img/23d1402649b6528b085cd6b900a2c9c3_87_0.png)

重要的是要理解，无论您使用 WCF-Custom Adapter 还是 WCF-SQL 适配器，都不会丢失功能。使用 WCF-SQL 适配器简化了维护 BizTalk 环境所涉及的管理任务。由于 WCF-Custom 适配器可以利用多种不同的绑定，包括 Oracle、SQL、SAP 以及允许我们连接到云的绑定，因此显式创建专用适配器以避免混淆可能是有益的。权衡之处在于，开发时为您生成的绑定文件将无济于事，因为它们使用的是 WCF-Custom 适配器。归根结底，没有对错之分；您可以根据个人风格和偏好选择其中之一。本章我们依赖 WCF-Custom；在 SAP 章节中，我们将使用专用的 WCF-SAP 适配器，以便您能够比较方法。为了结束本次讨论，我们想为您提供一个 URL，如果您选择使用 WCF-SQL 适配器并且因此需要手动配置 SOAP 操作，您可能会觉得有用。按照位于 http://msdn.microsoft.com/en-US/library/dd788515(v=BTS.10).aspx 页面上提供的链接，您将找到 ASDK SQL 适配器支持的操作的 SOAP 操作。

## ASDK SQL 适配器示例

在本节中，我们将介绍 ASDK SQL 适配器提供的一些常用操作。我们准备了一些易于遵循的示例，重点关注适配器操作的细节，而不是构建整个 BizTalk 应用程序本身或所使用的 SQL Server 对象的细节。

对于这些示例，我们有一个场景，其中销售订单详细信息保存在 SQL Server 数据库中的几个不同表中。生成数据库工件的脚本随本章附带的示例代码一起提供。

示例中使用的三个主要表是 SalesOrderHeader、SalesOrderStatus 和 SalesOrderDetails。

SalesOrderHeader 表包含有关订单的基本信息，包括到期时间、发货时间、帐号、CustomerID、SalesPersonID 等：

![](img/23d1402649b6528b085cd6b900a2c9c3_88_0.png)

**SalesOrderStatus** 表从系统集成角度跟踪订单的状态。由于我们不希望 BizTalk 多次处理订单，因此需要管理已消耗的记录。此表允许我们通过在 **ProcessingIndicator** 字段中指定 GUID 来标记记录正在被处理。当其设置为 NULL 时，表示行需要由 BizTalk Server 处理。

**OrderStatus** 列用于标识销售订单处理的各个阶段，而 SalesOrderID 是与实际保存销售订单信息的表的链接：

![](img/23d1402649b6528b085cd6b900a2c9c3_89_0.png)

**SalesOrderDetail** 表为我们提供了有关订单的一些补充信息，包括 **UnitPrice**、**Discount**、**CarrierTracking Number** 等：

![](img/23d1402649b6528b085cd6b900a2c9c3_89_1.png)

![](img/23d1402649b6528b085cd6b900a2c9c3_90_0.png)

就设计接收方 T-SQL 代码以避免死锁而言，旧 SQL 适配器的最佳实践仍然适用于新的 ASDK SQL 适配器。主要区别在于新适配器不需要在存储过程末尾使用 FOR XML，并且它生成的架构更易于配置以进行拆批。旧 SQL 适配器的最佳实践可以在此处在线找到：http://msdn.microsoft.com/en-us/library/cc507804(BTS.10).aspx

如果适配器调用的 T-SQL 代码无法更改以遵循最佳实践，并且有多个 BizTalk Server 主机实例映射到运行轮询接收位置的主机，则可能会发生 SQL Server 死锁或检索到重复数据。在这些情况下，对主机进行集群可以确保解决方案在避免死锁的同时保持高度可用。

有关集群 BizTalk 主机的更多信息，可在此处找到：http://kentweare.blogspot.com/2009/04/clustering-biztalk-hosts.html。

另一个选项是根据您的场景设置适当的事务隔离级别。有关详细信息，请访问此链接 http://msdn.microsoft.com/en-us/library/dd788041(v=bts.10).aspx。

### 示例 1—TypedPolling 和拆批

第一个示例将演示如何使用 ASDK SQL 适配器的 **TypedPolling** 操作来轮询数据库，以查找尚未由 BizTalk Server 处理的销售订单。

**TypedPolling** 操作是当 BizTalk Server 解决方案需要轮询 SQL Server 获取数据时最常用的操作。轮询结果的架构是强类型的，可以轻松用于映射或将其元素提升以进行基于内容的路由。

我们创建了一个存储过程，该过程返回尚未由 BizTalk Server 处理的前 100 行。我们稍后将更详细地描述该存储过程。

由于我们已经有了存储过程，如本示例的数据库脚本中提供的那样，我们下一步是生成表示该过程生成的 resultset 的类型化架构。在 Consume Adapter Service Wizard 中，我们将 **sqlBinding** 设置为绑定并单击 **Configure** 以打开 **Configure Adapter** 窗口以指定 URI 和绑定属性。在 **URI Properties** 选项卡上，我们设置属性以连接到我们的示例 **AdventureWorksCh2** 数据库。注意，如果您的 SQL 服务器是本地的，您可以为 **Server** 属性指定 "localhost"：

