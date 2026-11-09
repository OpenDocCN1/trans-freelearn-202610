
## Orchestration setup

接下来，我们创建一个新的逻辑端口，用于接收来自 Dynamics AX 通过 AIF 队列发送的所有消息，设置如下：

| 端口名称 | ReceiveAXOutboundMessage_Port |
| 端口类型名称 | ReceiveAXOutboundMessage_PortType |
| 通信模式 | 单向 |
| 通信端口方向 | 我将始终在此端口接收消息 |
| 端口绑定 | 稍后指定 |

此外，创建一个新的发送端口。在此示例中，我们只需使用 FILE 适配器发送到文件夹，以便可以轻松查看 XML 文档。实际上，其他 BizTalk 应用程序很可能会处理这些消息，因此您可以根据需要修改发送端口。发送端口设置：

| 端口名称 | SendAXDocument_Port |
| 端口类型名称 | SendAXDocument_PortType |
| 通信模式 | 单向 |
| 通信端口方向 | 我将始终在此端口发送消息 |
| 端口绑定 | 稍后指定 |

接下来，我们需要在编排中添加以下内容：

- 接收形状（接收 msgAXOutboundMessage 消息）
- 表达式形状（确定 msgAXDocument 的文件名）
- 消息分配（构造 msgAXDocument 消息）
- 发送形状（发送 msgAXDocument 消息）

我们还将添加两个变量（aifActionName 和 xpathExpression），类型为 System.String，以及 xmlDoc，类型为 System.Xml.XmlDocument。

![](img/23d1402649b6528b085cd6b9002c9c3_463_0.png)

在表达式形状中，我们希望提取 AIF 操作，以便以类似的方式命名传出 XML 文档。这将使我们能够轻松识别来自 AX 的消息类型。

接下来，我们将在接收下方表达式形状中放入以下内容以提取 AIF 操作名称，我们将在后续传出消息中使用它：

```
aifActionName = msgAXOutboundMessage (DynamicsAx5.Action);
aifActionName = aifActionName.Substring(55,aifActionName.
LastIndexOf('/') - 55);
```

现在，我们需要提取正文消息的内容，即我们感兴趣的 XML 文档。在消息分配形状中，我们将使用 XPath 来提取消息。我们感兴趣的是从 AX 通过 AIF 队列接收的 DynamicsAX5.Message.Envelope 消息中 Body 节点的内容。将以下代码添加到分配形状中以提取 XML，将其分配给我们传出的消息，并设置一个可以在发送端口中使用的通用名称：

```
// Extract Contents of Body node in Envelope Message
xpathExpression = "/*[local-name()='Envelope' and namespace-uri()='http://schemas.microsoft.com/dynamics/2008/01/documents/Message']/*[local-name()='Body'
and namespace-uri()='http://schemas.microsoft.com/dynamics/2008/01/documents/Message']";
```

```
xmlDoc = xpath(msgAXOutboundMessage, xpathExpression);

// Extract the XML we are interested in
xmlDoc.LoadXml(xmlDoc.FirstChild.FirstChild.InnerXml);

// Set the message to the XML Document
msgAXDocument = xmlDoc;

// Assign FILE.ReceivedFileName property
msgAXDocument(FILE.ReceivedFileName) = aifActionName;
```

我们现在可以构建并部署解决方案。接下来，我们将介绍物理端口的设置以及将其绑定到我们的编排。

## Port configuration

创建一个新的接收端口 ReceiveAxOutboundMessage_ReceivePort，以及一个新的接收位置 ReceiveAxOutboundMessage_ReceiveLocation。将类型设置为 Microsoft Dynamics AX 2009，并且我们需要为接收管道选择 PassThruReceive。

![](img/23d1402649b6528b085cd6b9002c9c3_464_0.png)

点击 **配置** 以打开 **Microsoft Dynamics AX 2009 传输属性** 窗口。在这里，我们将 **身份验证类型** 设置为 **主机用户**，将 **网关用户** 设置为我们的 BizTalk 主机实例运行所在的 Active Directory 账户，**AOS 端口** 设置为 **2712**，**AOS 服务器** 设置为我们的 AX 服务器名称（**AX 2009-SHARED**）。此外，我们将 **轮询间隔** 设置为 **1** 分钟，最后，我们必须将 **直通** 设置为 **True**。这允许我们的解决方案在不解析消息的情况下使用适配器。如果我们将其设置为 `False`，适配器将在从 AIF 队列中移除消息之前尝试解析消息类型，如果不成功则将其设置为错误状态。

![](img/23d1402649b6528b085cd6b9002c9c3_465_0.png)

接下来，我们将创建一个 **静态单向发送端口**，其 **名称** 设置为 **SendAxDocument_SendPort** 以绑定到我们的逻辑发送端口。将 **类型** 设置为 **文件**，**发送管道** 设置为 **PassThruTransmit**，因为我们可能将不同类型的消息作为 XML 文档发送出去。

