
## IDOC 架构生成

在完成 SAP Adapter 的安装和配置后，我们现在将重点关注 IDOC。我们将探索 IDOC 的以下功能：

- IDOC 是什么
- 如何生成 IDOC 架构并理解重要属性

### IDOC 是什么？

IDOC 是 Intermediate Document（中间文档）的缩写，是一种 SAP 规范，用于在 SAP 系统内部以及与 BizTalk 等外部系统之间交换信息。SAP 人员将 IDOC 视为类似于 BizTalk 人员思考 XSD 架构的方式。不同之处在于 IDOC 结构类似于平面文件，而 XSD 架构基于 XML。

SAP 具有一致的结构，包括包含与 IDOC 关联的元数据的 Header 段。Header 在所有 IDOC 类型中保持一致，但 IDOC 的主体将根据 IDOC 支持的功能而不同。SAP 为不同的系统功能提供特定的 IDOC 结构。HR Timesheet IDOC 看起来会与采购订单大不相同。每个 IDOC 都有特定于该 IDOC 的段。我们可以将段视为类似于 XML 节点。如果您问 SAP 人员 IDOC 是否有重复的节点，他们可能不理解您的问题。但是，如果您问 IDOC 是否有重复的段，他们会立即理解。

### 如何生成 IDOC 架构

现在我们对 IDOC 有了一些基本了解，让我们继续生成一个 IDOC 以获得更好的理解。就像我们之前为 SQL Server 生成架构一样，我们将利用 Consume Adapter Service。我们可以通过右键单击 BizTalk 项目并选择 Add - Add Generated Items...Consume Adapter Service 来启动此向导。

1. 我们现在处于熟悉的领域，因为在之前的章节中使用过此向导。这次我们想要从“选择绑定”下拉列表中选择 sapBinding。

![](img/23d1402649b6528b085cd6b900a2c9c3_193_0.png)

2. 我们现在需要配置 URI，可以通过单击 **Configure** 按钮来完成。我们需要处理的第一个区域是 **Security** 选项卡。在此选项卡中，关于 **Client credential type** 我们有几个不同的选项：

- None
- Windows
- Username
- Certificate

我大胆猜测我们永远不会遇到不需要提供凭据的情况，所以我们不会使用 **None**。本演练中使用的 SAP 系统不驻留在 **Windows** 平台上，因此该选项不可用。如果您的 SAP 系统支持 Windows 身份验证，那么使用这种类型的身份验证是值得考虑的。证明我们身份没有特定要求，因此不需要 **Certificate**。这就剩下提供标准的 **Username** 和 **Password**。通常，这些凭据由 SAP 安全管理员分配，有时由 BASIS 管理员分配。这些是用户通过 SAP GUI 访问 SAP 时提供的同类型凭据，只是具有额外的权限。

![](img/23d1402649b6528b085cd6b900a2c9c3_193_1.png)

3. 我们接下来要关注的选项卡是 **URI Properties**。我们需要填充的基础属性包括：

- Application Server Host
- System Number
- Client

**Application Server Host** 是物理 SAP 应用服务器的名称，或更可能是虚拟 DNS 别名。此值在各个 SAP 景观，即 DEVELOPMENT/TEST/PRODUCTION 中很可能唯一。

**System Number** 是 SAP 特定实例的唯一数字标识符。**Client** 在此上下文中用于在 SAP 系统内分隔数据。例如，如果您有子公司共享一个 SAP 系统，但希望数据隔离，那么您可以创建多个客户端，以便区分一家公司的数据与另一家公司的数据。如果您熟悉 Dynamics CRM，**Client** 相当于一个 Organization。此数字在各个 SAP 景观中很可能相同，但在共托管情况下您可能有多个值。这些都是您很可能从 BASIS 管理员处获得的值。

![](img/23d1402649b6528b085cd6b900a2c9c3_194_0.png)

4. 我们需要关心的最后一个选项卡是 **Binding Properties** 选项卡。在此选项卡中，我们将指定与 BizTalk 如何与 SAP 交互相关的一些属性。需要注意的是，其中许多设置可以在您准备配置应用程序时在 BizTalk Administration 控制台中更改。在下一张截图中，我们将集中讨论两个特定属性：

- **ReceiveTimeout** – 可能是需要设置的最重要的生产属性之一。此属性指定 BizTalk 在关闭之前等待传入消息的持续时间。默认值是 10 分钟，时间几乎不够。如果我们不修改此设置，在生产环境中当超过此阈值时，BizTalk 停止监听 IDOC 时，我们可能会得到一些不理想的结果。最大值是 24.20:31:23.6470000（24 天）。要了解有关此属性的更多信息，请参阅以下博文：http://kentweare.blogspot.com/2010/03/biztalk-adapter-pack-sap-binding.html
- **EnableBizTalkCompatibilityMode** – 指定是否应加载 BizTalk 分层通道绑定元素，这将允许 BizTalk 事务流向 SAP。使用 BizTalk 中的 WCF-SAP Adapter 时，此值应始终设置为 true。

![](img/23d1402649b6528b085cd6b900a2c9c3_195_0.png)

5. **Binding Properties** 选项卡中还有一些我们感兴趣的其他属性：

- **EnableSafeTyping** – 并非所有 SAP 数据类型都有 .NET 数据类型等价物。通过启用安全类型，BizTalk 在尝试将 SAP 数据类型绑定到 .NET 数据类型时对类型的要求会“更宽松”。了解此属性如何改变应用程序的行为非常重要。如果您期望从 SAP 获取日期，当此属性设置为 True 时，数据很可能将被转换为字符串。说实话，根据个人经验，我在与 IDOC 集成时不需要将此属性设置为 true，但在调用 RFC 和 BAPI 时需要更改该属性。
- **GenerateFlatFileCompatibleIDOC** – 此属性指定 `<appinfo>` 节点是否应包含段类型或段定义以解析平面文件 IDOC。通过将属性设置为 True，并不意味着您必须使用平面文件来处理 IDOC。最好将此值保持为 true，因为它为您将来使用平面文件提供了灵活性。
- **ReceiveIDOCFormat** – 以前使用基于 .NET Connector 的 SAP Adapter 时，您必须将 IDOC 作为平面文件处理。在 WCF 版本的 Adapter 中情况不再如此。我们现在有机会在不使用平面文件架构和管道的情况下以 XML 发送和接收消息。如果我们指定 **Typed** 则我们将以 XML 消息接收消息。我们也有能力指定 String 或 RFC。如果我们想使用平面文件架构/管道则需要指定 String。如果我们想以带有 RFC 参数的 WCF 消息接收 IDOC 则需要指定 RFC。请注意，这些属性可以在 BizTalk Administration 控制台中配置接收位置时进行修改。

![](img/23d1402649b6528b085cd6b900a2c9c3_197_0.png)

6. 现在配置已设置好，单击 OK 按钮返回主向导。

7. 只要我们正确配置了向导，现在就可以单击 Connect 按钮建立与 SAP 的连接。

![](img/23d1402649b6528b085cd6b900a2c9c3_197_1.png)

8. 我们接下来需要问自己的问题是这是一个入站还是出站操作。重要的是记住提出此问题时的上下文。该问题是从 BizTalk 角度提出的。 此 IDOC 将是 BizTalk 的出站操作还是入站操作？由于我们将 IDOC 发送到 SAP，因此我们希望指定客户端（出站操作）。

![](img/23d1402649b6528b085cd6b900a2c9c3_197_2.png)
