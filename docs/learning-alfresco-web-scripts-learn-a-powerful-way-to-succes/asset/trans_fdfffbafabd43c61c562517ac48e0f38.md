

- 可以使用不同的 HTTP 方法定义一个 web 脚本。例如，在一个合同管理解决方案中，可以根据 HTTP 方法区分 web 脚本，以从存储库中创建/更新/删除/检索合同（基于自定义内容类型的内容）如下：
  - `contract.get.desc.xml`：这会从存储库中获取合同
  - `contract.put.desc.xml`：这会更新存储库中指定的合同
  - `contracts.post.desc.xml`：这会在存储库中创建合同
  - `contract.delete.desc.xml`：这会从存储库中删除合同

## Web script arguments

了解 web 脚本需要哪些参数非常重要。作为一般做法，在创建 web 脚本时，应在描述文档中指定参数。让我们来看看指定 web 脚本参数的两种方式。

### Explicit arguments

显式参数通常作为查询字符串提供给 web 脚本 URL。在描述符文档中，参数名称及其占位符通常作为查询字符串指定在 web 脚本 URL 的 `<url>` 标签下。例如，我们为第一个 web 脚本实现添加参数的方式如下：

```
`<url>/helloworld?name={argumentName}</url>`
```

你可以通过使用和号字符 `&amp;` 来区分，为 web 脚本 URL 添加多个参数。

> 在 web 脚本描述符文档中指定参数时，重要的是使用转义的和号字符 `&amp;`。这是为了确保 web 脚本描述符始终是有效的 XML 而必须这样做。

因此，例如，如果你想为第一个 web 脚本添加另一个参数，可以在描述符文档中按如下方式声明：

```
`<url>/helloworld?name={argumentName}&amp;organisation={argumentOrganisation}</url>`
```

## Response formats supported by the web script framework

Alfresco 中的 web 脚本框架支持不同的响应格式。以下是可用响应格式及其 mime 类型的列表：

- html - text/html
- text - text/plain
- xml - text/xml
- xsd - text/xml
- atom - application/atom+xml
- atomentry - application/atom+xml;type=entry
- atomfeed - application/atom+xml;type=feed
- atomsvc - application/atomsvc+xml
- rss - application/rss+xml
- json - application/json
- opensearchdescription - application/opensearchdescription+xml
- mediawiki - text/plain
- portlet - text/html
- fbml - text/html
- php - text/html
- js - text/javascript
- calendar - text/calendar
- csv - text/csv
- xls - application/vnd.ms-excel
- xlsx - application/vnd.openxmlformats-officedocument.spreadsheetml.sheet

## Specifying the response format

通过模型-视图-控制器模式的实现，我们可以有一个控制器实现和多个视图。因此，可以为任何 web 脚本提供多种响应格式，以便按请求以不同格式返回 web 脚本输出。

你可以从支持的响应格式列表中为你的 web 脚本选择任何响应格式。有两种不同的方法可以为 web 脚本指定响应格式。

### Specifying the response format via URL – extension on web script ID

一旦你创建了所需的描述符文件、控制器和多个响应模板文档用于你的 web 脚本，你就可以在调用 web 脚本 URL 时指定要使用的响应格式。你可以将其指定为 web 脚本 ID 的扩展。

例如，如果为 hello world web 脚本创建了不同的响应格式文档，例如 helloworld.get.xml.ftl 或 helloworld.get.json.ftl，并且在调用 web 脚本时你想显式指定 XML 格式，那么你可以按如下方式操作：

```
http://localhost:8080/alfresco/service/helloworld.xml?name=Ramesh
```

### Specifying the response format via URL – explicit format argument

指定响应格式的另一种方法是显式地向 web 脚本 URL 传递 format 参数。例如，如果你想为 hello world web 脚本显式指定 XML 格式，那么你可以按如下方式操作：

```
http://localhost:8080/alfresco/service/helloworld?name=Ramesh&format=xml
```

### The default format

