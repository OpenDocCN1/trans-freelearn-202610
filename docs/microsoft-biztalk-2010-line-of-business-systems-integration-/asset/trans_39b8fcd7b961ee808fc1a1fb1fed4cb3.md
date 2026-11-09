
10. 将操作名称设置为 **SetAzureToken**，从 **要更新的字段** 列表中选择 **AzureValidationToken** 字段，并指定一个 GUID 作为新值。

![](img/23d1402649b6528b085cd6b900a2c9c3_500_0.png)

11. 选择为我们的 **BillingStreetChanged** 规则添加另一个工作流操作。选择 **New Outbound Message** 操作以利用 Salesforce.com 中的传出消息功能。

12. 将传出消息名称设置为 **CallBillingUpdateService**，并输入一个临时的 **Endpoint URL**，例如 http://localhost。我们目前还没有本地服务的基于云的地址，因此暂时使用占位符地址即可。

如果我们想要支持回调模式，可以选择在消息负载中发送 **SessionID**。选择发送以下字段：**AccountNumber**、**BillingStreet**、**BillingCity**、**BillingState**、**BillingPostalCode**、**BillingCountry**、**AzureValidationToken__c**、**Name** 和 **LastModifiedDate**。**LastModifiedDate** 字段在您怀疑本地处理存在积压并且需要一种方法来查找最近更新时很有用。

![](img/23d1402649b6528b085cd6b900a2c9c3_501_0.png)

- 1. 保存传出消息配置。
- 2. 在 App Setup 菜单下，在 Create 菜单和 Workflows & Approvals 菜单下，选择 Outbound Messages 链接。
- 3. 点击我们的传出消息的 Name 以查看其详细信息。将 Endpoint WSDL 以 workflowOutboundMessage.wsdl 的名称保存到您的开发机器上。

![](img/23d1402649b6528b085cd6b900a2c9c3_501_1.png)

## Building the BizTalk recipient of the Outbound Message

在设置好传出消息后，我们需要配置 BizTalk 以理解消息、公开接收服务并发送预期的确认：

- 1. 在现有的 Visual Studio 2010 解决方案中，创建一个新的空 BizTalk 项目，命名为 `Chapter10-SFDC.AccountProcessing.ChangeRouting`。
- 2. 为项目分配一个强名称密钥，并将部署应用程序设置为 **Chapter10**。
- 3. 我们可以选择手动构建接收和处理 Salesforce.com 传出消息的架构和匹配的编排端口，或者利用 BizTalk 的开发向导来完成大部分工作。右键单击 BizTalk 项目并选择 **Add Generated Items**。
- 4. 选择 **Consume WCF Service** 选项并选择 **Metadata files** 源。虽然我们通常使用此向导来使用 BizTalk 将调用的服务，但我们也可以用它来生成入站服务工件。
- 5. 在向导的 Metadata files 页面上，浏览到您之前保存到机器上的 Salesforce.com WSDL。完成向导并导入引用。
- 6. 向导的输出包括包含所有消息定义的架构以及包含描述交互的端口类型的编排。

![](img/23d1402649b6528b085cd6b900a2c9c3_502_0.png)

- 7. Salesforce.com 传出消息进程需要服务提供确认。这意味着我们不能仅仅将消息接收到 BizTalk 中并进行路由。相反，我们需要一个能够向 Salesforce.com 返回同步布尔确认的编排。向项目添加一个新的编排，命名为 `ProcessSFDCMessage.odx`。
- 8. 创建两个编排消息。第一个消息名为 **BillingChangeInput**，类型为 **Chapter10-SFDC.AccountProcessing.ChangeRouting.notificationsRequest**，而第二个消息名为 **BillingChangeOutput**，类型为 **Chapter10-SFDC.AccountProcessing.ChangeRouting.notificationsResponse**。
- 9. 我们通常使用任何生成的编排端口作为调用我们引用的服务的方式。然而，我们也可以使用此生成的端口来描述 **inbound** 交互。将新配置的端口拖到 **Port Surface** 并选择现有的 Port Type，命名为 **Chapter10-SFDC.AccountProcessing.ChangeRouting.NotificationPort**。选择接收请求并发送响应，然后完成端口配置向导。
- 10. 按以下顺序排列编排形状：**Receive** 形状、包含 **Message Assignment** 形状的 **Construct Message** 形状、一个 **Send** 形状和另一个 **Send** 形状。此编排接收 Salesforce.com 传出消息，生成确认消息，将其返回给 Salesforce.com，最后将传出消息发布到 BizTalk 总线以进行进一步处理。

