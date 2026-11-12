

FlightDescriptionView.mxml 显示一个 **新建 FlightDescription** 按钮，如果单击它，将调用 showForm 方法。FlightDescriptionView.mxml 的 showForm 方法显示用于创建 FlightDescription 实体实例的表单（由 FlightDescriptionForm.mxml 表示），如下所示：

![](img/b8c379a2ed921ead3aa2b01811dc29fa_283_0.png)

下面的序列图显示单击新建 FlightDescription 按钮时幕后发生了什么：

![](img/b8c379a2ed921ead3aa2b01811dc29fa_283_1.png)

该序列图显示 showForm 方法创建 FlightDescriptionForm 和 FlightDescription 对象。FlightDescription 对象（对应 FlightDescription JPA 实体）充当 Web 应用程序中常见的表单支撑对象。showForm 方法将 FlightDescription 对象设置到 FlightDescriptionForm 实例中。另外，showForm 向 FlightDescriptionForm 添加了 FlightDescriptionEvent.CREATE 事件的事件侦听器。

下面的代码显示了 showForm 方法：

```
private function showForm
(flightDescription:FlightDescription = null):void {
var form:FlightDescriptionForm =
PopUpManager.createPopUp(this, FlightDescriptionForm, true)
as FlightDescriptionForm;
form.flightDescription = flightDescription != null ?
flightDescription : new FlightDescription();

form.addEventListener(FlightDescriptionEvent.CREATE,
flightDescriptionView_flightDescriptionCreateEventHandler);
}
```

这段代码表明，addEventListener 方法接受 FlightDescriptionForm 对象监听的事件类型，即 FlightDescriptionEvent.CREATE。addEventListener 还接受当 FlightDescriptionForm 对象接收到事件时要调用的处理程序方法的名称。因此，如果 FlightDescriptionForm 对象接收到 FlightDescriptionEvent.CREATE 事件，将导致调用 flightDescriptionView_flightDescriptionCreateEventHandler 方法。我们稍后会回到处理程序方法，但首先来看看 FlightDescriptionEvent.CREATE 事件是如何生成的。

下面的序列图显示，当用户按下 Save 按钮以创建 FlightDescription JPA 实体实例时，会生成 FlightDescriptionEvent.CREATE 事件：

![](img/b8c379a2ed921ead3aa2b01811dc29fa_284_0.png)

该序列图显示，单击 **Save** 按钮会导致调用 FlightDescriptionForm.mxml 中定义的 processSave 方法。processSave 方法使用 mx.validators.Validator 验证用户输入的表单数据。如果数据验证成功，则将表单数据设置到 FlightDescription ActionScript 对象中。processSave 方法现在会创建一个类型为 FlightDescriptionEvent.CREATE 的 FlightDescriptionEvent 事件，并将 FlightDescription ActionScript 对象作为事件的有效载荷传递。调用 dispatchEvent 方法会将新创建的事件分发给侦听器。

因此，接收到 FlightDescriptionEvent.CREATE 事件后，将调用 flightDescriptionView_flightDescriptionCreateEventHandler，如前所述。下面的代码显示了 flightDescriptionView_flightDescriptionCreateEventHandler 方法，它调用 FlightDescriptionService 的 create 方法来创建 FlightDescription JPA 实体的实例：

```
protected function
flightDescriptionView_flightDescriptionCreateEventHandler
(event:FlightDescriptionEvent):void {
    flightDescriptionService.create(event.flightDescription);
}
...
```

前面代码中的 flightDescriptionService 对象代表一个 mx.rpc.remoting.RemoteObject，Flex 客户端使用它来访问远程处理目标。RemoteObject 使用 <RemoteObject> 标签在 FlightDescriptionView.mxml 中定义，如下所示：

```
<s:RemoteObject channelSet="{remotingChannels}"
    destination="flightDescriptionService"
    fault="flightDescriptionService_faultHandler(event)"
    id="flightDescriptionService">
...
</s:RemoteObject>
```

在这段代码中，{remotingChannels} 标识用于与服务器端 Java 对象通信所使用的 ChannelSet。我们在前面的示例中看到，flightapp_flex 应用程序使用的远程处理通道是在 flightapp_flex_scaffold.mxml 文件中使用 <ChannelSet> 标签定义的。destination 属性指定通过 RemoteObject 访问的远程处理目标。

### 更多内容...

