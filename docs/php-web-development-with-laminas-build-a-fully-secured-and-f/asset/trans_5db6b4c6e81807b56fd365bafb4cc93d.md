

## 控制器与视图层之间交换消息

如果单击 Flash 信使按钮，您将被带到以下页面：

![](img/e6cf5550804ccae9d1859270da8688f5_370_0.png)

图 13.14 – FlashMessenger 插件存储的消息

Flash 信使按钮以如下方式调用 IndexController->flashMessengerAction 方法：

```php
public function flashMessengerAction()
{
    $this->flashMessenger()->addMessage('You called
        this action at ' . date('H:i:s'));
    $messages = $this->flashMessenger()->getMessages();
    return new ViewModel(['messages' => $messages]);
}
```

控制器中使用的 FlashMessenger 类将消息存储在会话容器中。消息存储在消息堆栈中，可以通过 getMessages 方法访问。简而言之，FlashMessenger 类是一个消息管理器。将 FlashMessenger 实例中存储的消息发送到视图脚本的常规方法是在 ViewModel 构造函数中注入 getMessages 方法的输出，正如我们在前面的代码片段中所看到的。但是，正如我们在下面的代码片段中所看到的，这并非必要：

### flash-messenger.phtml

```php
<h1>Messages</h1>
<?=$this->flashMessenger()->render() ?>
```

请注意，与 `flashMessengerAction` 方法相关的视图脚本不使用在 `ViewModel` 中注入的 `$messages` 变量。相反，视图脚本使用 `FlashMessenger` 的视图助手，该助手带有一个 `render` 方法。`render` 方法避免遍历消息数组，将内容作为纯文本输出。

`FlashMessenger` 是一个控制器插件，它不是 `laminas-mvc` 的一部分。您需要通过 Composer 分别安装它。

使用非内置控制器插件有两种方法。最简单的方法是将插件作为模块添加到 `modules.config.php` 文件中。您必须添加 **完全限定类名 (FQCN)**：

```php
'Laminas\\Mvc\\Plugin\\FlashMessenger'
```

另一种更详细的方法是在将要使用的模块的 `module.config.php` 文件中配置插件。您需要创建一个 `controller_plugins` 键，包含 `factories` 和 `aliases` 子键。以下代码片段展示了如何在 `tripsandtricks` 应用程序中配置 `FlashMessenger` 插件：

```php
'controller_plugins' => [
    'factories' => [
        FlashMessenger::class => InvokableFactory::class
    ],
    'aliases' => [
        'flashMessenger' => FlashMessenger::class
    ]
],
```

虽然 `factories` 键定义了如何创建 `FlashMessenger` 实例，但 `aliases` 键定义了您将在控制器操作中用来调用该插件的名称。

![](img/e6cf5550804ccae9d1859270da8688f5_371_0.png)

这是控制器插件的配置。要使用 FlashMessenger 视图助手，如果您未将组件配置为模块，则需要向 modules.config.php 文件添加一个 view_helpers 键，如下所示：

```php
'view_helpers' => [
    'invokables' => [
        'flashMessenger' => \Laminas\Mvc\Plugin\FlashMessenger\View\Helper\FlashMessenger::class
    ]
]
```

控制器插件配置对您来说应该是新的，但视图助手配置则不然。事实上，您在第 8 章「创建表单并实现过滤器和验证器」中已编写过此配置，以启用表单视图助手。

接下来，我们将了解结束控制器操作的不同方式。

## 结束控制器操作

在大多数情况下，返回以下对象之一时，控制器操作可以终止：

- 一个 ViewModel 实例，如果它是对期望 HTML 页面的同步请求的响应。
- 一个 JSONModel 实例，如果它是对期望 JSON 数据的异步请求的响应。
- 对 `$this->redirect()->toRoute()` 方法的调用。该方法需要两个参数；第一个是路由名称，第二个是包含路由参数的数组。该方法会导致 HTTP 重定向请求（HTTP 状态码 302）。

还有其他 Laminas\View\Model 类：ConsoleModel，用于为终端生成输出；以及 FeedModel，用于为 RSS 消费者生成输出。您也可以扩展 Laminas\View\Model ModelInterface 并创建特定输出。

