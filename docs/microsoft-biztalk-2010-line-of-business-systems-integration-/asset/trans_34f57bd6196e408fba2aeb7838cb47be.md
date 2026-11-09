
- 6. 返回 Visual Studio 2010 并打开解决方案中存在的 `Chapter6.TrainingService` 项目。
- 7. 打开服务的 `web.config` 文件并进行以下更改：
  - a. 在 `system.ServiceModel/services/service` 下为每个端点完成端点地址，填写您自己的 Service Bus 命名空间。
  - b. 在 `system.serviceModel/behaviors/endpointBehaviors` 处为两个行为设置有效的发行者密钥。
- 8. 构建并部署 TrainingService，并在浏览器中查看服务以确保成功绑定到云。

我们的 TrainingService 现已完成，可供 BizTalk Server 调用。

## 构建 BizTalk 解决方案组件

我们将构建一些 BizTalk 组件，用于调用 TrainingService 并将其结果与访问请求消息合并，以便发出一条包含请求访问的员工以及授予访问权限所需培训的单一消息。

- 1. 在 Visual Studio 2010 解决方案中打开现有的 BizTalk Server 项目。
- 2. 创建一个名为 `AccessRequirements_XML.xsd` 的新架构。此架构保存访问请求详细信息和与访问请求相关的必需培训课程的组合。所有元素的 数据类型均为“string。”
- 3. 确保上述架构将 `Course` 节点设置为无界，以支持无限数量的分配课程。
- 4. 现在我们准备引用云托管的 TrainingService 并生成调用该服务所需的 BizTalk 工件。在 Visual Studio 2010 中右键单击 BizTalk 项目并选择 **添加生成项**。选择 **使用 WCF 服务** 选项。

![](img/23d1402649b6528b085cd6b900a2c9c3_310_0.png)

- 5. 在提示时，输入服务的 MEX 地址（例如，`https://<your namespace>.servicebus.windows.net/TrainingServices/TrainingService.svc_mex`）并单击 **获取** 按钮。您可能会看到错误，但这不会产生负面影响。MEX 查询通过 SOAP 进行，而此向导仅执行简单的 HTTP GET 请求。因此，错误是预期的。

![](img/23d1402649b6528b085cd6b900a2c9c3_311_0.png)

- 6. 完成 BizTalk WCF 服务使用向导，注意向导会在 BizTalk 项目中添加架构、编排和绑定文件。
- 7. 在 BizTalk 项目中创建一个名为 `VerifyRileyAccessRequirements.odx` 的新编排。此编排监听来自 Riley Semiconductor 公司的建筑访问请求，并通过云回调到 Riley Semiconductor 的培训系统，以查找请求建筑所需的培训。从概念上讲，我们可以为每个合作伙伴创建一个编排，或者也可以构建一个更松散耦合的编排，该编排更依赖于发送端口映射以实现特定于目标的格式。在此场景中，我们将构建特定于供应商的编排。
- 8. 定义四个编排级消息：
  - a. **AccessRequestInput** of type **Chapter6.BuildingAccessManagement.BizTalk.AccessRequestEvent_XML**
  - b. **EmployeeTrainingOutput** of type **Chapter6.BuildingAccessManagement.BizTalk.AccessRequirements_XML**
  - c. **TrainingServiceRequest** of multi-part type **Chapter6.BuildingAccessManagement.BizTalk.ITrainingService_GetTrainingForBuildingAccess_InputMessage**
  - d. **TrainingServiceResponse** of multi-part type **Chapter6.BuildingAccessManagement.BizTalk.ITrainingService_GetTrainingForBuildingAccess_OutputMessage**
- 9. 在编排的初始接收形状上添加过滤器。此过滤器将确保只有来自 Riley Semiconductor 的消息进入此编排。

![](img/23d1402649b6528b085cd6b900a2c9c3_312_0.png)

- 10. 在最顶层的接收形状之后，添加一个 **构造/转换** 块，负责构建服务的请求消息。
- 11. 双击 **转换** 形状并选择创建名为 **BizTalk.AccessRequest_To_TrainingServiceRequest** 的新映射的选项。此映射应以 **AccessRequestInput** 消息开始并输出 **TrainingServiceRequest** 消息。
- 12. 在映射中，将 **LocationId**、**BuildingNumber** 和 **RoomNumber** 连接到目标架构中的相应节点。保存映射并返回到编排。
- 13. 在之前的 **构造** 形状下方添加 **发送** 形状和 **接收** 形状。这些新形状负责调用 TrainingService 并接收响应消息。将 **发送** 形状设置为使用 **TrainingServiceRequest** 消息，而 **接收** 形状将使用 **TrainingServiceResponse** 消息。
- 14. 在编排的 **端口表面** 部分创建一个新的已配置端口。对于 **端口类型** 值，选择使用现有端口类型并指向由服务引用创建的端口类型。我们的编排应如下图所示：

