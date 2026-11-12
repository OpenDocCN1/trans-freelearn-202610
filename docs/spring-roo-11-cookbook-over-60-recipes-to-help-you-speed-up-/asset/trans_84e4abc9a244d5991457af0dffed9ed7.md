
如果你想在 Spring MVC 控制器实现中将请求重定向或转发到另一个 URL，则不需要返回视图名称，而是可以返回带有 `redirect:` 或 `forward:` 前缀的字符串值。如果控制器返回重定向或转发 URL，则 `UrlBasedViewResolver` 不执行视图解析。相反，它将请求重定向或转发到控制器返回的 URL。

由于 Roo 使用 Apache Tiles 2 框架来简化 JSP 开发，`UrlBasedViewResolver` 的 `viewClass` 属性设置为 `TilesView`。Tiles 支持通过 Spring 的 `TilesConfigurer` 在 Web 应用程序上下文中配置，如我们稍后将看到。TilesView 对应的 Tile 定义名称是由 `UrlBasedViewResolver` 解析的 *实际视图 URL*。在 *飞行应用* 的示例中，我们讨论过，`UrlBasedViewResolver` 生成的 *实际视图* URL 与视图的逻辑名称相同；因此，Tile 定义的名称也与控制器返回的视图逻辑名称 *相同*。

要全面了解 Roo 生成的 Web 应用程序中视图是如何解析的，请考虑以下 `<view-controller>` 元素配置的控制器：

```
<mvc:view-controller path="/" view-name="index" />
```

如前所述，由给定 `view-controller` 元素配置的 `ParameterizableViewController` 将返回视图名称为 `index`。由于未配置 `UrlBasedViewResolver` 的 `suffix` 或 `prefix` 属性，实际视图 URL 也为 `index`。现在，TilesView 指向与 `UrlBasedViewResolver` 生成的 *实际视图* URL 值同名的 Tile 定义；因此，Tile 定义的名称也是 `index`。总结一下，我们之前看到的配置了的 `ParameterizableViewController` 将显示其 Tile 定义名称为 `index` 的视图（你可以在 `WEB-INF/views/views.xml` 文件中找到此 Tile 定义名称）。

## Tiles 定义

Tiles 定义在 XML 文件中定义，并通过 `TilesConfigurer` 在 Web 应用程序上下文中配置，如下所示：

```
<bean class="org.springframework.web.servlet.view.tiles2.TilesConfigurer" id="tilesConfigurer">
<property name="definitions">
<list>
<value>/WEB-INF/layouts/layouts.xml</value>
<value>/WEB-INF/views/**/views.xml</value>
</list>
</property>
</bean>
```

`TilesConfigurer` 的 `definitions` 属性指定 Tiles 定义 XML 文件。这些文件的路径也可以使用通配符。例如，`/WEB-INF/views/**/views.xml` 路径会加载 `/WEB-INF/views/` 目录中所有 `views.xml` 文件的 Tiles 定义。

## 异常处理

在典型的 Web 应用程序中，需要优雅地处理控制器抛出的异常。当控制器抛出异常时，DispatcherServlet 会使用 Spring 的 HandlerExceptionResolver 来解析异常。Roo 将 SimpleMappingExceptionResolver（HandlerExceptionResolver 的一个实现）配置为飞行应用程序的异常解析器，如下所示：

```
<bean
    class="org.springframework.web.servlet.handler.
    SimpleMappingExceptionResolver"
    p:defaultErrorView="uncaughtException">
    <property name="exceptionMappings">
        <props>
            <prop key=".DataAccessException">dataAccessFailure</prop>
            ...
        </props>
    </property>
</bean>
```

SimpleMappingExceptionResolver 将异常类名映射到错误视图名称。在给定的代码中，此映射通过 exceptionMappings 属性设置。你可以指定异常的完全限定类名，也可以使用子字符串将多个异常类名匹配到一个错误视图。例如，.DataAccessException 将映射到 my.custom.DataAccessException 以及 org.springframework.dao.EmptyResultDataAccessException，在这两种情况下，DispatcherServlet 都会尝试呈现 dataAccessFailure 视图。SimpleMappingExceptionResolver 的 defaultErrorView 属性用于标识当未找到异常映射时应解析到的视图。

## 其他配置

Spring Roo 还在 webmvc-config.xml 中配置了以下类：

