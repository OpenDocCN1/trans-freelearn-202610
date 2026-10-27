# 第10章：扩展 Web 脚本框架

在本章中，我们将介绍：

- 扩展 web 脚本运行时以创建自定义实现
- Web 脚本容器的自定义实现
- 向 web 脚本框架添加自己的身份验证器实现
- 脚本处理器和模板处理器自定义实现
- 自定义格式

我们开始学习 Alfresco web 脚本的旅程，首先熟悉了 web 脚本。然后我们进行了一个动手练习，创建了 Alfresco 中的第一个 web 脚本，并深入了解了理解 Alfresco 中 web 脚本框架的技术知识。之后，我们对 web 脚本的构建块以及调用 web 脚本的不同方式有了深入的了解。我们还了解了如何创建 Java 支持的 web 脚本，然后详细探讨了基于 JavaScript 的 web 脚本。接下来，我们研究了 web 脚本的部署、调试和故障排除，然后学习了如何使用 Maven 设置开发环境。到目前为止，你应该已经获得了在实际项目中实际实施和使用 Alfresco web 脚本所需的知识。此外，你应该能够调试、排除故障并修复项目中 web 脚本的问题。现在，作为本书的最后一部分要学习的内容，是时候了解在 Alfresco 中扩展 web 脚本框架的可能性了。

在开始之前，你可能希望复习一下我们在 *第 3 章、理解 Web 脚本框架* 中学到的 web 脚本框架的知识。简要总结一下，web 脚本框架的核心组件是 web 脚本运行时、web 脚本容器和身份验证器。web 脚本容器的其他支持组件是脚本处理器注册表，用于注册脚本处理器；模板处理器注册表，用于注册模板处理器；以及格式注册表，用于注册新格式。我们已经了解了所有这些组件的开箱即用实现类。Alfresco 中的 web 脚本框架非常强大，并且为 web 脚本框架的核心组件提供的开箱即用实现可以满足你与项目 web 脚本实现相关的所有需求。然而，为了了解 Alfresco 中 web 脚本框架的强大之处，值得一看如何扩展 web 脚本框架的核心组件并为其添加自定义实现。一般情况下，你不需要对 web 脚本框架的组件做任何更改。然而，了解扩展 web 脚本框架的可能性将帮助你在需要时拥有自定义实现。它还将让你熟悉开源实现的强大之处，它提供了在需要时添加自定义实现的灵活性。

我们将研究如何在 Alfresco 中扩展 web 脚本框架，以实现 web 脚本运行时、web 脚本容器、身份验证器、脚本处理器和模板处理器的自定义实现，以及自定义格式。

### Web 脚本运行时的自定义实现

让我们首先简要了解 web 脚本运行时在 web 脚本框架中的作用。在 web 脚本执行流程中，一旦请求调度程序发送请求，web 脚本运行时将请求委托给 web 脚本容器以便进一步处理以执行 web 脚本。web 脚本运行时有不同的实现，例如 servlet 运行时、portlet 运行时、JSF 运行时、Facebook 运行时和 SURF web 框架运行时。作为开发者，如果要实现新的 web 脚本运行时实现，你必须感兴趣地知道需要做什么。让我们在这里看一下。

为了了解如何在 web 脚本框架中创建 web 脚本运行时的自定义实现，首先让我们看一下 web 脚本框架提供的服务提供者接口，它提供了创建自定义实现的可能性。如果你查看 `spring-webscripts-*.jar` 内部的实现，会有一个接口定义，`org.springframework.extensions.webscripts.Runtime`，它声明了任何 web 脚本运行时必须实现的方法。为了简化实现，有一个抽象类实现，`org.springframework.extensions.webscripts.AbstractRuntime`，它实现了 `org.springframework.extensions.webscripts.Runtime` 接口。它以一种通用的方式为接口中定义的方法提供了实现，使得 web 脚本框架中的任何运行时都能够以通用方式提供 web 脚本请求。AbstractRuntime 类将一些方法保留为抽象，留给子类提供抽象方法的实现，以便为 web 脚本运行时做出通用实现。如果你查看 web 脚本框架中所有现有的 web 脚本运行时，例如 WebScriptServletRuntime 和 JSFRuntime，你可以看到它们都扩展自 AbstractRuntime 类。这些类提供了 AbstractRuntime 类中所需的抽象方法的实现。

对服务提供者接口及其在 Alfresco 中开箱即用的 web 脚本框架中的实现有了整体了解后，让我们尝试理解如何利用这一知识创建 web 脚本运行时的自定义实现。我们将研究创建自定义 web 脚本运行时所需的高层次步骤。





## 扩展Web脚本框架

要创建自定义的Web脚本运行时，请执行以下步骤：

