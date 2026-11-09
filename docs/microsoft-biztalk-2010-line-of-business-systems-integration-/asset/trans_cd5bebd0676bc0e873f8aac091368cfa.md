
我们可以将 Web 浏览器指向 ListData.svc 服务并提供 BizTalk Environments 列表的名称。相应地，我们列表中存在的所有数据都将作为 Atom feed 返回。该服务需要权限，就像 SharePoint 站点中的任何其他资源一样。该服务不能匿名调用，因此如果您想通过 BizTalk 操作此列表，请确保您的主机实例用户具有参与权限。我们还能够提供 REST 查询以返回列表中数据的子集，就像数据库查询一样。除了通过此接口读取数据外，我们还可以执行插入和更新操作。

我们可以在站点下的 _vti_bin 文件夹中找到 ListData.svc。与 WSS 适配器不同，如果我们的列表名称包含空格，我们需要将其删除，而不是提供空格键的 HTML 等价形式：%20。

```
http://localhost:8080/sites/BizTalkLineOfBusiness/_vti_bin/ListData.svc/BizTalkEnvironments
```

当我们在浏览器中输入此 URL 并按 Enter 时，我们将收到一个 XML 响应，其中包含在列表中捕获的 BizTalk 环境数据。

![](img/23d1402649b6528b085cd6b900a2c9c3_382_0.png)

值得注意的是，BizTalk WCF 适配器开箱即用不支持这些 RESTful 接口。当 BizTalk 消费 RESTful 列表服务时，它无法解析部分 XML。在 <entry m:etag="W/"1""> 标签内，存在一个嵌套的引号 ""，BizTalk 将其视为无效 XML。最终结果是此消息在接收管道中被挂起。如果使用 WCF 端点是必不可少的，我们可以编写一个自定义管道组件，在消息发送到消息框之前删除此额外的引号。为了本书的目的，我们将重点关注 Lists.asmx，因为与此服务交互时无需额外编码。

```
<?xml version="1.0" encoding="utf-8" standalone="yes" ?>
<feed xml:base="http://win2k8r2:8080/sites/BizTalkLineOfBusiness/_vti_bin/ListData.svc/"
xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices"
xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/200
<title type="text">BizTalkEnvironments</title>
<id>http://win2k8r2:8080/sites/BizTalkLineOfBusiness/_vti_bin/ListData.svc/BizTalkEnvironments/</id>
<updated>2010-10-03T21:59:19Z</updated>
<link rel="self" title="BizTalkEnvironments" href="BizTalkEnvironments" />
<entry m:etag="W/"1"">
<id>http://win2k8r2:8080/sites/BizTalkLineOfBusiness/_vti_bin/ListData.svc/BizTalkEnvironments(1)</id>
<title type="text">Dev</title>
<updated>2010-10-03T11:35:03-06:00</updated>
<author>
<name />
```

由于 Lists.asmx Web 服务基于经典的 ASMX 风格 Web 服务，因此在消费此服务时我们有两个 BizTalk 适配器选项：

- SOAP
- WCF-BasicHttp

SOAP 适配器自 BizTalk 2009 起已被弃用，并在 BizTalk 2010 中继续被弃用。虽然使用此适配器仍然是一个可支持的方案，但如果可能的话，我们最好避免使用它，因为此适配器将在 BizTalk 的未来版本中消失。请参阅官方 MSDN 文档了解更多详细信息：http://msdn.microsoft.com/en-us/library/aa559720(BTS.70).aspx

WCF-BasicHttp 适配器支持消费 WCF 和传统 ASMX Web 服务。该适配器于 BizTalk 2006 R2 引入，并在消费 Web/WCF 服务时继续成为 BizTalk 2010 中可靠的适配器。我们将开发的方案基于 WCF 适配器。

