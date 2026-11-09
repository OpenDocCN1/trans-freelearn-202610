

![](img/23d1402649b6528b085cd6b900a2c9c3_374_0.png)

注意：连接到 SharePoint 的发送端口的 BizTalk 主机实例用户需要对文档或表单库具有“Contributor”访问权限。

## 测试应用程序

配置好我们的应用程序后，现在可以启动它并执行测试以确保其正常工作。

- 1. 我们希望在接收位置的文件夹中放置一条与下面类似的消息。此消息的示例可在本章源代码的 Chapter7-SendingDocuments\FileDrop\ 文件夹中找到。

```
<ns0:WOMIncident_XML INCIDENT_DATE="2010-10-10"
  xmlns:ns0="http://Chapter7_SendingDocuments.WorkOrderManagementIncident_InfoPath">
  <Location CITY="Regina" PROVINCE="Saskatchewan" PLANT="2135"
  SERVICE_POINT="23" />
  <Personnel>
  <PERSON NAME="Kurt Weare" ID="9999" POSITION="Area Foreman"
  EMP_OR_CONTR="Employee" />
  </Personnel>
  <VEHICLE VEHICLE_ID="1400" DAMAGE="Front End Damage to Truck" />
  <ENVIRONMENT IS_OIL_SPILL="false" IS_ENV_HAZARD="false"
  IS_WILD_LIFE_IMPACT="true">
  <COMMENTS>While travelling back from Oilfield install, a moose ran into the road
  and I couldn't stop in time to avoid him. Extensive damage to the front end of
  the truck has occurred.</COMMENTS>
  </ENVIRONMENT>
</ns0:WOMIncident_XML>
```

- 2. BizTalk 应拾取此文件，将其转换为 InfoPath 所期望的格式，并发送到 SharePoint。在此过程中，BizTalk 还应使用消息负载中的数据填充 SharePoint 列。

![](img/23d1402649b6528b085cd6b900a2c9c3_375_0.png)

- 3. 请注意，在 Type 列中包含一个 InfoPath 图标。这意味着 SharePoint 已确定该文档中的数据与可在该表单库中找到的 InfoPath 模板相匹配。使此演示有趣的是，在 BizTalk 中我们并未指定 InfoPath 模板名称。事实上 BizTalk 并不知晓此数据与模板匹配，因为我们无需提供任何像在 BizTalk 2006 中那样必须提供的处理指令。为了验证这一点，请打开此发送端口的跟踪消息正文功能并查看消息的内容。您在任何地方都找不到表明此数据属于 InfoPath 模板的迹象。

![](img/23d1402649b6528b085cd6b900a2c9c3_375_1.png)

- 4. 当我们单击此文档的链接时，我们还会发现 InfoPath 客户端已加载，并且我们的 XML 消息已加载到我们的 InfoPath 模板中。

![](img/23d1402649b6528b085cd6b900a2c9c3_376_0.png)

## 摘要

在本章中，我们看到了两个示例，展示了 InfoPath、SharePoint 和 BizTalk 之间紧密的集成。在第一个场景中，我们接收来自现场员工的工时表请求，并仅将已批准的请求发送到我们的工资系统。在第二个场景中，我们演示了工作订单管理系统输出的现场事件被加载到 SharePoint 中。这些场景展示了紧密的集成，使我们能够构建引人注目的解决方案。这使我们作为 BizTalk 开发人员能够快速构建在业务内部具有高度可见性的稳健解决方案。

我们还能够将 SharePoint 扩展到企业中，并将企业带入 SharePoint，这使 IT 能够利用之前的投资。SharePoint 不再是数据被创建并变得陈旧的孤岛。借助 BizTalk 及其适配器套件，我们可以将 SharePoint 功能扩展到 BizTalk 能够通信的任何系统。

Windows SharePoint Services 适配器与 Windows SharePoint Services 适配器 Web 服务的组合在集成文档和表单库时提供了全面的解决方案。然而存在一个限制，即 Windows SharePoint 适配器只能与文档和表单库集成。如果我们想与 SharePoint 的其他功能（如列表）通信，则无法使用适配器进行操作。不要担心，与列表通信时确实存在其他选项，它们将在下一章中讨论。

