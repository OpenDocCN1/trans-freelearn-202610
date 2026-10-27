

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

