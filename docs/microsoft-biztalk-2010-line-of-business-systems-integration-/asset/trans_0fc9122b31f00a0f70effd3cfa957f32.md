
![](img/23d1402649b6528b085cd6b900a2c9c3_413_0.png)

2. 我们将要使用的 ERP 消息的架构名为 SharePointListVehicleMaintenance_FF.xsd，它是一个平面文件架构。我们想要生成的消息的示例存在于 SampleDocs 文件夹中，该文件夹可在本章示例代码中找到。文件的名称为 Vehicle_Maintenance_For_ERP.csv。

![](img/23d1402649b6528b085cd6b900a2c9c3_413_1.png)

3. 你会注意到平面文件架构的结构与我们之前创建的自定义 SharePoint 列表相似。

![](img/23d1402649b6528b085cd6b900a2c9c3_414_0.png)

4. 由于这是一个平面文件架构，我们还需要一个平面文件管道来在消息通过发送端口传输时组装该消息。负责此操作的发送管道称为 SendVehicleMaintenance_FF.btp。在 Assemble 阶段，我们希望将一个平面文件装配组件拖到管道的表面上。然后我们希望将文档架构设置为我们的平面文件架构：Chapter8ManipulatingLists.SharePointListVehicleMaintenance_FF。

![](img/23d1402649b6528b085cd6b900a2c9c3_414_1.png)

5. 我们现在希望在编排中放置一个 **Send** 形状并创建一个逻辑发送端口。在 **Send** 形状中，我们希望指定一个名为 msgVehicleMaintenance_FF 的消息，其类型为 **Chapter8ManipulatingLists.SharePointListVehicleMaintenance_FF** 并且逻辑 **Send Port** 为单向，并且其端口绑定设置为 **Specify Later.**

![](img/23d1402649b6528b085cd6b900a2c9c3_415_0.png)

## 部署和配置 BizTalk 解决方案

我们现在准备部署和配置我们的 BizTalk 解决方案。

- 1. 要部署我们的解决方案，请右键单击它，然后单击 **Deploy Solution.**
- 2. 在 BizTalk 管理控制台中，我们应该能找到一个名为 **Chapter8-ManipulatingLists** 的应用程序。此解决方案的绑定文件 `Chapter8-ManipulatingLists.xml` 存在于本章源代码中的 `Bindings` 文件夹内。如果我们导入此文件，我们会发现我们有一个接收端口、一个接收位置和两个发送端口。请注意，此绑定文件中包含的端点可能与您机器上的目标不匹配。

3. 我们的接收端口名为 **RetrieveSharePointGUIDs**，它有一个名为 **RetrieveSharePointGUIDsFromSQL** 的接收位置。接收位置将使用 WCF-Custom 适配器、**BizTalkServerApplication** 主机实例和 **XMLReceive** 管道。

![](img/23d1402649b6528b085cd6b900a2c9c3_416_0.png)

> 注：我们也可以在此解决方案中使用 WCF-SQL 适配器。由于 Consume Adapter Service Wizard 在其自动生成的绑定文件中指定了 WCF-Custom 适配器，我们将使用它。本质上，两个适配器都利用 sqlBinding，因此底层功能相同。

4. 在此接收位置中，我们需要提供一个 **Address (URI)**。在这种情况下，我们可以提供以下内容： `mssql://localhost//SharePointList?InboundId=1`。**Localhost** 表示托管 **SharePointList** 数据库的数据库服务器。我们还必须为此连接提供唯一的 **InboundId**，以防我们配置了多个轮询接收位置使用同一数据库。有关 **InboundId** 属性的更多信息，请参阅第 2 章。

![](img/23d1402649b6528b085cd6b900a2c9c3_417_0.png)

5. 在 **Binding** 选项卡中，有一些我们需要修改的属性，如下所述：

| 属性名称 | 属性值 | 说明 |
|---|---|---|
| Binding Type | sqlBinding | 由于我们使用的是 WCF-Custom 适配器，我们需要提供绑定，并且由于我们连接到的是 SQL Server，我们需要提供 **sqlBinding**。 |
| inboundOperation Type | TypedPolling | 我们的方案使用 TypedPolling，因为我们期望从 **pollingStatement** 查询返回强类型消息。 |
| polledData Available | Select count(*) from Lists | 此属性需要一个查询，在执行时将确定是否继续执行 **pollingStatement**。如果此查询未返回任何数据，则 **pollingStatement** 查询将不会被执行。 |
| pollingIntervalIn Seconds | 30 | 我们希望联系 SQL Server 检索数据的间隔。此值在生产环境中可能无效，但在我们的演示目的中可以使用。在生产环境中，我们可能不希望如此频繁地轮询，因为每次轮询都会给数据库带来一定的压力。 |
| pollingStatement | Select ListGUID from Lists | 此查询将返回我们用于联系 SharePoint 以检索自定义列表数据的 GUID。 |

