## #6 Portlets

在上一章中，我们了解了如何构建包含我们示例公司要求及其供应商报价信息的页面。现在，我们有兴趣向该页面添加一个图表，以图形方式显示待处理和已关闭要求的状态百分比。但是，此图表不仅在此页面需要，而且在许多其他页面也需要。因此，有必要将此图表构建为可重用组件。在这种情况下，portlet 技术可以帮助我们。

Portlet 是软件组件，可以是页面的一部分。因此，页面可以由许多 portlet 丰富，使我们能够获取各种数据源的内容。本章将帮助我们理解与该技术相关的概念以及如何将其应用于我们的 Oracle WebCenter 项目中。

在本章中，您将学习以下内容：

- JSF 规范概念
- 可以使用 WebCenter 构建的 portlet 类型
- 使用 ADF 开发 portlet
- 将 portlet 与自定义应用程序集成

## #Portlets, JSR-168 specification

规范 JSR-168 定义了 Java 技术，为 Java portlet 提供了精确的定义：

_Portlets are web components – like Servlets – specifically designed to be aggregated in the context of a composite page. Usually, many Portlets are invoked to in the single request of a Portal page. Each Portlet produces a fragment of markup that is combined with the markup of other Portlets, all within the Portal page markup._

您可以在以下页面查看此规范的更多详细信息：
http://jcp.org/en/jsr/detail?id=168

虽然定义与 servlet 进行了比较，但重要的是要注意，portlet 不能直接通过 URL 访问；相反，有必要使用 portlet 的页面类似容器。

因此，我们可以将 portlet 视为返回动态内容（HTML、WML）到 Portal 页面区域的小型 Web 应用程序。

从图形上看，我们可以将带有 portlet 的页面视为如下所示：

![](img/fccf6c511126cf79e98510edd3b343ac_146_0.png)

此外，我们必须强调，portlet 并非与页面中的其他组件隔离，还可以共享信息并响应在其他组件或 portlet 中发生的事件。





## WSRP 规范

WSRP 规范允许将 portlet 公开为 Web 服务。为此，客户端通过接口 (*.wsdl) 访问 portlet 并获取关联的图形内容。可选地，portlet 可能能够通过在其上发生的事件直接与用户交互。

这种调用方式提供以下优势：

- 共享 portlet 的门户将支持集中在单一点。
- portlet 与门户的集成简单，无需编程。
- 使用托管在不同站点上的 portlet 有助于减轻服务器负载。

## WebCenter portlets

Portlet 可以以不同方式构建，使用 Oracle WebCenter 开发的应用程序可以使用这些类型的任何 portlet。

- JSF Portlets：这种类型的 portlet 基于 JSF 应用程序，用于使用 JSF Portlet Bridge 创建 portlet。
- Web Clipping：使用此工具，我们可以仅使用浏览器以声明方式构建 portlet。这些 portlet 显示来自其他站点的内容。
- OmniPortlet：这些 portlet 可以从不同类型的数据源（XML、CSV、数据库等）检索信息，以不同方式呈现内容，例如表格、表单、图表等。
- Content Presenter：这允许将 UCM 中的内容放置到页面上，并以任何喜欢的方式或使用模板显示此内容。
- Ensemble：这是一种“混搭”或生成可在页面上显示的 portlet 或“pagelet”信息的方式。
- Programmatic Portlets：显然，除了之前便于构建 portlet 的技术之外，还可以以编程方式构建。以这种方式构建时，我们可以达到高度的个性化和控制。然而，我们需要专业的 Java 知识才能以这种方式编程。

如我们所见，构建 portlet 有多种方式；但是，为了使用 ADF Faces 框架提供的丰富组件，我们将专注于 JSF Portlets。

## 使用 ADF 开发 portlet

我们将构建的 portlet 将包含一个图表，用于显示公司请求的状态。为此，我们必须创建一个表示业务逻辑并在页面上公开此信息的模型层。

因此，我们将执行以下步骤：

1. 创建 ADF 应用程序。
2. 开发业务组件。
3. 创建图表页面。
4. 使用页面生成 portlet。
5. 部署 portlet。

> 在此示例中，我们使用页面来构建 portlet；但是，ADF 还提供通过使用 ADF TaskFlows 基于页面流创建 portlet 的功能。您可以在以下链接中找到更多信息：
> http://download.oracle.com/docs/cd/E15523_01/web.1111/b31974/taskflows.htm#BABDJEDD

## 创建 ADF 应用程序

要创建应用程序，请执行以下步骤：

1. 打开 JDeveloper。
2. 在菜单中，选择 File | New 选项以启动创建应用程序的向导。在显示的窗口中，选择 Application 类别并选择 Fusion Web Application ADF 选项，然后按 OK 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_149_0.png)

