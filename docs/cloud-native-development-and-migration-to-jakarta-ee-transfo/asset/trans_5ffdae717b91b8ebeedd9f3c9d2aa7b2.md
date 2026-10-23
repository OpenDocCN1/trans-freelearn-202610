

```
@GET
@Path("/add")
public Response addSpan() {
    Span span = tracer.spanBuilder("addSpan")
        .setSpanKind(SpanKind.INTERNAL)
        .setParent(Context.current().with(this.span))
        .setAttribute("identifier", UUID.randomUUID().toString())
        .startSpan();
    span.end();
    return Response.ok().build();
}
```

注意，当你手动向追踪添加 span 时，需要调用 span.end()。如果不这样做，span 将不会被添加到追踪中。

## 代理插桩

代理插桩允许你通过指定一个 JAR 文件将 Java 代理附加到应用程序。通过这种方式，你可以为多个流行的 Java 框架和库添加追踪支持。

Java 代理通过 javaagent 参数附加到 JVM，如下所示，但其具体配置可能因你使用的服务器而异：

```
-javaagent: opentelemetry-javaagent.jar
```

## 其他规范

在本节中，我们仅简要介绍其他几个规范，目的是让你快速了解其功能。

## OpenAPI

该规范为 OpenAPI 提供了 Java API，OpenAPI 是一种允许开发者公开其 API 文档的规范。

它包含不少于 46 个可添加到代码中的注解，使你能够完整地描述 API、资源、参数和返回值。

以下示例摘自官方 MicroProfile OpenAPI 规范文档：

```
@GET
@Path("/{username}")
```

```
@Operation(summary = "Get user by user name")
@APIResponse(description = "The user",
    content = @Content(mediaType = "application/json",
        schema = @Schema(implementation =
User.class))),
@APIResponse(responseCode = "400", description = "User not found")
public Response getUserByName(
    @Parameter(description = "The name that needs to be fetched. Use user1 for testing. ", required = true) @PathParam("username")
String username)
{...}
```

## RestClient

MicroProfile RestClient 提供了一种类型安全的方式来通过 HTTP 调用 RESTful 服务。这可以通过指定一个接口来实现，该接口包含你要调用的远程 API 的方法，并使用 RESTful 注解进行标注。

同样摘自官方文档，以下是一个示例接口：

```
@Path("/movies")
public interface MovieReviewService {
    @GET
    Set<Movie> getAllMovies();
```

然后，该接口以这种方式用于创建客户端：

```
URI apiUri = new URI("http://localhost:9080/movieReviewService");
MovieReviewService reviewSvc = RestClientBuilder.newBuilder()
    .baseUri(apiUri)
    .build(MovieReviewService.class);
Set<Review> reviews = reviewSvc.getAllMovies();
```

## JSON Web Token 认证

JSON Web Token (JWT) 认证是一种用于识别已认证用户及其角色的认证机制。

它在成功认证后由服务器生成，然后通过 HTTP 发送到系统想要访问的其他服务。通常，JWT 既经过加密也经过签名。接收系统可以验证签名者并解密 JWT。

这种认证和授权系统通常用于服务到服务的环境中，其中每个服务都会传递 JWT。

## Jakarta EE 10 核心概要

该规范是 MicroProfile 6.0 中的新规范。在此之前，MicroProfile 核心规范包括 Jakarta RESTful Web Services、Jakarta JSON Binding、Jakarta JSON Processing、Jakarta Dependency Injection 和 Jakarta Annotations。

在 MicroProfile 6.0 中，决定通过采用 Jakarta EE 的 Core Profile 将这些规范纳入一个总括规范。这个新的核心规范轻量级，但足以满足微服务和边缘计算的需求。

## 总结

在本章中，你了解了 MicroProfile 的全部内容。我们逐一介绍了每个必需的规范。我们还详细讨论了升级 Cargo Tracker 应用程序为云原生版本时感兴趣的规范。

在下一章中，当我们升级 Cargo Tracker 应用程序时，将会看到 Config、Health、Fault Tolerance 和 Metrics 规范。

## 附录 A

## Java EE 与 Jakarta EE 名称对照

当 Java EE 被移交给 Eclipse 基金会并更名为 Jakarta EE 时，底层技术的名称也必须重命名以避免法律问题。以下是旧 Java EE 和新 Jakarta EE 名称的列表：

