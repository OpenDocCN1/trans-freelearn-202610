# 第5章：调用Web脚本
### 调用Web脚本

本章将介绍如何以不同方式调用Alfresco Web脚本，例如：

- 通过Web浏览器和REST客户端插件调用Web脚本
- 通过独立的Java程序执行Web脚本
- 从基于Spring的服务中调用Web脚本
- 从Alfresco Share中以不同方式调用Web脚本
- 从命令行调用Web脚本
- 从JSF页面或JSR-168门户调用Web脚本
- 处理客户端的限制

在Alfresco中使用Web脚本时，您可能会遇到一些情况，想知道如何执行Web脚本。例如，一些常见的实际场景：在开发Web脚本时，您会希望对其进行单元测试。您需要调用Web脚本以验证它工作正常并返回预期输出。在支持项目中，当使用后端Alfresco存储库开发自定义前端应用程序时，在调试期间修复问题的过程中，您需要执行Web脚本并验证响应，以确定问题是前端问题还是后端问题。在定制Alfresco Share时，您需要调用Alfresco存储库的Web脚本。有时，您可能需要从独立的Java类调用Web脚本。可能您有一个基于Spring的应用程序，想要在调用Alfresco Web脚本。有时，您可能还需要从Linux shell脚本调用Web脚本，此类场景还有很多。

了解如何在这些基本的、可能的实际实现场景中调用Web脚本非常有用。在本章中，我们将介绍如何在前面提到的常见场景中调用Web脚本，这些场景是任何开发人员在使用Alfresco时都会遇到的。

## 从Web浏览器调用Web脚本

调用Web脚本最简单的方法之一是通过Web浏览器。这对每个人来说都是一个方便的选择。Web浏览器是大家在开发Web脚本时通常用来调用Web脚本进行测试的常见客户端。

您开发的每个HTTP GET Web脚本都可以通过浏览器访问Web脚本URL来执行。如果Web脚本需要任何身份验证，则会弹出身份验证窗口以提供身份验证详细信息。

## 使用Web浏览器插件执行Web脚本

要测试您为POST、PUT或DELETE创建的Web脚本，不能直接从Web浏览器执行。您需要通过浏览器提供的插件来调用HTTP POST、PUT或DELETE请求。浏览器插件有多种选择。这里我们将介绍两个插件。

## Poster——Firefox插件

一个插件选项是名为Poster的Firefox插件。您可以从Firefox插件链接下载该插件：https://addons.mozilla.org/。如果您已经安装了Firefox，请打开Firefox浏览器，进入插件链接并搜索Poster，然后从显示的插件列表中添加到Firefox。另一种更快更简单的安装方式是进入“工具 | 附加组件”，搜索Poster并点击“安装”按钮。

安装插件后，进入“工具 | Poster”打开Poster窗口。在此窗口中，您可以提供Web脚本URL、身份验证凭据、请求标头、参数，并指定调用Web脚本的HTTP方法。

## 适用于Chrome的Advanced REST client

另一个好的插件选项是适用于Chrome的Advanced REST client扩展。您可以从此Chrome应用商店链接下载该插件：https://chrome.google.com/webstore/category/apps。如果您已经安装了Chrome，请打开Chrome浏览器，进入我们刚才提到的链接并搜索advanced rest client，然后从显示的结果列表中按照Chrome扩展安装流程安装Advanced REST client。另一种更快更简单的安装方式是通过“工具 | 扩展程序”并点击“获取更多扩展程序”链接。现在，搜索Advanced REST client，并从显示的结果列表中按照Chrome扩展安装流程安装该插件。

安装Advanced REST client插件后，要启动它，请在Chrome浏览器地址栏中输入chrome-extension://hgmloofddfdnphfgcellkdfbfbjeloo/RestClient.html。另一种快速简便的启动方式是直接在地址栏中输入chrome://apps并回车。您会在这里看到Advanced REST client，点击即可。现在，您可以通过适用于Chrome的Advanced REST client插件执行所有GET、POST、PUT和DELETE方法的Web脚本。