![](img/23d1402649b6528b085cd6b900a2c9c3_418_0.png)

- 6. 已设置的其余属性是使用 **sqlBinding** 的 **WCF-Custom** 适配器的默认值。
- 7. 我们用于与 SharePoint 通信的发送端口名为 **cfSendPort_Lists_ListsSoap_Custom**。此发送端口还将使用 **WCF-Custom** 适配器、**BizTalkServerApplication** 主机实例、**PassThruTransmit** 发送管道和 **XMLReceive** 接收管道。

![](img/23d1402649b6528b085cd6b900a2c9c3_419_0.png)

8. 我们现在需要指定 Lists.asmx Web 服务的 URL 和我们的 Soap Action 头。下面提供的 URL 代表用于构建此示例的 URL。您需要更新此 URL 以反映您的 SharePoint 服务器。当我们使用 Consume WCF Service Wizard 时，它还生成了一个绑定文件，我们可以用它来填充我们的发送端口。这种方法的一个关键好处是生成的绑定文件将包含所有 Soap Action 头，包括 GetListItemChanges 如下所示：

```
<Operation Name="GetListItems" Action="http://schemas.microsoft.com/sharepoint/soap/GetListItems" />
```

![](img/23d1402649b6528b085cd6b900a2c9c3_420_0.png)

9. 在 Binding 选项卡上，有一些我们需要修改的重要属性，描述如下：

| 属性名称 | 属性值 | 说明 |
|---|---|---|
| Binding Type | basicHttpBinding | 此绑定是我们使用经典 ASP.Net ASMX Web 服务时的最佳选择。 |
| Security | mode=Transport CredentialOnly | 由于我们使用的数据不敏感，因此此方案不需要任何特殊的基于消息的安全性。我们需要为 SharePoint 提供凭据，以便我们可以进行身份验证和授权以操作 SharePoint 列表。在此方案中，我们的 BizTalk 主机实例帐户将被传递。我们的 BizTalk 主机实例帐户还需要连接到站点的 Contribute 访问权限。 |
| Security-Transport | clientCredential Type=Ntlm | 由于我们的 BizTalk 服务器和 SharePoint 服务器存在于同一域中，我们可以利用 Ntlm 凭据类型。 |

![](img/23d1402649b6528b085cd6b900a2c9c3_421_0.png)

10. 在 Behavior 选项卡中，我们需要确保我们有一个 EndpointBehaviour，其类型为 clientCredentials 并启用 Windows Impersonation。

![](img/23d1402649b6528b085cd6b900a2c9c3_421_1.png)

11. 此发送端口的其余设置利用 WCF-Custom 适配器对 basicHttpBinding 的默认属性。

12. 我们的最终发送端口负责将平面文件发送到我们的 ERP 系统。此发送端口的名称为 SendVehicleMaintenanceListToERP。它将利用 FILE 适配器、BizTalkServerApplication 主机实例和我们的自定义 SendVehicleMaintenance_FF 管道。

![](img/23d1402649b6528b085cd6b900a2c9c3_422_0.png)

13. 我们现在需要配置我们的编排，以便我们可以设置主机实例并将我们的逻辑端口与物理端口链接。

![](img/23d1402649b6528b085cd6b900a2c9c3_422_1.png)

## 测试 BizTalk 应用程序

在启用 BizTalk 应用程序之前，让我们回到我们的列表，以确保我们有一些数据可以通过 SharePoint Lists.asmx Web 服务检索。

- 1. 一旦我们到达列表，我们可以通过单击 Add new item 链接来添加新记录。

![](img/23d1402649b6528b085cd6b900a2c9c3_423_0.png)

- 2. SharePoint 为我们提供了一个很好的界面来向我们的自定义列表添加数据。

![](img/23d1402649b6528b085cd6b900a2c9c3_423_1.png)
