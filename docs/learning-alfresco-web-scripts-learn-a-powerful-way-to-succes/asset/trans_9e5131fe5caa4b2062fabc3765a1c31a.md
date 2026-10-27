
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
