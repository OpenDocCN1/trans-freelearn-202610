

## 工作原理...

安全设置命令由 Roo 的 **Security 插件** 处理。安全设置命令仅在你通过执行 Roo 的某个控制器命令安装了 Spring Web MVC 构件后，*之后* 才可用。这限制了安全插件仅用于使用 Spring Web MVC 的项目。

Security 插件处理安全设置命令并执行以下操作：

- 向 flightapp-web 项目的 pom.xml 文件添加名为 `spring-security-version`、值为 3.0.5 的属性，如下所示：

```xml
<project ...>
  ...
  <name>flightapp-web</name>
  <properties>
    <roo.version>1.1.2.RELEASE</roo.version>
    <spring.version>3.0.5.RELEASE</spring.version>
    <spring-security.version>
      3.0.5.RELEASE</spring-security.version>
    ...
  </properties>
  ...
</project>
```

spring-security-version 属性标识应用程序所需的 Spring Security 框架版本。pom.xml 中的 `<dependency>` 元素引用了 Spring Security 版本号属性，如下所示：

```xml
<dependency>
  <groupId>org.springframework.security</groupId>
  <artifactId>spring-security-config</artifactId>
  <version>${spring-security.version}</version>
</dependency>
```

```xml
<dependency>
  <groupId>org.springframework.security</groupId>
  <artifactId>spring-security-web</artifactId>
  <version>${spring-security.version}</version>
</dependency>
```

如给定配置所示，将 flightapp-web 应用程序所需的 Spring Security 版本号定义为 pom.xml 文件中的属性，可以确保 pom.xml 定义的依赖属于同一版本的 Spring Security 的 JAR 文件。

- 在 SRC_MAIN_RESOURCES/META-INF/spring 目录中创建应用程序上下文 XML 文件：applicationContext-security.xml。applicationContext-security.xml 文件配置应用程序的身份验证和授权要求。

- 向 flightapp-web 应用程序的 web.xml 文件添加 Spring Security 的 DelegatingFilterProxy Servlet 过滤器，如下所示：

```xml
<filter>
<filter-name>springSecurityFilterChain</filter-name>
<filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
</filter>

<filter-mapping>
<filter-name>springSecurityFilterChain</filter-name>
<url-pattern>/*</url-pattern>
</filter-mapping>
```

DelegatingFilterProxy Servlet 过滤器充当进入 Spring Security Web 模块的入口点，该模块处理 Web 请求安全。过滤器 springSecurityFilterChain 的名称引用在 applicationContext-security.xml 文件中配置的 Spring Security FilterChainProxy 实例的名称。DelegatingFilterProxy 过滤器将 Web 请求委派给 FilterChainProxy 实例以执行 Web 请求安全。

- 在 SRC_MAIN_WEBAPP/WEB-INF/views 目录中创建登录 JSPX 页面 login.jspx。

- 在 SRC_MAIN_WEBAPP/WEB-INF/views/views.xml Tiles 定义 XML 文件中为登录页面添加 Tiles 定义，如下所示：

```xml
<definition extends="public" name="login">
<put-attribute name="body"
value="/WEB-INF/views/login.jspx"/>
</definition>
```

- 在 webmvc-config.xml 文件（位于 SRC_MAIN_WEBAPP/WEB-INF/spring 目录）中配置 ParameterizableViewController（通过 Spring mvc 命名空间的 view-controller 元素），用于将请求分发到 login.jspx 页面，如下所示：

```xml
<mvc:view-controller path="/login"/>
```

- 更新 flightapp-web 项目的 pom.xml 文件，以包含对 Spring Security JAR 文件的依赖，例如 spring-security-core、spring-security-config 等等。

让我们现在详细查看 applicationContext-security.xml 文件。

## Spring Security 应用程序上下文 XML 文件

applicationContext-security.xml 文件配置 Spring Security Bean，这些 Bean 用于请求的身份验证和授权。如我们稍后将看到的，Roo 生成的 applicationContext-security.xml 并没有做太多事情，但为配置应用程序特定的安全性提供了一个良好的起点。

### AuthenticationManager 配置

应用程序的身份验证机制在 applicationContext-security.xml 中通过 Spring 安全命名空间的 <authentication-manager> 元素进行配置，如下所示：

```xml
<authentication-manager alias="authenticationManager">
<authentication-provider>
<password-encoder hash="sha-256" />
<user-service>
<user name="admin" password="..." 
authorities="ROLE_ADMIN" />
<user name="user" password="..." 
authorities="ROLE_USER" />
</user-service>
</authentication-provider>
</authentication-manager>
```

让我们详细查看给定配置中的每个元素，以及它们如何协同工作为应用程序提供身份验证服务：

