
这段代码表明 ApplicationEntityTypesProcessor<T> 泛型类与 Activity 类型相关联，用于检索特定于 FlightProxy 或 FlightDescriptionProxy 的 Activity。同样，Roo 生成的 GWT 代码使用 ApplicationEntityTypesProcessor<T> 类来执行其他特定于实体代理的处理，例如在 **主显示区域** 中呈现实体列表。

## 还有更多...

现在让我们来看看：

- 如何在嵌入式 Jetty 容器中编译和运行 Roo 生成的 GWT 应用程序
- 如何访问 Roo 生成的 flightapp-gwt 应用程序的移动版本
- Roo 生成的 GWT 应用程序中的往返支持
- 如何启用远程日志

## 在嵌入式 Jetty 容器中编译和运行 GWT 应用程序

在开发模式下，GWT 应用程序不会被编译为 JavaScript。您可以通过执行 `mvn jetty:run-exploded` 命令，将 GWT 应用程序编译为 JavaScript 并使用嵌入式 Jetty 容器运行，如下所示：

```
C:\roo-cookbook\ch05-gwt> mvn jetty:run-exploded
```

现在，您可以通过输入以下 URL 来访问 `flightapp-gwt` 应用程序：`http://localhost:8080/flightapp-gwt/index.html`

## 访问 GWT 应用程序的移动版本

如果您已使用 `mvn jetty:run-exploded` 命令编译并部署了 `flightapp-gwt` 应用程序，则可以使用以下 URL 访问 `flightapp-gwt` 应用程序的移动版本：

```
http://localhost:8080/flightapp-gwt/index.html&m=true
```

如果您正在开发模式下运行 `flightapp-gwt` 应用程序，则请使用以下 URL：

```
http://127.0.0.1:8888/ApplicationScaffold.html?gwt.codesvr=127.0.0.1:9997&m=true
```

无论哪种情况，您都会看到 flightapp-gwt 应用程序的移动版本，如下所示：

![](img/b8c379a2ed921ead3aa2b01811dc29fa_260_0.png)

在此屏幕截图中，**航班** 和 **航班描述** 选项可点击，选择它们后即可开始对 JPA 实体实例执行 CRUD 操作。

## GWT 应用程序的往返支持

如果您在 Roo 脚手架的 GWT 应用程序中添加、修改或删除了 JPA 实体中的任何字段，Roo 将相应地对 GWT 制品进行必要的更改。要查看 Roo 对脚手架 GWT 应用程序的往返支持，请从 flightapp-gwt 项目的根目录启动 Roo shell，并使用 field 命令向 FlightDescription 实体添加 aircraftModel 字段，或直接从 IDE 中编辑 FlightDescription.java 文件。作为对 aircraftModel 属性添加的响应，Roo shell 显示 Roo 采取的以下操作：

```
Updated ...FlightDescriptionProxy.java
Updated ...FlightDescriptionListView_Roo_Gwt.java
Updated ...FlightDescriptionDetailsView_Roo_Gwt.java
Updated ...FlightDescriptionDetailsView.ui.xml
Updated ...FlightDescriptionEditView_Roo_Gwt.java
Updated ...FlightDescriptionEditView.ui.xml
...
```

输出显示，Roo 会更新 GWT 实体代理 FlightDescriptionProxy 及其他 GWT 制品，以反映对 FlightDescription JPA 实体的修改。需要注意的另一重要点是，大部分修改仅限于 *\_Roo_Gwt.java 文件——即由 Roo 管理的文件。因此，如果您对不遵循 *\_Roo_Gwt.java 命名约定的文件进行更改，则 Roo 会保留此类更改（修改 *.client.managed.* 包中的 Java 文件的情况除外）。在 Roo 脚手架的 GWT 应用程序中，*\_Roo_Gwt.java 文件相当于 *\_Roo_.aj AspectJ ITD 文件，也就是说，Roo 通过仅修改 *\_Roo_Gwt.java 文件来尽量减少对脚手架 GWT 代码的影响。

## 启用远程日志

GWT 日志框架模拟 Java Logging API，从而可以记录来自 sample.roo.client 包及其子包中 Java 代码的消息。如前所述，sample.roo.client 包及其子包中包含的 Java 类会被 GWT 编译器转换为 JavaScript。GWT 中的远程日志功能使客户端代码能够将日志消息发送到服务器端日志基础架构。要配置远程日志，请在 ApplicationScaffold.gwt.xml 文件中将 gwt.logging.simpleRemoteHandler 属性的值设置为 ENABLED：

```
<set-property name="gwt.logging.simpleRemoteHandler"
value="ENABLED"/>
```

上述配置可启用消息的远程日志。GWT 提供了 RemoteLoggingServiceImpl servlet，它充当处理从客户端接收的日志消息的处理程序。您需要在 web.xml 文件中配置 RemoteLoggingServiceImpl servlet，如下所示：

```
<servlet>
<servlet-name>remoteLogger</servlet-name>
<servlet-class>
com.google.gwt.logging.server.RemoteLoggingServiceImpl
</servlet-class>
</servlet>
```

```
<servlet-mapping>
<servlet-name>remoteLogger</servlet-name>
<url-pattern>
/applicationScaffold/remote_logging
</url-pattern>
</servlet-mapping>
```