## 8

## 与 SharePoint 集成
2010 Web 服务

在第 7 章中，发送和接收演练均演示了使用 Windows SharePoint Services（WSS）适配器与 SharePoint 的集成。这些演示真正突出了 SharePoint 的优势，包括在最终用户熟悉的环境中进行表单管理和协作：Microsoft Office。

Windows SharePoint Services 适配器的问题在于它非常以文档和表单库为中心。适配器在与这些库交互时非常出色，但我们不可避免地会被问到 BizTalk 是否可以操作自定义 SharePoint 列表。对此问题的回答是肯定的，也是否定的。适配器本身不支持操作自定义列表，但由于 SharePoint 将操作列表的功能作为服务公开，因此 BizTalk 可以通过该机制与 SharePoint 集成。

在本章中，我们将介绍：

- SharePoint 列表概述
- SharePoint 的列表 Web 服务
- 消费 SharePoint 列表场景演练
- 创建自定义 SharePoint 列表
- 自定义列表 GUID
- 构建 BizTalk 应用程序
- 部署和配置 BizTalk 应用程序
- 测试 BizTalk 应用程序
- 与 SharePoint 集成的其他方式

## SharePoint 列表概述

SharePoint 列表代表 SharePoint 的基础组件。在使用 SharePoint 时几乎不可能以某种方式避免列表。从技术上讲，文档/表单库是一种列表类型，但为本次讨论的目的，我们将把文档/表单库排除在此类别之外。列表在结构和行为上通常类似于数据库表。列表具有列和可以附加的元数据。不同之处在于列表具有可由用户操作的图形用户界面（GUI）。

SharePoint 开箱即提供列表，例如日历和任务列表。我们的讨论将集中在自定义列表上。自定义列表允许我们配置与业务流程相关的特定字段。

下面举例说明了一个名为 BizTalk Environments 的自定义列表。它是一个简单的列表，可用于跟踪 BizTalk 服务器所连接的各种端点。

![](img/23d1402649b6528b085cd6b900a2c9c3_380_0.png)

## SharePoint 的列表 Web 服务

使用 MOSS 2007/Windows SharePoint Services 3.0 时，通过 Web 服务操作列表时您只有一个选择。开箱即用的 Web 服务 `Lists.asmx` 允许您通过多种 Web 方法操作列表数据。以下图像说明了 `Lists.asmx` Web 服务公开的许多 Web 方法：

## 列表

支持以下操作。有关正式定义，请查看服务说明。

- AddAttachment
- AddDiscussionBoardItem
- AddList
- AddListFromFeature
- AddWikiPage
- ApplyContentTypeToList
- CheckInFile
- CheckOutFile
- CreateContentType
- DeleteAttachment
- DeleteContentType
- DeleteContentTypeXmlDocument
- DeleteList
- GetAttachmentCollection
- GetList

您将在自定义列表的 `_vti_bin` 文件夹中找到 `Lists.asmx` Web 服务。例如，这里是 `lists.asmx` Web 服务的位置，如果我们要操作前面描述的 BizTalk Environments 列表，我们将使用它。

```
http://localhost:8080/sites/BizTalkLineOfBusiness/Lists/BizTalk%20Environments/_vti_bin/lists.asmx
```

> 注意：如果您在非标准端口上设置了 SharePoint，则只需提供端口号（:8080）。

SharePoint 2010 和 SharePoint 2010 Foundation 引入了另一种消费列表数据的方式，即通过 WCF RESTful 服务。这使我们能够消费 RESTful 端点并将列表中的数据用作 Atom 提要。

**REST** 代表 **Representational State Transfer**，是一种基于 HTTP 的分布式系统灵活软件架构。REST 的详细概述超出了本书的范围。

> 有关 REST 的更详细解释，请访问以下链接：http://en.wikipedia.org/wiki/Representational_State_Transfer

