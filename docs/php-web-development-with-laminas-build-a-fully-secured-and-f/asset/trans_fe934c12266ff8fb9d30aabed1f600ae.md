

## 请求控制与数据视图

本章将演示如何在 MVC 应用程序中使用 Laminas 对控制器和视图层的实现。我们将先简要回顾 PHP 与 **超文本传输协议（HTTP）** 之间的关系。我们将了解 Laminas 如何处理 HTTP 请求和响应。之后，我们将实现 `Inventory` 模块的控制器和视图，即学习模块在 Laminas MVC 中如何工作。最后，我们将学习如何创建页面控制器和页面模板，并了解页面控制器的工作原理。

本章将涵盖以下主题：

- 了解 HTTP 与 PHP 之间的关系
- Laminas MVC 中的请求生命周期
- 使用控制器和视图页面实现 CRUD

本章结束时，您将能够使用 Laminas 模块和 MVC 模式实现注册功能，并了解 `laminas-mvc` 组件如何处理 HTTP 请求。此外，您将了解 Laminas 控制器页面类的工作原理，并能够使用 Laminas 模块管理 HTML 页面。

### 技术要求

本章所有相关代码均可在 `https://github.com/PacktPublishing/PHP-Web-Development-with-Laminas/tree/main/chapter07/whatstore` 中找到。

### 了解 HTTP 与 PHP 之间的关系

HTTP 的本质是这样的：一台名为客户端的计算机向另一台名为服务器的计算机发送一条文本消息（或请求）。服务器用一条文本消息（或响应）进行回复。自 2014 年起，HTTP 已由 RFC 7230 至 7237 定义。根据 RFC 7230，“客户端”和“服务器”是角色，因此同一台计算机既可以是客户端也可以是服务器。例如，在基于微服务的分布式系统中，每个微服务既可以充当另一个微服务的客户端，也可以充当另一个微服务的服务器。

有些编程语言被改编用于构建 Web 应用程序。然而，PHP 是一种最初为使用 HTTP 而创建的语言。Rasmus Lerdorf 开发 PHP 是因为他对用于创建动态页面（或者更准确地说，与数据库集成的 HTML 页面）的现有工具不满意。PHP 与用于 Web 应用程序开发的协议并行发展。使用 PHP 可以非常快速地提供动态页面。挑战在于不要使 PHP 应用程序变得复杂，并随着应用程序的增长保持应用程序可控。

Laminas 有一个将 HTTP 元素封装为对象的组件。这就是 `laminas-http` 组件。该组件由 `laminas-mvc` 使用。`laminas-http` 组件实现了由 `laminas-stdlib` 定义的接口。`laminas-stdlib` 组件代表了 Laminas 框架中最高级别的复用，包含大多数 Laminas 组件使用的接口和抽象类。

我们将看到 `laminas-mvc` 如何使用 `laminas-http` 中的类。在下一节中，我们将了解 Laminas MVC 应用程序中的请求生命周期。

### Laminas MVC 中的请求生命周期

此刻，您可以快速启动 `whatstore` 应用程序，并使用 PHP 的内嵌 Web 服务器查看主页。从 `whatstore` 目录中，您可以键入以下命令：

```
php -S localhost:8000 -t public/
```

当您在浏览器中打开 `localhost:8000` 时，将看到与第 2 章《为我们的电子商务应用程序设置环境》中看到的相同的欢迎页面。当然，我们尚未更改任何视图。

在第 5 章《创建虚拟商店项目》中，我们了解了 `public` 目录的一些知识。我们了解到，MVC Laminas 应用程序的结构就像一座中世纪城堡，唯一进入方式是通过吊桥。这里的吊桥就是 `public` 目录。所有 HTTP 请求都将传递给 `public` 目录内的 `index.php` 文件。那么，`index.php` 到底做什么？它会执行以下操作：

- 导入 `autoload.php` Composer 文件。
- 检查 Laminas Application 类是否存在。
- 检索配置。如果存在开发配置，则该配置将覆盖生产配置。
- 使用 `Application::init` 方法实例化 `Application` 类。该实例通过依赖注入接收来自 `laminas-http` 的对象。
- 从创建的实例中执行 `Application->run` 方法。

