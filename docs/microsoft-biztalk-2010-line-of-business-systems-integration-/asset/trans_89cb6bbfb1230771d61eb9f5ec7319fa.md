
![](img/23d1402649b6528b085cd6b900a2c9c3_365_0.png)

15. 进入表单库设置页面后，单击“高级设置”链接。

![](img/23d1402649b6528b085cd6b900a2c9c3_365_1.png)

16. 在此屏幕中，我们需要设置模板 URL。通过设置文档模板，当 BizTalk 将文档加载到此表单库中时，SharePoint 将自动尝试将上传的文件与此模板关联。

![](img/23d1402649b6528b085cd6b900a2c9c3_365_2.png)

> 注：我们不需要提供文档模板的完整路径。我们只需要提供基本站点之后的路径。
> 例如，我们站点的基 URL 为：
> 因此，对于文档模板的位置，我们只需要提供文件夹的名称和模板的名称。

```
http://win2k8r2:8080/sites/BizTalkLineOfBusiness/
```

```
Field Incident Forms/Forms/FieldIncidentType/
Chapter7-WorkOrderManagement_Incident.xsn
```

17. 如果我们想要浏览 SharePoint 库使用的文件系统，请单击“库”，然后单击“连接并导出”。这将启动 Windows 资源管理器，并带我们进入存储文档库文件的文件夹。为了更方便，请确保在 Windows 资源管理器中可以看到隐藏文件和文件夹，以便查看 Forms 文件夹。

![](img/23d1402649b6528b085cd6b900a2c9c3_366_0.png)

18. 如果我们导航到表单库并单击“新建文档”按钮，我们的现场事故模板现在将默认启动。

![](img/23d1402649b6528b085cd6b900a2c9c3_366_1.png)

19. 现在我们需要添加之前创建的 FieldIncidentType 中的列。我们可以通过从表单库设置页面单击“从现有站点列添加”链接来完成此操作。

![](img/23d1402649b6528b085cd6b900a2c9c3_367_0.png)

20. 从“从以下位置选择站点列：”下拉列表中选择“自定义列”，然后将 Incident City、Incident Date 和 Incident Plant 添加到“要添加的列：”组合框中，然后单击“确定”按钮。

![](img/23d1402649b6528b085cd6b900a2c9c3_367_1.png)

21. 现在我们将在表单库中看到这些列的显示。

![](img/23d1402649b6528b085cd6b900a2c9c3_367_2.png)

22. 到目前为止，我们已执行以下操作：
- 设计了一个可用于设计 InfoPath 表单的架构
- 设计了一个 InfoPath 表单
- 创建了一个内容类型
- 创建了一个表单库
- 为表单库设置了模板
- 修改表单库的视图以包含我们在内容类型中指定的自定义列

## 构建 BizTalk 应用程序

SharePoint 配置完成后，我们现在需要专注于解决方案的 BizTalk 部分。作为 BizTalk 解决方案的一部分，我们需要执行以下步骤：
- 为我们将从工作订单管理系统接收的消息建立架构
- 创建映射以将此工作订单管理数据转换为 InfoPath 架构类型数据
- 设置 Windows SharePoint Services 适配器上下文属性，以便保留原始文件名的名称
- 将消息发送到 SharePoint 并配置 Windows SharePoint Services 适配器，以便我们可以填充之前配置的内容类型列

1. 工作订单管理系统使用的文件的消息结构与 InfoPath 架构的结构非常相似。不同之处在于工作订单管理系统使用属性而不是元素。

![](img/23d1402649b6528b085cd6b900a2c9c3_369_0.png)

2. 我们将使用的映射非常简单，因为架构结构非常相似。

![](img/23d1402649b6528b085cd6b900a2c9c3_369_1.png)

3. 一旦我们将传入的工作订单管理消息转换为 InfoPath 消息，我们需要在消息分配形状中修改 InfoPath 消息。此消息分配形状的目的是检索传入消息的名称，以便在发布到 SharePoint 后我们可以保留文件的名称。默认情况下，如果您未提供静态或动态文件名，SharePoint 适配器将使用 GUID 作为文件名。

![](img/23d1402649b6528b085cd6b900a2c9c3_370_0.png)