在使用 Lists.asmx Web 服务时，我们需要利用一种名为协作应用程序标记语言（CAML）的技术。CAML 是一种基于 XML 的标记语言，用于通过 Web 服务、客户端对象模型或 SharePoint API 操作 SharePoint。在本书的上下文中，我们将从高层次介绍 CAML，因为本书并不打算成为全面的 CAML 资源。

## 消费 SharePoint 列表方案演练

我们将要演练的真实世界方案是一个车辆维护列表。现场员工将使用此列表来填充与其公司车辆相关的任何维护信息。就像第 7 章中的方案一样，我们不希望现场员工登录我们的 ERP 系统，因为用户体验不适合在外执行日常职责的现场员工。

SharePoint 列表中填充的信息仍需要传到我们的 ERP 系统。此信息由财务和监管部门跟踪，因此重要的是此信息与我们的记录系统进行交换。BizTalk 开发人员面临的一个常见挑战是安排流程以启动编排的能力。大多数 BizTalk 方案始于 BizTalk 接收消息，进而该消息实例化编排。在我们的方案中并非如此，而且由于您无法通过发送消息来开始 BizTalk 编排，我们需要触发一条消息来启动 BizTalk 编排。我们有几个选项：

- BizTalk Scheduled Task Adapter
- SQL Server Polling Adapter

BizTalk Scheduled Task Adapter 是一个开源适配器，可在 codeplex 上获取；http://biztalkscheduledtask.codeplex.com。该适配器允许您安排触发消息，以便在设定时间实例化编排。

SQL Server 适配器，无论是 WCF 还是传统，都可以像 BizTalk Scheduled Task Adapter 一样使用，因为它可以安排每 X 分钟/小时/天执行一次。对于某些环境，运行开源或社区可执行文件是禁止的，因此我们将利用基于 WCF 的 SQL Server 适配器。

使用 SQL Server 适配器（传统或基于 WCF）另一个好处是需要存储我们想要通信的 SharePoint 列表的 SharePoint 列表 GUID。我们可以将此 GUID 存储在 SQL Server 表中，并在消费 SharePoint 列表数据之前从数据库检索此值。随着我们将此解决方案从各种环境（开发/测试/生产）迁移，我们拥有一些可移植性，因为每个 SharePoint 环境将具有唯一的列表 GUID。

一旦我们获得了 SharePoint 列表的 GUID，我们将在构建 CAML 查询时使用此 GUID。然后我们将此查询发送到 Lists.asmx 服务。我们期望 CAML 查询将返回车辆维护列表的结果。此查询的结果将被映射到 ERP 系统期望的平面文件格式。

## 创建自定义 SharePoint 列表

![](img/23d1402649b6528b085cd6b900a2c9c3_385_0.png)

> 注：以下示例假定您已有一个现有的 SharePoint 站点。以下链接描述了如何为 WSS 3.0 SP2 和 SharePoint 2010 Foundation 创建 SharePoint 站点：
> WSS 3.0 SP2：
> http://technet.microsoft.com/en-us/library/cc288969(office.12).aspx
> SharePoint 2010 Foundation：
> http://technet.microsoft.com/en-us/library/cc263094.aspx

我们需要创建一个自定义列表，让我们的现场工作人员输入与其车辆维护相关的数据。BizTalk 稍后将连接到此列表并检索数据，以便将其发送到我们的 ERP 系统。

1. 我们可以通过在 SharePoint 功能区中 **主页** 菜单下方的 **列表** 链接来创建自定义列表。

![](img/23d1402649b6528b085cd6b900a2c9c3_385_1.png)

2. 我们现在想要点击 Create 链接。

![](img/23d1402649b6528b085cd6b900a2c9c3_386_0.png)

3. 选择自定义列表，提供列表名称 Vehicle Maintenance，然后点击创建按钮。

![](img/23d1402649b6528b085cd6b900a2c9c3_386_1.png)

4. 我们现在有一个空列表，想要通过点击创建列按钮来创建自定义列。

5. 向列表添加以下列：

- 标题（系统生成）
- 位置
- 开始时间
- 结束时间
- 单位
- 里程表读数
