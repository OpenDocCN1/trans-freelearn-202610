
## 使用 Apache Tomcat 入门

Servlet 被部署到 servlet 容器中，而 servlet 容器是运行并跟踪与 servlet 相关的多个方面的组件。Apache Tomcat（或简称为 Tomcat）就是 servlet 容器的一个例子。简而言之，Tomcat 是 Apache 软件基金会（http://tomcat.apache.org）提供的开源 Web 服务器和 Java servlet 容器。Tomcat 用 Java 编写，并实现了 Oracle 公司的 Java Servlet 和 JSP 规范。作为主要是一个 servlet 容器，Tomcat 在 servlet 的生命周期中发挥着核心作用：它加载类、实例化并初始化 servlet、将 URL 映射到 servlet、强制执行对 servlet 的正确访问权限，并创建（或分配）线程来运行 servlet。大致来说，Tomcat 对于 servlet 和 JSP 就如同 JVM 对于基于桌面的应用程序——不过请记住，Tomcat 运行在 JVM 上。

Tomcat 拥有众多的功能、配置和选项。仅了解其中一小部分就能应付（我们在本章中会尽量涵盖），但你应该知道，Tomcat 远不止下载并运行起来这么简单。然而，对这个功能齐全的 servlet 容器的深入解释超出了本书的范围。如果你正在寻找有关如何快速上手使用 Tomcat 的详细参考，请务必阅读 Tanuj Khare 所著的《Apache Tomcat 7 Essentials》。

## 安装 Tomcat

在将 Tomcat 与 Eclipse 集成之前，你需要下载并安装 Tomcat。有多种方法可以让 Tomcat 运行。在光谱的一端，你可以下载 Tomcat 的源代码并自行构建和测试。另一端，你可以简单地下载其二进制版本。在这里，我们采用了 arguably 最简单的方法来安装 Tomcat：下载、配置并运行编译好的二进制文件。在本章中，我们假设你使用的是 Tomcat 的最新发行版。截至本文撰写时，最新版本是 7.0。

![](img/d7c3f2018603810fae83e1a6fd98749e_144_0.png)

> 也可通过让 Eclipse 为你下载来安装 Tomcat。只要你已安装 WTP（如何安装该插件将在后面介绍），在 Eclipse 下创建一个新服务器并将其类型设为 **Apache Tomcat** 时，你可以使用服务器定义向导上的 **Download and Install** 按钮来安装 Tomcat。你甚至可以选择要安装服务器容器的文件夹。

你可以在 `http://tomcat.apache.org/download-70.cgi` 下载最新版本的二进制版本。下载后，只需解压 Tomcat 即可安装，这会创建以下目录层次结构：

![](img/d7c3f2018603810fae83e1a6fd98749e_145_0.png)

可以说，最重要的目录是 webapps 目录。这是部署目录，需要提供给客户端浏览器的文件、JAR 文件、配置文件等都放置在其中。在本章中，我们将提供有关如何在该目录下组织 Web 应用程序的详细信息。

要在 Eclipse 外部设置和运行 Tomcat，你需要配置两个环境变量，即 `JAVA_HOME` 和 `CATALINA_HOME`。`JAVA_HOME` 环境变量应设置为 JDK 的基本路径。Tomcat 7.0 需要 Java SE 6.0 或更高版本。`CATALINA_HOME` 应指向 Tomcat 目录层次结构的基本路径。有关如何在所有操作系统上设置这些环境变量的说明，可在 `http://tomcat.apache.org/tomcat-7.0-doc/setup.html` 找到。此外，Tomcat 根目录中的文件 `RUNNING.txt` 也包含安装说明。

> 值得一提的是，虽然本章中使用 Tomcat，但这里展示的示例并非 Tomcat 专属。因此，你可以在其他 servlet 容器中部署并运行本章中展示的所有示例。

