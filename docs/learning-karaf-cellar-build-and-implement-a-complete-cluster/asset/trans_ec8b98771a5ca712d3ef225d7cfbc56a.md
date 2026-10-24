

## Apache Karaf – Provisioning
and Clusters

**Open Software Gateway initiative (OSGi)** 长期以来一直被“隐藏”，仅保留给中间件，如 IDE 或应用服务器。然而，OSGi 可应用于许多不同的上下文和应用。OSGi 应用需要一个运行环境。Apache Karaf 是一个轻量级、功能强大且面向企业的 OSGi 容器，您可以在其中部署应用程序。在生产系统上，特别是关键任务平台，有意义的是能够管理一组 Apache Karaf 容器，并将应用程序的部署（或配置）分发到这些不同的实例。

在本章中，我们将涵盖以下主题：

- 什么是 OSGi 及其关键特性？
- OSGi 框架的作用
- OSGi 基础构件 – OSGi bundle 以及 bundle 之间依赖关系的概念
- Apache Karaf OSGi 容器以及在容器中配置应用程序
- 如何在多个 Karaf 实例上管理配置？

### The OSGi bundle

在 OSGi 中，组件被打包为 OSGi bundle。OSGi bundle 是一个简单的 Java JAR（Java ARchive）文件，其中包含 OSGi 框架使用的额外元数据。这些元数据存储在 JAR 文件的清单文件中。

以下是元数据：

```
Manifest-Version: 1.0
Bundle-ManifestVersion: 2
Bundle-Version: 2.1.6
Bundle-Name: My Logger
Bundle-SymbolicName: my_logger
Export-Package: org.my.osgi.logger;version=2.1
Import-Package: org.apache.log4j;version="[1.2,2)"
Private-Package: org.my.osgi.logger.internal
```

我们可以看到 OSGi 非常描述性且冗长。我们明确描述所有 OSGi 元数据（头），包括我们以指定版本或版本范围导出的或导入的包。

由于 OSGi 头定义在 JAR 文件中包含的 META-INF/MANIFEST 文件中，这意味着 OSGi bundle 是一个常规 JAR 文件，您可以在 OSGi 之外使用它。

OSGi 框架的生命周期层是安装、启动、停止、更新和卸载 OSGi bundle 的 API。

### Dependency between bundles

OSGi bundle 可以以两种方式使用 OSGi 框架中的其他 bundle。

第一种方式是静态代码共享。当我们说这个 bundle 导出包时，意味着一个 bundle 可以向其他 bundle 暴露一些代码。另一方面，当我们说这个 bundle 导入包时，意味着一个 bundle 可以使用其他 bundle 的代码。

例如，我们有 bundle A（打包为 bundleA.jar 文件），其 META-INF/MANIFEST 文件如下：

```
Manifest-Version: 1.0
Bundle-ManifestVersion: 2
Bundle-Version: 1.0.0
Bundle-Name: Bundle A
Bundle-SymbolicName: bundle_a
Export-Package: com.bundle.a;version=1.0
```

我们可以看到 bundle A 暴露（导出）com.bundle.a 包，版本为 1.0。另一方面，我们有 bundle B（打包为 bundleB.jar 文件），其 META-INF/MANIFEST 文件如下：

```
Manifest-Version: 1.0
Bundle-ManifestVersion: 2
Bundle-Version: 2.0.0
Bundle-Name: Bundle B
Bundle-SymbolicName: bundle_b
Import-Package: com.bundle.a;version="[1.0,2)"
```

我们可以看到 bundle B 导入（因此将使用）com.bundle.a 包，版本在 1.0 到 2（不包括）之间。因此，这意味着 OSGi 框架将连接这些 bundle，因为 bundle A 提供了 bundle B 使用的包（因此约束得到解决）。

这种机制类似于常规 Java 应用程序，但不是将所需的 JAR 文件嵌入到您的应用程序中，您只需声明预期的代码即可。OSGi 框架负责不同 bundle 之间的链接；这是由 OSGi 框架的模块层完成的。这种方法在您想要使用未为 OSGi 原生设计的代码时很有趣。这是组件重用的进步。然而，它对本章前面看到的目的，尤其是可见性和发现性，提供了一个有限的答案。

