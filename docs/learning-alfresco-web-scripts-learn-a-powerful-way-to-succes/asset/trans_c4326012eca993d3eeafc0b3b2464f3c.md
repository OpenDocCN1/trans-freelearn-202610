

Transaction: required
Format Style: extension
Default Format: json
Id: example/contentlocation.get
Description:classpath:alfresco/extension/templates/webscripts/example/contentlocation.get.desc.xml

# 测试 Web 脚本

现在是时候测试我们刚刚创建的 Web 脚本的功能了。我们基本上要确保我们已在 Java 支持的控制器中加入的逻辑按预期工作。

为了测试 Web 脚本，首先在 Alfresco 仓库中上传一些内容（例如一张图片）。记下其 nodeid。内容上传后，使用 Alfresco Share 获取上传内容 nodeid 的方法如下：将鼠标指针移到内容名称上，然后右键单击并点击复制链接位置。将此链接粘贴到文本编辑器中的某处并复制 workspace://SpacesStore; 之后的值，这就构成了 nodeid。现在，创建两个用户并为其中一个授予消费者访问权限，而对另一个用户不授予此内容的任何权限。确保继承权限也被取消选中。

现在，使用其中任意两个用户开始测试 Web 脚本。

## 测试用例 1 – 必填检查

nodeid 是我们创建的 Web 脚本的必填参数。作为第一个测试用例，让我们测试在不传递 nodeid 作为请求参数的情况下调用 Web 脚本。因此，第一个测试是不在 Web 脚本请求中传递 nodeid 参数。在浏览器中访问 http://localhost:8080/alfresco/service/getContentLocation URL。

由于我们将默认格式指定为 JSON，JSON 响应将按以下代码片段所示呈现。由于我们没有创建自定义状态模板并且使用了 status.setRedirect 为 true，Alfresco 中 Web 脚本框架提供的默认状态响应模板将被呈现。如果在调用 Web 脚本请求时明确请求 JSON 格式，例如 http://localhost:8080/alfresco/service/getContentLocation.json，也会显示相同的输出。

以下是 Web 脚本输出：

```
{
  "status" :
  {
    "code" : 400,
    "name" : "Bad Request",
    "description" : "Request sent by the client was syntactically incorrect."
  }
}
```

如果您想查看 XML 响应，则访问以下 URL http://localhost:8080/alfresco/service/getContentLocation.xml，响应如下：

```
<response>
  <status>
    <code>400</code>
    <name>Bad Request</name>
  </status>
</response>
```

## 测试用例 2 – 无效参数值

作为第二个测试用例，我们将通过为 nodeid 参数提供无效值来测试 Web 脚本。在调用 Web 脚本时提供 nodeid；但是为 nodeid 提供无效值并检查 JSON 和 XML 格式的响应。

对于 JSON 响应，访问 http://localhost:8080/alfresco/service/getContentLocation.json?nodeid=invalidnode 或 http://localhost:8080/alfresco/service/getContentLocation?nodeid=invalidnode。

JSON 格式的输出将如下所示：

```
{
  "status" :
  {
    "code" : 404,
    "name" : "Not Found",
    "description" : "Requested resource is not available."
  }
}
```

对于 XML 响应，访问 `http://localhost:8080/alfresco/service/getContentLocation.xml?nodeid=invalidnode`。它将以 XML 格式显示响应，提及 404 状态码。

## 测试用例 3 – 无效访问

我们在前面的测试用例中对 nodeid 参数进行了必填检查并使用无效值测试了 Web 脚本。现在，为 nodeid 提供正确的值，调用 Web 脚本，并为没有访问该内容权限的用户提供身份验证。我们的第三个测试用例是使用我们未授予任何内容访问权限的用户测试 Web 脚本。检查 JSON 和 XML 响应，如我们在前面的测试用例中所做的那样。它应给出 403 作为状态码，显示访问被禁止。

## 测试用例 4 – 使用有效数据测试

现在是时候使用一些有效测试数据测试 Web 脚本了，这意味着具有内容访问权限的用户和有效的 nodeid 应作为 Web 脚本请求参数传递。访问 Web 脚本 URL 并提供有效的 nodeid 作为参数，检查 JSON 和 XML 格式的响应。您现在应该能够看到显示内容在文件系统上的物理位置及其大小的响应。以下显示了示例 XML 响应：

```
```xml
<content>
    <filesystem_location>store://2014/7/20/xx/x/xxxxxx.bin
    </filesystem_location>
    <size>500</size>
</content>
```
```

