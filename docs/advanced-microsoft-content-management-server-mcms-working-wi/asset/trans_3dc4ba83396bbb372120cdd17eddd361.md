

![](img/442e26bf28f2df81fbf8dff3478f88e3_137_0.png)

现在，我们需要为我们的 SearchCrawler 帐户授予对整个 Tropical Green 网站的订阅者权限。

> 我们假定你已经安装了 MCMS Connector for SharePoint Technologies，因为其安装程序会在 Site Manager 中创建一个 MCMS Search 订阅者组，用于搜索你的 MCMS 频道结构。有关安装 MCMS Connector 的帮助，请参阅附录 B。

1. 通过指向“开始 | 所有程序 | Microsoft Content Management Server | Site Manager”启动 Site Manager。
2. 在 Site Manager 的左侧面板中选择“用户角色”按钮。
3. 选择“订阅者”用户角色。
4. 然后右键单击“MCMS Search User Subscribers”角色并选择“属性”。
5. 单击“组权限”选项卡，查看 MCMS Search User 角色拥有权限的所有频道、模板和资源。所有频道、模板和资源都应处于勾选状态。
6. 单击“组成员”选项卡，并单击“修改”按钮。
7. 输入我们上面添加的 `MCMSBOOK\SearchCrawler` 用户作为 SharePoint 爬网帐户，然后点击“确定”。
8. 再次点击“确定”以关闭属性窗口。

现在，我们已经配置 SharePoint 使用专用帐户爬网我们的站点，并授予该帐户访问 Tropical Green 站点内所有内容的权限。

## 为 Tropical Green 启用来宾访问

由于我们的站点将向公众开放，我们需要确保它不会要求访问者登录。要将我们的 Tropical Green 站点配置为允许来宾查看，我们需要启用来宾访问。如果你想了解如何完成此操作的完整详细信息，在第一本书《*使用 Microsoft Content Management Server 构建网站*》（Packt Publishing，2005 年 1 月，ISBN 1-904811-16-7）中给出了完整说明。另请参阅第 18 章中的“*欢迎来宾访问站点*”一节。

> 如果你已经为站点配置了来宾访问，则可以跳过此步骤。

为了允许来宾进入我们的站点，我们需要：

1. 在域中或作为服务器上的本地用户创建一个新的 MCMS 来宾帐户。
2. 使用 SCA 配置 MCMS，以允许来宾进入，并使用步骤 1 中创建的帐户作为来宾登录帐户。
3. 现在 MCMS 已配置为允许来宾进入站点，请将步骤 1 中创建的帐户添加到订阅者权限组，并授予该权限组访问站点中当前存在的所有频道、资源库和模板库的权限，但位于 Gardens 频道中的 Members 频道除外。

> 为了简单起见，我们选择启用站点的来宾访问。不过，SharePoint 也可以使用 Forms 或 Windows 身份验证来索引我们的站点。对于 Forms 身份验证，我们需要创建一个特殊的首页，该页面使用预定义帐户自动将用户登录，以便访问站点并让 SharePoint 开始爬网。我们必须授予 SharePoint 爬网帐户相应的订阅者权限组权限。
>
> 如果你选择创建一个备用首页，让 SharePoint 自动登录到你的站点，请记住，任何用户都可以使用这个页面来访问你的站点。如果使用这种方法，应特别小心，例如向此页面添加 IP 限制，以便只有 SharePoint 服务器才能访问它。

## 发布项的输出缓存和 Last-Modified 日期

在生成 HTTP 状态代码方面，ASP.NET 并不特别复杂，它只是为每个请求返回 HTTP 状态代码 200（OK），而不是发送 Last-Modified HTTP 头。当 SharePoint Portal Server 执行增量索引爬网时，它会向发现的站点上的每个页面发送 HTTP GET 请求。如果某个页面之前已被索引并返回了 Last-Modified 头，SharePoint Portal Server 会发送一个条件 HTTP GET 请求，其中包含 If-Modified-Since HTTP 头，其日期为之前 Last-Modified HTTP 头中返回的日期。如果响应是 HTTP 状态代码 304（未修改），SharePoint 将不会重新索引该页面。但是，由于 ASP.NET 总是返回状态代码 200，该站点将永远不会被 SPS 增量爬网，实际上每次执行采集器时都会进行一次完整索引。由于 MCMS 模板文件实际上是一种特殊的 ASP.NET Web 窗体，因此这也会影响基于这些模板文件的发布项。有关如何解决此问题的说明，请参阅第 4 章“准备发布项以进行搜索索引”。