> 注：BizTalk 2010 的新功能是长期以来许多开发人员希望清单上的一个功能：可扩展的表达式编辑器！我从来不明白为什么我们被限制在固定窗口中工作，但很高兴看到此功能终于被添加了。

4. 在此示例中，我们已从编排中操作了适配器上下文属性。我们也可以从自定义管道组件中访问这些属性。这允许仅消息解决方案也利用 Windows SharePoint Services 适配器提供的强大功能。有关操作 SharePoint 适配器上下文属性的完整功能演练，请参阅 Kent Weare 的博客文章：http://kentweare.blogspot.com/2010/01/sharetalk-integration-sharepointbiztalk_16.html
- 总体而言，我们的编排非常简单。我们接收消息，对其进行转换，更新以设置适配器上下文属性，然后将其发送到 SharePoint。

![](img/23d1402649b6528b085cd6b900a2c9c3_371_0.png)

5. 我们现在可以通过右键单击解决方案名称并单击 **Deploy Solution** 来部署应用程序。

6. 在 BizTalk 管理控制台中，我们现在应该看到一个名为 Chapter7-SendingDocuments 的应用程序。我们现在需要一个接收位置来从工作订单管理系统获取文件。此接收位置的名称为 **ReceiveXMLIncident**。它将使用 **FILE** 适配器、**BizTalkServerApplication** 接收处理程序和 **XMLReceive** 接收管道。

![](img/23d1402649b6528b085cd6b900a2c9c3_371_1.png)

7. 发送端口的配置比应用程序的其他部分更有趣。发送端口将利用 Windows SharePoint Services 适配器，通过利用 BizTalkServerApplication 主机实例和 PassThruTransmit 发送管道。

8. 如果我们单击 **Configure** 按钮，我们可以进一步探索适配器的配置设置。

9. 我们的 `processFieldIncidents.odx` 的配置非常简单。我们有一个接收端口，将从中接收现场事故，以及一个发送端口，将用于将 InfoPath 消息发送到 SharePoint。我们再次使用默认主机实例 **BizTalkServerApplication.**

![](img/23d1402649b6528b085cd6b900a2c9c3_372_0.png)

| 属性 | 值 | 注释 |
|---|---|---|
| 适配器 Web 服务端口 | 8080 | 在用于构建此解决方案的服务器上，SharePoint 站点是在端口 8080 上创建的，因为该服务器还安装了 BAM 门户。如果您将 SharePoint 安装为默认网站，则您的端口将为 80。 |
| 目标文件夹 URL | Field Incident Forms | 我们将把这些 InfoPath 文档存放在此处的表单库。 |
| 命名空间别名 | ns0="http://Chapter7_SendingDocuments.WorkOrderManagementIncident_InfoPath" | 这是构建 InfoPath 表单时使用的 `WorkOrderManagementIncident_InfoPath.xsd` 架构的命名空间。 |
| SharePoint 站点 URL | http://localhost:8080/sites/BizTalkLineOfBusiness/ | 用于托管创建和存储现场事故表单的表单库的 SharePoint 站点的位置。 |
| Microsoft Office 集成 | Yes (InfoPath Form Library) | 这指示 SharePoint 尝试使用 InfoPath 模板加载此文档。 |
| 列 01 | Incident City | 我们在建立 FieldIncidentType 内容类型时创建的 SharePoint 列的名称。 |
| 列 01 值 | %XPATH=//ns0:WOMIncident_InfoPath/Location/City% | 这是一个 XPath 表达式，将从我们的消息有效负载中检索事故的城市。 |
| 列 02 | Incident Plant | 我们在建立 FieldIncidentType 内容类型时创建的 SharePoint 列的名称。 |
| 列 02 值 | %XPATH=//ns0:WOMIncident_InfoPath/Location/Plant% | 这是一个 XPath 表达式，将从我们的消息有效负载中检索事故的工厂。 |
| 列 03 | Incident Date | 我们在建立 FieldIncidentType 内容类型时创建的 SharePoint 列的名称。 |
| 列 03 值 | %XPATH=//ns0:WOMIncident_InfoPath/IncidentDate% | 这是一个 XPath 表达式，将从我们的消息有效负载中检索事故的日期。 |