## 总结

在本章中，我们学习了一些与 MVC 应用程序三层相关的 Laminas 实用技巧和窍门。

我们首先学习了如何使用 Laminas 接口为模型层自定义过滤器和验证器。我们了解了使用映射模型或与表单集成的模型所需的方法。

随后，我们学习了，或者更确切地说，回忆了如何为视图层生成 JSON 响应。我们还学习了如何使用 laminas-form 上传文件以及如何更改布局配置。

在本章末尾，我们学习了如何在控制器中检测 AJAX 请求以及如何使用 laminas-mvc-plugin-flashmessenger 管理消息。最后，我们了解了结束控制器操作的可能方式。

在下一章中，我们将介绍最后几件事。是的，很遗憾，但本书即将结束...

## 最后考虑

本章标志着本书的结束，展示了未来可使用的未在示例应用程序中使用的其他组件的可能性。

在本章中，我们将概览本书早期章节示例应用程序中未使用的一些 Laminas 组件。我们将了解用于创建分布式和去中心化应用程序的组件，以及用于创建 API 的组件。最后，我们将了解在哪里可以获得 Laminas 组件的帮助，以及如何成为 Laminas 社区的一部分并改进 Laminas 框架。

本章将介绍以下主题：

- 其他 Laminas 组件
- 面向微服务的开发
- 构建 API
- 社区资源

### 其他 Laminas 组件

在本节中，我们将概览本书项目中未使用的一些 Laminas 组件。您可以在 `https://docs.laminas.dev/components` 阅读所有 Laminas 组件的文档。

### 缓存

让我们用旅行作为类比。我们在旅行中花费时间和金钱。既然时间就是金钱，我们在旅行中花费大量金钱。此外，我们走得越远，花费的金钱就越多。换句话说，最便宜的去处就是您当前所在的地方。原地停留是最便宜的旅行类型。这就是缓存的理念：将数据保存在尽可能最近的位置，以减少传输的时间和成本。`laminas-cache` 组件允许我们使用多个存储适配器缓存数据，这些适配器使您的应用程序能够从文件系统、Memcached、Redis 或 MongoDB 数据库等保存和读取数据。

### CLI

尽管我们使用 Laminas 开发 Web 应用程序，但该框架并不局限于此目的。事实上，PHP 手册指出 PHP 可以做任何事情。使用 Laminas，我们可以做任何事情，但控制力更强。

> **PHP 能做什么？**
> 您可以在 https://www.php.net/manual/en/intro-whatcando.php 找到 PHP 可能用途的描述。

信不信由你，您可以使用 Laminas 创建命令行 PHP 程序。为此，您应该使用 laminas-cli 组件。

### 加密

在我们的 whatstore 项目中，我们说过加密密码很重要。但您不应使用本书示例中使用的算法。建议使用强大的加密和密码哈希来保护和验证敏感数据。laminas-crypt 组件提供了一种在 PHP 应用程序中实现加密的简便且安全的方法。该组件由多位杰出的应用加密专家开发，例如 Enrico Zimuel，他是 PHP 框架互操作性组的核心成员。

### CSRF

实际上，没有名为 laminas-crsf 的组件。但是，有两个类处理 CSRF。CSRF 是跨站请求伪造的缩写，是一种攻击，有人试图通过伪造的表单发送恶意数据。laminas-form 组件提供 Element\Csrf 以向表单添加带超时配置的哈希元素。要验证哈希元素的值，可以使用 Laminas\Validator\Csrf 类。

### DOM

DOM 是文档对象模型的缩写。这是一个 Web 标准，文档位于 https://www.w3.org/DOM/DOMTR。根据该标准，HTML 标签可以作为对象层次结构来处理。此外，可以使用 XPath 语言和 CSS 选择器来搜索和选择对象。laminas-dom 组件使您可以使用这两种方法在 XML 和 HTML 文档中查找元素。自动化读取这些格式的文档非常有用，以便导入数据或通过实现网络爬虫在网站上搜索数据。