OSGi bundle 可以使用 OSGi 框架中的其他 bundle 的第二种方式更有趣。它使用面向服务的架构（SOA）用于低级组件。这里，除了暴露代码之外，OSGi bundle 还暴露 OSGi 服务。另一方面，另一个 bundle 可以使用 OSGi 服务。OSGi 框架的服务层提供服务注册表和所有用于连接服务的管道机制。

OSGi 服务提供了一个非常动态的系统，为 bundle 提供发布-查找-绑定模型。

![](img/86f349f6a758f2c3088dbeb4d35dcaca_22_0.png)

### Downloading the example code

您可以从 http://www.packtpub.com 在您的账户中下载您购买的所有 Packt 书籍的示例代码文件。如果您在其他地方购买了本书，您可以访问 http://www.packtpub.com/support 并注册以直接将文件发送到您的电子邮件。

- Apache Karaf 是一个面向企业的容器，并提供面向企业的特性。以下企业特性默认未安装（为最小化容器的默认大小和占用空间），但一个简单的命令允许您使用企业功能扩展容器：
  - WebContainer 允许您部署 **Web Application Bundle (WAB)** 或 WAR 文件。得益于 Pax Web，Apache Karaf 是一个完整的 HTTP 服务器，支持 JSP/servlet。
  - **Java Naming and Directory Interface (JNDI)** 在 Apache Karaf 中添加命名上下文支持。您可以将 OSGi 服务绑定到 JNDI 名称并使用该名称查找这些服务，得益于 Aries 和 Xbean naming。
  - **Java Transaction API (JTA)** 允许您在 Apache Karaf 中添加事务引擎（作为 OSGi 服务公开），得益于 Aries JTA。
  - **Java Persistence API (JPA)** 允许您在 Apache Karaf 中添加持久性适配器（作为 OSGi 服务公开），得益于 Aries JPA。现成的持久性引擎也可以非常轻松地安装（尤其是 Apache OpenJPA 和 Hibernate）。
  - **Java Database Connectivity (JDBC)** 或 **Java Message Service (JMS)** 是方便的特性，允许您轻松创建 JDBC DataSources 或 JMS ConnectionFactories 并直接在 shell 控制台中使用它们。
- 如果您可以使用 shell 命令和 JMX MBeans 完全管理 Apache Karaf，您还可以安装 Web Console。此 Web Console 使用 Felix Web Console，允许您使用简单浏览器管理 Karaf。

得益于这些特性，Apache Karaf 是一个完整、丰富且面向企业的容器。我们可以将 Apache Karaf 视为一个 OSGi 应用服务器。

### Provisioning in Apache Karaf

此外，Apache Karaf 提供了三种核心功能，可在 Apache Karaf 内部使用，也可被部署在容器中的外部应用程序使用：

- OSGi bundle 管理
- 配置管理
- 使用 Karaf Features 的配置

正如我们之前学到的，OSGi 中的默认构件是 bundle。再次强调，它是一个常规 JAR 文件，其中 MANIFEST 文件包含额外的 OSGi 元数据。bundle 由 OSGi 框架直接管理，但为了方便，Apache Karaf 将 bundle 的使用封装在特定的命令和 MBeans 中。

bundle 有特定的生命周期。特别是在您安装 bundle 时，OSGi 框架会尝试解析 bundle 所需的所有依赖项，以将其提升为已解析状态。以下是 bundle 的生命周期：

![](img/86f349f6a758f2c3088dbeb4d35dcaca_26_0.png)

OSGi 框架检查是否有其他 bundle 提供您的 bundle 导入的包。对于 OSGi 服务，等效操作在您启动 bundle 时执行。这意味着一个 bundle 可能需要大量其他 bundle 才能启动，以此类推对于传递性 bundle。

此外，一个 bundle 可能需要配置才能工作。Apache Karaf 提出了一种非常方便的方式来管理配置。etc 文件夹会定期监控以发现新的配置文件并加载相应的配置。另一方面，您有专用的 shell 命令和 MBeans 来管理配置（和配置文件）。如果一个 bundle 需要配置才能工作，您首先必须在 etc 文件夹中创建配置文件（使用预期的文件名）或使用 `config:*` shell 命令或 ConfigMBean 来创建配置。

考虑到 OSGi 应用程序是一组 bundle，手动安装 OSGi 应用程序可能很长且痛苦。