3. 接下来，输入创建应用程序的以下属性：

- Name: ParacasPortlet
- Directory: c:\\ParacasPortlet
- Application Package Prefix : com.paracasportlet

![](img/fccf6c511126cf79e98510edd3b343ac_149_1.png)

4. 单击 **Finish** 创建应用程序。

## 开发业务组件

在开始此活动之前，请确保已创建到数据库的连接。要回顾此过程，请参阅 _Chapter 3, Setting up the Development Environment_。

1. 在项目 Palette 中，右键单击 **Project Model**，并选择 **New**。

![](img/fccf6c511126cf79e98510edd3b343ac_150_0.png)

2. 在下一页，选择类别 **Business Tier | ADF Business Components** 并选择 **Business Components from Tables**。接下来，按 OK 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_150_1.png)

3. 在以下页面，您配置到数据库的连接。此时，选择连接 **db_paracas** 并按 **OK** 按钮。

4. 为了构建带有图表的页面，我们需要创建一个只读视图。因此，不要更改任何内容，只需按 **Next** 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_151_0.png)

5. 在此下一步中，我们可以创建可更新的视图。但是，我们不需要这种类型的组件。因此，不要更改任何内容。单击 **Next** 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_151_1.png)

6. 现在，我们需要允许创建只读视图。我们将在页面中使用这种类型的组件；因此选择表 REQUEST，如下所示并按 Next。

![](img/fccf6c511126cf79e98510edd3b343ac_152_0.png)

7. 我们的下一步将允许创建应用程序模块。此组件对于在整个应用程序中显示只读视图是必要的。保持此屏幕的建议值并单击 Finish 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_152_1.png)

8. 检查 Application Navigator。您必须以如下截图所示的方式排列组件：

![](img/fccf6c511126cf79e98510edd3b343ac_153_0.png)

9. 我们的查询必须确定按状态划分的请求数量。因此，将需要对创建的组件进行一些更改。首先，双击视图 **RequestView**，选择 **Query** 类别，并单击 **Edit SQL Query** 选项，如下截图所示：

![](img/fccf6c511126cf79e98510edd3b343ac_153_1.png)

## 10. 在显示的窗口中，如下所示修改 SQL 并单击 OK 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_154_0.png)

```sql
SELECT Request.STATUS,
       COUNT(*) COUNT_STATUS
FROM REQUEST Request
GROUP BY Request.STATUS
```

## 11. 我们只使用属性 Status 和 CountStatus。因此，选择 Attributes 类别，选择未使用的属性，并按 Delete selected attribute(s)，如下截图所示：

![](img/fccf6c511126cf79e98510edd3b343ac_154_1.png)

## 12. 保存所有更改并验证视图类似于如下所示：

![](img/fccf6c511126cf79e98510edd3b343ac_155_0.png)

## 创建图表页面

完成构建模型阶段后，我们将创建托管图表的页面。

1. 在 Application Navigator 中，右键单击项目 **ViewController**，然后选择 **New**。

![](img/fccf6c511126cf79e98510edd3b343ac_155_1.png)

2. 在下一窗口中，选择类别 **Web Tier | JSF**，选择选项 **JSF Page**，并单击 **OK** 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_156_0.png)

3. 然后，我们定义页面名称：`requestGraph.jspx` 并按 **OK** 按钮。

![](img/fccf6c511126cf79e98510edd3b343ac_156_1.png)

4. 现在，找到 Data Controls 调色板并将视图 **RequestView1** 拖动到页面。然后，从上下文菜单中选择 **Graphs** 选项，如下截图所示：

![](img/fccf6c511126cf79e98510edd3b343ac_157_0.png)

5. 接下来，选择 **Pie** 类别和 Pie 类型项，如下所示：

![](img/fccf6c511126cf79e98510edd3b343ac_157_1.png)

## 6. 完成上一步后，将出现一个屏幕，指示用于填充图表的属性。在 Pie 项中选择 CountStatus，在 Slice 中选择 Status。按 OK。

![](img/fccf6c511126cf79e98510edd3b343ac_158_0.png)

## 7. 最后，更改图形外观。为此，创建组件后，转到 Property Inspector，选择 Appearance 区域，并将属性 Style 的值更改为 Comet，将 3D Effect 更改为 true，如下截图所示：

![](img/fccf6c511126cf79e98510edd3b343ac_158_1.png)

## 8. 保存所有更改。





## 使用页面创建 Portlet

现在我们有了页面，将使用它。

1. 转到 Application Navigator，右键单击页面 `requestGraph.jspx` 并选择 **Create Portlet Entry**，如下图所示：

![](img/fccf6c511126cf79e98510edd3b343ac_159_0.png)

2. 在下一个窗口中，更改 Portlet 的名称和描述，如下所示，然后单击 OK 按钮。