| Java EE 名称 | Jakarta EE 名称 |
|---|---|
| Java Servlet | Jakarta Servlet |
| Java Server Faces | Jakarta Faces |
| Java API for WebSocket | Jakarta WebSocket |
| Concurrency Utilities for Java EE | Jakarta Concurrency |
| Interceptors | Jakarta Interceptors |
| Java Authentication SPI for Containers | Jakarta Authentication |
| Java Authorization Contract for Containers | Jakarta Authorization |
| Java EE Security API | Jakarta Security |
| Java Message | Jakarta Messaging |
| Java Persistence API | Jakarta Persistence |
| Java Transaction API | Jakarta Transaction |
| Batch Applications for the Java Platform | Jakarta Batch |
| JavaMail | Jakarta Mail |
| Java EE Connector Architecture | Jakarta Connectors |
| Common Annotations for the Java Platform | Jakarta Annotations |
| Java Beans Activation Framework | Jakarta Activation |
| Bean Validation | Jakarta Bean Validation |
| Expression Language | Jakarta Expression Language |
| Enterprise JavaBeans | Jakarta Enterprise Beans |
| Java Architecture for XML Binding | Jakarta XML Binding |
| Java API for JSON Binding | Jakarta JSON Binding |
| Java API for JSON Processing | Jakarta JSON Processing |
| JavaServer Pages | Jakarta Server Pages |
| Java API for XML-based Web Services | Jakarta XML Web Services |
| Java API for RESTful Web Services | Jakarta RESTful Web Services |
| Java Server Pages Standard Tag Library | Jakarta Standard Tag Library |
| Contexts and Dependency Injection for Java | Jakarta Contexts and Dependency Injection |

## 索引

## 符号

12要素应用 121
管理进程 127
后端服务 123
构建 124
代码库 122
并发 125
配置 123
依赖 122
开发/生产一致性 126
可处置性 126
日志 127
端口绑定 125
进程 124
发布 124
运行 124

Amazon Web Services (AWS) 118
注解，MicroProfile Fault Tolerance 153
@Asynchronous 153, 154
@Bulkhead 155
@CircuitBreaker 156
@Fallback 156, 157
@Retry 154, 155
@Timeout 155
应用服务器
升级 49
应用服务器 4-6
自动数据源
设置 69, 70
Azure 132
部署到 132

## A

管理界面，Cargo Tracker 应用程序 16
预订货物界面 19, 20
货物信息 17
货物路线界面 17, 18
提前编译 (AOT) 52

## B

后端服务 123
Bean 管理持久性 (BMP) 5, 23
Bean 管理事务 (BMTs) 28
字节码 3

## C

Cargo Tracker 应用程序 12, 59
管理界面 16
优点 12, 13
特性 14
Java EE 特性，使用 22
移动事件记录器 20, 21
公共跟踪界面 14-16
CDN
参考链接 106
Chunk 29
云
容器指标 144-146
云原生 117, 118
监控 59
弹性 57
转换，执行 128
云原生原则 118
CI/CD 120, 121
容器 119
DevOps 120
微服务 118
编排 119
CNCF
参考链接 111
完整的 Docker 文件
展示 72, 73
ConfigSources 149
容器实例
创建 139-144
容器管理持久性 (CMP) 5, 23
容器管理事务 (CMTs) 28
容器管理系统 (CMSs) 125
容器编排 106
容器注册表
创建 132-136
镜像，上传 137-139
容器运行时 (CRI) 113
容器 94
创建 94
上下文和依赖注入 24
核心概要 52
规范 52

## D

仪表盘
设置 70, 71
数据库管理系统 (DBMS) 127
依赖迁移 40
Payara 应用服务器，升级 41, 42
PrimeFaces，升级 42, 43
项目，升级 40, 41
Red Hat MTA 44-48
依赖注入 (DI) 24
Derby 42
DevOps 120
开发/生产一致性，12要素应用
人员 126
资源 127
时间 126
Docker 95
安装 96, 97
参考链接 96
使用 95
Docker API 95
Docker 客户端 95
Docker Compose 101
容器，停止 102, 103
文件，创建 101
设置 65-69
使用 101, 102