## 从独立Java程序执行Web脚本

从Java程序调用Web脚本有多种选项。在这里，我们将结合代码片段详细演练Apache commons HttpClient API，以了解如何从Java程序执行Web脚本，并简要介绍一些其他可用于从Java程序调用Web脚本的替代方案。

## HttpClient

执行Web脚本的一种方式是使用`org.apache.commons.httpclient.HttpClient` API调用Web脚本。该类可在`commons-httpclient-3.1.jar`中找到。使用HttpClient API执行Web脚本还需要`commons-logging-*.jar`和`commons-codec-*.jar`作为支持JAR包。这些JAR包位于Alfresco安装目录中的`tomcat\webapps\alfresco\WEB-INF\lib`下。您需要将它们包含到项目的构建路径中。我们将尝试从独立Java程序使用HttpClient执行hello world Web脚本。使用HttpClient时，通常需要遵循以下步骤：

- 1. 创建一个新的HttpClient实例。
- 2. 下一步是创建一个方法实例（我们将使用GetMethod）。URL需要传递给方法的构造函数。
- 3. 如果需要，设置任何参数。
- 4. 如果需要，提供身份验证详细信息。
- 5. 让HttpClient执行该方法。
- 6. 读取响应状态码和响应。
- 7. 最后，释放连接。

## 理解如何使用HttpClient调用Web脚本

考虑前面提到的步骤，让我们看以下代码片段。为了测试此代码，您可以创建一个带有main方法的独立Java程序，将以下代码片段放入Java程序中，然后根据需要修改Web脚本URL/凭据。以下代码片段中提供了注释，方便您将前面的步骤与代码对应起来：

```java
// Create a new instance of HttpClient
HttpClient objHttpClient = new HttpClient();

// Create a new method instance as required. Here it is GetMethod.
GetMethod objGetMethod = new GetMethod("http://localhost:8080/alfresco/service/helloworld");

// Set querystring parameters if required.
objGetMethod.setQueryString(new NameValuePair[] { new NameValuePair("name", "Ramesh") });

// set the credentials if authentication is required.
```



Credentials defaultcreds = new UsernamePasswordCredentials("admin","admin");
objHttpClient.getState().setCredentials(new
AuthScope("localhost",8080, AuthScope.ANY_REALM), defaultcreds);

try {
    // Now, execute the method using HttpClient.
    int statusCode = objHttpClient.executeMethod(objGetMethod);
    if (statusCode != HttpStatus.SC_OK) {
        System.err.println("Method invocation failed: " +
            objGetMethod.getStatusLine());
    }

    // Read the response body.
    byte[] responseBody = objGetMethod.getResponseBody();

    // Print the response body.
    System.out.println(new String(responseBody));

} catch (HttpException e) {
    System.err.println("Http exception: " + e.getMessage());
    e.printStackTrace();
} catch (IOException e) {
    System.err.println("IO exception transport error: " +
        e.getMessage());
    e.printStackTrace();
} finally {
    // Release the method connection.
    objGetMethod.releaseConnection();
}

> 请注意，Apache commons 客户端现在是一个遗留项目，不再进行开发。该项目已被 Apache HttpComponents 项目中的 HttpClient 和 HttpCore 模块所取代。我们在此使用了来自 Apache commons 客户端的 HttpClient，以获得整体的理解。

在后续章节中提到了从 Java 程序调用 Web 脚本的其他一些选项。

## URLConnection

从 Java 程序执行 Web 脚本的一个选项是使用 java.net.URLConnection。有关更多详细信息，您可以参考 http://docs.oracle.com/javase/tutorial/networking/urls/readingWriting.html。

## Apache HTTP components

