# 第4章：Web脚本的构建块
Web 脚本的
构建块

在本章中，我们将详细介绍创建 Web 脚本的不同组件：

- 描述文档
- Controller 实现
- 响应模板
- I18N 消息包
- 配置文档
- 每个组件的命名约定

## Web 脚本的构建块

Web 脚本是 Alfresco 企业内容管理系统的一个关键特性，实现起来非常容易。深入了解构成 Web 脚本的组件将使你在 Alfresco 中开发 Web 脚本的工作变得更加轻松。

本章将为你提供有关 Web 脚本构建块的详细知识。彻底理解用于在 Alfresco 中开发 Web 脚本的不同工件是非常必要的。在你在任何项目中实施 Alfresco Web 脚本的整个过程中，你在开发 Web 脚本时始终需要使用这些组件，因此，清楚地理解它非常重要，这样你就可以在需要时快速轻松地实现 Web 脚本。

Alfresco 提供了一个强大的 Web 脚本框架，使用熟悉的技术（如脚本和模板语言）即可轻松创建 Web 脚本。每个 Web 脚本都必须有一个描述文档，并且至少有一个或多个 FreeMarker 响应模板文档。Web 脚本可以可选地有一个作为后端实现的控制器，它将根据定义的业务逻辑对 Alfresco 仓库执行必要的处理。也可以进行一些可选的高级实现，为 Web 脚本响应提供消息包以及根据需要对 Web 脚本进行一些配置选项。

让我们详细了解 Web 脚本的每个组件，以便更好地理解它们。

## 描述文档 – 必不可少！

在开发 Web 脚本时，首先要做的是进行简要练习，确定 Web 脚本的 URI 和 HTTP 方法、提供给 Web 脚本的任何参数、为 Web 脚本使用的响应格式、检查是否需要对 Web 脚本使用任何身份验证、以及检查是否需要与仓库进行后端处理，然后开始实现并首先为 Web 脚本创建描述文档。

描述文档是 Web 脚本实现的核心，它基本上是一个 XML 文件。它描述了有关 Web 脚本的重要细节。任何查看描述文档的开发人员都可以大致了解 Web 脚本的作用。

这是创建 Alfresco 中的 Web 脚本时需要开发的第一个文件。Web 脚本描述文档中必须提供某些强制性声明。除此之外，还可以为 Web 脚本可选地提供额外的声明。现在让我们看看可以在描述文档中提供的各种声明。我们将逐一查看选项，这将让你清楚地了解如何从头开始创建描述文档 `helloworld.get.desc.xml`，用于我们之前创建的 hello world Web 脚本。

## Mandatory declarations

现在让我们回顾一下在创建 Web 脚本时必须在描述文档中提供的元素。

### The `<webscript>` tag

有关 Web 脚本的所有信息都包含在 Web 脚本描述文档中，位于根元素 `<webscript>` 内：

```
```xml
<webscript>

</webscript>
```
```

如果未指定上述根元素标签并创建空白描述文档 XML 文件，则在注册 Web 脚本时，日志中会出现错误，并且你将无法注册 Web 脚本。

以下是日志中的错误片段：

```
```text
Unable to register script workspace://SpacesStore/app:company_home/app:dictionary/cm:extensionwebscripts/helloworld.get.desc.xml due to error:
Failed to parse web script description document helloworld.get.desc.xml; Error on line -1 of document: Premature end of file. Nested exception: Premature end of file.
```
```

### The `<shortname>` tag

对于任何 Web 脚本，都会给出一个人类可读的 Web 脚本名称。它可以按以下代码行所示在 `<shortname>` 标签内提供。此标签没有属性。

```
```xml
<shortname>Hello World</shortname>
```
```

如果尝试在未提供 shortname 值的情况下注册 Web 脚本，则它将无法注册。以下是在日志中出现的错误片段：

> **Unable to register script workspace://SpacesStore/app:company_home/app:dictionary/cm:extensionwebscripts/helloworld.get.desc.xml due to error: Failed to parse web script description document helloworld.get.desc.xml; Expected <shortname> value.**

### The `<url>` tag

每个 Web 脚本都绑定到一个 URI，该 URI 将用于调用 Web 脚本。可以为 Web 脚本声明一个或多个 URI，并且可以使用任何 URI 来执行 Web 脚本。此标签没有属性：

