
首先，让我们向类中添加一个 getNamespace 方法：

```
protected function getNamespace() : string
{
    $fqcn = explode('\\',static::class);
    return $fqcn[0];
}
```

此方法将返回被调用类的命名空间。这意味着它并不完全是一个 Generic 命名空间。如果该方法由子类调用，则考虑的命名空间将是子类的命名空间。例如，如果由 Inventory\\Model\\IdentityManager 调用 getNamespace 方法，它将返回 Inventory。

第二步是修改 Generic\\Model\\IdentityManager->__construct 方法：

```
public function __construct(AdapterInterface $adapter,
array $encryptionMethod)
{
    $this->adapter = $adapter;
    $this->encryptionMethod = $encryptionMethod;
    $this->auth = new AuthenticationService();
    $storage = new Session($this->getNamespace());
    $this->auth->setStorage($storage);
}
```

请注意，我们向构造方法中添加了两行新代码。我们实例化了 Laminas\\Authentication\\Storage\\Session 类，并将 getNamespace 方法返回的命名空间值定义为命名空间。因此，我们将在会话中拥有一个命名空间，从而避免不同 IdentityManager 实例之间可能出现的会话键覆盖问题。

第三步是修改 Generic\\Model\\IdentityManager->hasIdentity 方法：

```
public function hasIdentity() : bool
{
    if ($this->auth->hasIdentity()) {
        $storage = $this->auth->getStorage();
        error_log('namespace: ' . $storage->getNamespace());
        return ($storage->getNamespace() == $this->getNamespace());
    }
    return false;
}
```

现在，我们不仅检查是否存在经过身份验证的会话，还检查存储会话命名空间是否与 IdentityManager 实例的命名空间相同。在下一节中，我们将使用这个新的 IdentityManager 类来重构 Store 模块的 StoreAuthenticationListener 和 IndexController 类。

## 重构侦听器和控制器类

在本节中，我们将重构 Store\\Listener\\StoreAuthenticationListener 和 Store\\Controller\\IndexController 类中的一些行。在 StoreAuthenticationListener 类中，我们首先定位这些行：

```
$authenticationService = new AuthenticationService();
if (!$authenticationService->hasIdentity()) {
```

将它们替换为以下行：

```
$identityManager = $event->getApplication()->getServiceManager()->get(IdentityManager::class);
if (!$identityManager->hasIdentity()) {
```

请记住，在这种情况下，IdentityManager 特指 Store\\Model\\IdentityManager。

在 Store\\Controller\\IndexController 中，我们将添加一个新属性并修改构造方法以使用该属性：

```
private IdentityManager $identityManager;
public function __construct(ProductTable $productTable,
IdentityManager $identityManager) {
$this->productTable = $productTable;
$this->identityManager = $identityManager;
}
```

在 Store\\Controller\\IndexController->indexAction 方法中，我们将向注入到 ViewModel 实例的数组中添加一个键：

```
return new ViewModel([
"form" => $form,
"products" => $products,
"authenticated" => $this->identityManager->hasIdentity()
]);
```

当然，如果我们修改了控制器构造方法，就需要调整其在 Store\\Controller\\IndexControllerFactory 中的工厂方法：

```
public function __invoke(ContainerInterface $container,
$requestedName, array $Options = null) {
$identityManager = $container->get(IdentityManager::class);
return new CustomerController($identityManager);
}
```

现在，我们将定位 view\\store\\index\\index.phtml 文件中的这些行：

```
$authenticationService = new AuthenticationService();
$link = ["action" => "index", "label" => "Login"];
if ($authenticationService->hasIdentity()) {
```

将它们替换为以下行：

```
$link = ["action" => "index", "label" => "Login"];
if ($this->authenticated) {
```

这样，我们避免了视图脚本与身份验证类的耦合，将视图脚本限制为仅使用变量，这更为合适。视图脚本的良好做法是仅使用控制器类发送的变量。

因此，我们已完成隔离 whatstore 应用程序中两个身份验证控件所需的更改。我们不再需要担心“坏家伙”了。

在本节中，我们已隔离身份管理，以避免一个模块的用户对另一个模块拥有特权和非允许的访问。为实现此目的，我们修改了用于身份管理的通用类 Generic\\Model\\IdentityManager。最后，我们重构了 StoreAuthenticationListener 和 CustomerController 类，以使用继承的 Store\\Model\\IdentityManager 类。