从 Java 程序执行 Web 脚本的另一个选项是使用 Apache HTTP components，它们是用于 HTTP 通信的最新可用 API。这些组件提供更好的性能和更大的灵活性，可在 httpclient-*.jar 和 httpcore-*.jar 中获得。这些 JAR 文件可在您的 Alfresco 安装目录中位于 tomcat\webapps\alfresco\WEBINF\lib 位置。有关更多详细信息，请参阅 https://hc.apache.org/httpcomponents-client-4.3.x/quickstart.html，以了解如何从 Java 程序执行 HTTP 调用。

## RestTemplate

另一个替代方案是使用位于 tomcat\webapps\alfresco\WEB-INF\lib 中您的 Alfresco 安装目录内的 org.springframework.web-*.jar 中的 org.springframework.web.client.RestTemplate。如果您使用的是 Alfresco Community 5，则 RestTemplate 类可在 spring-web-*.jar 中获得。通常，RestTemplate 用于 Spring 基础服务中以调用 HTTP 通信。

## Calling web scripts from Spring-based services

如果您需要从 Spring 基础服务调用 Alfresco Web 脚本，则需要使用 RestTemplate 来调用 HTTP 调用。这是从 Spring 基础类执行 HTTP 调用的最常用的技术。为此，需要执行以下步骤。代码片段也已提供：

+   1. 在 Spring 上下文文件中定义 RestTemplate：
+   ```
+   <bean id="restTemplate" class="org.springframework.web.client.RestTemplate" />
+   ```

+   2. 在 Spring 上下文文件中，将 restTemplate 注入到您的 Spring 类中，如以下示例所示：
+   ```
+   <bean id="httpCommService" class="com.test.HTTPCallService">
+       <property name="restTemplate" value="restTemplate" />
+   </bean>
+   ```

+   3. 在 Java 类中，按如下方式定义 `restTemplate` 的 setter 方法：

```
private RestTemplate restTemplate;
public void setRestTemplate(RestTemplate restTemplate) {
    this.restTemplate = restTemplate;
}
```

+   4. 为了调用具有用户身份验证级别的 Web 脚本，您可以在 Java 类中使用 RestTemplate，如以下代码片段所示。以下代码片段是从 Spring 基础服务中使用 RestTemplate 调用 hello world Web 脚本的示例：

// setup authentication
String plainCredentials = "admin:admin";
byte[] plainCredBytes = plainCredentials.getBytes();
byte[] base64CredBytes = Base64.encodeBase64(plainCredBytes);
String base64Credentials = new String(base64CredBytes);

// setup request headers
HttpHeaders reqHeaders = new HttpHeaders();
reqHeaders.add("Authorization", "Basic " + base64Credentials);
HttpEntity<String> requestEntity = new
HttpEntity<String>(reqHeaders);

// Execute method
ResponseEntity<String> responseEntity = restTemplate.
exchange("http://localhost:8080/alfresco/service/
helloworld?name=Ramesh", HttpMethod.GET, requestEntity, String.
class);
System.out.println("Response:"+responseEntity.getBody());

## Invoking a web script from Alfresco Share

在定制 Alfresco Share 时，您需要调用 Alfresco 仓库 Web 脚本。在 Alfresco Share 中，您可以从两个地方调用仓库 Web 脚本。一个是组件级展示 Web 脚本，另一个是客户端 JavaScript。

## Calling a web script from the presentation web script JavaScript controller

Alfresco Share 使用展示 Web 脚本渲染用户界面。这些展示 Web 脚本调用仓库 Web 脚本以渲染仓库数据。仓库 Web 脚本在组件渲染文件（例如，`get.html.ftl`）加载之前被调用。

在开箱即用的 Alfresco 安装中，您应该能够在 `tomcat\webapps\share\WEB-INF\classes\alfresco\site-webscripts` 下看到组件的展示 Web 脚本。

在开发自定义组件时，您需要编写一个展示 Web 脚本。展示 Web 脚本将调用仓库 Web 脚本。您可以按如下方式调用仓库 Web 脚本：

```
var response = remote.call("url of web script as defined in description document");
var obj = eval('(' + response + ')');
```