- <authentication-manager>：它注册 Spring Security 的 AuthenticationManager 实现的一个实例，该实例负责提供身份验证服务。AuthenticationManager 将身份验证委托给使用 <authentication-provider> 子元素配置的 AuthenticationProvider。

- <authentication-provider>：它注册 Spring Security 的 AuthenticationProvider 实现的一个实例。Spring Security 提供了 AuthenticationProvider 接口的几个内置实现，以简化在应用程序中融合不同的身份验证机制。例如，如果你使用 JA-SIG CAS 进行身份验证，可以使用 CasAuthenticationProvider 实现；如果你使用 LDAP 服务器进行身份验证，可以使用 LdapAuthenticationProvider，等等。AuthenticationProvider 实现的使用通过 <authentication-provider> 元素的 ref 属性指定。如果未指定 ref 属性（如 Roo 生成的 applicationContext-security.xml 的情况），则注册 DaoAuthenticationProvider 实现。DaoAuthenticationProvider 使用 Spring Security 的 UserDetailsService 来验证用户。UserDetailsService 根据应用程序用户输入的用户名加载包含用户名、密码和授予的权限的用户详细信息。DaoAuthenticationProvider 通过将应用程序用户输入的密码与 UserDetailsService 加载的用户详细信息进行比较来验证用户。

- <user-service>：它创建一个内存中的 UserDetailsService 实例，该实例从属性文件或嵌套的 <user> 元素中读取用户详细信息。

- <user>：它定义应用程序的一个用户。name 和 password 属性标识身份验证所需的用户名和密码。

- <password-encoder>：它在将提交的密码与 UserDetailsService 检索的密码进行比较之前，将提交的密码转换为哈希版本。hash 属性指定用于编码密码的哈希算法。

### Web 请求安全配置

以下 `<http>` 元素显示了如何在 applicationContext-security.xml 文件中配置 Web 请求安全：

```xml
<http auto-config="true" use-expressions="true">
    <form-login
        login-processing-url="/resources/j_spring_security_check"
        login-page="/login"
        authentication-failure-url="/login?login_error=t"/>
    <logout logout-url="/resources/j_spring_security_logout"/>
    <intercept-url pattern="/choices/**"
        access="hasRole('ROLE_ADMIN')"/>
    <intercept-url pattern="/member/**"
        access="isAuthenticated()" />
    <intercept-url pattern="/resources/**"
        access="permitAll" />
    <intercept-url pattern="/**" access="permitAll" />
</http>
```

让我们现在详细查看给定配置中的元素如何定义 Web 请求安全：

- <http>：它包含 HTTP 安全配置元素。它创建一个 bean 名称为 springSecurityFilterChain 的 Spring Security FilterChainProxy 实例。需要注意的是，FilterChainProxy Bean 的名称与 web.xml 文件中配置的 DelegatingFilterProxy Servlet 过滤器的名称相同。

auto-config 属性自动配置提供基于表单的登录、注销和 HTTP BASIC 身份验证服务的 Spring Security Bean。

use-expression 属性指定 <intercept-url> 元素（在本配方后面讨论）的 access 属性是否可以接受 EL 表达式。

- <form-login>：它在应用程序上下文中配置 Spring Security 的 UsernamePasswordAuthenticationFilter 过滤器 Bean（一个实现 Servlet API 的 javax.servlet.Filter 接口的 Bean）和 LoginUrlAuthenticationEntryPoint Bean。UsernamePasswordAuthenticationFilter 过滤器 Bean 由 FilterChainProxy 用于执行身份验证。

UsernamePasswordAuthenticationFilter 使用提交请求中的用户名和密码，针对配置的身份验证提供程序尝试进行身份验证。需要注意的是，包含用户名和密码的请求参数的名称必须分别为 j_username 和 j_password。如果你检查 Roo 生成的 login.jspx 文件中 flightapp-web 项目的参数，你会发现用户名和密码字段的名称分别为 j_username 和 j_password。

LoginUrlAuthenticationEntryPoint Bean 使用 UsernamePasswordAuthenticationFilter 启动基于表单的登录身份验证。

login-page 属性指定登录页面的 URL。该属性的值由 LoginUrlAuthenticationEntryPoint 用于呈现登录页面。该属性的值为 /login，这意味着 webmvc-config.xml 中配置的 <mvc:view-controller path="/login" /> 控制器负责呈现登录页面。

login-processing-url 属性指定登录表单提交到的 URL。UsernamePasswordAuthenticationFilter 处理提交到其 filterProcessesUrl 属性标识的 URL 的请求。login-processing-url 属性的值用于设置 UsernamePasswordAuthenticationFilter 的 filterProcessesUrl 属性。login-processing-url 属性的值为 /resources/j_spring_security_check，这与 Roo 生成的 flightapp-web 项目的 login.jspx 文件中 HTML <form> 元素的 action 属性值相同。

