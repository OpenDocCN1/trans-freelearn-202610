# 第2章：现在就来实现第一个Web脚本

在本章中，我们将涵盖以下主题：

- 如何逐步在Alfresco中创建一个简单的Web脚本
- 第一个Web脚本的实现
- 扩展第一个Web脚本以添加控制器
- Web脚本命中时幕后发生的事情
- 对任何Web脚本来说最重要的事情





现在是时候在 Alfresco 中实际实现你的第一个 Web 脚本并更熟悉 Alfresco Web 脚本的实现方式了。本章将帮助你清楚地了解如何在 Alfresco 中创建一个简单的 Web 脚本、如何通过控制器实现扩展 Web 脚本，以及在处理 Alfresco Web 脚本时非常重要的一些基础知识。

考虑这样一个场景：你正在管理一个将自定义前端应用与 Alfresco 作为后端存储库集成的解决方案的项目执行工作。现在，你分配了一名新开发者来开发 Alfresco Web 脚本，他对 Web 脚本实现并不十分熟悉，而你希望让他快速上手 Web 脚本开发。没关系，你可以让他阅读本章，阅读后他应该能够很好地理解在 Alfresco 中开发 Web 脚本。

## 在 Alfresco 中创建你的第一个 Web 脚本

每当我们开始学习任何新的编程语言时，我们实现的第一个程序都是“Hello world”程序，其中我们只是将文本 **Hello world** 打印为程序的输出。我们在这里也会做同样的事情。我们将在 Alfresco 中创建一个基本的 Web 脚本，它将简单地显示 **Hello world!** 作为 Web 脚本的输出。

为了在 Alfresco 中创建一个基本且简单的 Web 脚本，你需要为 Web 脚本准备一个描述符以及一个用于渲染 Web 脚本输出的 FreeMarker 模板。

Web 脚本可以从可用的 Alfresco 用户界面创建，也可以在 Alfresco 外部开发然后部署它们。由于我们在这里要实现一个非常基本的 Web 脚本，我们将使用 Alfresco 用户界面选项并创建 Web 脚本。你可以使用 Alfresco Explorer Web 客户端或 Alfresco Share UI 来实现此操作。然而，由于 Alfresco Explorer 现在通常不被鼓励使用，因为它不在 Alfresco 的路线图上，并且不再为 Alfresco Explorer 提供新的实现支持，我们也不会使用 Alfresco Explorer。相反，我们将使用 Alfresco Share UI，这是通常推荐使用的。

让我们现在创建 Web 脚本。为了在 Alfresco 中创建你的第一个 Web 脚本，请按照以下步骤操作：

- 1. 登录 Alfresco Share UI。
- 2. 点击顶部标题中的 **Repository** 链接。
- 3. 进入 **Data Dictionary** | **Web Scripts Extensions**。
- 4. 创建一个名为 helloworld.get.desc.xml 的新文件，内容如下：

```
<webscript>
  <shortname>Hello World</shortname>
  <description>First webscript Hello world</description>
  <url>/helloworld</url>
</webscript>
```

Downloading the example code
你可以从你在 http://www.packtpub.com 的账户下载你购买的所有 Packt 书籍的示例代码文件。如果你是在其他地方购买本书，你可以访问 http://www.packtpub.com/support 并注册以便将文件直接发送到你的邮箱。

- 5. 创建一个名为 helloworld.get.html.ftl 的新文件，内容如下：

```
<html>
  <body>
    <p>Hello world!</p>
  </body>
</html>
```

- 6. 现在，在浏览器中访问 URL http://localhost:8080/alfresco/service/index。提供身份验证弹出窗口中显示的用户名和密码。点击底部可用的 Refresh Web Scripts 按钮。
- 7. 点击 List Web Scripts 链接。你可以通过点击 Browse all Web Scripts 链接并搜索 helloworld 来看到 Hello world Web 脚本。
- 8. 现在，访问 URL http://localhost:8080/alfresco/service/helloworld。好了，你应该在浏览器中看到输出 Hello world! 被渲染。

