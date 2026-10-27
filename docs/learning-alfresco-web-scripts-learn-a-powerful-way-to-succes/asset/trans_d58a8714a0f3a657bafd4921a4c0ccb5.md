

# 深入理解基于 JavaScript 的 Web 脚本

| 根对象 | 类名 | 描述 |
|---|---|---|
| activities | `org.alfresco.repo.activities.script.Activity` | 这提供了发布活动和 feed 控制的方法 |
| appUtils | `org.alfresco.repo.jscript.ApplicationScriptUtils` | 这提供了一些对外部应用程序可能有用的方法 |

除此之外，如果你在 Alfresco 安装目录中搜索 XML 文件中的 `parent="baseJavaScriptExtension"`，你将获得其他上下文文件中额外的根对象及其相关类的列表。以下是如前所述定义的其它可用根对象：`actionTrackingService`、`groups`、`formService`、`invitations`、`preferenceService`、`ratingService`、`renditionService`、`replicationService`、`siteService`、`slingshotDocLib`、`taggingService`、`thumbnailService`、`transfer`、`webprojects`、`cmisserver`、`commentService`、`bulkFSImport`、`jmx`，和 `adminIndexCheckService`。

要了解更多信息，你应该做一个简短的练习并找到这些根对象相关的 Java 类并阅读它们。这个练习将使你熟悉 JavaScript 后端控制器中可用的方法。

在开发 JavaScript 后端控制器时，基本概念很简单；查看为 JavaScript API 定义的 Java 类中可用的方法，并使用 extensionName 属性下给出的名称访问这些方法。

## 其他可用根对象

我们已经看到了其 API 方法在 Java 类中实现的根对象。Alfresco 中的 web 脚本框架还提供了一些其他默认根对象，你可以在 web 脚本的 JavaScript 控制器中使用。我们在查看 *第 3 章，理解 Web 脚本框架* 中 web 脚本执行的幕后实现时看到了执行 web 脚本时涉及的不同类。你可能想要查看 `spring-webscripts-*.jar` 和 `alfresco-remote-api-*.jar` 中的 web 脚本框架类，以详细了解这些根对象是如何在 JavaScript 控制器中提供的。以下是来自 web 脚本框架不同类的可用根对象：

AbstractWebScript 中的根对象如下表所示：

| 根对象 | 类名/对象名 | 描述 |
|---|---|---|
| webscript | org.springframework.extensions.webscripts.Description | 这提供了有关当前正在执行的 web 脚本的信息 |
| format | org.springframework.extensions.webscripts.FormatModel | 这表示渲染响应的所选格式 |
| args | 这是一个 map 对象 | 这保存传递给 web 脚本的参数 |
| argM | 这是一个 map 对象 | 这保存传递给 web 脚本的多值参数参数 |
| headers | 这是一个 map 对象 | 这保存传递给 web 脚本的请求头值 |
| headersM | 这是一个 map 对象 | 这保存传递给 web 脚本的多值请求头值 |
| guest | 这是一个 Boolean 值 | 这指示当前用户是否为访客用户 |
| url | org.springframework.extensions.webscripts.URLModel | 这提供了对 web 脚本 URL 及其各部分的访问 |
| msg | org.springframework.extensions.webscripts.ScriptMessage | 这提供了对 web 脚本的本地化消息的访问 |
| config | org.springframework.extensions.webscripts.ScriptConfigModel | 这提供了对 web 脚本的配置文档的访问 |
| formdata | object | 这保存为 web 脚本提交的表单数据 |
| requestbody | Content | 这表示请求正文的内容 |
| json | Object, JSONArray, or JSONObject | 这是从发布到 web 脚本的 JSON 请求生成的 |

## 深入理解基于 JavaScript 的 Web 脚本

`DeclarativeWebScript` 中的根对象如下表所示：

| 根对象 | 类名/对象名 | 描述 |
|---|---|---|
| model | 这是一个 map 对象 | 这将由控制器用于将数据传递给响应模板 |
| status | `org.springframework.extensions.webscripts.Status` | 这表示响应状态 |
| cache | `org.springframework.extensions.webscripts.Cache` | 这允许控制缓存 web 脚本的响应 |

`TenantRepositoryContainer` 中的根对象如下表所示：

