
我们可以重建之前构建的、由 BizTalk 生成的服务，但这次选择将我们的端点扩展到云端。请注意，为了实现未来的云启用，我们之前创建了两个不同的服务，而不是一个具有两个端点的服务。新的向导不支持在同一解决方案中包含两个服务端点的概念，因此我们需要两个不同的服务项目。对于本示例，我们将只重建（并启用云）**RequestAccessService**。

- 1. 在 Visual Studio 2010 中，启动 BizTalk WCF 服务发布向导并选择 **WCF-CustomIsolated** 适配器。还选择在 **Chapter 6** 应用程序中拥有元数据终结点和接收位置。
- 2. 在下一个向导页面中，选择 **添加 Service Bus 端点。** 选项。如果看不到此选项，请验证是否确实安装了 BizTalk Server 2010 功能包。
- 3. 选择从模式构建 Web 服务的选项。
- 4. 重建服务，使其与我们最初的 **RequestAccessService** 相同。

![](img/23d1402649b6528b085cd6b900a2c9c3_304_0.png)

- 5. 将服务的目标命名空间设置为 `http://Chapter6/BuildingAccessManagement`
- 6. 将部署位置设置为 `http://localhost/RequestAccessService` 并确保选择覆盖现有服务的选项。
- 7. 后续的向导页面出现是因为我们之前选择了构建 Service Bus 端点的选项。

- 8. 选择 **BasicHttpRelayBinding** 并在 **服务命名空间** 字段中输入您个人的 Service Bus 命名空间值。此外，选择 **在云上启用元数据交换** 选项。这意味着我们的服务中有一个独立的云端点，用于公开元数据描述。您也可以选择使此服务在 Service Bus 命名空间的 ATOM feed 中可发现。

![](img/23d1402649b6528b085cd6b900a2c9c3_305_0.png)

- 9. 在定义 Service Bus 端点后，我们选择服务身份验证参数。在我们的例子中，我们使用 WatsonBSafe 发行者和关联的发行者密钥。下面的配置显示，获取服务元数据不需要客户端身份验证，但实际调用服务需要调用方使用 Service Bus 进行身份验证。

![](img/23d1402649b6528b085cd6b900a2c9c3_305_1.png)

- 10. 完成向导，该向导会覆盖我们现有的 Web 服务和接收位置。进入 BizTalk 管理控制台，找到生成的接收位置并将绑定重置为 **BasicHttpBinding**（不是云中继绑定），并使用之前概述的值重新添加我们的自定义声明授权行为。
- 11. 找到向导生成的 Web 服务并检查 `web.config` 文件。请注意，我们现在总共有三个端点：一个用于本地元数据交换，一个用于云元数据交换，一个用于云中继。向导没有利用接收位置配置来定义服务端点，而是将云所需设置添加到 Azure AppFabric Web 服务端点。然而，这意味着我们的自定义授权行为不会自动应用于我们的云中继端点。行为必须手动添加到 AppFabric 端点，因为它不会从接收位置的配置中提取。
- 12. 找到名为 `sharedSecretClientCredentials` 的端点行为，并使用与接收位置行为相同的值添加我们自定义行为的配置块。
- 13. 保存 `web.config` 文件并在浏览器中浏览服务端点，确认我们的端点配置正确。

```
<ClaimVerificationBehavior
serviceNamespace="<your namespace>"
trustedTokenPolicyKey= <your key here>
acsHostName="accesscontrol.windows.net"
trustedAudience="http://localhost/BuildingAccessManagementService"
claimType="ActivityPermission"
claimValue="Create" />
```

## 重新配置服务使用者以使用 Service Bus 绑定

现在剩下的就是通过云中继调用我们的服务。以下步骤概述了使之成为可能的客户端代码更改。

- 1. 要在 `Chapter6.ServiceConsumer.WinUI` Windows 窗体项目中调用我们的新云端点，我们需要对现有代码进行一些更改。首先，添加对 `System.ServiceBus.dll` 的引用，并在窗体代码中添加一个“using”语句。然后，在窗体的按钮单击事件中，使用 `BasicHttpRelayBinding` 类型重新定义 `binding` 变量。

```
BasicHttpRelayBinding binding = new BasicHttpRelayBinding(EndToEndBasicHttpSecurityMode.Transport, RelayClientAuthenticationType.RelayAccessToken);
```

