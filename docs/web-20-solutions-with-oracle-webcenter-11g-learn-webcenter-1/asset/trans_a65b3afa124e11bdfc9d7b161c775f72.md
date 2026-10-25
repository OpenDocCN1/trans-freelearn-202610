

## 设置开发环境

3. 在下一个窗口中，将应用程序名称更改为 Test 并单击 Finish。

![](img/fccf6c511126cf79e98510edd3b343ac_70_0.png)

4. 使用选项 **运行 | 启动服务器实例** 启动 WebLogic Server。

![](img/fccf6c511126cf79e98510edd3b343ac_70_1.png)

5. 日志控制台中的消息 **DefaultServer 已启动** 表示服务器已启动。

![](img/fccf6c511126cf79e98510edd3b343ac_71_0.png)

6. 最后，要检查我们的 portlet 容器是否处于活动状态，请选择选项 **帮助 | WebCenter 预配置服务器自述文件**。

![](img/fccf6c511126cf79e98510edd3b343ac_71_1.png)

## 设置开发环境

7. 然后，您将看到一个带有 WebCenter 服务器提供的各种服务链接的页面。要检查 portlet 容器的可用性，请单击链接 **PortalTools 欢迎页面**。

![](img/fccf6c511126cf79e98510edd3b343ac_72_0.png)

8. 互联网浏览器必须显示创建 portlet 的页面，这确认 portlet 容器已成功安装。

![](img/fccf6c511126cf79e98510edd3b343ac_72_1.png)

## 摘要

本章帮助我们准备了开发环境和应用程序服务器，以支持我们的 WebCenter 应用程序。

我们将使用 JDeveloper 开发 WebCenter 解决方案，并将其容器 WebLogic 用作 portlet 的存储库。

此外，我们还安装了服务器版本管理软件，这将帮助我们保持对项目更改的控制，并且还将允许多名开发人员参与解决方案的构建。

一旦我们的开发环境准备就绪，在下一章中，我们就可以开始使用 JDeveloper 构建我们的第一个 WebCenter 应用程序了。

> 请注意，WebLogic Server（已包含 WebCenter Services）运行在端口 7101 上。您可以从选项 **视图 | 运行管理器** 中更改端口。

# 4 开始 WebCenter 开发：Hello World(s)

在我们开始开发 Enterprise 2.0 内联网之前，我们将创建一个简单的 WebCenter 应用程序来演示工具、开发方法论和自定义 WebCenter 应用程序的部署。

本章将详细介绍逐步说明，为您提供在实现本书余下部分的示例以及您自己的项目时所需的知识。

我相信您注意到了本章标题中包含短语 "Hello World(s)"，其中的 "(s)" 非常突兀。这是有意为之，但并非表示我们要构建多个 WebCenter 示例，也不是为了潜意识地宣告和承认外星种族的存在。事实是，当单个 WebCenter Hello World 应用程序由几个 Hello World 组件构建时，其完整性最能体现。我们将向您展示一个 Web 服务，由 portlet 消费，再由 WebCenter 应用程序消费。

- **HelloWorldWS**：一个简单的 Web 服务，公开一个单一方法，用于向输入的名称打招呼
- **HelloWorldPortlet**：一个基于标准的 Java Portlet，使用 HelloWorldWS Web 服务
- **HelloWorldApp**：一个消费 HelloWorldPortlet 的 WebCenter 应用程序

![](img/fccf6c511126cf79e98510edd3b343ac_76_0.png)

为了明确，最简单的 WebCenter Hello World 应用程序不需要所有这些组件，但我们选择这条路，以便您拥有更广泛的基础知识。

# 先决条件

本教程使用在 Windows 平台上运行的 JDeveloper 11g。我们将使用 JDeveloper 中集成的 WebLogic Server 来测试本教程过程中开发的应用程序。本章不会介绍操作系统或 JDeveloper 11g 的安装过程。

# 创建 Hello World Web 服务

我们将从基于 Java 类创建一个 `HelloWorld` Web 服务开始。该类公开一个方法 `sayHello`，该方法将字符串 `name` 作为输入参数，并输出 `Hello name`。