当 index.php 执行 Application->run 时，会发生控制反转。从这一点开始，Laminas 将接管控制权并会调用您的特定代码，换句话说，您的业务代码。

> 注：控制反转
> 您可以在 Martin Fowler 的以下文章中了解有关控制反转模式的更多信息：https://martinfowler.com/bliki/InversionOfControl.html。

图 7.1 展示了 Laminas MVC 应用程序中请求生命周期的总体情况：

![](img/e6cf5550804ccae9d1859270da8688f5_177_0.png)

图 7.1 – Laminas MVC 应用程序中的 HTTP 请求生命周期

Laminas 使用事件驱动编程来处理请求生命周期的步骤：引导、路由、分发控制器和呈现视图。我们可以将此生命周期划分为两个主要时刻：

- 执行 `Application::init` 方法时，`ServiceManager` 将获取 `ModuleManager` 的实例，该实例加载在 `modules.config.php` 文件中注册的模块。`Application::init` 方法还使用 `bootstrap` 方法触发 `EVENT_BOOTSTRAP` 事件。`EventManager` 类会通知该事件的侦听器以执行引导任务。

- 执行 `Application->run` 方法时，`Application` 会触发 `EVENT_DISPATCH` 事件。`EventManager` 类会通知该事件的侦听器，其中一个侦听器会实例化一个控制器类。该控制器类随即为 HTTP 响应呈现一个视图。此视图可以是 JSON 对象、HTML 页面，或者其他应用程序可消费的任何格式。

等一下。到底会调用哪个控制器？这取决于路由。如前所述，在 *第 4 章，从对象关系映射到 MVC 控制器* 中，Laminas 在其模块中定义路由。路由是 URL 模式与控制器类之间的关联。`Application->run` 方法还会触发一个名为 `EVENT_ROUTE` 的事件，`EventManager` 会通知与 `ModuleManager` 相关的某些侦听器（`ModuleRouteListener` 和 `RouteListener`）。这些侦听器会根据与 URL 匹配的路由定义要调用的控制器。

我们将在下一节中更好地理解此生命周期，届时我们将为产品注册实现控制器和视图页面。

### 使用控制器和视图页面实现 CRUD

我们将在 `Inventory` 模块中创建一个用于管理产品的控制器。遵循 TDD 的良好实践，我们将从测试开始创建此控制器。

在本节中，我们将首先测试当前的测试用例。然后，我们将创建用于插入、检索、更新和删除产品的测试。

### 测试当前的测试用例

第一步是按照以下步骤检查当前测试用例的状态：

- 1. 选择 `whatstore` 项目，右键单击 **运行方式/PHPUnit 测试**。您将在 **PHPUnit** 选项卡中看到 PHPUnit 的输出，如 *图 7.2* 所示：

![](img/e6cf5550804ccae9d1859270da8688f5_179_0.png)

图 7.2 – PHPUnit 输出

如您所见，`Inventory` 模块中的 `IndexControllerTest` 类失败了。失败是因为该类是从 `Application` 模块的 `IndexControllerTest` 复制而来的，而路由和页面不同。让我们修复此类。

- 2. 首先，将 `Inventory\\Controller\\IndexControllerTest` 的 `testIndexActionCanBeAccessed` 方法更改为以下代码：

```
public function testIndexActionCanBeAccessed(): void
{
    $this->dispatch('/inventory', 'GET');
    $this->assertResponseStatusCode(200);
    $this->assertModuleName('inventory');
    $this->assertControllerName(IndexController::class); // as specified in router's controller name alias
    $this->assertControllerClass('IndexController');
    $this->assertMatchedRouteName('inventory');
}
```

- 3. 然后，从 `InventoryTest\\Controller\\IndexControllerTest` 中删除 `testIndexActionViewModelTemplateRenderedWithinLayout` 方法。此方法仅适用于 *whatstore* 主页。

- 4. 还要删除 `testInvalidRouteDoesNotCrash` 方法。该方法已为 `Application` 模块定义。

如果您再次执行 **运行方式/PHPUnit 测试**，`IndexControllerTest` 将再次失败。现在，原因是 `inventory` 路由尚不存在。我们需要在 `Inventory` 模块的 `module.config.php` 文件中创建此路由。