我们已经在 Alfresco 中实现了第一个 Web 脚本。现在，让我们尝试理解我们刚刚为了实现第一个 Web 脚本所做的事情。我们首先按如下方式创建了 Web 脚本：

- - 我们实现了一个 GET Web 脚本来显示 Hello World! 作为输出
- - 我们为 Web 脚本创建了描述符文档 helloworld.get.desc.xml
- - 我们还创建了 FreeMarker 模板 helloworld.get.html.ftl 来渲染 Web 脚本响应

通常，为了在 Alfresco 中创建 Web 脚本，至少需要两个文件。一个是描述符文档，另一个是 FreeMarker 模板。

## 是时候进行第一个 Web 脚本了

接下来，我们按如下方式部署了 Web 脚本：

- - 我们已将创建 Web 脚本所需的<|message|> 文件部署到 Alfresco 存储库中
- - 描述符文档和 FreeMarker 模板放置在 **Data Dictionary | Web Scripts Extensions**

> Web 脚本文件可以直接在 Alfresco 存储库的 **Data Dictionary | Web Scripts Extensions** 中创建/上传，也可以通过代码库在文件系统上创建并部署 Web 脚本文件。

然后我们按如下方式注册了 Web 脚本：

- - 我们访问了 URL `http://localhost:8080/alfresco/service/index` 并点击了 **Refresh Web Scripts** 按钮以注册第一个 Web 脚本

> 注册 Web 脚本有两种方式。一种是通过访问 URL `http://localhost:8080/alfresco/service/index` 并点击 **Refresh Web Scripts** 按钮。另一种方式是通过代码库/存储库部署 Web 脚本文件并重启服务器。服务器重启也会注册 Web 脚本。

最后，我们按如下方式执行了第一个 Web 脚本：

- - 我们从 Web 浏览器通过访问 URL `http://localhost:8080/alfresco/service/helloworld` 调用了 Web 脚本
- - Web 脚本在 Web 浏览器上将其输出作为响应显示

## 理解 Web 脚本 URI

理解 Web 脚本 URI 很重要。Web 脚本 URI 可以以通用形式表示为 `http[s]://<host>:<port>/[<contextPath>/]/<servicePath>/[<scriptPath>] [?<scriptArgs>]`

Web 脚本 URI 中使用的术语解释如下：

- - `http[s]`: 这是调用 Web 脚本的协议。这可以是 `http` 或 `https`。
- - `host`: 这是部署 Web 脚本的服务器的名称或地址。
- - `port`: 这是托管 Web 脚本的服务器上 Web 脚本暴露的端口。
- - `contextPath`: 这是应用程序部署到的路径。对于数据 Web 脚本，这通常是 `/alfresco`。
- - `servicePath`: 这是 Web 脚本服务映射到的路径。通常这将是 `/service`。
- - `scriptPath`: 这是 Web 脚本的路径，如在 Web 脚本描述符文档 `*.desc.xml` 的 `<url>` 标签中定义的那样。
- - `scriptArgs`: 这些是要传递给 Web 脚本的参数，通常在 Web 脚本描述符文档 `*.desc.xml` 的 `<url>` 标签中定义。参数可以在 Web 脚本 `scriptPath` 之后紧跟 ? 后指定。

现在你了解了构成 Web 脚本 URI 的不同术语，现在你可以轻松地通过将我们的第一个 Web 脚本 URI 映射到 Web 脚本 URI 的通用形式来理解我们的第一个 Web 脚本的 URI。请注意，我们的第一个 Web 脚本没有添加参数，因此你将无法映射 `scriptArgs` 部分。现在让我们看看需要做什么来向 Web 脚本添加参数。

## 向 Web 脚本添加参数