## 总结

在本章中，我们首先讨论了一些敏捷开发概念，这些概念对软件维护非常重要。在这些概念之后，我们学习了如何使用免费开源的 phpcpd 工具轻松检测 PHP 程序中的重复代码。在检测模型类中需过滤和验证的重复代码块后，我们展示了如何在 Laminas\\InputFilter\\InputFilter 类的扩展中统一这些代码块。

接下来，我们学习了如何创建产品搜索框。为此，我们必须修改 ProductTable 类以执行带有 LIKE 运算符的 SQL SELECT 查询。

然后，我们学习了如何使用免费开源的 swagger-docs 工具生成 API 文档。在演示如何使用 OpenAPI Specification 生成带有 REST 端点的丰富 HTML 页面时，我们还学习了如何实现 REST 请求以列出集合。

最后，我们学习了如何隔离身份管理，以避免在存在多个身份验证控件时出现安全问题。

在下一章中，我们将介绍一些技巧和窍门，帮助开发人员解决在构建面向对象的 PHP Web 应用程序时可能最终出现的一些问题。

## 技巧和窍门

本章提供了一些技巧和窍门，将帮助你作为开发人员解决在构建 Web 应用程序时可能最终出现的一些问题。

我们将首先学习创建映射模型（即连接到数据库表的类）所需的方法是什么。

之后，我们将学习如何自定义过滤器和验证器，以在模型层中实现特定业务规则。

然后，我们将更好地了解 Laminas 视图层，学习如何生成 JSON 响应、管理文件上传以及更改应用程序布局。

最后，我们将掌握 Laminas 控制器层，学习如何检测 AJAX 请求、如何在控制器层和视图层之间交换消息，以及结束控制器操作的不同方式。

到本章结束时，你将能够使用 Laminas 框架满足 Web 应用程序的最终需求。

本章将涵盖以下主题：

- 创建映射模型
- 自定义过滤器和验证器
- 了解 Laminas 视图层
- 掌握控制器层

## 技术要求

本章相关的所有代码均可在两个仓库中找到：

- https://github.com/PacktPublishing/PHP-Web-Development-with-Laminas/tree/main/chapter13/tipsandtricks
- https://github.com/PacktPublishing/PHP-Web-Development-with-Laminas/tree/main/chapter13/whatstore

第一个 URL 包含一个名为 `tipsandtricks` 的 Laminas 应用程序。本章中的所有代码示例均可在 `tipsandtricks` 中找到。你可以使用 PHP 作为 Web 服务器运行此应用程序，就像我们在 _第 2 章，为我们的电子商务应用程序设置环境_ 中使用 `myfirstlaminas` 应用程序一样。

第二个 URL 是用于我们的电子商务应用程序 `whatstore`，其中与使用过滤器相关有一些小修改。这将在 _创建自定义过滤器_ 部分中解释。

## 创建映射模型

这是一个有用的技巧，可在使用 `laminas-db` 创建映射到表的模型时避免麻烦。这些模型需要实现两个方法，即 `exchangeArray` 和 `getArrayCopy`：

```
public function exchangeArray($data)
```

`exchangeArray` 方法由 `Laminas\Db\ResultSet` 类用于使用表记录中的数据填充模型。

```
public function getArrayCopy()
```

`getArrayCopy` 方法由 `Laminas\Db\ResultSet` 类用于从模型获取数据。它还被 `Laminas\Form\Form` 用于将模型属性值绑定到表单字段值。

实际上，`Laminas\Db\ResultSet` 类也接受 `toArray` 方法，用于替换 `getArrayCopy` 方法。我建议在模型中实现 `toArray` 方法，以提取要使用 `TableGateway` 类持久化的数据。有时，要发送到表单对象的模型数据与要发送到数据映射器的模型数据不同。你可以在 HTML 表单中显示不会保存到数据库表的数据，因为 HTML 表单可以显示来自两个关联表的数据，而其中一个表是只读的，不可写入。

这是一个简短的技巧。这是个不错的开始。在下一节中，我们仍将讨论模型层，学习如何自定义过滤器和验证器。它不会那么简短，但会更加精彩。
