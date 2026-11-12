

```
<goal>enhance</goal>
</goals>
</execution>
</executions>
</plugin>
```

在插件配置中，`<execution>` 元素指定 datanucleus:enhance 目标在编译构建生命周期阶段执行。因此，在编译 Java 源文件时，Maven DataNucleus 插件会增强已编译的 JPA 领域类。`<mappingIncludes>` 元素指定应包含在增强中的类。`<mappingExcludes>` 指定不应考虑用于增强的类。

`<api>` 元素指定增强是针对 JPA 还是 JDO。由于在 flightapp-gae-gwt 项目中使用的是 JPA，`<api>` 元素的值为 jpa。`<enhancerName>` 元素指定 ASM 作为值，这基本上是指 DataNucleus 用于增强字节码的 ASM 框架 (`http://asm.ow2.org/`)。

让我们来看看由 Roo 生成的 FlightDescription 实体：

## 持久实体

由于 GAE datastore 不是关系型数据库，您会发现，在使用关系型数据库的 JPA 时适用的一些概念，在将 JPA 与 GAE datastore 一起使用时将不再适用。

以下代码显示了由 Roo 生成的 FlightDescription JPA 实体。

```
@RooJavaBean
@RooToString
@RooEntity
public class FlightDescription {

    @NotNull
    private String origin;

    @NotNull
    private String destination;

    @NotNull
    private Float price;

}
```

给定的代码显示，我们没有使用 `@Column` 和 `@Table` JPA 注解来标识保存实体实例的表以及持久实体字段映射到的表列。由于 GAE datastore 是无模式的，您无需指定表或列信息。您仍然可以使用 JSR 303 注解，例如本代码中的 `@NotNull`，来验证您的领域对象。

以下代码显示了 FlightDescription_Roo_Entity.aj AspectJ ITD 文件：

```
privileged aspect FlightDescription_Roo_Entity {

    declare @type: FlightDescription: @Entity;

    @PersistenceContext
    transient EntityManager FlightDescription.entityManager;

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private Long FlightDescription.id;

    @Version
    @Column(name = "version")
    private Integer FlightDescription.version;

    ...
}
```

值得注意的是，主键生成策略指定为 GenerationType.IDENTITY。在 GAE 中，这意味着在相关事务完成或您显式调用 EntityManager 的 flush 方法之前，标识符值不会分配给 FlightDescription 实体。

让我们来看看 flightapp-gae-gwt 应用程序如何确保只有经过身份验证的用户才能访问它。

## GAE 应用程序中的身份验证和授权

与其他任何 Web 应用程序一样，flightapp-gae-gwt 应用程序的 Web 请求安全约束在应用程序的 web.xml 文件中指定。以下清单显示了 flightapp-gae-gwt 应用程序 web.xml 文件中的 `<security-constraint>` 元素：

```
<security-constraint>
<display-name>...</display-name>
<web-resource-collection>
    <web-resource-name>...</web-resource-name>
    <url-pattern>*.html</url-pattern>
</web-resource-collection>
<auth-constraint>
    <role-name>*</role-name>
</auth-constraint>
</security-constraint>
```

`<url-pattern>` 指定任何匹配 `*.html` 模式的 URL 都受到保护，并需要身份验证。对于 flightapp-gae-gwt 应用程序，应用程序的主页是 index.html，它根据 `<url-pattern>` 元素指定的 URL 模式受到保护。由于对 flightapp-gae-gwt 应用程序的入口受到限制，用户在访问应用程序之前需要使用其 Google 账户凭据进行身份验证。`<role-name>` 元素将 * 指定为值，这意味着任何经过身份验证的用户都可以访问该应用程序。如果您希望 GAE 上的应用程序也可供匿名用户访问，则从 web.xml 文件中删除 `<security-constraint>` 元素。