```
```xml
<url>/helloworld?name={argumentName}</url>
<url>/hello/world/{argumentName}</url>
```
```

如果在描述文档中未提供 URL 并尝试注册 Web 脚本，它将给出错误并且不会注册 Web 脚本。以下是当 Web 脚本描述文档中没有提供任何 URL 时日志中的错误片段：

> **Unable to register script workspace://SpacesStore/app:company_home/app:dictionary/cm:extensionwebscripts/helloworld.get.desc.xml due to error: Failed to parse web script description document helloworld.get.desc.xml; Expected at least one <url> element.**

如果一个 URL 已在一个 Web 脚本中定义，并且你尝试在另一个 Web 脚本中定义相同的 URL，则 Web 脚本引擎将不会注册 Web 脚本，并且日志中会出现如下示例所示的错误：

> **Unable to register script classpath:alfresco/extension/templates/webscripts/test/helloworld1.get.desc.xml due to error: Web Script document test/helloworld1.get.desc.xml is attempting to define the url '/helloworld1:GET' already defined by test/helloworld.get.desc.xml.**

一个包含 hello world Web 脚本所有强制元素的最小描述文件将如下所示：

```
```xml
<webscript>
    <shortname>Hello World</shortname>
    <url>/helloworld?name={argumentName}</url>
    <url>/hello/world/{argumentName}</url>
</webscript>
```
```

## Optional declarations

除了强制性声明之外，还可以在描述文档中声明一些额外的元素。让我们了解一下 Web 脚本描述文档可能的 optional 声明。

### <format>

此元素允许你通过在调用 Web 脚本时使用 Web 脚本 URI 来指定响应内容类型。它基本上允许你控制如何指定 Web 脚本响应格式。可以为此元素定义不同的可能有效值，并在后续章节中提及。此元素有一个可选属性名为 default。

### argument

让我们看看描述文档中格式标签的值为 argument 的情况。它将如下所示：

```
<format default="html">argument</format>
```

为 format 元素指定值 argument 表示可以通过向 Web 脚本 URL 添加参数来选择 Web 脚本的响应格式。使用查询字符串参数 format，可以按如下方式指定响应内容类型：

```
http://localhost:8080/alfresco/service/helloworld?name=Ramesh&format=xml
```

### extension

格式标签的值为 extension 时将在描述文档中如下所示：

```
<format default="html">extension</format>
```

可以通过在调用 Web 脚本时向 Web 脚本 ID 添加扩展名来指定响应内容类型，如下所示：

```
http://localhost:8080/alfresco/service/helloworld.xml?name=Ramesh
```





## any

在描述文档中，当 `any` 被指定为 `format` 标签的值时，响应格式可以使用前面提到的任意方法来指定。在描述文档中没有指定 `format` 元素的情况下，它会被视为与将 `format` 值指定为 `any` 相同。其值为 `any` 的 `format` 标签在描述文档中会显示如下：

```
<format default="html">any</format>
```

> 在调用 Web 脚本时，如果没有指定响应内容类型，那么默认内容类型将使用 `format` 标签的 `default` 属性。如果未设置 `default` 属性，则其默认值为 HTML。某些 Web 脚本也可能在运行时决定响应内容类型。对于这些脚本，需要将 `default` 属性设为空白，例如 `default=""`。

## <authentication>

为了安全地访问 Web 脚本，指定 Web 脚本所需的身份验证级别非常重要。这个可选的标签声明可以用于定义该级别。它有一个可选属性：`runas`。该元素可以指定的不同值将在后续章节中解释。

### none

如果你要实现一个不需要身份验证的 Web 脚本，可以在 Web 脚本的描述文档中按如下方式指定 `authentication` 标签：

```
<authentication>none</authentication>
```

当描述文档中也没有指定 `authentication` 标签时，这是默认值。它表示运行该 Web 脚本不需要任何身份验证。

> 在身份验证被指定为 `none` 的 Web 脚本中，将无法与仓库进行交互。

### guest

你可以按如下方式使用 `guest` 值指定 `authentication` 标签：

```
<authentication>guest</authentication>
```

上一行表示运行该 Web 脚本时，至少需要 guest 级别的身份验证。

### user

你可以按如下方式将 `authentication` 标签的值指定为 `user`：

```
<authentication>user</authentication>
```

