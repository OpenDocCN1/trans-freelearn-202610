

由于我们在应用程序中使用了 Spring Security，因此无需从头编写与安全相关的任务，因为 Spring Security 已在内部提供了这些功能。

让我们进一步讨论 Spring Security 的概念。

## Spring Security 的特性

Spring Security 主要专注于将身份验证和授权集成到应用程序中。两者相比，**身份验证**是指验证用户可以访问你的应用程序并识别用户身份。这主要指登录页面本身。另一方面，**授权**用于更复杂的应用程序；这与特定用户在应用程序内可以执行的操作或动作相关。

授权可以通过集成角色来实现用户访问控制。Spring Security 还提供了不同的密码编码器——单向转换密码——如下所示：

- `BCryptPasswordEncoder`
- `Argon2PasswordEncoder`
- `Pbkdf2PasswordEncoder`
- `SCryptPasswordEncoder`

上述列表是最常用的密码编码器，在使用 Spring Security 时可以直接访问。它还提供了不同的功能来帮助你满足安全需求，如下所示：

- **轻量级目录访问协议 (LDAP)**：一种用于在互联网协议上包含和访问分布式目录信息服务的协议。
- **记住我**：此功能提供记住来自单台机器的用户的能力，以防止再次登录。
- **单点登录 (SSO)**：此功能允许用户使用单个账户访问多个应用程序，集中用户信息。
- **软件本地化**：此功能提供使用我们首选语言开发用户界面的能力。
- **HTTP 授权**：此功能提供授权。
- **基本访问认证**：此功能提供基础认证流程，要求请求提供用户名和密码。
- **摘要访问认证**：此功能提供更安全的认证，在访问资源前确认用户身份。
- **Web 表单认证：** 将生成一个表单，直接从 Web 浏览器对用户凭据进行认证。

Spring Security 为应用程序提供了广泛的功能。在这种情况下，Spring Security 的设计是根据其功能划分为独立的 Java Archive (JAR) 文件，只需为我们的开发安装所需的部分即可。

以下是 Spring Security 模块中包含的 JAR 文件列表：

- `spring-security-core`：应用程序使用 Spring Security 的标准需求。Spring-security-core 由核心认证类和接口组成。
- `spring-security-web`：此 JAR 文件用于 Web 认证和基于 URL 的访问控制。它位于 `org.springframework.security.web` 下。
- `spring-security-config`：此 JAR 文件用于实现 Spring Security 配置，使用 XML 和 Java。所有类和接口都位于 `org.springframework.security.config` 下。
- `spring-security-ldap`：此 JAR 文件是实现应用程序中 LDAP 所必需的。所有类和接口都位于 `org.springframework.security.ldap` 下。
- `spring-security-oauth2-core`：此 JAR 文件用于实现 OAuth 2.0 授权框架和 OpenID Connect Core。类位于 `org.springframework.security.oauth2.core` 下。
- `spring-security-oauth2-client`：此 JAR 文件提供 OAuth 登录和 OpenID 客户端支持。所有类和接口都位于 `org.springframework.security.oauth2.client` 下。
- `spring-security-openid`：此 JAR 文件用于 OpenID Web 认证支持，以使用外部 OpenID 服务器验证用户。
- `spring-security-test`：此 JAR 文件用于支持 Spring Security 应用程序的测试。
- `spring-security-cas`：此 JAR 文件使用 CAS SSO 服务器实现 Web 认证。所有类和接口都位于 `org.springframework.security.cas` 下。
- `spring-security-acl`：此 JAR 文件用于将安全集成到应用程序的领域对象中。我们可以在 `org.springframework.security.acls` 下访问类和接口。

我们现在已经了解了 Spring Security 提供的不同特性和模块。在下一节中，我们将学习如何在 Spring Boot 应用程序中使用 Spring Security 实现身份验证和授权。

## Spring Boot 中的身份验证和授权

我们在上一节中已经讨论了 Spring Security 的概念；现在，我们将学习如何将 Spring Security 集成到我们的 Spring Boot 应用程序中。随着我们进入示例，我们将使用 Spring Boot Security 的所有模块和功能。

身份验证和授权是我们实现应用程序安全时最常见的概念。这些是我们为使应用程序安全而应用的两种验证。

### 配置 Spring Boot 并实现身份验证

我们将首先在应用程序中实现身份验证。我们首先需要在项目中添加 Spring Boot Security 依赖项。要添加依赖项，我们将在 pom.xml 中添加以下内容：

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

重新加载项目以安装新依赖项并运行服务器。让我们尝试访问 localhost:8080 在浏览器中打开 Spring Boot 应用程序项目。如我们所见，由于我们安装了 Spring Boot Security，登录页面现在已应用于我们的项目：

![](img/7c3ddd2e5893c9b5a0220148ed886548_158_0.png)

图 7.1 – 从 Spring Boot Security 集成的登录页面

要为登录创建凭据，我们可以在 application.properties 文件下配置用户名和密码，放置以下设置：

```
spring.security.user.name=admin
spring.security.user.password=test
```

在前面的示例中，我们使用了 admin 作为用户名，test 作为密码用于 Spring Boot Security 登录，这将允许我们成功登录到我们的应用程序。

我们现在已成功为项目设置了 Spring Boot Security，并且这会自动将身份验证应用于我们的端点。我们下一步需要做的是为我们的安全添加配置；我们希望覆盖默认配置并为我们的应用程序实现自定义登录端点，以便提供对我们其他端点的访问。

要开始配置，让我们首先在 config 文件下创建一个名为 SecurityConfig 的新类。我们将扩展我们的新 SecurityConfig 类以继承 WebSecurityConfigurerAdapter。此适配器允许我们覆盖和自定义 WebSecurity 和 HttpSecurity 的配置，并在扩展类后，我们将覆盖前两种方法：

```
@Override
protected void configure(AuthenticationManagerBuilder auth)
throws Exception {
    //We will place the customized userdetailsservice here in
the following steps
}

@Bean(name = BeanIds.AUTHENTICATION_MANAGER)
@Override
public AuthenticationManager authenticationManagerBean() throws Exception {
    return super.authenticationManagerBean();
}
```

我们将在 WebSecurityConfigurerAdapter 上覆盖的第一个方法是 configure(AuthenticationManagerBuilder auth) 方法，它接受 AuthenticationManagerBuilder，用于构建 LDAP 认证、基于 JDBC 的认证、添加自定义 UserDetailsService 以及添加 AuthenticationProviders。在这种情况下，我们将使用它来访问 userDetailsServiceMethod() 以自定义我们的认证。我们将在以下步骤中执行此操作，因为我们尚未创建我们修改的 UseDetailsService。

第二个方法是 authenticationManagerBean()；我们覆盖此方法是为了在我们的应用程序中将 AuthenticationManager 暴露为 Bean，我们稍后将在 AuthenticateController 中使用它。下一步是实现我们希望对 HTTP 请求的配置。为此，我们将覆盖 configure(HttpSecurity http) 方法。

