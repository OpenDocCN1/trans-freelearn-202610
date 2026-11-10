
http://download.eclipse.org/releases/juno/ （如下截图所示）：

![](img/d7c3f2018603810fae83e1a6fd98749e_149_0.png)

前面的截图所示为可用更新列表。在浏览可用软件选项时，选择项目 **Web、XML、Java EE 和 OSGi 企业开发**，如前一截图所示。确保选中属于 **Web、XML、Java EE 和 OSGi 企业开发** 项目下的所有条目，但 **PHP 开发工具 (PDT) SDK 功能** 除外。选择所需项目后，单击 **下一步** 进行安装。单击 **下一步** 后会发生以下情况：

- 单击 **下一步** 后，Eclipse 会将您的选择与已安装的软件进行验证。此外，Eclipse 还负责管理所选插件之间的依赖关系。因此，如果所选项目需要其他软件才能运行，则所需软件也会自动下载。这就是使用 Eclipse 更新管理器更简单的原因之一。
- 在您同意许可协议（选中 **我接受许可协议条款** 单选按钮）后，Eclipse 开始安装所选扩展。
- 下载完成后，Eclipse 会提示您重新启动；单击 **是** 重新启动 Eclipse。

如果一切顺利，您将看到以下截图（与首次打开 Eclipse 时看到的类似）：

![](img/d7c3f2018603810fae83e1a6fd98749e_150_0.png)

## 配置服务器

现在，您已成功安装 Eclipse WTP。然而，在使用 Eclipse 急于开发 Web 应用程序之前，还需要进行一些配置。下一步是设置 Eclipse WTP，通过指定要使用的服务器以及其安装位置（即您解压 Tomcat 的位置）。让我们先告诉 Eclipse 我们想要使用哪个服务器。为此，您需要导航到 首选项 | 服务器 | 运行时环境。您将看到如下截图所示的 首选项 窗口。在开发过程中，每当您需要添加或删除服务器时，您都会回到此窗口：

![](img/d7c3f2018603810fae83e1a6fd98749e_151_0.png)

接下来，单击对话框右侧的 **添加...** 按钮。包含服务器列表的窗口将弹出。除了 Tomcat，Eclipse WTP 还支持其他多个服务器。如果在列表中找不到所需的服务器，您可以下载额外的服务器适配器（如以下图窗口右上角所示）。要选择 Tomcat，请展开 `Apache` 文件夹，然后选择您想要的 Tomcat 版本。在本章中，我们将使用 Apache Tomcat v7.0（如以下截图所示）。选择正确的 Tomcat 版本后，您应单击 **下一步**：

![](img/d7c3f2018603810fae83e1a6fd98749e_152_0.png)

除了选择所需的服务器运行时环境外，您还需要指定其安装目录。如以下截图所示，为了告知 Eclipse Tomcat 的安装位置，您需要输入解压 Tomcat 的根目录。实际上，您可以通过单击 **浏览...** 来浏览并选择 Tomcat 根目录。例如，在以下截图中，Tomcat 根目录为 `/usr/local/apache-tomcat-7.0.33`。之后，单击 **完成**。

![](img/d7c3f2018603810fae83e1a6fd98749e_153_0.png)

> 请确保字段 Workbench default JRE（在前一图底部）指向所需的 JDK 版本。

下一步是在 Eclipse 中创建一个新的 Tomcat 服务器。为此，导航到 窗口 | 显示视图 | 其他...。然后单击 Server 选项以展开它，如以下截图所示。选择 Servers，然后按 OK：

![](img/d7c3f2018603810fae83e1a6fd98749e_153_1.png)

现在，如以下截图所示，服务器视图显示 Tomcat 的状态。通过此视图，您可以通过右键单击 Tomcat 的图标来启动、停止和重新启动 Tomcat：

![](img/d7c3f2018603810fae83e1a6fd98749e_154_0.png)

