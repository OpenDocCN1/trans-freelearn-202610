
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

