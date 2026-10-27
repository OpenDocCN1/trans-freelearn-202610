

## 配置文件——可选

可以为Alfresco Web脚本指定配置参数。它们可以从控制器以及响应模板中访问。为了理解这个概念，我们来看一个简单的示例：您希望在Web脚本输出中显示服务器环境（开发、测试、预发布或生产服务器）。您可以做的一件事是在控制器中指定相同内容，但理想情况下，控制器不应操心此类配置。相反，这可以单独处理，控制器或模板只需使用这些值即可。通过Web脚本配置文件，可以实现脚本特定的配置。

在Repository | Data Dictionary | Web Scripts Extensions位置创建helloworld.get.config.xml，并向其中添加以下内容：

```
<properties>
    <envname>Dev</envname>
</properties>
```

## 在控制器中访问配置

在JavaScript控制器中，envname属性可以如下方式访问：

```
var env = new XML(config.script);
logger.log(env.envname);
```

您可以在模型对象中设置envname属性的值，并在响应模板上渲染该值。

## 在模板中访问配置

当Web脚本没有可用的控制器实现时，可以在响应FreeMarker模板中直接访问envname属性，如下所示：

```
${config.script["properties"]["envname"]}
```

我们已经看到了两种从Web脚本配置显示值的不同方式。相信您现在可以轻松地在hello world Web脚本中进行测试并查看结果。

## 5
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

