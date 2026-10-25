

## 步骤 7：将 portlet 部署到集成 WebLogic 服务器

从 Application Navigator 中，选中 **HelloWorldPortlet** 并右键单击。从上下文菜单中选择 Deploy | **HelloWorldPortlet** | to | **IntegratedWLSConnection**。这将把 WAR 文件部署到集成 WebLogic 服务器。

![](img/fccf6c511126cf79e98510edd3b343ac_103_0.png)

在 **Select deployment type** 对话框中，选择 **Yes** 以允许 JDeveloper 将 portlet 公开为 WSRP 服务。单击 **OK**。

![](img/fccf6c511126cf79e98510edd3b343ac_103_1.png)

在 **Deployment Configuration** 对话框中单击 **Deploy** 以完成部署。

![](img/fccf6c511126cf79e98510edd3b343ac_104_0.png)

## 步骤 7：测试 portlet

使用浏览器导航到部署 URL，`http://localhost:7101/HelloWorldPortlet/`。将显示 WSRP producer 测试页面。页面底部显示 WSDL 链接。单击 **WSRP v2 WSDL** 链接以查看 portlet producer 的 WSDL。

![](img/fccf6c511126cf79e98510edd3b343ac_104_1.png)

记下 WSDL 链接。在 WebCenter 应用程序中使用该 portlet 时会用到它。

![](img/fccf6c511126cf79e98510edd3b343ac_105_0.png)

至此，我们已开发、部署并测试了 portlet producer 应用程序。接下来，我们将创建一个 WebCenter 应用程序来使用该 portlet。

## Hello World WebCenter 应用程序

我们将创建一个 WebCenter 应用程序来使用创建的 portlet。该应用程序基于 WebCenter Application 模板，并将使用 ADF Faces 创建 Web 应用程序。

## 步骤 1：创建 WebCenter 应用程序

使用 Application | New 创建新应用程序。从应用程序模板中选择 WebCenter Application。这将确保设置所有适当的技术范围。为应用程序命名；我们使用了 HelloWorldWCApp。单击 Next。

![](img/fccf6c511126cf79e98510edd3b343ac_106_0.png)

使用 WebCenter Application 模板时会创建两个项目：Model 和 ViewController。将 Web Services 技术添加到选定的技术范围中。单击 Next。

![](img/fccf6c511126cf79e98510edd3b343ac_107_0.png)

![](img/fccf6c511126cf79e98510edd3b343ac_107_1.png)

## WebCenter 开发入门：Hello World(s)

接下来，将创建 **ViewController** 项目。默认值即可。
单击 **Finish**。

![](img/fccf6c511126cf79e98510edd3b343ac_108_0.png)

## 步骤 2：使用 portlet producer

从 Application Resources 中，选择 **Connections** 并右键单击。从上下文菜单中选择 **WSRP Producer**。我们将使用它来使用创建的 portlet。

![](img/fccf6c511126cf79e98510edd3b343ac_109_0.png)

![](img/fccf6c511126cf79e98510edd3b343ac_109_1.png)

接下来，配置 portlet producer。为此，我们需要 portlet 的 WSDL。
单击 Next。

![](img/fccf6c511126cf79e98510edd3b343ac_110_0.png)

选择超时的默认值并单击 Finish。

![](img/fccf6c511126cf79e98510edd3b343ac_110_1.png)

## 步骤 3：创建用于托管 portlet 的 JSF 页面

从 Application Navigator 中，选中 Web Content 并右键单击，然后从上下文菜单中选择 New。

![](img/fccf6c511126cf79e98510edd3b343ac_111_0.png)

从 New Gallery 中，在 Web Tier 节点下选择 JSF。在右侧窗格中，从 Items 列表中选择 JSF Page。

![](img/fccf6c511126cf79e98510edd3b343ac_111_1.png)

在 Create JSF Page 对话框中，选择文件名；我们使用了 helloworld.jspx。选择 Create as XML Document 选项并选择 Quick Start Layout。单击 OK 继续。

![](img/fccf6c511126cf79e98510edd3b343ac_112_0.png)

## 步骤 4：创建页面布局

从 Application Navigator 中，选择 HelloWorld.jsp 并为其创建页面布局。
我们创建了一个标题表格、一个用于 portlet 的中间区域和一个页脚表格。