| 根对象 | 类名/对象名 | 描述 |
|---|---|---|
| cmis | `org.alfresco.repo.cmis.client.CMISLocalConnectionManagerImpl` | 这表示 CMIS 客户端 |
| paging | `org.alfresco.repo.web.util.paging.Paging` | 这是一个分页 API |

`RepositoryContainer` 中的根对象如下：

| 根对象 | 类名/对象名 | 描述 |
|---|---|---|
| roothome | `org.alfresco.repo.jscript.ScriptNode` | 这表示存储库根节点。仅限已认证用户访问。 |
| companyhome | `org.alfresco.repo.jscript.ScriptNode` | 这表示公司主目录对象。仅限已认证用户访问。 |
| person | `org.alfresco.repo.jscript.ScriptNode` | 这表示当前已认证用户。 |
| userhome | `org.alfresco.repo.jscript.ScriptNode` | 这表示当前用户的主空间。 |

AbstractRuntimeContainer 中的根对象如下：

| 根对象 | 类名/对象名 | 描述 |
|---|---|---|
| server | org.alfresco.repo.web.scripts.RepositoryServerModel | 这提供了服务器详细信息 |
| atom | org.springframework.extensions.webscripts.atom.AtomService | 这用于解析和生成 atom 文档 |
| jsonUtils | org.springframework.extensions.webscripts.json.JSONUtils | 这用于解析和生成 JSON 对象 |
| stringUtils | org.springframework.extensions.webscripts.ScriptableUtils | 这是字符串的实用工具 |

Alfresco 中 web 脚本框架提供的大多数根对象也在响应模板中可用。如果你查看 web 脚本框架类，它们有实现可提供脚本根对象和模板根对象。一旦你了解了如何在 JavaScript 控制器中使用根对象，你也可以在响应模板中使用它们。在开发 web 脚本时，始终是一个好的做法在控制器中执行业务逻辑处理并填充 model 对象以在模板上显示输出。然而，在某些场景中，你可能也需要在需要时利用为模板提供的根对象来形成响应。

## 必知的 ScriptNode API

如果你正在创建 web 脚本并通过 JavaScript 控制器在 Alfresco 中访问或操作节点，那么你必须了解 ScriptNode API。这提供了对节点各种属性的访问，并提供了节点在安全、缩略图、所有权、版本控制、标记、转换等方面的不同有用方法。你可以阅读 ScriptNode 的代码来了解不同的方法。它的代码可在 alfresco-repository-*.jar 中找到，其完全限定类名是 org.alfresco.repo.jscript.ScriptNode。

同样，为了在响应模板中访问节点的属性并执行其他各种操作，你应该查看 alfresco-repository-*.jar 中的 org.alfresco.repo.template.TemplateNode。

## 代码示例

现在你已经了解了可在 JavaScript 控制器中使用的不同根对象，让我们看一些代码示例，以更好地理解如何将根对象付诸实践以实现所需功能。我们将介绍一些通用功能的代码示例，每个开发人员在某个时间点都会使用。我们将使用之前介绍的一些根对象。

## 创建文件夹

考虑这样一个场景：你有一个自定义前端应用程序和 Alfresco 作为后端存储库。现在，你希望用户能够从自定义前端在公司主目录下在 Alfresco 中创建文件夹。文件夹创建后，你可能还想在文件夹内创建一些内容或在文件夹上运行某些操作。首先，我们的目标是在公司主目录下创建一个文件夹。

为此，我们将创建一个新的 web 脚本 foldercreation，它将接受参数 foldername 并在公司主目录下创建同名文件夹，并以 XML 格式作为响应返回 noderef 值。你可以进一步使用返回的 noderef 根据需要调用其他 web 脚本，例如运行操作或创建内容。

在后续章节中，你将找到需要在 JavaScript 控制器和响应模板内部实现的核心代码片段，以实现上述场景。你也可以查看本章提供的代码文件，以查看此 web 脚本的 web 脚本实现。

## 检索显式参数

为了实现此场景，作为 JavaScript 控制器中的第一件事，我们需要读取参数 foldername。显式参数可以使用 args 根对象检索。因此，我们将按如下方式读取参数 foldername：

```
if (args.foldername != null && args.foldername != "")
```

> 如果你在 web 脚本 URL 中提供了隐式参数，那么这些参数将无法使用 args 根对象获取。对于它们，你可以使用 url 根对象获取其值，如 url.templateArgs.foldername。