- 1. 打开Eclipse，在你的项目中，创建一个继承自`org.springframework.extensions.webscripts.AbstractRuntime`的Java类。
- 2. 你会看到Eclipse在类声明处显示红色标记。不必担心，这是因为我们还没有提供父类中抽象方法的实现。右键点击该类，选择 **Source | Override/Implement Methods**，然后点击 **OK**。你应该能看到来自`AbstractRuntime`父类的抽象方法的默认实现。以下是创建自定义Web脚本运行时需要提供实现的方法：
  
  ```java
  public String getName()
  protected String getScriptMethod()
  protected String getScriptUrl()
  protected WebScriptRequest createRequest(Match paramMatch)
  protected WebScriptResponse createResponse()
  protected Authenticator createAuthenticator()
  protected WebScriptSessionFactory createSessionFactory()
  ```
- 3. 你还需要为你的Java类提供显式的构造函数实现；否则，会显示**隐式超构造函数AbstractRuntime()对默认构造函数未定义**的错误。右键点击该类，选择 **Source | Generate Constructors from Superclass**。以下是你自定义Java类的显式构造函数的样子：
  
  ```java
  public CustomWebScriptRuntime(RuntimeContainer container) {
      super(container);
  }
  ```
- 4. 每次都应创建Web脚本运行时的新实例，并调用`executeScript()`方法，其通用实现已在`AbstractRuntime`类中提供。

Web脚本容器类必须为`AbstractRuntimeContainer`类中未实现的其他方法提供定义。为了理解我们刚刚学到的内容，你应该追踪`alfresco-remote-api-*.jar`中`org.alfresco.repo.web.scripts.RepositoryContainer`类以及`spring-webscripts-*.jar`中`org.springframework.extensions.webscripts.PresentationContainer`类的代码实现。

现在我们对Web脚本容器是如何实现的有了整体了解，下面来看一下理解如何为Web脚本容器创建自定义实现的高级步骤。要创建你自己的Web脚本容器，需要执行以下步骤：

- 1. 创建一个继承自`org.springframework.extensions.webscripts.AbstractRuntimeContainer`的Java类。
- 2. 然后，你需要为所需的抽象方法提供实现。在类内部右键点击，选择 **Source | Override/Implement Methods**，然后点击 **OK**。你应该能看到在`AbstractRuntimeContainer`父类中没有提供实现的那部分抽象方法的默认实现。以下是创建自定义Web脚本容器时需要提供实现的方法：
- 3. 然后，应将Web脚本容器在Spring上下文文件中注册为一个Spring bean条目。你可以为bean ID指定任意名称：

```
public void executeScript(WebScriptRequest req, WebScriptResponse res,Authenticator auth) throws IOException
public ServerModel getDescription()
```

```
<bean id="webscripts.custom.container" class="自定义webscript容器的完全限定类名" parent="webscripts.abstractcontainer" init-method="setup">
...
</bean>
```

现在的问题是，如何让Web脚本引擎使用自定义的Web脚本容器呢？答案是在创建Web脚本运行时的新实例时，应从上下文中获取该bean并将其注入Web脚本运行时。你应该追踪`org.springframework.extensions.webscripts.servlet.WebScriptServlet`的代码，看看Web脚本容器实例是如何获取并传递给Web脚本运行时的。

> 对于Alfresco仓库Web脚本，Web脚本引擎使用`org.alfresco.repo.web.scripts.RepositoryContainer`实现；对于Alfresco Share Web脚本，则使用`org.springframework.extensions.webscripts.PresentationContainer`实现作为Web脚本容器。

## 自定义认证器实现

为了安全地访问Web脚本，身份认证必不可少。Web脚本认证器是在Web脚本中提供认证访问的组件。我们首先了解一下Web脚本框架是如何引用认证器实现的。如果你查看Alfresco安装目录下`tomcat\webapps\alfresco\WEB-INF`中的web.xml，你会在某个servlet的`<init-param>`条目下找到`<param-name>authenticator</param-name>`条目，该servlet基本上是用于Web脚本执行的请求分发servlet。请求分发servlet在创建Web脚本运行时实例来服务Web脚本请求时，会使用上述认证器。

认证器的Spring bean ID可以在web.xml中某个servlet的init-param条目下的`<param-value>`标签中找到。Spring bean条目可以在`web-scripts-application-context.xml`或`web-client-application-context.xml`中找到。认证器的bean实例是使用工厂设计模式实现创建的。因此，你会找到与bean ID对应的工厂类名称。例如，以下是bean ID为`webscripts.authenticator.basic`的Spring bean条目代码片段：

```xml
<bean id="webscripts.authenticator.basic" class="org.alfresco.repo.web.scripts.servlet.BasicHttpAuthenticatorFactory">
```

