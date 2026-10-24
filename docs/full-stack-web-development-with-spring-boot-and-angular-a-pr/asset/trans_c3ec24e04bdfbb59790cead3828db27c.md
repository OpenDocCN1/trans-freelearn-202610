

以及调用端点时应期望的响应体。这也有助于减少 API 集成方面的错误和冲突，因为可用的端点是透明的。然而，手动创建文档的主要缺点是它繁琐且容易出错。这就是 springdoc-openapi 发挥作用的地方。

首先，让我们讨论一下 springdoc-openapi 是什么。springdoc-openapi 是一个库，可自动化 Spring Boot 项目中 API 文档的生成。这种自动化之所以成为可能，是因为该库使用注解、类结构和 Spring 配置来识别可用的 API。

springdoc-openapi 可以生成必要的 JSON/YAML 和 HTML API 文档，这些文档可以通过我们在应用程序上新生成的 URL 查看。它还支持多个框架和协议，包括以下内容：

- spring-boot
- JSR-303，特别是针对 @NotNull、@Min、@Max 和 @Size
- swagger-ui
- OAuth 2

现在，让我们讨论 springdoc-openapi 的属性和插件。

## springdoc-openapi 的属性

我们可以根据自己的偏好修改 springdoc-openapi 的行为和设置。它具有不同的属性，我们可以在 application.properties 文件下设置其值。

以下是 springdoc-openapi 中一些常用的属性：

- springdoc.swagger-ui.path：默认值是 /swagger-ui.html。它定义了访问 HTML 文档的路径。
- springdoc.swagger-ui.enabled：默认值是 true。它启用或禁用 swagger-UI 端点。
- springdoc.swagger-ui.configUrl：默认值是 /v3/api-docs/swagger-config。它是一个用于检索外部配置文档的 URL。
- springdoc.swagger-ui.layout：默认值是 BaseLayout。它是 Swagger UI 用于显示文档的顶级布局。
- springdoc.swagger-ui.tryItOutEnabled：默认值是 false。它启用或禁用 Try it out 部分，用户可以在此测试端点。
- springdoc.swagger-ui.filter：默认值是 false。它启用或禁用过滤并添加一个文本框以放置过滤条件。它可以是布尔值或字符串；这将作为过滤表达式。
- `springdoc.swagger-ui.operationsSorter`：这对 API 的操作列表应用排序。值可以是 `'alpha'`（按路径字母排序）、`'method'`（按 HTTP 方法排序），或一个将识别排序标准的函数。
- `springdoc.swagger-ui.tagsSorter`：这对 API 的操作列表应用排序。值可以是 `'alpha'`（按路径字母排序）或一个将识别排序标准的函数。

## springdoc-openapi 的插件

springdoc-openapi 也有我们可以用来生成文档的插件。让我们来看看。

### *springdoc-openapi-maven-plugin*

springdoc-openapi-maven-plugin 在构建期间生成 JSON 和 YAML OpenAPI 描述。插件也在集成阶段工作。要启用插件，我们需要在 pom.xml 文件的 `<plugin>` 部分添加以下声明：

```xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <version>${spring-boot-maven-plugin.version}</version>
    <configuration>
        <jvmArguments>
            -Dspring.application.admin.enabled=true
        </jvmArguments>
    </configuration>
    <executions>
        <execution>
            <goals>
                <goal>start</goal>
                <goal>stop</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

我们刚刚添加了 `spring-boot-maven-plugin` 插件。复制上述代码并粘贴到你的 `.pom` 文件中。

```
<id>integration-test</id>
<goals>
    <goal>generate</goal>
</goals>
</execution>
</executions>
<configuration>
    <apiDocsUrl>
        http://localhost:8080/v3/api-docs</apiDocsUrl>
    <outputFileName>openapi.json</outputFileName>
    <outputDir>/home/springdoc/output-folder</outputDir>
    <skip>false</skip>
    <headers>
        <header1-key>header1-value</header1-key>
    </headers>
</configuration>
</plugin>
```

在前面的示例 XML 代码中，我们添加了多个属性以对 OpenAPI 应用自定义配置。我们手动设置了输出文件名、目录和用于生成 API 文档的头部。

## springdoc-openapi-gradle-plugin

springdoc-openapi-gradle-plugin 从 Gradle 构建为 Spring Boot 应用程序生成 OpenAPI 规范。要启用插件，我们必须在 plugins 部分放置以下代码：

```
plugins {
    id "org.springframework.boot" version "${version}"
    id "org.springdoc.openapi-gradle-plugin"
        version "${version}"
}
```

一旦插件和依赖项已添加，以下任务将被创建：

- generateOpenApiDocs：运行以生成 OpenAPI 文档的任务。generateOpenApiDocs 会向应用程序的文档 URL 发出 REST 调用，以将 OpenAPI 文档以 JSON 格式存储。
- forkedSpringBootRun：使用此任务在后台运行 Spring Boot 应用程序。

我们还可以通过指定以下属性来自定义 openapi-gradle-plugin 的行为：

- apiDocsUrl：可以下载 Open API 文档的 URL
- outputDir：文档生成的目录
- outputFileName：生成的输出文件名
- waitTimeInSeconds：在调用 REST API 生成 OpenAPI 文档之前，等待 Spring Boot 应用程序启动的时间（秒）
- forkProperties：运行 Spring Boot 应用程序所需的一个系统属性
- groupedApiMappings：一组可以从中下载 OpenAPI 文档的 URL

要使用这些属性，我们必须使用 generateOpenApiDocs 指定它们：

```
openApi {
    apiDocsUrl.set("https://localhost:4000/api/docs")
    outputDir.set(file("$buildDir/docs"))
    outputFileName.set("swagger-test.json")
    waitTimeInSeconds.set(310)
    forkProperties.set("-Dspring.profiles.active=special")
    groupedApiMappings.set(
        ["https://localhost:8000/v3/api-docs/group-1" to "swagger-group-1.json",
        "https://localhost:8000/v3/api-docs/group-2" to "swagger-group-2.json"])
}
```

至此，我们已经了解了可在 OpenAPI 文档中使用的属性和插件。现在，让我们为我们的 Spring Boot 应用程序配置插件。

## 配置 springdoc-openapi

现在，我们将在 Spring Boot 应用程序中安装和配置 springdoc-openapi。首先，我们必须将依赖项添加到之前创建的项目中。前往 pom.xml 文件并添加以下 XML 代码：

```
<dependency>
    <groupId>org.springdoc</groupId>
```

```
<artifactId>springdoc-openapi-ui</artifactId>
<artifactId>            <version>1.6.4</version>
</dependency>
```

成功安装 OpenAPI 依赖项后，我们可以运行 Spring Boot 项目。服务器启动后，我们可以前往 http://localhost:8080/v3/api-docs/ 来访问 OpenAPI 文档的 JSON 格式。你将看到所有端点及其关联的 HTTP 请求都显示为 JSON 对象。以下是我们项目生成的 JSON 文档片段：

```
{
  "servers": [
    {
      "url": "http://localhost:8080/",
      "description": "Generated server url"
    }
  ],
  "paths": {
    "/api/v1/anti-heroes/{id}": {
      "get": {
        "tags": [
          "anti-hero-controller"
        ],
        "operationId": "getAntiHeroById",
        "parameters": [
          {
            "name": "id",
            "in": "path",
            "required": true,
            "schema": {
              "type": "string",
              "format": "uuid"
            }
          }
        ],
        "responses": {
          "200": {
            "description": "OK",

```