![](img/23d1402649b6528b085cd6b9002c9c3_466_0.png)

点击 **配置** 以打开 **FILE 传输属性** 窗口。将 **目标文件夹** 设置为 **C:\LOBIntegration\Chapter09\Chapter9-AXMessageOutflow\AXOutboundDocuments**，文件名设置为 **%SourceFileName% - %datetime% - %MessageID%.xml**。每个消息类型的 **SourceFileName** 都是相同的，因为它在我们上面的编排中已设置为 Dynamics AX 操作名称。**datetime** 添加到文件名属性中，因为我们通常在排查错误时发现它很有帮助，有时它也是未来集成逻辑所必需的。

![](img/23d1402649b6528b085cd6b9002c9c3_466_1.png)

现在，我们将两个端口绑定到编排并启动 BizTalk 应用程序。由于 AIF 队列中没有我们需要提取的内容，因此在 BizTalk 服务器端我们看不到太多内容。但是，如果您登录到 Dynamics AX，转到 **在线用户（Administration | Online users）**，您可以验证您的 BizTalk 应用程序是否确实已连接。将有一个连接，其 **会话类型** 为 **Worker** 以验证您的连接。持续点击刷新按钮，您将看到 **会话类型** 为 **Business Connector** 每分钟出现一次（我们的应用程序轮询间隔已设置在端口上）。

![](img/23d1402649b6528b085cd6b9002c9c3_467_0.png)

现在我们已经验证了连接性，我们需要向 AIF 队列发送一条消息以运行测试。现在禁用接收位置，以便我们可以在 AX 内部看到正在发生的事情，然后再将其从队列中移除。

在 Dynamics AX 中的许多表单（例如汇率）中，您会看到 **电子发送** 按钮。点击汇率表上的按钮，您将看到 **电子发送文档** 窗口弹出。您需要选择一个 **端点 ID**，并且如果愿意，可以从选择按钮中输入筛选条件。请注意，只有启用了 LedgerExchangeRates AIF 服务操作的端点才会出现在下拉列表中。

![](img/23d1402649b6528b085cd6b9002c9c3_467_1.png)

点击 **确定** 将消息发送到 AIF 队列。现在，尝试在 **会计科目表** 表（**总账 | 会计科目表**）上点击 **电子发送**，并注意以下屏幕截图中显示的错误消息。由于我们尚未激活此 AIF 服务，也未将服务的任何操作添加到任何端点，因此 Dynamics AX 不允许我们发送此消息。

![](img/23d1402649b6528b085cd6b9002c9c3_468_0.png)

接下来，我们将打开 AIF **队列管理器** 表单（**基础 | 设置 | AIF | 队列管理器**），在这里我们可以看到刚刚从汇率表发送的消息。请注意，日志条目缺少通道和源端点。这是因为默认情况下，**电子发送** 按钮是一个异步过程，需要运行批处理以根据 **目标端点** 的配置指定通道（在我们的例子中是 BizTalk）。

![](img/23d1402649b6528b085cd6b9002c9c3_468_1.png)

## Batch setup in Dynamics AX

现在我们需要设置一个批处理，以便通过 BizTalk 通道发送此传出消息。如果我们之前以异步模式从 BizTalk 发送过消息，它也会出现在这里，并且需要批处理才能处理。请注意，在 Dynamics AX 中也可以自定义方法，这些方法仍会将消息写入 AIF 队列，但不需要批处理即可被我们的 BizTalk 应用程序提取。

Dynamics AX 2009 允许在服务器上运行批处理作业；但是，早期版本需要活动客户端才能运行批处理作业。可以想象，在生产环境中保持活动客户端打开以运行连续批处理作业，或在需要时手动启动批处理作业可能会非常麻烦。即使具有服务器端批处理作业功能，您也可以选择仅进行同步集成以消除对任何批处理作业的需求。

因此，对于此示例，我们需要创建一个批处理。首先，我们将在批处理作业（Basic | Inquiries | Batch job）表中创建一个新条目。我们输入作业描述为 AIF BATCH 并点击保存。

![](img/23d1402649b6528b085cd6b9002c9c3_469_0.png)

接下来，点击查看任务按钮，这将打开批处理任务表单。在这里，我们需要添加四个新条目，每个 AIF 类（AifOutboundProcessingService、AifInboundProcessingService、AIFgatewaySendService、AIFGatewayReceiveService）各一个，从下拉列表中选择并为每个设置公司账户。由于批处理任务是公司特定的，可以想象这可能会给您的集成流程带来显著的管理开销。

![](img/23d1402649b6528b085cd6b9002c9c3_469_1.png)