起初我们将在 Eclipse 外部执行 Tomcat，本章后面会解释如何在 Eclipse 内部执行它。在设置了上述环境变量（按照之前提供的 URL 中针对你操作系统的说明）之后，你需要切换到 Tomcat 的 bin 目录。从那里，如果你是 Windows 用户，只需输入 startup；如果是基于 Unix 的操作系统用户，则输入 startup.sh。如果一切顺利，Tomcat 已启动。要验证 Tomcat 是否正在运行，请打开浏览器并输入 http://localhost:8080。你应该会看到 Tomcat 的欢迎页面，如以下屏幕截图所示。关于此 URL，有几点说明。首先，localhost 是你用来访问本地 Web 服务器的名称。此 URL 部分解析为以下 IP 地址：127.0.0.1。至于 8080 部分，这对应于端口号。通常 Web 服务器使用端口 80。Tomcat 则配置为监听端口 8080，以避免与其他 Web 服务器发生可能的冲突。

![](img/d7c3f2018603810fae83e1a6fd98749e_146_0.png)

在某些操作系统中，例如 Ubuntu，Tomcat 会自动启动。因此，每当你想停止和启动 Tomcat 时，请使用 bin 目录中的 stop 和 restart 脚本。

Tomcat 附带一个基于 Web 的管理控制台。该应用程序名为 Manager Webapp，并且会随 Tomcat 一起自动启动。但是，在使用它之前你需要进行一些配置。为了允许登录并使用 Manager Webapp，你必须先配置位于 conf 目录中的文件 tomcat-users.xml。通过添加以下行来编辑此文件：

```
<tomcat-users>
    <role rolename="manager-gui"/>
    <user username="tomcat" password="tomcat" roles="manager-gui"/>
</tomcat-users>
```

请记住，修改 Tomcat 的配置文件后，更改不会立即生效。要通知 Tomcat 有所更改，你需要停止服务器然后重新启动它。重新启动 Tomcat 后，访问以下 URL 以访问该应用程序：

```
http://localhost:8080/manager/html
```

Manager Webapp 的 HTML 界面显示在以下屏幕截图中。如你所见，它列出了所有已部署的 Web 应用程序。Manager 允许你安装、重新加载、卸载和停止 Web 应用程序。如果你想了解如何使用 Manager Webapp 执行这些操作，请查阅随附的文档。

![](img/d7c3f2018603810fae83e1a6fd98749e_147_0.png)

## Eclipse Web Tools 平台

我们将使用 Eclipse 平台的一个扩展来帮助我们编写、部署和运行本章的应用程序。更具体地说，我们将使用 Eclipse **Web Tools Platform (WTP)**，这是一组插件，包含用于支持 Java Web 应用程序开发和部署的工具。其中包括，你可以从 Eclipse 内部启动、停止和重启 Web 服务器。因此，在部署 Web 应用程序时，你无需在 Eclipse 和 Tomcat（或你选择的服务器）之间来回切换。要了解所有 Eclipse WTP 插件及其功能的概览，请访问项目的网站：`http://www.eclipse.org/webtools/`。

你可以从项目网站下载 Eclipse WTP 的二进制发行版：`http://download.eclipse.org/webtools/downloads/`。不过，这并不是最好的方法。让 WTP 快速运行的最直接方法是下载预先打包了 WTP 组件的 Eclipse 版本：`www.eclipse.org/downloads` 页面上的 Eclipse IDE for Java EE Developers。然而，大多数 Eclipse 版本不附带 Eclipse WTP。截至本文撰写时，只有 Eclipse 4.2 提供了附带 WTP 的版本。如果你需要使用 Eclipse 3.8 与 WTP，你将不得不使用本章中解释的方法。

另外，如果你下载的是用于 Java 开发的 Eclipse 版本，你可以通过 Eclipse Update Manager 更新 IDE 来安装 Eclipse WTP，这也是一种简单的方法。

要使用 Eclipse Update Manager 安装 Eclipse WTP，请点击 **Help**，然后点击 **Install New Software**。你需要输入软件站点的地址或使用安装窗口顶部 **Work with** 下拉菜单选择现有站点。请在 **Work with** 字段中输入以下内容：