假设我们想修改我们的第一个 Web 脚本来在 Web 脚本响应中也显示姓名，例如 **Hello! Ramesh**。可以通过简单的方式实现，即将姓名作为 Web 脚本的 URL 参数传递，然后在 FreeMarker 模板中使用此参数生成响应。为了做到这一点，我们将不得不执行以下步骤：

- 1. 登录 Alfresco Share UI。
- 2. 点击顶部标题中的 **Repository** 链接。
- 3. 按如下方式编辑 **Data Dictionary | Web Scripts Extension** 位置的描述符文件 `helloworld.get.desc.xml`：
```
<webscript>
    <shortname>Hello World</shortname>
    <description>First webscript Hello world</description>
    <url>/helloworld?name={argumentName}</url>
</webscript>
```
- 4. 按如下方式编辑 **Data Dictionary | Web Scripts Extension** 位置的 FreeMarker 模板 `helloworld.get.html.ftl`：
```
<html>
    <body>
```



```html
<p>Hello! ${args.name}.</p>
</body>
</html>
```

- 5. 访问 `http://localhost:8080/alfresco/service/index` 来注册Web脚本，并点击**刷新Web脚本**按钮。
- 6. 现在，访问 URL `http://localhost:8080/alfresco/service/helloworld?name=Ramesh` 来执行这个Web脚本。执行时，你应该能看到 **你好！Ramesh。** 作为输出。

> 这里需要重点理解的一点是，即使在第三步中没有提供`name={argumentName}`，并且按照第六步所述访问URL来执行Web脚本，你也会得到相同的结果。在描述文档中指定`name={argumentName}`有助于你了解Web脚本所使用的参数。这样，只需要查看描述文档，你就能知道Web脚本的参数。作为一种良好的实践，你应该始终在描述文档中添加Web脚本所使用的参数。

## 扩展第一个Web脚本以使用控制器

Alfresco中的Web脚本框架通过遵循模型-视图-控制器模式来开发Web脚本，从而轻松实现关注点分离。所有业务逻辑都位于控制器中，并且可以按需使用多个视图，以Alfresco Web脚本框架支持的不同响应格式返回响应。模型对象是一种数据结构，用于将信息从控制器传递到视图。控制器使用所需数据填充模型对象，并将其传递给视图以生成响应。

在第一个Web脚本中，我们创建了一个基本且简单的Web脚本，它仅渲染输出，不与Alfresco存储库交互。理想情况下，与业务逻辑相关的任何处理，例如查询Alfresco存储库、创建内容、更新内容、从存储库删除内容以及执行操作，都应该在控制器中完成。控制器可以是服务器端JavaScript或基于Java的控制器。

让我们扩展第一个Web脚本，添加一个控制器，以了解控制器如何填充数据并将数据传递给FreeMarker模板，以及FreeMarker模板如何使用模型数据生成响应：

- 1. 登录到Alfresco Share UI。
- 2. 从顶部标题栏点击**存储库**链接。
- 3. 转到 **数据字典 | Web 脚本扩展**。
- 4. 按如下内容编辑描述文件 `helloworld.get.desc.xml`：

    ```
    <webscript>
      <shortname>Hello World</shortname>
      <description>First webscript Hello world</description>
      <url>/helloworld?name={argumentName}</url>
      <authentication>user</authentication>
    </webscript>
    ```

- 5. 创建一个名为 `helloworld.get.js` 的新文件，内容如下：

    ```
    model.email="Ramesh.chauhan@cignex.com";
    ```

- 6. 按如下内容编辑 FreeMarker 模板 `helloworld.get.html.ftl`：

    ```
    <html>
      <body>
        <p>Hello! ${args.name}.</p>
        <p>email address: ${email}</p>
      </body>
    </html>
    ```