- 2. 接下来，将端点地址更改为指向云托管端点。

```
EndpointAddress address = new EndpointAddress("https://richardseroter.servicebus.windows.net/RequestAccessService/RequestAccessService.svc");
```

- 3. 为了传入我们的 Service Bus 身份验证令牌，该令牌为我们提供调用服务所需的声明，我们需要定义一个 TransportClientEndpointBehavior。我们使用 SharedSecret 凭据类型并传入 Service Bus 的供应商凭据。

```
TransportClientEndpointBehavior sbBehavior =
    new TransportClientEndpointBehavior();
sbBehavior.CredentialType =
TransportClientCredentialType.SharedSecret;
sbBehavior.Credentials.SharedSecret.IssuerName =
"Riley Semiconductor";
sbBehavior.Credentials.SharedSecret.IssuerSecret =
"HwWbATe00bCpxszylPdJoVns+PNfJCdgK2lUErcWJAE=";
```

- 4. 最后，在我们的通道工厂定义之后，我们添加端点行为。

```
factory.Endpoint.Behaviors.Add(sbBehavior);
```

- 5. 保存并编译客户端应用程序。

如果我们在向云端点提交消息时使用正确的发送者（“Employee”），则可以看到消息通过 BizTalk Server 传输并最终到达文件系统。如果我们尝试使用无效的发送者类型（“Site Manager”）调用服务，则会收到 HTTP 401 错误。

在本演示中，我们看到了如何构建自定义授权管理器并将其应用于本地 BizTalk 生成的 Web 服务。然后很容易稍后扩展该本地服务并添加云端点。这是将现有内部端点通过云扩展到选定外部用户群的有力场景。

## 将 BizTalk Server 消息发布到 Service Bus

BizTalk Server 2010 是 Azure Service Bus 消息的直接使用者。随着越来越多的组织在其企业架构中引入云解决方案，我们将看到对无缝编排这些端点与本地数据和系统日益增长的需求。云服务可以非常轻松地由 BizTalk Server 调用，使 BizTalk 在云与云之间或本地与云之间集成时成为宝贵的资产。

## 场景描述

在此场景中，我们继续与 WatsonBSafe 和 Riley Semiconductor 组织的合作。如果您还记得上一次练习，WatsonBSafe 需要查询 Riley Semiconductor 的培训系统，以便在授予员工进入建筑物或房间的权限之前确定所需的课程。培训服务通过 Service Bus 公开，需要由 BizTalk Server 2010 调用。

![](img/23d1402649b6528b085cd6b900a2c9c3_308_0.png)

## 解决方案设计

对于此解决方案，我们将使用 BizTalk Server 的 WCF-CustomIsolated 适配器以及 Service Bus 绑定来调用培训服务。由于该服务仅查找数据且不需要细粒度安全性，因此我们不会在标准 Service Bus 身份验证流程之外应用任何安全性。

## 构建访问控制服务组件

我们首先需要的是供应商培训服务的 Service Bus STS 配置。在之前的演示中，WatsonBSafe 托管了一个 Riley Semiconductor 发送到的服务。在这里，角色发生了逆转。

- 1. 打开 AcmBrowser 应用程序并使用您的命名空间（加上“-sb”后缀）和默认发行者密钥登录 Service Bus STS。
- 2. 添加一个名为 **TrainingServices** 的新范围，并将范围的 **应用到 URL** 设置为 `http://<your namespace>.servicebus.windows.net/TrainingServices`
- 3. 创建一个名为 **Listen** 的新规则，其中输入声明 **发行者** 为 **Riley Semiconductor**，**类型** 为 **Issuer**，**值** 为 **Riley Semiconductor**。输出声明类型为 `net.windows.servicebus.action`，值为 **Listen**。

![](img/23d1402649b6528b085cd6b900a2c9c3_309_0.png)

- 4. 定义第二个名为 **Send** 的规则，该规则使用 **WatsonBSafe** 发行者，类型为 **Issuer**，值为 **WatsonBSafe**。输出声明类型为 `net.windows.servicebus.action`，值为 **Send**。此规则颁发令牌，允许服务使用者在知道 WatsonBSafe 发行者的凭据时调用服务。
- 5. 将 Service Bus STS 配置保存回云端。