- Portlet Name: **ParacasPortlet**
- Display Name: **ParacasPortlet**
- Portlet Title: **Requests by Status**
- Short Title: **Requests by Status**
- Description: **Requests by Status**

![](img/fccf6c511126cf79e98510edd3b343ac_159_1.png)

## Portlet 部署

1. 构建 Portlet 后，需要将其包含在我们的 Portal 中。为此，我们必须从存储库中获取此 Portlet。首先，必须确保服务器处于活动状态。要启动此服务器，请选择菜单选项 **Run | Start Server Instance**。

2. 最后一步将足以基于数据页面创建 Portlet。单击 **OK**，然后检查文件 _portlet.xml_ 和 _oracle-portlet.xml_ 是否已创建，如下图所示：

![](img/fccf6c511126cf79e98510edd3b343ac_160_0.png)

![](img/fccf6c511126cf79e98510edd3b343ac_160_1.png)

## 第 6 章

3. 控制台中的以下消息确认服务器已初始化：
DefaultServer started.

4. 右键单击 **ParacasPortlet** 应用程序，选择 **Deploy | ParacasPortlet_application1 | to | IntegratedWLSConnection**，方式与下图所示相同：

![](img/fccf6c511126cf79e98510edd3b343ac_161_0.png)

5. 在下一个窗口中，单击按钮 **OK** 以完成该过程。

![](img/fccf6c511126cf79e98510edd3b343ac_161_1.png)

[ 143 ]

## 5. 最后，为了检查 Portlet 的部署，请转到以下页面：

http://localhost:7101/ParacasPortlet-ViewController-context-root/

![](img/fccf6c511126cf79e98510edd3b343ac_162_0.png)

此页面显示服务器上已部署 Portlet 的信息，并且，其中之一，提供了接口的 WSRP 链接。这些接口是 Portlet 之间面向规范的通信的一部分。我们将使用此接口从其他站点使用 Portlet。因此，单击链接 WSRP v2 WSDL 并查看此 URL：

http://localhost:7101/ParacasPortlet-ViewController-context-root/portlets/wsrp2?WSDL

![](img/fccf6c511126cf79e98510edd3b343ac_162_1.png)

## 使用 Portlet

在构建和部署 Portlet 之后，现在是让其他应用程序重用它的时候了。为此，需要在需要使用它的应用程序中创建对该 Portlet 的引用。让我们看看此集成的步骤：

1. 在 **Application Navigator** 中，将当前应用程序 ParacasPortlet 更改为我们在 _Chapter 5, Design and Personalization_ 中构建的 Paracas Application。

![](img/fccf6c511126cf79e98510edd3b343ac_163_0.png)

> 或者，您可以使用包含在书籍代码包中的 ParacasEndOfChapter5.zip 文件。

2. 在 Application Navigator 中，找到 Application Resources 部分。然后，右键单击 **Connections** 并选择 **New Connection | WSRP Producer**。

![](img/fccf6c511126cf79e98510edd3b343ac_163_1.png)

## 3. 在下一个窗口中，将 Portlet 的注册重命名为 WsrpPortletParacas 并单击 Next。

![](img/fccf6c511126cf79e98510edd3b343ac_164_0.png)

## 4. 在端点的 URL 中，键入先前部署的 Portlet 关联的 URL 并按 Next。

http://localhost:7101/ParacasPortlet-ViewController-context-root/portlets/wsrp2?WSDL

![](img/fccf6c511126cf79e98510edd3b343ac_164_1.png)

## 5. 现在，单击 Finish 按钮以完成与 Portlet 的连接建立。

6. 让我们返回 Application Navigator 并双击页面 requestMaterial 以在编辑器中显示它。

![](img/fccf6c511126cf79e98510edd3b343ac_165_0.png)

7. 随后，将 Portlet 引用 Requests by Status 拖动到页面的 customRight 区域，如下所示：

![](img/fccf6c511126cf79e98510edd3b343ac_165_1.png)

## 8. 右键单击页面并在 Application Navigator 中选择 Run。

![](img/fccf6c511126cf79e98510edd3b343ac_166_0.png)

## 9. 最后，页面将显示以下结果：

![](img/fccf6c511126cf79e98510edd3b343ac_166_1.png)

## 摘要

本章向我们展示了 Portlet 的强大功能，这是一种可以在页面之间重用的机制。在此示例中，我们利用了 JDeveloper 提供的 JSF 和向导，快速构建了图形组件。

在本章中，我们发现了 Portlet 作为组件重用技术所提供的功能。此外，我们应用此技术使用 JDeveloper 构建了一个 Portlet。接下来，我们将该组件托管在 Oracle WebCenter 提供的存储库中。最后，我们通过我们之前在本书中构建的 Paracas 门户使用了此 Portlet。