所有与认证器相关的工厂类都实现了`org.springframework.extensions.webscripts.servlet.ServletAuthenticatorFactory`接口，并实现`create(WebScriptServletRequest req, WebScriptServletResponse res)`方法来返回相应的认证器对象。你会在工厂类内部找到内部类实现，该类代表该工厂类提供的认证器对象。

现在，让我们来看看用于创建认证器对象的服务提供者接口。在Web脚本框架中，要创建新的认证器，必须实现`org.springframework.extensions.webscripts.Authenticator`接口，并且必须为该接口的`authenticate()`和`emptyCredentials()`方法提供实现。

现在我们对认证器实现如何在Web脚本框架中使用有了整体了解，接下来看一下创建自定义认证器并让Web脚本框架在服务Web脚本请求时使用新建认证器所需的高级步骤。执行以下步骤：

- 1. 创建一个实现`org.springframework.extensions.webscripts.Authenticator.ServletAuthenticatorFactory`接口的自定义认证器工厂类。
- 2. 为以下方法提供实现；此时，只需让它返回null以避免出现任何错误：
  
  ```
  public Authenticator create(WebScriptServletRequest req,
  WebScriptServletResponse res)
  ```
- 3. 在Spring上下文文件中为这个类提供Spring bean条目，如下所示：
  
  ```
  <bean id="webscripts.custom.authenticator" class="自定义认证器工厂类的完全限定类名">
  ...
  </bean>
  ```
- 4. 在自定义认证器工厂类内部，创建一个实现`org.springframework.extensions.webscripts.Authenticator`的自定义认证器类，并为以下两个方法提供实现：
  
  ```
  public boolean authenticate(Description.RequiredAuthentication required, boolean isGuest)
  public boolean emptyCredentials()
  ```
- 5. 现在，在我们第2步实现的方法内部，不要返回null，而是返回我们刚刚实现的自定义认证器的新对象。
- 6. 为了确保Web脚本请求分发servlet使用自定义认证器，请修改servlet的web.xml条目，将其authenticator的init-param值提供为自定义认证器工厂的bean ID，如第3步所定义。




![](img/f02f759059b86af67e38b543d498a0d8_167_0.png)

> 为了更好地理解我们刚刚学习的如何创建自定义认证器，你应该通读开箱即用的实现代码，看看 `BasicHttpAuthenticator` 是如何被 Web 脚本框架实现和使用的。以下文件名供你参考：请参阅 `web.xml` 中的 `apiServlet` 条目、`web-scripts-application-context.xml`、`org.alfresco.repo.web.scripts.servlet.BasicHttpAuthenticatorFactory`、`org.springframework.extensions.webscripts.servlet.WebScriptServlet`。

## 自定义脚本处理器实现

我们现在应该有了一个高层次的了解：Alfresco 中的 Web 脚本框架支持基于 JavaScript 的 Web 脚本，并使用 RhinoScript 处理器来执行基于 JavaScript 的 Web 脚本。在 `spring-webscripts-*.jar` 提供的开箱即用 Web 脚本框架实现中，脚本处理器实现是 `org.springframework.extensions.webscripts.processor.JScriptProcessor`，并已注册到脚本处理器注册表中。然而，在 Alfresco 中，有一个扩展实现，用于将脚本处理器注册到 Alfresco Web 脚本框架的 Web 脚本处理器注册表中。让我们尝试理解 JavaScript 处理器是如何在 Alfresco 中与 Web 脚本框架配置的。如果你查看 Alfresco 安装目录下的 `tomcat\webapps\alfresco\WEB-INF\classes\alfresco` 中的 `web-scripts-application-context.xml`，会看到 `org.springframework.extensions.webscripts.ScriptProcessorRegistrar` 类的一个 bean 条目。如果你使用的是 Alfresco Community 5，该文件位于 `alfresco` 包下的 `alfresco-remote-api-*.jar` 中。以下是来自 `web-scripts-application-context.xml` 的 bean 条目代码片段：

```xml
<bean class="org.springframework.extensions.webscripts.ScriptProcessorRegistrar" init-method="init">
    <property name="registry" ref="webscripts.repo.registry.scriptprocessor" />
    <property name="factory" ref="webscripts.repo.scriptprocessor.factory" />
    <property name="name"><value>Repository Script Processor</value></property>
    <property name="extension"><value>js</value></property>
</bean>
```

ScriptProcessorRegistrar 类负责将用于 JavaScript 的仓库脚本处理器注册到 Web 脚本框架中。在 ScriptProcessorRegistrar 类中，由工厂类 bean ID webscripts.repo.scriptprocessor.factory 提供的 ScriptProcessor 会注册到由 bean ID webscripts.repo.registry.scriptprocessor 指定的脚本处理器注册表类中。

由上述工厂类返回的 ScriptProcessor 由以下 bean 条目表示：