您现在已准备好使用 Eclipse 和 Tomcat 来开发 servlet 和 JSP。不过，在实现第一个 servlet 之前，让我们先看看 servlet 是如何由 servlet 容器执行的。

## Servlet 生命周期

在继续之前，有必要先介绍一下 servlet 的生命周期。实现 servlet 生命周期有三个方法：init()、service() 和 destroy()。在处理客户端请求之前，servlet 会由 servlet 容器（在本例中为 Tomcat）实例化并初始化。servlet 容器只实例化每个 servlet 的一个实例。在此初始化阶段，容器调用 init() 以给 servlet 一个运行初始化代码的机会。Servlet 以多线程方式运行，这意味着其方法可能由多个线程调用。在收到第一个客户端请求时，容器启动一个新线程或从池中分配一个线程。然后，servlet 的执行被分配给该线程，该线程通过调用 servlet 的 service() 方法继续执行。

> 值得强调的是，init() 只执行一次。此外，容器在 init() 完成之前无法调用 service()。

service() 接受两个参数：一个请求对象（它是 javax.servlet.http.HttpServletRequest 的实例）和一个响应对象（它是 javax.servlet.http.HttpServletResponse 的实例）。容器正是通过这两个实例来访问客户端请求并生成输出。在 service() 中，会检查请求对象以确定请求的类型。确定请求类型后，service() 会将请求和响应对象分派给负责实现该请求的 HTTP 相关方法。HTTP 相关方法的两个示例是 doGet() 和 doPost()。这些方法分别对应 HTTP GET 和 POST 操作。

> 除了最常用的 doGet() 和 doPost() 之外，HttpServlet 还包含 doPut() 和 doDelete() 方法。这些方法分别对应 HTTP PUT 和 DELETE 操作。但由于这两种方法很少使用，我们将不详细介绍。您只需知道它们存在即可。

![](img/d7c3f2018603810fae83e1a6fd98749e_155_0.png)

最后，destroy() 的主要目的是给 servlet 一个清理的机会。总结一下，在实现自己的 servlet 时，大多数情况下您需要重写的方法是 init、doGet 和 doPost（如果需要清理某些资源，则可能还需要 destroy）。

## 使用 Eclipse WTP 实现您的第一个 Java servlet

首先，我们将解释如何使用 Eclipse WTP 实现 servlet，然后稍后我们再介绍如何实现 JSP。我们将创建一个相当简单的 servlet，它打印经典短语 "Hello, World!"。您会看到，使用 Eclipse WTP 编写和部署 servlet 非常轻松。使用 Eclipse WTP 开发 Web 应用程序的某些方面如此简单，以至于您可能几乎不需要输入任何内容。因此，如果您真的想认真学习如何使用 servlet 和 JSP 开发 Web 应用程序，我们建议您先浏览本章以了解如何使用 WTP，然后完全离开 Eclipse，开始手动安排 Web 应用程序的所有结构。这将帮助您更好地理解此类应用程序的组织方式以及 Eclipse 为您优雅自动化的步骤数量。在掌握了如何组装、开发和部署 Web 应用程序的要点后，您可以返回 Eclipse 以加快开发过程。我们将感兴趣的读者引导至 http://tomcat.apache.org/tomcat-7.0-doc/appdev/index.html 以获取有关 Web 应用程序开发的更多信息。

要使用 Eclipse WTP 开发 servlet 和 JSP，首先，我们需要创建一个新的动态 Web 项目：转到 文件 | 新建 | 其他... | Web。在新建向导中，展开 Web，然后选择 动态 Web 项目。您应到达以下截图所示的向导。

如果您花点时间观察 Web 选项下的项目，您会看到有两种类型的 Web 项目：动态和静态。动态 Web 项目包含动态资源，例如 servlet、JSP 以及大量元数据，仅举几项。静态 Web 项目仅包含静态资源，例如层叠样式表。