一旦获得响应，您可以尝试导航到 `filesystem_location` 标签中收到的路径。目录结构可以在 `alfresco-global.properties` 中提到的 `alf_data` 位置下找到，该文件位于您 Alfresco 安装目录内的 `tomcat\shared\classes` 中。内容大小以字节为单位显示。重要的是要理解，我们不会在 `alf_data` 及其子文件夹中更新或删除任何内容，因为这可能会损坏仓库。

因此，对于 Alfresco 中的任何内容，您都可以找到其大小及其在文件系统上的物理位置。

## DeclarativeWebScript 与 AbstractWebScript

Alfresco 中的 Web 脚本框架提供了两个不同的帮助类，Java 支持的控制器可以从中派生。了解它们之间的区别很重要。

第一个帮助类是我们在本章开发 Web 脚本时使用的 `org.springframework.extensions.webscripts.DeclarativeWebScript`。第二个是 `org.springframework.extensions.webscripts.AbstractWebScript`。

`DeclarativeWebScript` 反过来仅扩展 `AbstractWebScript` 类。

如果 Java 支持的控制器派生自 `DeclarativeWebScript`，则执行辅助由 `DeclarativeWebScript` 类提供。此帮助类基本上封装了 Web 脚本的执行并检查是否与 Web 脚本关联了用 JavaScript 编写的控制器。如果为 Web 脚本找到 JavaScript 控制器，则此帮助类将执行它。此类将定位 Web 脚本的关联响应模板以用于请求的格式，并将填充的模型对象传递给响应模板。

对于扩展 `DeclarativeWebScript` 的控制器，Web 脚本的控制器逻辑应提供在 `Map<String, Object> executeImpl(WebScriptRequest req, Status status, Cache cache)` 方法中。大多数时候在开发 Java 支持的 Web 脚本时，控制器将仅扩展 `DeclarativeWebScript`。

`AbstractWebScript` 不以 `DeclarativeWebScript` 的方式提供执行辅助。它将整个执行过程的完全控制权交给派生类，并允许扩展类决定如何呈现输出。`DeclarativeWebScript` 类本身就是一个很好的例子。它扩展了 `AbstractWebScript` 类并提供了一种使用 FTL 模板呈现响应的机制。在像流式传输内容这样的场景中，将不需要响应模板；相反，内容本身需要直接呈现。在这种情况下，Java 支持的控制器类可以从 `AbstractWebScript` 扩展。

> 如果一个 Web 脚本同时具有基于 JavaScript 的控制器和基于 Java 的控制器，则：
> - 如果基于 Java 的控制器派生自 `DeclarativeWebScript`，则首先将执行基于 Java 的控制器，然后控制权将传递给基于 JavaScript 的控制器，然后再将模型对象返回给响应模板。
> - 如果基于 Java 的控制器派生自 `AbstractWebScript`，则仅执行基于 Java 的控制器。JavaScript 控制器将不会执行。

## 智能使用控制器

通常建议对 Web 脚本仅使用基于 Java 的控制器或基于 JavaScript 的控制器之一，并且不应同时使用两个控制器。然而，让我们看看一些可以利用同时使用两个控制器来解决一些关键问题的场景。

考虑一个场景，在生产系统中，您已经有一个具有扩展 `DeclarativeWebScript` 的基于 Java 的控制器并返回一些输出的 Web 脚本。现在，作为修复关键问题的一部分，需要在 Web 脚本响应中返回额外信息，并且您没有重启生产服务器的选项，因为服务器停机可能会影响客户业务。在这种场景下，您可以为 Web 脚本编写额外的 JavaScript 控制器，在模型对象中设置额外信息，并修改响应模板以包含新添加的信息。注册 Web 脚本，Web 脚本现在应返回额外信息以及之前的输出。因此，我们可以说我们完成了一种控制器执行的链式处理。可以将其视为类似于 servlet 链式处理。这就是 Alfresco Web 脚本框架的强大之处，使得非常有效地解决关键问题变得容易。

另一个场景可能是您想要具有过滤器之类的功能。在执行基于 JavaScript 的 Web 脚本之前，您需要执行一些与外部应用程序的业务逻辑通信，并根据其输出处理 Web 脚本的剩余部分。在这里，您可以考虑开发扩展 `DeclarativeWebScript` 的 Java 控制器。对于 Java 控制器，更容易为 spring 服务接线。使用它，调用外部应用程序并设置模型对象。现在，在基于 JavaScript 的控制器中，基于从 Java 控制器可用的模型对象的值，继续处理。这样就可以为 Web 脚本实现 servlet 过滤器之类的功能。