在前面的代码片段中，我们使用了开箱即用的 remote 对象来调用仓库 Web 脚本。需要注意的重要一点是，我们必须提供在描述文档中定义的方式的 Web 脚本 URL。没有必要提供主机或端口名称、应用程序名称和服务路径等初始部分，就像我们在从 Web 浏览器调用 Web 脚本时使用的方式一样。一旦收到响应，就可以使用 `eval` 函数解析 Web 脚本响应。

在 Alfresco Share 的开箱即用代码中，您可以找到展示 Web 脚本调用仓库 Web 脚本，如我们在前面的代码片段中所见。例如，查看位于您的 Alfresco 安装目录内 `tomcat\webapps\share\components\site-members` 位置的 `site-members.get.js` 文件中的 `main()` 方法。您可以在 `tomcat\webapps\share\WEB-INF\classes\alfresco\site-webscripts` 中查看其他 JavaScript 控制器实现，这些实现使用前面提到的方法调用仓库 Web 脚本。

> 在指定路径以引用开箱即用的 Web 脚本时，它是从 `tomcat\webapps` 开始提及的。该位置可在您的 Alfresco 安装目录中找到。

## Invoking a web script from client-side JavaScript

客户端 JavaScript 控制文件可以与 Alfresco Share 中的组件关联。如果您需要进行仓库 Web 脚本调用，可以从通常位于 tomcat\webapps\share\components 的客户端 JavaScript 控制文件执行。您可以使用基于 YUI 的客户端 JavaScript 文件以不同的方式进行仓库 Web 脚本调用。以下是从客户端 JavaScript 文件调用 Web 脚本的一些方式。还提供了引用，以便查看 Alfresco 开箱即用实现以实际了解其用法：

+   - Alfresco.util.Ajax.request: 查看 tomcat\webapps\share\components\console\groups.js 并参考 _removeUser 函数。
+   - Alfresco.util.Ajax.jsonRequest: 查看 tomcat\webapps\share\components\documentlibrary\documentlist.js 并参考 onOptionSelect 函数。
+   - Alfresco.util.Ajax.jsonGet: 直接调用获取 Web 脚本，请查看 tomcat\webapps\share\components\console\groups.js 并参考 getParentGroups 函数。
+   - YAHOO.util.Connect.asyncRequest: 查看 tomcat\webapps\share\components\documentlibrary\tree.js 并参考 _sortNodeChildren 函数。

> 在位于 tomcat\webapps\share\js 的 alfresco.js 中，提供了 YAHOO.util.Connect.asyncRequest 的包装实现，并且可以在 alfresco.js 中找到各种可用方法，例如我们在前面的列表中看到的 Alfresco.util.Ajax.request、Alfresco.util.Ajax.jsonRequest 和 Alfresco.util.Ajax.jsonGet。因此，前一个列表中的前三个选项在内部仅使用 YAHOO.util.Connect.asyncRequest（前一个列表中的最后一个选项）进行调用。

## Calling a web script from the command line

有时在处理项目时，可能需要从 Linux 机器调用 Web 脚本或创建 shell 脚本来调用 Web 脚本。可以使用 cURL 从命令行调用 Web 脚本，它是在处理 Web 脚本时使用的有价值的工具。




你可以在 Linux、Mac 或 Windows 上安装 cURL，并从命令行执行 web script。有关 cURL 的更多详细信息，请参阅 http://curl.haxx.se/ 。你需要先安装 cURL。在 Linux 上，你可以使用 apt-get 安装 cURL。在 Mac 上，你应该可以通过 MacPorts 安装 cURL，在 Windows 上则可以通过 Cygwin 安装 cURL。

一旦安装了 cURL，你可以按如下方式从命令行调用 web script：

```
curl -u admin:admin "http://localhost:8080/alfresco/service/
helloworld?name=Ramesh"
```

这将显示 web script 的响应。

## 从 JSR-168 门户和 JSF 页面调用 web script