```xml
<bean id="webscripts.repo.scriptprocessor" class="org.alfresco.repo.web.scripts.RepositoryScriptProcessor">
    <property name="scriptService" ref="scriptService" />
    <property name="searchPath" ref="webscripts.searchpath" />
</bean>
```

当执行基于 JavaScript 的控制器时，在 Web 脚本框架的内部执行过程中，ScriptProcessor 类的 executeScript 方法会在 AbstractWebScript 类中被调用。如果你查看 RepositoryScriptProcessor 类中的 executeScript 方法，它会调用 `org.alfresco.repo.processor.ScriptServiceImpl` 类的 executeScript 方法，并将初始参数传递为 javascript。进一步地，查看 `script-services-context.xml` 中的 scriptService bean 条目，有一个名为 javascript 的默认脚本处理器。名为 javaScriptProcessor 的 bean ID 包含了 javascript 处理器实现 `org.alfresco.repo.jscript.RhinoScriptProcessor` 的条目。该处理器通过从 BaseProcessor 类（RhinoScriptProcessor 的父类）调用的 ScriptService 的 registerScriptProcessor 方法进行注册。

我们刚刚深入了解了 javascript 处理器在 Alfresco Web 脚本框架中是如何注册和使用的技术细节。为了创建自定义脚本处理器并将其注册到 Web 脚本框架中，你需要在高层面上完成以下步骤：

- 1. 创建一个脚本处理器实现，扩展 BaseProcessor 类并实现 ScriptProcessor、ScriptResourceLoader 和 InitializingBean 接口。你应该详细研究 RhinoScriptProcessor 的实现方式，以便创建你自己的自定义脚本处理器。
- 2. 创建一个类似于 bean ID webscripts.repo.scriptprocessor.factory 的自定义工厂实现，以返回你的自定义脚本处理器对象。
- 3. 创建一个类似于 `ScriptProcessorRegistrar` 的注册器类实现，引用自定义工厂实现，以便将其注册到 Web 脚本框架中。

## 自定义模板处理器实现

既然我们已经对 Web 脚本框架中的脚本处理器实现进行了详细的技术演练，现在是时候做一些练习，来了解 Web 脚本框架中的模板处理器实现方式，以及如何为模板处理器创建自定义实现。你现在应该能够通过代码追踪，了解 FreeMarker 模板处理器是如何被 Web 脚本框架使用的，以及如何创建自定义模板处理器实现。这里有一个提示帮助你完成这个练习。为了理解模板处理器实现，你只需要查看 `web-scripts-application-context.xml` 中 `org.springframework.extensions.webscripts.ScriptProcessorRegistrar` 的 bean 条目，并按照我们在上一节中的方式追踪代码；你应该会对模板处理器实现以及创建自定义模板处理器的细节有一个全面的了解。希望你喜欢这个练习。

## 自定义格式

让我们首先了解不同的格式是如何注册到 Web 脚本框架中的。如果你查看 `spring-webscripts-*.jar` 中 `spring-webscripts-application-context.xml` 里 ID 为 `webscripts.formats` 的 bean 条目，它会包含与可用格式相对应的格式和 MIME 类型条目。该 bean 定义条目 `webscripts.formats` 的父类 `org.springframework.extensions.webscripts.FormatMap` 会将格式和 MIME 类型注册到 Web 脚本框架中 Web 脚本容器使用的 `FormatRegistry`。要添加自定义格式，你只需要提供相应的格式和 MIME 类型条目。如果你查看 `spring-webscripts-application-context.xml` 中的 bean ID `webscripts.formats.IE`，以及它在 `web-scripts-application-context.xml` 中的自定义条目，会发现它定义了自定义的 IE 特定格式。它将 rss 的格式从 text/xml 改为了 application/rss+xml。类似地，你也可以对格式进行自定义更改。如果你计划添加一个在 `spring-webscripts-application-context.xml` 中未定义的完全新的格式和 MIME 类型，那么你将需要创建自定义的 `FormatReader` 类和自定义的 `FormatWriter` 类。你可以查看 `webscripts.adaptors` bean 条目；它会为你提供一些关于如何创建自定义 FormatReader 和 FormatWriter 并注册到 Web 脚本框架的 `FormatRegistry` 的提示。

## 总结

在本书的最后一章中，我们了解了 Alfresco Web 脚本框架中的一些关键组件如何在需要时进行自定义。在理解如何自定义的同时，我们也了解了这些组件目前在 Web 脚本框架中的工作方式。在本章中，我们领略了开源技术的强大之处，它为我们按需进行自定义实现打开了大门。

现在，没有下一章了！我希望你享受了学习 Alfresco Web 脚本的旅程。现在是时候享受使用 Alfresco 进行 Web 脚本开发的体验，根据你的业务需求创建出色的解决方案了。