- 7. 访问 `http://localhost:8080/alfresco/service/index`，点击**刷新Web脚本**按钮。
- 8. 现在，访问 URL `http://localhost:8080/alfresco/service/helloworld?name=Ramesh` 来执行该Web脚本。它会要求提供用户凭据。提供用户凭据后，你应该看到如下响应：
    **你好！Ramesh。**
    **电子邮件地址：Ramesh.chauhan@cignex.com**

这就是我们以简单方式在Web脚本中添加控制器的方法。在前面的示例中，我们将要在Web脚本响应中返回的电子邮件地址进行了硬编码。我们没有与Alfresco存储库进行任何交互。我们只是填充了模型对象，并从服务器端JavaScript控制器将其传递给FreeMarker模板。

> 模型基本上是一个根对象，Alfresco通过它实现服务器端JavaScript控制器与FreeMarker模板视图之间的数据共享。

你可以修改控制器，根据接收到的name参数，从Alfresco存储库动态检索电子邮件地址。借助Alfresco JavaScript API，你可以首先根据name参数从存储库中检索用户，然后获取该用户的电子邮件地址。在服务器端JavaScript控制器中，你可以利用可用的Alfresco JavaScript API与存储库交互。这样，我们就可以让Web脚本根据从Alfresco存储库检索到的数据来渲染响应。

> 在服务器端JavaScript控制器中，你可以利用可用的Alfresco JavaScript API访问Alfresco存储库，并根据需要执行业务逻辑。

## Web脚本执行的幕后

### 没有控制器的Web脚本

为了执行我们创建的第一个没有控制器的Web脚本，我们只需在Web浏览器中访问 URL http://localhost:8080/alfresco/service/helloworld?name=Ramesh，这基本上是一个HTTP GET请求调用。

所有与URL模式 `/alfresco/service/*` 匹配的传入请求都会映射到Alfresco Web应用程序配置中的Web脚本请求分发器Servlet。

然后，Servlet将请求传递给Web脚本运行时以处理Web脚本。

Web脚本运行时将控制权传递给Web脚本容器。Web脚本容器根据所调用的Web脚本URL、HTTP方法和Web脚本描述文档 helloworld.get.desc.xml 在内部识别要执行哪个Web脚本。我们没有在描述文档中显式指定任何响应格式，在执行Web脚本时也没有显式指定。因此，Web脚本框架将HTML作为默认格式渲染。根据FreeMarker模板的命名约定，我们回想起我们已将FreeMarker模板创建为 `helloworld.get.html.ftl`。这为Web脚本生成了HTML响应。随后，格式化的响应被发送回Web浏览器，以在Web脚本响应中显示HTML文本以及参数name的值。

### 带控制器的Web脚本

你一定已经注意到，在第一个Web脚本中，我们先是创建了没有控制器的Web脚本，然后扩展了该Web脚本以包含控制器。调用此Web脚本的URL保持不变。URL没有任何区别。只是在后端实现中额外实现了一个控制器。

幕后，当调用带有控制器的Web脚本时，Alfresco中的Web脚本框架会遵循与没有控制器的Web脚本相同的过程。唯一的区别是，在渲染响应之前，Alfresco中的Web脚本框架会首先执行控制器实现。控制器将首先完成与Alfresco存储库交互所需的业务逻辑处理，并生成要作为Web脚本响应返回的所需数据。控制器将填充模型对象并传递给FreeMarker模板。

## 任何Web脚本的重要事项

在Alfresco中实现Web脚本时，理解一些需要特别注意的重要事项至关重要。在开发Alfresco Web脚本时，你应该始终牢记三件重要的事情：

- 为Web脚本选择HTTP方法
- 指定Web脚本的URL和参数
- Web脚本的响应格式

### Web脚本支持的HTTP方法

在开发Web脚本并与Alfresco存储库交互时，你应确保为Web脚本选择适当的HTTP方法。理解Alfresco中Web脚本框架支持的各种HTTP方法非常重要，这样你可以为Web脚本选择合适的方法。




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