Alfresco 中的 Web script 框架提供了一个辅助工具，用于从某些不了解 HTTP 的客户端环境调用 Alfresco web script。因此，它可以轻松地从这样的环境中调用 web script，并且将通过调用环境所熟悉的机制来调用 web script。为了允许 JSR-168 门户和 JSF 页面根据其环境自然地调用 web script，Alfresco 中的 web script 框架为它们提供了辅助工具。让我们从高层次来看一下它们及其工作方式。

### 从 JSR-168 门户调用 web script

可以将 Alfresco web script 当作 JSR 门户 portlet 来调用。我们将看看为此需要在 Alfresco 中进行的配置。web script 的条目应在 portlet.xml 中提供。web script URL 应配置为 scripturl 参数下的 JSR 门户 URL，以便能够将 web script 作为 portlet 调用。基本上，它会在 web script 描述文档中定义的 URL 前添加 /alfresco/168s。

例如，你可以查看位于 tomcat\webapps\alfresco\WEB-INF\classes\alfresco\templates\webscripts\org\alfresco\portlets\mytasks.get.desc.xml 和 mytasks.get.html.ftl 的 mytasks web script 的开箱即用实现。如果你使用的是 Alfresco Community 5，这两个文件位于 alfresco-remote-api-*.jar 内的 alfresco 包下。mytasks web script 作为 portlet 的条目配置在 tomcat\webapps\alfresco\WEB-INF\portlet.xml 中。

### 从 JSF 页面调用 web script

可以从 JSF 页面将 web script 当作标签库来调用。Alfresco 中提供了标签实现，使 JSF 页面能够调用 web script。它位于 Alfresco 安装目录下 `tomcat\webapps\alfresco\WEB-INF` 位置的 `repo.tld` 中。

请查看 `tomcat\webapps\alfresco\jsp\dashboards\dashlets\mytasks-webscript.jsp` 以了解其用法。

### 处理客户端限制

其中一个限制是并非所有客户端都能发出所有 HTTP 调用。可能仅局限于 GET 或 POST 调用。Alfresco 中的 web script 框架提供了一种使用 POST 方法隧道传输 HTTP 方法的方式，通过这种方式，你可以调用其他 HTTP 方法调用。这可以通过在 HTTP 请求中使用 `X-HTTP-Method-Override` 头来实现，其值为方法名称，例如 GET。另一种方式是指定 `alf_method` 作为查询字符串参数。

另一个客户端限制可能是并非所有客户端都能优雅地处理非成功状态的 HTTP 状态码。在这种情况下，有一种机制可以强制 HTTP 响应在响应头中指定成功。响应主体仍然表示如同收到非成功状态一样的内容，这允许客户端了解 web script 提供的错误代码或消息（如果有的话）。为了能够优雅地处理非成功场景下的 HTTP 状态码，可以通过在 HTTP 请求上设置 `alf-force-success-response` 头并将值设为 `true`，从而每次都强制在 HTTP 响应中指定成功。

## 总结

在本章中，我们了解了调用 web script 的不同方式，例如从 Web 浏览器调用。使用可用于 Web 浏览器的附加组件，可以调用 web script。我们已经看到了从独立 Java 类调用 web script 的不同方式。我们还了解了如何从基于 Spring 的服务和命令行调用 web script。我们了解了将 web script 作为 JSR-168 portlet 和 JSF 页面调用的可能性。我们还看到了 Alfresco 中的 web script 框架如何为我们提供处理调用客户端某些限制的方法。

我们将在下一章学习如何开发基于 Java 的 web script。

在 Alfresco 中开发 web script 时，为 web script 编写控制器实现有两种选择：基于 JavaScript 的控制器和基于 Java 的控制器。根据你的偏好，你可以在需要时从这两者中选择控制器实现。与基于 Java 的控制器相比，使用基于 JavaScript 的控制器可以加快开发周期。在基于 JavaScript 的控制器中，你可以使用 Alfresco JavaScript API 完成许多事情。然而，有时这可能不足以实现你的业务需求。在这种情况下，你可以使用基于 Java 的 web script，在其中可以使用所有 Alfresco 服务，如 NodeService、SearchService、ContentService 等，并且可以根据需要构建 web script。