- ReloadableResourceBundleMessageSource：Spring 的内置 MessageSource 实现，用于从 WEB-INF/i18n 文件夹加载资源包。
- ResourceBundleThemeSource：Spring 的内置 ThemeSource 实现，用于为 Web 应用程序支持的每个主题加载 ResourceBundle。在飞行应用程序的上下文中，此配置加载 alt.properties 和 standard.properties 文件。
- CookieThemeResolver：Spring 的内置 ThemeResolver 实现，用于在浏览器中存储 Cookie 以识别用户选择的主题。
- CookieLocaleResolver：Spring 的内置 LocaleResolver 实现，用于在浏览器中存储 Cookie 以识别用户选择的语言环境。
- CommonsMultipartResolver：Spring 的内置 MultipartResolver 实现，它使用 Jakarta Commons FileUpload 1.2 (http://commons.apache.org/fileupload/) 或更高版本来支持 Web 应用程序中的文件上传。

现在，既然我们知道 Spring Roo 在我们的飞行应用程序中创建的配置，让我们来看看 Roo 生成的控制器类。

## Roo 生成的控制器

Roo 为每个尚未有对应控制器的 JPA 实体生成一个控制器。例如，在飞行应用程序中，Roo 分别为 Flight 和 FlightDescription JPA 实体创建了 FlightController 和 FlightDescriptionController 控制器。FlightController.java 文件中的以下代码显示了由 Spring Roo 生成的 FlightController：

```java
@RooWebScaffold(path = "flights", formBackingObject = Flight.class)
@RequestMapping("/flights")
@Controller
public class FlightController {
}
```

@Controller 注解表示 FlightController 是一个 Spring MVC 控制器组件。

@RequestMapping 类级别注解将传入请求映射到控制器类。控制器映射的 URI 由 @RequestMapping 注解中指定的值确定。例如，FlightController 映射到 /flights URI。如我们很快将看到，@RequestMapping 注解也可以在方法级别使用，以缩小类级别指定的映射范围。

@RooWebScaffold Roo 注解指示 Roo 生成一个包含 Flight JPA 实体（由 formBackingObject 属性标识）CRUD 操作的 ITD，并为对 Flight JPA 执行 CRUD 操作创建 JSPX 视图。path 属性标识 /WEB-INF/views/ 中的子目录，FlightController 的视图工件（JSPX 视图和 Tiles 定义 XML）将在此子目录中创建。请参阅 "Creating a Spring MVC controller for a specific JPA entity" 配方，以查看 @RooWebScaffold 注解属性的详细列表。

以下代码显示为 FlightController 创建的 AspectJ ITD：

```java
import javax.validation.Valid;

privileged aspect FlightController_Roo_Controller
{

@RequestMapping(method = RequestMethod.POST)
public String FlightController.create(@Valid Flight flight,
...)
```

```java
{
    if (bindingResult.hasErrors())
    {
        ...
        return "flights/create";
    }
    ...
    flight.persist();
    return "redirect:/flights/" + ...;
}
...
```

关于给定代码，需要注意的重要点如下：

- `@RequestMapping` 方法级别注解的 `method` 属性指定，如果控制器接收到 HTTP POST 请求，则会调用 `FlightController` 的 `create` 方法。`create` 方法表示将 `Flight` 实体持久化到数据库中的控制器方法。
- `create` 方法的返回值可以是 `flight/create` 或 `redirect:/flights/...` 字符串值。如果返回值是 `flight/create`，则飞行应用程序显示映射到 `flight/create` Tiles 定义名称的网页。如果返回值是 `redirect:/flights/`，则将其解释为重定向 URL（如前所述），`DispatcherServlet` 会将请求重定向到该 URL。
- 你可能注意到，`create` 方法的 `Flight` 参数带有 `@Valid` JSR 303 Bean 验证注解。使用 `@Valid` 注解会导致在将 `Flight` 实体持久化到数据库之前调用对其的验证。

在 `FlightController_Roo_Controller.aj ITD` 中定义的以下 `createForm` 方法，突出了 `@RequestMapping` 注解的另一种用法：

```java
@RequestMapping(params = "form", method = RequestMethod.GET)
public String FlightController.createForm(Model uiModel)
{
    ...
    return "flights/create";
}
```

`createForm` 方法显示用于在其中输入创建 `Flight` 实体所需详细信息的 HTML 表单。如果 `FlightController` 处理程序接收到的请求包含名为 `form` 的请求参数（由 `params` 属性指定），并且请求使用的方法是 HTTP GET（由 `method` 属性指定），则会调用 `createForm` 方法。