需要考虑的一点是频道渲染脚本，以及包含指向其他发布项的链接的动态列表的发布项。虽然这些脚本和发布项自上次索引以来可能未发生变化，但这些脚本生成的内容可能会在发布项的不同调用之间发生变化。如果这些内容是你希望搜索的内容，则应确保包含此类控件的发布项始终被索引，方法是不返回 Last-Modified HTTP 头。

## 使用 SharePoint 搜索 MCMS

![](img/442e26bf28f2df81fbf8dff3478f88e3_140_0.png)

5. 由于 Visual Studio .NET 设计器不允许我们将控件拖放到页面的 `<head></head>` 部分，我们需要将 SearchMetaTagGenerator 控件的代码声明从页面主体移动到 head 部分。切换到 HTML 视图，找到我们刚刚添加的 SearchMetaTagGenerator 控件，并将其向上移动到 `<head>` 和 `</head>` 标记之间。
6. 与任何更改一样，我们现在应该重新生成 Tropical Green 项目。
7. 打开浏览器，在站点中导航到该站点植物目录部分中的一个植物发布项。花点时间查看你导航到的发布项的源代码。请注意所有已添加的额外 META 标签。下面是一个示例：

![](img/442e26bf28f2df81fbf8dff3478f88e3_140_1.png)

> 请注意 META 标签顶部列出的 FIRSTSAVEDBY 属性。这是已添加到发布项中的自定义属性。它会添加到 META 标签中，是因为我们在 SearchMetaTagGenerator 控件中选择了 CustomAndStandardProperties 属性类型。其他 META 标签是 SearchMetaTagGenerator 控件生成的标准属性。

渲染后，任何使用 Plant 模板实现的发布项都将在页面的 `<HEAD>` 部分为页面的每个自定义属性和标准属性包含 META 标签。

PropertyType 字段中可供我们使用的选项之一是 `PropertiesFromXMLFile`。此选项允许我们使用位于 `Microsoft Content Management Server\Server\IIS_CMS\WssIntegration\SearchPropertyCollection.xml` 的 XML 文件来精确指定哪些属性将作为 META 标签导出。

一旦你指定了要使用的属性，包括你添加的自定义属性，就需要告诉 SharePoint 在爬网时索引这些属性。MCMS Connector 附带的控制台应用程序 `SearchPropertiesSetup.exe` 会将更新后的 XML 文件告知 SharePoint。请使用以下语法运行它：

```
SearchPropertiesSetup.exe -file "<path to file>\SearchPropertyCollection.xml"
```

> SearchPropertiesSetup.exe 实用工具可在以下位置找到：
> `<install drive>:\Program Files\MCMS 2002 Connector for SharePoint Technologies\WSS\bin\`。

请按上述方式执行 SearchPropertiesSetup.exe 实用工具，因为我们的自定义搜索解决方案将使用它生成的其中一个 META 标签。

> 如果更改 SearchPropertyCollection.xml 文件，则需要重新执行 SearchPropertiesSetup.exe 实用工具。

MCMS Connector for SharePoint Technologies 包含一个帮助文件，其中介绍了如何修改此 XML 文件。请注意，微软支持知识库中有一篇文章专门说明该帮助文件说明中的错误。MSKB 文章“在 Content Management Server 2002 Connector for SharePoint Technologies 中向模板添加 SearchMetaTagGenerator 控件时出现问题”（#872932）包含更正后的说明。

我们的 Tropical Green 站点现已配置为允许来宾访问；我们的模板已经过修改，以便对 SPS 搜索更友好；并且我们已在所有渲染发布项的 `<HEAD>` 部分中包含了额外的元数据。现在，让我们继续在 SharePoint 中创建内容源来索引我们的站点。

## 配置 SharePoint Portal Server 搜索

我们的 MCMS 站点已准备好进行索引，现在转向 SPS。首先，我们将配置 SharePoint 为我们的 Tropical Green 站点建立索引。创建索引后，我们将创建一个源组，该源组将