这表示运行该 Web 脚本时，至少需要仓库的命名用户身份验证。

### admin

你可以按如下方式将 `authentication` 标签的值指定为 `admin`：

```
<authentication>admin</authentication>
```

上一行表示运行该 Web 脚本时需要仓库的管理员用户身份验证。管理员用户基本上就是属于 `ALFRESCO_ADMINISTRATORS` 组的用户。

> 也可以指定以某个特定的 Alfresco 仓库用户身份执行 Web 脚本，而无需考虑是谁发起了该 Web 脚本。这可以通过 `authentication` 元素的 `runas` 属性来指定，例如 `runas="admin"`。
> 需要注意的是，此属性仅适用于放置在 Java 类路径中的 Web 脚本。

## <transaction>

还可以使用这个可选的 `transaction` 元素来指定执行 Web 脚本的事务级别。该元素有两个可选属性：`allow` 和 `buffersize`。该元素可以使用的不同有效值如下列小节所示：

### none

其值为 `none` 的 `transaction` 在描述文档中会显示如下：

```
<transaction>none</transaction>
```

上一行表示运行该 Web 脚本不需要任何事务。

`family` 标签的一个示例如下：

```
<family>CMIS</family>
```

当你访问 http://localhost:8080/alfresco/service/index 页面时，你会找到一个基于 `family` 浏览 Web 脚本的选项。例如，你会找到一个链接 **浏览“CMIS”Web 脚本**，用来查看所有归类为 CMIS 家族的 Web 脚本。

## <cache>

缓存是一个非常好的特性，有助于避免为每个请求重新生成响应。Alfresco 中的 Web 脚本框架使用 HTTP 定义的缓存机制。对于 Web 脚本，可以使用这个可选元素来提供所需的缓存级别。当 Web 脚本被调用时，Web 脚本框架会将该元素的声明转换为合适的响应头来管理缓存。带有其子元素的 `cache` 标签在描述文档中会显示如下：

```
<cache>
    <never>false</never>
    <public>true</public>
    <mustrevalidate/>
</cache>
```

该标签的不同可选子元素将在后续章节中解释。

### never

这表示是否应对 Web 脚本响应进行缓存。默认值为 `true`，表示 Web 脚本响应将永远不会被缓存。另一个可选值为 `false`，用于指定允许缓存 Web 脚本响应。

### public

这用于指定经过身份验证的 Web 脚本响应是否应缓存在公共缓存中。`true` 值表示可以缓存 Web 脚本响应，而 `false` 表示不能缓存 Web 脚本响应。默认值为 `false`。

### mustrevalidate

这用于指示缓存是否应重新验证其 Web 脚本响应版本，以提供最新的响应数据。`true` 值表示必须始终进行验证，而 `false` 表示可能需要进行验证。默认值始终为 `true`。

## <negotiate>

对于特定的 Web 脚本响应格式，可以使用这个可选元素来提供 Accept 头的 MIME 类型。在描述文档中可以有多个该元素的条目。必须为此标签提供值。该值将包含响应格式的类型。同时，必须提供包含 MIME 类型的 `accept` 属性。

```xml
<negotiate accept="text/html">html</negotiate>
<negotiate accept="text/xml">xml</negotiate>
```

在描述文档中定义一个 `negotiate` 元素后，内容协商即被启用。

> 理解 `negotiate` 和 `format` 元素之间的关联是很有趣的。每个 Web 脚本都会以响应流的形式返回结果，并且可能会根据 MIME 类型进行编码。如果在调用 Web 脚本时没有提供任何关于 MIME 类型的提示，那么 Web 脚本框架将使用其预定义的编码。在调用 Web 脚本时，可以提供提示，让脚本框架知道响应的 MIME 类型。使用 `format` 元素，可以以 URL 扩展名或格式参数的形式指定响应格式，该格式会映射到 Web 脚本框架中的 MIME 类型。另一种选择是指定 Accept 头，这可以通过 `negotiate` 元素来完成。

## <lifecycle>

`lifecycle` 元素用于描述 Web 脚本的生命周期阶段。该元素可以在描述文档中按如下方式指定：

```xml
<lifecycle>sample</lifecycle>
```

Web 脚本在其开发生命周期中，最初可能处于草稿状态，然后在生产模式中完全使用，最后在不再使用时被弃用。可以使用该元素为 Web 脚本指定这些阶段。该元素的不同可能值如下列小节所示。