在那些你可能最终开发大量 web script 的项目中，你必须根据项目执行的可行性来决定将实现哪种类型的控制器实现。你可以选择开发基于 JavaScript 控制器的 web script 以加快开发速度，并且仅在 JavaScript API 不适合你的需求时才开发基于 Java 的 web script。或者，你始终可以使用基于 Java 的 web script，以便为你的项目采用标准化的开发方法。

了解使用两种类型的控制器实现 web script 是很重要的。我们之前创建 hello world web script 时已经看到了如何使用基于 JavaScript 的控制器。现在，是时候了解基于 Java 的 web script 了。在本章中，我们将为示例用例在 Alfresco 中实现一个基于 Java 的 web script，以便你逐步了解如何创建一个具有 Java 后端控制器实现的 web script。然后，你可以根据项目中的业务需求开发基于 Java 的 web script。

## 用例场景

我们将采用一个非常简单的场景，以全面了解创建具有 Java 后端控制器的 web script。我们的主要目标是了解如何创建基于 Java 的 web script，以及如何在基于 Java 的控制器中使用 Alfresco API。让我们开始吧。

你必须知道，Alfresco 将内容的元数据存储在数据库中，并将实际内容存储在文件系统上。现在，一旦内容在 Alfresco 中可用，你可能希望了解其位置，并找出它在文件系统上的实际位置。所以，下次你与某人讨论 Alfresco 中的内容时，你实际上可以展示 Alfresco 中内容与内容存储下文件系统上实际物理位置的映射。

此外，在涉及外部应用程序与 Alfresco 集成的项目中，你可能在某个时候还想知道内容的大小以及它在文件系统上的位置。

让我们将此作为构建基于 Java 的 web script 的用例场景。我们基本上想在 Alfresco 中创建一个 web script，为给定的节点提供其在文件系统上的实际位置及其内容大小。

## 高层级的 web script 功能

在理解了我们要实现的整体场景之后，让我们从高层级了解这个 web script 将如何工作：

- 需要将参数 nodeid 作为请求参数传递给 web script。
- 只有经过身份验证的用户才能访问该 web script。
- 如果用户对给定节点至少具有读取访问权限，那么才会提供该节点的文件系统物理位置和内容大小。
- 响应将以 JSON 和 XML 格式提供。格式可以在调用 web script 时指定。

## 准备工作

让我们回顾一下对 web script 构建模块的了解，并确定需要为此 web script 创建的组件，如下所示：

- 每个 web script 都需要有一个描述文档。因此，第一步是创建 web script 描述文档。
- web script 需要以 XML 和 JSON 格式返回响应，因此有必要提供用于渲染 XML 响应的响应模板和用于渲染 JSON 响应的响应模板。
- 获取文件系统位置和大小的整个逻辑将位于 web script 控制器中。在这种情况下，需要创建一个基于 Java 的 web script 控制器来处理业务逻辑。
- 为了向 web script 注册基于 Java 的控制器，需要 Spring 配置。因此，需要创建一个 Spring 上下文文件，以向 web script 注册基于 Java 的控制器。并非总是需要创建新的 Spring 上下文文件。你可以根据系统逻辑模块拥有一个上下文文件，并且属于该模块的所有 web script 都可以在该上下文文件中注册。





对于此 Web 脚本，您可以使用您喜欢的编辑器创建 Web 脚本所需的<|message|> 文件并开发控制器 Java 类。我们将在第 9 章《Mavenizing Web Scripts》中介绍使用 Maven 设置开发环境。在本章中，我们主要旨在了解如何实现一个由 Java 支持的 Web 脚本。您需要在类路径中添加 `spring-webscripts-*.jar` 和 `alfresco-data-model-*.jar` 才能编译控制器 Java 类。这两个 JAR 都可以在 Alfresco 安装中的 `tomcat\webapps\alfresco\WEB-INF\lib` 位置找到。JAR 名称中的 `*` 符号指的是版本号。