需要注意的是，RemoteLoggingServiceImpl servlet 应映射到 /<module_name>/remote_logging URL。

由于 GWT 编译器将客户端 Java 类中的方法名称转换为 JavaScript 时会对其进行混淆，因此您需要在 ApplicationScaffold.gwt.xml 文件中设置以下属性来重新符号化或反混淆它们：

```
<set-property name="compiler.emulatedStack" value="true" />
<set-configuration-property
name="compiler.emulatedStack.recordLineNumbers"
value="true" />
<set-configuration-property
name="compiler.emulatedStack.recordFileNames"
value="true" />
```

此外，您还需要使用 GWT 编译器的 `-extra` 选项创建符号映射目录，并将其放置在服务器端代码可访问的目录中，例如生成的 WAR 文件的 `WEB-INF/classes` 目录。

## 另请参阅

- 有关 Roo 如何从 JPA 实体脚手架 Spring MVC 应用程序的详细信息，请参阅第 4 章（使用 Spring Web MVC 进行 Web 应用程序开发）中的“从 JPA 实体自动生成 Spring MVC 控制器和 JSPX 视图”配方。

## 开始进行 Flex 应用程序开发

在前一个配方中，我们了解了如何使用 Roo 脚手架一个完整的 GWT 应用程序。在本配方中，我们将了解 Roo 如何简化为 Roo 项目设置 Flex。在下一个配方“从 JPA 实体脚手架 Flex 应用程序”中，我们将了解 Roo 如何生成功能齐全的 Flex 应用程序，以便对 JPA 实体实例执行 CRUD 操作。

## 准备工作

编写本书时，Flex 插件与 Spring Roo 1.1.3 及以上版本不兼容，因此请下载 Spring Roo 1.1.2。

编写本配方时，所使用的 Flex 插件是 Flex Spring Roo 插件仓库中日期为 15-Aug-2011 的快照版本 (`http://s3browse.springsource.com/browse/maven.springframework.org/snapshot/org/springframework/flex/roo.addon/org.springframework.flex.roo.addon/1.0.0.BUILD-SNAPSHOT/`)。下载名为 `org.springframework.flex.roo.addon-xx.jar` 的 JAR 文件，并将其复制到 Roo 的 bundle 目录中，或使用 `osgi start` 命令（如第 7 章“开发插件和从项目中删除 Roo”中所述）进行安装。

Flex 插件是可安装插件的一个示例。有关可安装插件的更多信息，请参见第 7 章“开发插件和从项目中删除 Roo”。

在系统中创建新目录 `C:\roo-cookbook\ch05-flex`，并从 `ch05-flex` 目录启动 Roo shell。输入 `help` 命令，检查输出中是否显示 `flex setup`、`flex removing scaffold` 和 `flex remoting all` 命令。如果在 `help` 命令的输出中看到 `flex` 命令，则表示您已成功安装 Flex 插件。

将随本书提供的 ch05_flex_app.roo 脚本复制到 ch05-flex 目录。现在，使用 script 命令执行 ch05_flex_app.roo 脚本。执行 ch05_flex_app.roo 脚本将创建 flightapp_flex Eclipse 项目，将 Hibernate 设置为持久化提供程序，将 MySQL 配置为应用程序的数据库，创建 Flight 和 FlightDescription JPA 实体，并定义 Flight 和 FlightDescription 实体之间的多对一关系。如果您使用的数据库不是 MySQL，或者连接设置与脚本中指定的不同，则请相应地修改脚本。

虽然不是必需的，但您可能还希望下载 Flash Builder 4 并将其作为 Eclipse 插件安装，以简化编辑本配方中 Roo 生成的 MXML 和 ActionScript 文件。

## 操作方法

要设置 Flex 应用程序，请按以下步骤操作：

- 1. 执行 flex setup 命令，在 flightapp_flex 项目中创建与 Spring BlazeDS 集成相关的配置制品：

```
... roo> flex setup
Created SRC_MAIN_WEBAPP\WEB-INF\flex
Created SRC_MAIN_WEBAPP\WEB-INF\flex\services-config.xml
...
Created SRC_MAIN_WEBAPP\WEB-INF\spring
Created SRC_MAIN_WEBAPP\WEB-INF\spring\flex-config.xml
Created SRC_MAIN_WEBAPP\WEB-INF\spring\webmvc-config.xml
...
Created SRC_MAIN_WEBAPP\WEB-INF\web.xml
Managed SRC_MAIN_WEBAPP\WEB-INF\web.xml
...
Managed SRC_MAIN_WEBAPP\WEB-INF\spring\webmvc-config.xml
Managed ROOT\pom.xml [Added dependency com.adobe.flex.
framework:flex-framework:4.0.0.14159]
Managed ROOT\pom.xml [Added dependency org.springframework.
flex:spring-flex-core:1.5.0.BUILD-SNAPSHOT]
...
Created ROOT\.flexProperties
Created ROOT\.actionScriptProperties
...
Created ROOT\src\main\flex\flightapp_flex_scaffold.mxml
Created SRC_MAIN_WEBAPP\flightapp_flex_scaffold.html
Created ROOT\src\main\flex\flightapp_flex_scaffold-config.xml
```