如果从 flightapp-gae-gwt 项目的 web.xml 中删除 `<security-constraint>` 元素并将应用程序上传到 GAE 服务器，您会发现尝试访问 flightapp-gae-gwt 应用程序仍会要求身份验证。这种行为背后的原因是 Roo 生成的 web.xml 文件中配置了 sample.roo.flightapp.server.gae.GaeAuthFilter Servlet 过滤器。GaeAuthFilter 是一个由 Roo 生成的 Servlet 过滤器，用于检查用户是否已登录。如果用户未登录，则它会将用户重定向到 Google 账户登录页面。GaeAuthFilter.java 中的以下代码清单显示了 GaeAuthFilter 类：

```
import com.google.appengine.api.users.UserService;
import com.google.appengine.api.users.UserServiceFactory;

public class GaeAuthFilter implements Filter {
...

public void doFilter(...) ... {
    UserService userService =
    UserServiceFactory.getUserService();
    ...

if (!userService.isUserLoggedIn()) {
    String requestUrl = request.getHeader("requestUrl");
    if (requestUrl == null) {
    requestUrl = request.getRequestURI();
    }
    response.setHeader("login",
    userService.createLoginURL(requestUrl));
    response.sendError(HttpServletResponse.SC_UNAUTHORIZED);
    return;
}
...
}
```

在给定代码中，UserServiceFactory 是 GAE 特定的类，其 getUserService 方法返回 UserService 的实例。UserService 接口定义了用于创建登录和注销 URL、获取当前登录用户详细信息、检查用户是否已登录等方法。在此代码中，GaeAuthFilter 通过调用 isUserLoggedIn() 方法检查用户是否已登录。如果用户未登录，GaeAuthFilter 将使用 UserService 的 createLoginURL(..) 方法创建登录 URL 并将用户重定向到该 URL。

关于 GaeAuthFilter 另一个值得注意的有趣点是它的映射。以下清单显示了 web.xml 文件中 GWT 的 RequestFactoryServlet 和 GaeAuthFilter 的映射：

```
<filter-mapping>
  <filter-name>GaeAuthFilter</filter-name>
  <url-pattern>/gwtRequest/*</url-pattern>
</filter-mapping>

<servlet-mapping>
  <servlet-name>requestFactory</servlet-name>
  <url-pattern>/gwtRequest</url-pattern>
</servlet-mapping>
```

GaeAuthFilter 和 GWT 的 RequestFactoryServlet 的 `<url-pattern>` 元素显示，发送到 RequestFactoryServlet 的 Web 请求会被 GaeAuthFilter 拦截。这确保了如果会话过期，应用程序用户将被重定向到 Google 账户登录页面。

UserService 提供了一个 getCurrentUser 方法，如果用户已登录，则返回 User 对象。User 对象包含经过身份验证的用户的用户 ID、昵称和电子邮件信息。如果您的应用程序需要捕获更多有关用户的信息（例如其首选项、地址等），则需要将此类信息作为应用程序数据的一部分来保存。

默认情况下，App Engine 定义的唯一角色是 admin，您可以将其指定为 `<role-name>` 元素的值。admin 角色分配给应用程序管理员，即您使用 GAE 的 Admin Console 添加的用户。Admin Console 让您对 GAE 上已部署的应用程序拥有完全控制权。它允许您管理 datastore、测试应用程序的不同版本、创建应用程序等等。您可以通过访问以下 URL 来访问 Admin Console：http://appengine.google.com。您可以使用 UserService 的 isUserAdmin 方法来确定登录用户是否属于 admin 角色。

尽管 GAE 仅支持 admin 角色，但您仍然可以通过引入应用程序特定角色，在 App Engine 应用程序中实现基于角色的安全性。您可以将应用程序特定角色信息作为应用程序数据的一部分来保存。例如，您可以将 Spring Security 框架与 GWT 或 Spring Web MVC 应用程序一起使用，以根据分配给用户的角色实现 Web 请求安全性和方法级安全性。