## 创建描述文档

由于 Web 脚本的主要目的是返回内容位置，让我们使用 Web 脚本 ID `contentlocation`。我们将使用 HTTP GET 方法，因为我们将检索节点的<|message|> 息。

作为第一步，为 Web 脚本创建描述文档。创建一个名为 `contentlocation.get.desc.xml` 的文件，其内容如下：

```
<webscript>
    <shortname>Get content location and size</shortname>
    <description>Gets the physical location for the content and size of the content</description>
    <url>/getContentLocation?nodeid={nodeid}</url>
    <url>/getContentLocation.json?nodeid={nodeid}</url>
    <url>/getContentLocation.xml?nodeid={nodeid}</url>
    <format default="json">extension</format>
    <authentication>user</authentication>
</webscript>
```

让我们理解一下我们刚刚做了什么。我们创建了一个描述文档，它在 `shortname` 和 `description` 标签中分别提供了关于 Web 脚本的人类可读名称和简要信息。

我们定义了三个可用于调用 Web 脚本的 URL。第一个是默认 URL，另外两个是显式提及以分别以 JSON 和 XML 格式获取响应的 URL。请注意，第一和第二个 URL 将产生相同的输出。

默认响应格式指定为 `json`。`format` 标签的 `extension` 值提及，可以在调用 Web 脚本时将响应内容类型指定为 Web 脚本 ID 的扩展。

而且最重要的是，Web 脚本在被调用时期望一个名为 `nodeid` 的参数。

## 创建响应模板

作为 Web 脚本的经验法则，我们必须遵循 Alfresco 中 Web 脚本框架期望的命名约定。由于 Web 脚本应该以 JSON 和 XML 格式返回响应，我们需要为 Web 脚本创建两个响应模板。按照命名约定，有两个文件需要创建——`contentlocation.get.json.ftl` 和 `contentlocation.get.xml.ftl` 作为响应模板。按如下方式在响应中返回 `filesystem_location` 和大小：

1. 要创建 JSON 响应模板，创建一个名为 `contentlocation.get.json.ftl` 的文件，其内容如下：

```json
{
    "content" :
    {
        "filesystem_location" : "${contentFSLocation}",
        "size" : ${contentSize}
    }
}
```

2. 要创建 XML 响应模板，创建一个名为 `contentlocation.get.xml.ftl` 的文件，其内容如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<content>
    <filesystem_location>${contentFSLocation}</filesystem_location>
    <size>${contentSize}</size>