# 步骤 0：启动 Oracle JDeveloper 11g

JDeveloper 的欢迎屏幕显示 **每日提示**。这是学习 JDeveloper 各种功能的好方法。关闭 **每日提示** 窗口。

![](img/fccf6c511126cf79e98510edd3b343ac_77_0.png)

# 步骤 1：创建新应用程序

在应用程序导航器中，单击 **新建应用程序** 以开始创建应用程序。您也可以通过从主菜单单击 **应用程序 | 新建** 来执行相同操作。

![](img/fccf6c511126cf79e98510edd3b343ac_77_1.png)

# 步骤 2：配置应用程序设置

从应用程序模板列表中，选择 **通用应用程序**。当您打算手动选择项目的技术范围时，通用应用程序模板非常合适。为应用程序选择一个名称，本例中为 `HelloWorldWSApp`。单击下一步。

> 请注意列表末尾存在各种模板，包括 `Portlet Producer Application` 和 `WebCenter Application`。如果您找不到上述模板，那么最可能的原因是未安装 JDeveloper 的 WebCenter 扩展。

![](img/fccf6c511126cf79e98510edd3b343ac_78_0.png)

# 步骤 3：配置项目设置

在项目名称对话框中，为项目选择一个名称，本例中为 HelloWorldWS。请注意可包含在项目中的各种可用技术。对于开发 Web 服务，我们必须将 Web Services 包含在项目范围内。选择 Web Services 后，Java 会自动作为先决条件包含。单击完成。

![](img/fccf6c511126cf79e98510edd3b343ac_79_0.png)

# 步骤 4：创建 Java 类

接下来，我们将创建一个供 Web 服务使用的 Java 类。在应用程序导航器中，右键单击 HelloWorldWS 项目，并从上下文菜单中选择新建。

![](img/fccf6c511126cf79e98510edd3b343ac_80_0.png)

在新建画廊中，选择常规节点。右侧显示可用项目列表。从列表中选择 Java 类，然后单击确定。

![](img/fccf6c511126cf79e98510edd3b343ac_80_1.png)

为类选择一个名称，本例中为 HelloWorld，然后单击确定。

![](img/fccf6c511126cf79e98510edd3b343ac_81_0.png)

编辑 HelloWorld.java 文件，添加一个 sayHello 方法，该方法将字符串作为输入，并返回带有 Hello 前缀的输入。单击生成以编译该类。消息 - 日志窗口应显示编译成功。

![](img/fccf6c511126cf79e98510edd3b343ac_81_1.png)

# 步骤 5：从 Java 类创建 Web 服务

在应用程序导航器中，选择 `HelloWorld.java` 文件并右键单击它。从上下文菜单中，选择 **创建 Web 服务**。这将启动 Web 服务创建向导。

![](img/fccf6c511126cf79e98510edd3b343ac_82_0.png)

我们将为 Weblogic 10.3 创建一个 Java EE 1.5 Web 服务。从 **平台** 列表中选择适当的平台。

# 选择部署平台

选择与您的要求匹配的部署平台。

## 平台

- J2EE 1.4 JAX-RPC，支持 10.3 WLS  
  支持将 JAX-RPC Web 服务部署到 WLS 10.3 版。

- Java EE 1.5，支持 JAX-WS 注释  
  支持使用 JAX-WS 注释规范的 Java 注释部署到 10.3 WLS。

- Java EE 1.5，支持 JAX-WS RI  
  支持部署到任何支持 Sun JAX-WS 参考实现的容器。

在 **常规选项** 对话框中，可以配置 Web 服务名称和端口。我们选择了 `HelloWorldService` 作为 Web 服务名称，`HelloWorldPort` 作为端口名称。本教程其余步骤的默认值均可接受。单击 **完成** 以完成此步骤。

# 生成选项

选择要发布的 Java 类或无状态会话 EJB，以及 Web 服务的 J2EE 和注释选项。

要发布的组件：  
helloworlds.HelloWorld

Web 服务名称：  
HelloWorldService

端口名称  
HelloWorldPort

添加 SEI