### none

这表示该 Web 脚本不属于任何生命周期。

### sample

这表示该 Web 脚本是一个示例 Web 脚本，不应用于生产环境。

### draft

这表示该 Web 脚本仍处于实验阶段，尚未完全完成。

### public_api

这表示该 Web 脚本是 Alfresco 公共 API 的一部分，因此经过了全面测试，必须保持稳定。

### draft_public_api

这表示它将成为 Alfresco 公共 API 的一部分，但尚未完全完成，或者仍在进行一些更改。

### deprecated

这表示该 Web 脚本已被弃用，不应再使用，并且该 Web 脚本可能会从未来的 Alfresco 版本中移除。

### internal

这表示该 Web 脚本仅供 Alfresco 内部使用。因此，对于不同版本，它可能会根据 Alfresco 内部使用情况而改变，并且有可能在未来的 Alfresco 版本中发生变化。

> `<lifecycle>` 元素是一个可选元素。通常，对于你为项目开发的自定义 Web 脚本，并不一定要为 Web 脚本指定 lifecycle 值。因此，你可能不会使用该元素。

## <formdata>

在 Web 脚本的描述文档中，你可以按如下方式指定 `formdata` 标签：

```
<formdata multipart-processing="false" />
```

它有一个名为 `multipart-processing` 的属性。这可用于指定 Web 脚本的多部分表单数据处理是开启还是关闭。可选值为 `true`（表示表单数据的 multipart 处理已开启）和 `false`（表示已关闭）。默认值为 `true`。





## <args>

The args element is used generally for documentation purposes to describe the arguments passed to the web script. It has the child element arg, which has child elements name, which specifies the name of the argument, and description, which specifies a description of the argument. The args tag with its child element will appear in the description document as follows:

```
<args>
  <arg>
    <name>name</name>
    <description>the name to display for helloworld webscript</description>
  </arg>
</args>
```

## <requests>

The requests element can be used to specify the collection of request types. It has request as a child element, which has type as an attribute. This tag will appear in the description document as follows:

```
<requests>
  <request type="cmis.atomentry" />
</requests>
```

## <responses>

The responses element can be used to specify the collection of response types. It has response as a child element, which has type as the attribute. This tag will appear in the description document as follows:

```
<responses>
  <response type="cmis.atomentry"/>
</responses>
```

## kind

The kind attribute is an optional attribute to the root tag <webscript>. A special kind of implementation can be done for web scripts, which is basically a Java backend implementation. For example, in a scenario where you want to develop web scripts that need to return large content as the response, a Java-backed web script implementation can be developed for this scenario. The kind attribute allows you to provide a name for this implementation. Now, for any web script you are developing that has to render large amount of content, the kind attribute can be used in the description document to explicitly mention that it should use the custom Java backend implementation created.

Here is an example of how the `kind` attribute is used in the out-of-the-box Alfresco web script, `tomcat/webapps/alfresco/WEB-INF/classes/alfresco/templates/webscripts/org/alfresco/repository/thumbnail/thumbnail.get.desc.xml`, inside your Alfresco installation directory. If you are using Alfresco Community 5, the `thumbnail.get.desc.xml` file can be located under the `alfresco` package in `alfresco-remote-api-*.jar`.

```
<webscript kind="org.alfresco.repository.content.stream">
```

## Controller implementation – not mandatory!

The actual business logic for a web script resides in the web script controller. The main functionalities such as interacting with repository, for example, to query the repository, add content, update content, delete content, and so on are part of the web script controller implementation.

There are two types of controller implementations possible to implement while creating web scripts in Alfresco:

- The JavaScript-backed controller
- The Java-backed controller

## The JavaScript-backed controller

JavaScript-backed controller is known as the web script controller script that has the web script business logic implemented in a JavaScript file. Alfresco provides a set of JavaScript APIs that can be used in the web script controller to interact with the Alfresco backend repository. A JavaScript controller can read the query string parameters of the web script URI, perform the required repository operations, and populate the model object to render the response.

We have already seen the basic implementation of JavaScript controller for our hello world web script.

## Understanding when to use the JavaScript controller

Here are a few generic points that explain when you should go for JavaScript controller implementation for your web script:

- When the repository operations you are trying to perform can be easily done using the available Alfresco JavaScript API, you might choose this controller

Building Blocks of Web Scripts

- The JavaScript controller has an advantage that if you have developed a web script that has a controller using the JavaScript implementation, you do not need to restart the server to deploy and use your web script
- No additional tooling is required; you can simply use any editor to create the web script, no compilation is required, and is easy to deploy in the repository

## The Java-backed controller

The Java-backed controller is another type of controller implementation possible to implement for a web script that might have the complete business logic processing implemented in Java.

When you are implementing a Java-backed controller, you will have access to all the content application services available in Alfresco. Business logic implementation for a web script can be provided in JavaScript- or Java-based controller; the only difference is that Java-backed controller is implemented in Java and it has access to all the available content application services in Alfresco.

## Understanding when to use a Java-backed controller

Let's take a look at a few points about when you should go for the Java-backed controller implementation for your web script:

- Generally, when the JavaScript controller cannot do the repository operations you want to do using the available Alfresco JavaScript API, you should implement the Java-backed controller.
- When you want to override the rendering of a response, for example, rendering large content in response, you should use this controller.
- Consider that you will require experience in Java programming. You will also require compilation of the Java code, classpath deployment of web scripts, and even server restart in order to use Java-backed web script. If all of this looks fine to achieve your business functionality in time, then you can go for implementing Java-backed controller.

> A web script can also have both JavaScript- and Java-backed controllers. In such a scenario, the Java-backed controller will first get executed and then JavaScript-based implementation gets executed when the web script is invoked.

## Response templates – yes, they are required!

Response templates are basically used to render the web script response. In MVC terms, response templates play a role of views. The output of the web script is rendered in the required formats using the web script response templates.

It is possible to have multiple response templates for a web script, and based on the requested response content type while invoking a web script, a response can be rendered using the appropriate response template.

Response template also has access to web script query string parameters along with the data items provided by the controller script and access to some common content repository entry points.

We have already seen rendering response for our hello world web script.

> For JavaScript-based web scripts, response templates are always required. There are certain implementations of Java-backed web scripts where response can be directly rendered from the Java controller. In such case, there is no need to specify response template for the web script. We are going to cover Java-backed web scripts in detail in Chapter 6, Creating Java-backed Web Scripts.

## I18N for a web script – it's optional

Internationalization is a cool feature that can be very useful while creating web scripts.

There may be times when you are required to develop a web script that will directly be used by the end users, and hence, you need a human-readable web script response in the user's language. No worries, it is possible to provide I18N support to the web scripts in Alfresco, and hence, you can have the text in the response rendered in the preferred supported user languages.

We will also extend our hello world web script to have internationalization support and will get understanding of how to make a web script render a response using internationalization.

There are two things to be done in order to achieve this:

- Creating a resource bundle for different languages
- Modifying the response template to use the labels from the resource bundle

## Adding a resource bundle for the hello world web script

In order to add a resource bundle for a web script, the first thing we need to do is create a resource bundle for the different languages.

After logging in to Alfresco Share, create `helloworld.get.properties` at the **Repository | Data Dictionary | Web Scripts Extensions** location, and add the following content to it:

```
greeting.title=Hello!
email.title=email address
```

Now, at the same location, create `helloworld.get_de.properties` and add the following content to it:

```
greeting.title=Hallo!
email.title=E-Mail-Adresse
```

We have just created two properties files and made the text able to be rendered in the web script response in two different languages: English and German.

## Modifying the response template to use labels

Now, in order to use the labels added in the resource bundle, we will have to modify the response template. Perform the following steps to modify the response template for the hello world web script:

- 1. Modify `helloworld.get.html.ftl` available at the **Repository | Data Dictionary | Web Scripts Extensions** location as follows:

```
<html>
  <body>
    <p>${msg("greeting.title")} ${args.name}.</p>
    <p>${msg("email.title")}: ${email}</p>
  </body>
</html>
```

We have just modified our response template to use the text from the resource bundle.

- 2. Now, go to `http://localhost:8080/alfresco/service/index` and click on the **Refresh Web Scripts** button.

- 3. To test this, temporarily change the preferred browser language to Germany de-DE and hit the `http://localhost:8080/alfresco/service/helloworld?name=Ramesh` URL. You should see the response rendered using the labels rendered from German properties file we created.





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