</content>
```

我们刚刚创建了两个响应模板，以返回 Web 脚本的输出。前述代码片段中 `${}` 内使用的名称是要从控制器类设置的模型变量名称。请确保在实现控制器时，模型变量名称与响应模板中使用的名称相同。

## 创建 Java 控制器

现在是进行一些 Java 编码的时候了。在开始实际编码之前，快速确定我们将要编写的代码需要处理的逻辑始终是一个好主意。这个练习确实非常有帮助。

### 高层次的控制器逻辑

为了为此场景开发控制器，让我们从高层次来看一下我们将包含在由 Java 支持的控制器中的实现逻辑：

- 控制器将期望 `nodeid` 作为输入参数
- 检查给定的节点是否存在于存储库中
- 检查用户是否至少对给定节点具有读取权限
- 获取给定节点的 filesystem 位置
- 获取给定节点在 filesystem 上的大小
- 准备模型对象并将其返回给 FTL 响应模板

### 让我们编码！

让我们执行以下步骤来创建 Web 脚本的 Java 支持控制器实现，该实现将执行上一节中提到的所有必需逻辑：

1. 创建一个新类，`com.example.content.ContentLocationWebScript`。通常提供描述性命名风格并将 Web 脚本分类到适当的包中是一个好主意。这在需要开发大量 Web 脚本的场景中会很有用。由于这是一个与内容相关的 Web 脚本，我们将其保留在 `com.example.content` 包下，并命名为 `ContentLocationWebScript`。

2. 您刚刚创建的 Java 类 `ContentLocationWebScript` 必须扩展 `org.springframework.extensions.webscripts.DeclarativeWebScript`，如下所示：

```
public class ContentLocationWebScript extends org.springframework.extensions.webscripts.DeclarativeWebScript {
```

3. 我们将使用 Node 服务，它基本上用于在 Alfresco 中执行节点相关操作，例如创建、更新、删除、设置属性和获取属性，以获取有关 filesystem 位置的详细信息。我们将使用 Permission 服务来识别用户是否具有读取访问权限。现在下一步是为它们提供声明。两者都将作为 Spring 依赖项注入，如下所示：

```
private NodeService nodeService;
private PermissionService permissionService;
```

4. 现在，检查用户是否至少对给定的 noderef 具有读取访问权限。控制器只有在用户至少对节点具有读取权限时才会继续，否则控制器将设置适当的错误响应代码。

```
if (permissionService.hasPermission(contentNodeRef,
PermissionService.READ) != AccessStatus.ALLOWED) {
    status.setCode(Status.STATUS_FORBIDDEN);
    status.setRedirect(true);
    return null;
}
```

5. 下一步是获取内容的文件系统位置和大小，如下所示：

```
ContentData contentData = (ContentData) nodeService.
getProperty(contentNodeRef, ContentModel.PROP_CONTENT);
String contentFileLocation = contentData.getContentUrl();
Long contentSize = contentData.getSize();
```

6. 创建并设置模型对象以在响应中呈现信息，如下所示：

```
Map<String, Object> model = new HashMap<String, Object>();
model.put("contentPSLocation", contentFileLocation);
model.put("contentSize", contentSize);
return model;
}
```

7. 在类的末尾，为节点服务和权限服务提供 setter 方法，如下所示：

```
public void setNodeService(NodeService nodeService) {
    this.nodeService = nodeService;
}
```

```
public void setPermissionService(PermissionService permissionService) {
    this.permissionService = permissionService;
}
}
```

8. 保存该类，Java 控制器实现现在已准备就绪。

我们刚刚创建了一个由 Java 支持的控制器实现，它将在返回文件系统上的内容位置和内容大小之前执行指定的逻辑处理。我们还在控制器中使用了节点服务和权限服务。

## 为 Web 脚本配置控制器

一旦 Java 控制器准备就绪，就需要为 Web 脚本配置它，以便 Alfresco 中的 Web 脚本框架知道这个 Java 类是 `contentlocation` Web 脚本的控制器。这可以通过创建 spring 上下文文件并在其中添加 Java 类的条目来完成。

创建一个文件 `custom-example-webscripts-context.xml`，其内容如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE beans PUBLIC '-//SPRING//DTD BEAN//EN' 'http://www.springframework.org/dtd/spring-beans.dtd'>
<beans>
    <bean id="webscript.example.contentlocation.get" class="com.example.content.ContentLocationWebScript" parent="webscript">
        <property name="nodeService" ref="nodeService" />
        <property name="permissionService" ref="permissionService" />
    </bean>
</beans>
```

我们创建了一个 spring 上下文文件，将 Java 控制器与 `contentlocation` Web 脚本关联起来，以便让 Alfresco 中的 Web 脚本框架知道 `ContentLocationWebScript` Java 类是 `contentlocation` Web 脚本的控制器。这是通过在上下文文件中提供 bean 条目时遵循适当的命名约定来完成的。

最重要的是要指定正确的 bean ID。需要遵循的命名约定是 `webscript.package.webscriptid.httpmethod`。

因此，构建方式是，如果 Web 脚本描述文件位于 `templates\webscripts\example\contentlocation.get.desc.xml` 位置，则 bean ID 将为 `webscript.example.contentlocation.get`。遵循此命名约定是必须的，以便将类注册为 Web 脚本控制器。如果您未能这样做，Alfresco 中的 Web 脚本框架将无法将该类注册为 Web 脚本控制器，并且当您尝试执行 Web 脚本时，控制器代码将不会被调用。





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