![](img/23d1402649b6528b085cd6b900a2c9c3_503_0.png)

- 11. 最上面的 **Receive** 形状应使用 **BillingChangeInput** 编排消息。**Construct** 形状应将 **Message Constructed** 值设置为 **BillingChangeOutput**。下一个 **Send** 形状应与 **BillingChangeOutput** 消息关联，而最终的发送形状应与 **BillingChangeInput** 消息绑定。
- 12. 创建一个新的编排变量，命名为 **tempXml**，其类型为 **System.Xml.XmlDocument**。
- 13. 在 **Message Assignment** 形状中，实例化 **XmlDocument** 变量，加载指示“true”确认的 XML 内容，并将变量设置为等于响应消息。
- 14. 最后，创建一个配置好的编排端口以将我们接收到的计费更改事件发送出去以供进一步处理。选择稍后指定传输通道的选项。最终的编排应如下所示：

```
tempXml = new System.Xml.XmlDocument();
tempXml.LoadXml("<ns0:notificationsResponse xmlns:ns0='http://soap.sforce.com/2005/09/outbound'><ns0:Ack>true</ns0:Ack></ns0:notificationsResponse>");

BillingChangeOutput.@response = tempXml;
```

![](img/23d1402649b6528b085cd6b900a2c9c3_504_0.png)

- 15. 构建并部署应用程序，然后切换到 BizTalk Administration Console。
- 16. 我们有几个服务托管选项可供选择。对于此场景，我们将直接在 BizTalk 进程内托管云端点，而不是在 IIS Web 服务器内托管。这不需要我们构建物理服务。通过创建一个新的 Request-Response 接收端口，命名为 **Chapter10.ReceiveBillingChangeNotification**，开始此步骤。
- 17. 向该端口添加一个新的 Request-Response 接收位置，命名为 **Chapter10.ReceiveBillingChangeNotification.HttpRelay**。将 **Type** 设置为 **WCF-Custom**，接收管道设置为 **XMLReceive**。
- 18. 单击适配器类型旁边的 **Configure** 按钮。在 **General** 选项卡上，将 **Address (URI)** 设置为 `https://<your namespace>.servicebus.windows.net/AccountProcessing/BillingChange.svc`。
- 19. 在 **Binding** 选项卡上，将 **Binding Type** 设置为 Azure 提供的 **basicHttpRelayBinding**。展开 **Security** 绑定配置，并确保安全模式设置为 **Transport**，并且 **relayClientAuthenticationType** 为 **None**。请记住，Salesforce.com 传出消息传输详细信息不能由开发人员调整，因此我们不能正式发送额外的凭据或直接向 Service Bus 进行身份验证。

![](img/23d1402649b6528b085cd6b900a2c9c3_505_0.png)

- 20. 导航到 **Behavior** 选项卡并添加一个新的 Endpoint 行为。从列表中选择 **transportClientEndpointBehavior**。此行为提供了 BizTalk 连接到 Service Bus 端点地址并监听所需的凭据。选择 **SharedSecret** 凭据类型并填写您的 AppFabric Service Bus 帐户的 **issuerName** 和 **issuerSecret** 值。

![](img/23d1402649b6528b085cd6b900a2c9c3_506_0.png)

- 21. 保存接收位置配置。
- 22. 创建一个新的单向发送端口，命名为 **Chapter10.SendBillingChange.FILE**，我们的编排将绑定到该端口并向其发送计费更改消息。该端口应将其输出发送到文件系统。
- 23. 将 **ProcessSFDCMessage** 编排绑定到主机、relay 接收位置，最后绑定到 FILE 发送端口。
- 24. 启动依赖的发送端口、编排，最后启动接收位置。如果接收位置配置正确，您现在应该在云上进行监听。

## Triggering the outbound message from Salesforce.com

配置完成后，我们可以发起传出消息并确认 BizTalk 能够接收它：

- 1. 返回您的 Salesforce.com 站点并编辑传出消息配置，使 Endpoint URL 现在指向 `https://<yournamespace>.servicebus.windows.net/AccountProcessing/BillingChange.svc`。确保我们的 workflow 规则已激活。