![](img/23d1402649b6528b085cd6b900a2c9c3_313_0.png)

- 15. 接下来，我们需要将原始消息与服务调用的结果合并。在编排底部添加一个新的 **构造/转换** 块。
- 16. 双击 **转换** 形状并创建名为 **AccessRequestTrainingResponse_To_EmployeeTrainingOutput** 的新映射。
- 17. 映射的输入同时为 **AccessRequestInput** 和 **TrainingServiceResponse** 消息，并输出 **EmployeeTrainingOutput** 消息。你可能记得可以在 BizTalk Server 中创建多部分映射，但只能在编排内部创建。不能在 Mapper 本身内部创建多部分映射。
- 18. 将相关的源节点连接到目标节点。

![](img/23d1402649b6528b085cd6b900a2c9c3_313_1.png)

- 19. 添加一个最终的发送形状，使用 **EmployeeTrainingOutput** 消息。
- 20. 为初始 **接收** 形状定义一个编排接收端口，为最后一个 **发送** 形状定义一个编排发送端口。最顶层的编排接收端口应为 **直接绑定**，这意味着它侦听 MessageBox 中的数据，而不是绑定到消息端口。
- 21. 构建并部署更新的 BizTalk 解决方案。

我们现在拥有从 BizTalk Server 消费云服务所需的所有编译组件。接下来，我们需要配置 BizTalk 消息组件以与 Windows Azure AppFabric 通信。

## 配置 BizTalk 解决方案以使用 Service Bus 绑定

我们的最终练习是配置 BizTalk 消息组件。

- 1. 切换到 BizTalk Server 管理控制台并找到 **Chapter6** 应用程序。
- 2. 我们需要两个消息端口。首先，我们需要通过 Service Bus 调用 TrainingService 的双向发送端口。无需手动构建此端口，我们可以使用 BizTalk WCF 服务使用向导生成的绑定文件。右键单击 **Chapter6** 应用程序并选择 **导入**，然后选择 **绑定**，并导航到与我们的 BizTalk 项目代码所在文件夹中的 `TrainingService_Custom.BindingInfo.xml` 文件。
- 3. 导入绑定后，我们可以在应用程序中看到一个新的发送端口。打开该端口会显示云地址、云绑定和有效的 WCF 操作。

![](img/23d1402649b6528b085cd6b900a2c9c3_315_0.png)

- 4. 切换到 **行为** 选项卡并添加一个新的端点行为。找到 **TransportClientEndpointBehavior** 并为 Service Bus 设置有效的“发送”凭据。在我们的例子中，这是来自 Service Bus STS 的 WatsonBSafe 发行者及其密钥。
- 5. 我们的最终消息端口是一个单向发送端口，用于传输编排的最终消息。创建一个文件发送端口，将消息发送到文件系统。
- 6. 消息端口完成后，我们可以完成编排绑定。转到 Chapter6 应用程序中的编排，并将传出逻辑端口与适当的发送端口/发送端口组匹配。

![](img/23d1402649b6528b085cd6b900a2c9c3_316_0.png)

- 7. 启动发送端口和编排。

我们可以通过启动客户端应用程序并提交新的建筑访问请求来测试整个端到端解决方案。选择建筑 10 进行第一次请求，注意输出消息中用户没有培训要求。如果发送另一个请求并使用建筑 41，你应该会看到一条消息，其中用户被分配了两门课程。

## 摘要

随着越来越多的组织开始依赖云来运行关键任务应用程序，对简单集成解决方案的需求将会增加。在本章中，我们首先看到了如何以最少的努力将消息从云发送到本地 BizTalk Server。我们执行的大部分工作围绕构建可重用的 WCF 行为，该行为可以验证令牌并在服务上叠加授权层。在 Windows Azure AppFabric 可用之前，这种集成只能通过通过反向代理将 BizTalk Server 暴露给互联网，或使用硬件/软件允许合作伙伴直接访问内部服务总线来实现。现在，通过最少的努力，我们可以选择性地将单个端点暴露给任何经过授权的、可以通过互联网访问的客户端。

除了向云发送消息之外，我们还需要能够消费云服务的能力。我们看到以 BizTalk 消费本地服务的方式消费云端点非常容易。可以想象，编排将内部和外部数据无缝结合的消息解决方案的令人兴奋的可能性。

在下一章中，我们将看到如何使用 Windows SharePoint Services 适配器和 InfoPath 构建集成的 SharePoint 解决方案。