在调用 web 脚本时，如果未使用前面提到的两种方法中的任何一种显式指定格式，则将返回描述符文档中提到的默认格式。默认格式可以在描述符文档中按如下方式指定：

```
<format default="json">any</format>
```

这表示 web 脚本响应的默认格式为 JSON，并且可以在调用 web 脚本时使用前面两种方法中的任何一种指定格式。

> 如果 web 脚本的描述符文档中未定义默认响应格式，并且在执行 web 脚本时既未通过 web 脚本 URL 中 web 脚本 ID 的扩展提供响应格式，也未通过显式指定 web 脚本 URL 中的 format 参数提供响应格式，则 Alfresco 假定默认响应格式为 HTML。

通常，响应格式的一些有用要点如下：

- 响应格式通过命名约定映射到 FreeMarker 模板，例如我们第一个 web 脚本中的 `helloworld.get.html.ftl`
- 对于任何 web 脚本，你可以定义多种响应格式，如 HTML、XML、JSON 等。
- 根据 web 脚本请求方法和请求的格式，Alfresco 中的 web 脚本框架返回 web 脚本响应

## Summary

在本章中，我们在 Alfresco 中实际实现了最简单的第一个 web 脚本，了解了 Alfresco web 脚本 URI，以便我们知道如何使用 web 脚本，如何向 web 脚本添加参数，然后扩展第一个简单 web 脚本以实现控制器。

此外，我们还高层次地了解了当使用控制器为 web 脚本提供服务时，web 脚本命中背后的发生情况，以及如果未实现控制器时会发生什么。

我们在 Alfresco 中实现 web 脚本时学到了一些非常重要的事情，例如 Alfresco 中 web 脚本框架支持的不同 HTTP 方法、何时选择哪种方法、指定 web 脚本参数的不同方式、Alfresco 中 web 脚本框架支持的响应格式列表，以及如何使你的 web 脚本根据请求的响应格式以不同的响应格式返回响应。

总体而言，通过在 Alfresco 中创建和扩展简单 web 脚本的示例，我们获得了关于在 Alfresco 中实现 web 脚本时一些基本事物的良好知识。

在下一章中，我们将深入探讨 Alfresco 中的 web 脚本框架，并提供有用的技术见解。

# 3
Understanding the Web Script Framework

在本章中，我们将涵盖以下主题：

- Web 脚本的端到端执行流程
- 共同构成 web 脚本框架的组件
- Web 脚本的部署架构

Alfresco 中的 web 脚本框架功能强大，其基本目标是确保：

- Alfresco 中的 web 脚本易于开发
- Web 脚本可以支持各种有用功能，例如支持国际化以在多种语言中呈现输出，为 web 脚本公开配置选项，类似于 servlet 的 ServletConfig

Alfresco 中的 web 脚本框架是按照 **模型-视图-控制器模式 (MVC 模式)** 设计的，使得开发新的 web 脚本以暴露 RESTful API 来与 Alfresco 后端存储库交互变得容易。

在本章中，我们将深入探讨 Alfresco 中的 web 脚本框架，以了解共同使其成为强大框架的各种组件。它是 Alfresco web 脚本注册和执行的骨干引擎。

## Execution flow of web scripts

让我们首先了解 web 脚本的执行流程，以便了解 Alfresco web 脚本框架。

### Overall flow for a web script

一旦 web 脚本 URI 的请求被命中，web 脚本的执行流程就开始了，并一直持续到请求以请求的响应格式提供服务为止。让我们了解一个由 JavaScript 支持的控制器的 web 脚本的整体执行流程。整个执行流程如下：

1. Web 脚本 URI 的请求到达 Alfresco 中的 web 脚本请求调度器。
2. 请求调度器将请求发送到 Web Script Runtime。
3. Web Script Runtime 将请求委托给 Web Script Container，后者将首先找到适合请求的 URI 和请求的 HTTP 方法的 web 脚本。
4. 下一步是在执行 web 脚本之前，如果需要，先对用户进行身份验证。
5. Web Script Container 然后执行控制器实现（如果可用）。脚本处理器将执行请求 web 脚本的 JavaScript 控制器。