![](img/fccf6c511126cf79e98510edd3b343ac_113_0.png)

![](img/fccf6c511126cf79e98510edd3b343ac_113_1.png)

## 步骤 5：为 WebCenter 应用程序创建部署配置文件

在 Application Navigator 中，选择 ViewController 项目并右键单击。从上下文菜单中选择 Project Properties。创建新的部署配置文件。从 Archive Type 下拉菜单中选择 WAR File。对于 Name 字段，我们选择了 HelloWorldApp。单击 OK 继续。

![](img/fccf6c511126cf79e98510edd3b343ac_114_0.png)

接下来，将 Context Root 更改为 HelloWorldApp。单击 OK 继续。

![](img/fccf6c511126cf79e98510edd3b343ac_114_1.png)

## 步骤 6：将 WebCenter 应用程序部署到集成 WebLogic 服务器

在 Application Navigator 中，选择 ViewController 项目并右键单击。
选择 Deploy | HelloWorldApp | to | IntegratedWLSConnection 将其部署到集成 WebLogic 服务器。

![](img/fccf6c511126cf79e98510edd3b343ac_115_0.png)

在 Deploy Configuration 对话框中，单击 Deploy 完成部署。

![](img/fccf6c511126cf79e98510edd3b343ac_115_1.png)

部署完成后，可以在 Deployment log 中看到 **Deployment Finished** 消息。

![](img/fccf6c511126cf79e98510edd3b343ac_116_0.png)

## 步骤 7：测试 WebCenter 应用程序

使用浏览器，导航到应用程序 URL（默认：http://localhost:7101/HelloWorldApp/faces/helloworld.jspx）。

![](img/fccf6c511126cf79e98510edd3b343ac_117_0.png)

恭喜！您已完成本教程并构建了一个完整的 Hello World WebCenter 应用程序。

## 摘要

在本教程中，您创建了一个 HelloWorld Web 服务，该服务由 HelloWorld portlet producer 使用。该 portlet producer 被 HelloWorld WebCenter 应用程序使用。

您已验证开发环境，并增强了构建 WebCenter 开发所需基础组件和技能的信心。

在下一章中，我们将从 ABC 的内联网开始构建基础页面。

# 5 设计与个性化

使用 Oracle WebCenter 开发的应用程序将传统信息系统的数据与 Wiki、讨论和博客等协作资源相结合。

因此，在本章中，我们将教您如何开发传统应用程序。在后续章节中，您将学习如何将协作资源添加到传统应用程序中，以将其转换为 Enterprise 2.0 解决方案。

在本章中，我们将涉及以下项目：

- 入门 ADF Faces Rich Client 框架
- 创建 WebCenter 应用程序
- 使用模板
- 构建数据组件
- 开发示例页面

好了，让我们开始构建传统应用程序。

## ADF Faces Rich Client 组件

ADF Faces Rich Client 是一组用于构建丰富用户界面的组件。这些组件的关键在于使用 AJAX 技术，这使用户能够享受与桌面应用程序（例如 EJM Swing 应用程序）类似的体验和视觉效果。这类应用程序称为富互联网应用程序（RIA）。

## ADF Faces 组件类型

ADF Faces Rich Client 组件可按功能分为以下类型。

| 组件 | 描述 |
|---|---|
| 布局组件 | 这些组件用于定义页面上的区域，这些区域将作为其他组件的容器。 |
| 输入组件 | 用于允许输入任何类型的信息。 |
| 表格和树组件 | 用于使用有组织的结构显示信息。 |
| LOV 组件 | 我们将使用这些组件来显示列表。 |
| 弹出组件 | 用于在页面中生成弹出窗口。 |
| 菜单和工具栏组件 | 用于在应用程序中创建选项菜单的组件类型。 |
| 输出组件 | 用于以不同格式显示信息。 |
| 标签和消息组件 | 此类组件将帮助我们在组件上放置描述性标签。 |
| 导航组件 | 用于促进页面之间的流动。 |

## 创建新应用程序

要使用 Oracle WebCenter 开发应用程序，请执行以下步骤：

1. 执行 JDeveloper，使用 Windows 开始菜单中的选项 **Oracle Fusion Middleware | JDeveloper Studio**。
2. 进入工具后，选择选项 **File | New** 以启动向导。接下来，选择 **WebCenter Application** 选项并按 **OK** 按钮，如下图所示：