Flex 插件提供往返支持，即，对 JPA 实体的修改会传播到 MXML 和 ActionScript 文件中。

Flex 插件不提供用于控制构成 Spring 管理的远程处理目标一部分的方法的任何支持。例如，你无法使用 `@RooFlexScaffold` 注解来控制属于 `FlightDescriptionService_Roo_Service.aj` 文件的方法。

如果你希望 Spring 管理的远程处理目标中的某个方法对 Flex 客户端不可访问，则只需执行 push-in 重构，或者在相应的 Java 类中定义该方法并向其添加 Spring 的 `@RemotingExclude` 注解即可。

Spring Roo 使用 JPA 实体中指定的 JSR 303 注解来在 MXML 文件中添加 Flex 验证器。例如，如果 JPA 实体字段指定了 `@NotNull` JSR 303 注解，则 Roo 会添加一个 Flex `StringValidator` 或 `NumberValidator`，用于检查表单中的字段不为空白。请注意，Flex 插件对 JSR 303 注解的支持有限。

### 另请参阅

- 请参阅“Flex 应用程序开发入门”，了解如何为 Roo 项目设置 Flex

## Spring Web Flow 入门

在第 4 章，*使用 Spring Web MVC 进行 Web 应用程序开发* 中，我们了解到 Roo 简化了 Spring Web MVC 应用程序的构建。在本示例中，我们将了解 Roo 如何设置 Roo 项目以使用 Spring Web Flow——这是一个构建在 Spring Web MVC 之上的框架。

Spring Web Flow 允许将 Web 应用程序建模为一组流，其中每个流表示一个有限状态机。

> Spring Roo 1.1.3 中对 Spring Web Flow 的支持已损坏；因此，必须使用 Spring Roo 1.1.4 或 1.1.5 来执行本示例。本示例使用 Spring Roo 1.1.5 开发。

### 准备工作

在系统中创建新目录 `C:\roo-cookbook\ch05-webflow`，并将随本书附带的 `ch05_webflow_app.roo` 脚本复制到 `ch05-webflow` 目录中。

从 ch05-webflow 目录启动 Roo shell，并使用 script 命令执行 ch05_webflow_app.roo 脚本。执行 ch05_webflow_app.roo 脚本会创建 flightapp-webflow Eclipse 项目，将 Hibernate 设置为持久化提供程序，将 MySQL 配置为应用程序的数据库，并创建 Flight、FlightDescription、Customer 和 Address JPA 实体。Customer 实体与 Address 实体具有一对一关系，Flight 实体与 FlightDescription 实体具有多对一关系。如果使用的数据库不是 MySQL，或者连接设置与脚本中指定的不同，则相应地修改脚本。

### 操作步骤

1. 执行 Web 流 Roo 命令以创建 Customer 流，如下所示：

```
roo> web flow --flowName customer
Created SRC_MAIN_WEBAPP\WEB-INF\spring
Created SRC_MAIN_WEBAPP\WEB-INF\spring\webflow-config.xml
Created SRC_MAIN_WEBAPP\WEB-INF\spring\webmvc-config.xml
...
Created SRC_MAIN_WEBAPP\WEB-INF\views\customer
Created SRC_MAIN_WEBAPP\WEB-INF\views\customer\flow.xml
Created SRC_MAIN_WEBAPP\WEB-INF\views\customer\view-state-1.jspx
Created SRC_MAIN_WEBAPP\WEB-INF\views\customer\view-state-2.jspx
Created SRC_MAIN_WEBAPP\WEB-INF\views\customer\end-state.jspx
Created SRC_MAIN_WEBAPP\WEB-INF\views\menu.jspx
Created SRC_MAIN_WEBAPP\WEB-INF\views\customer\views.xml
...
Updated ROOT\pom.xml [...; added dependency org.springframework.webflow:spring-webflow:2.2.1.RELEASE]
```

2. 执行 perform eclipse 命令以更新项目的类路径设置：

```
roo> perform eclipse
```

3. 现在可以将 flightapp_webflow 项目导入 Eclipse IDE 中。

4. 退出 Roo shell，并从 ch05-webflow 目录执行 Tomcat Maven 插件的 tomcat:run 目标，将 flightapp_webflow 项目部署到嵌入式 Tomcat 容器中：

```
C:\roo-cookbook\ch05-webflow> mvn tomcat:run
```

