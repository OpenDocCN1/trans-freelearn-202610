

- 5. 将以下代码添加到 module.config.php 文件中，使文件中的 routes 键如下所示：

```php
'routes' => [
    'inventory' => [
        'type' => Segment::class,
        'options' => [
            'route' => '/inventory[:/controller[/:action[/:key]]]',
            'defaults' => [
                'controller' => Controller\IndexController::class,
                'action' => 'index'
            ],
        ],
    ],
],
```

我们已创建一种 Segment 类型的路由，即接受参数的路由。我们在第 4 章“从对象关系映射到 MVC 容器”中已经学习过此路由。方括号之间的单词为可选项。冒号前的单词为变量。inventory 路由允许我们定义要实例化的控制器、要执行的动作以及用于选择要处理对象的 key 参数。如果未提供这些参数，则默认控制器为 IndexController，默认动作为 index。

- 6. 将 StoreTest\Controller\IndexControllerTest 类的 testIndexActionCanBeAccessed 方法更改为以下代码：

```php
public function testIndexActionCanBeAccessed() : void
{
    $this->dispatch('/store', 'GET');
    $this->assertResponseStatusCode(200);
    $this->assertModuleName('store');
    $this->assertControllerName(IndexController::class); // as specified in router's controller name alias
    $this->assertControllerClass('IndexController');
    $this->assertMatchedRouteName('store');
}
```

- 7. 然后，根据我们已完成的工作，从 StoreTest\Controller\IndexControllerTest 中移除 testIndexActionViewModelTemplateRenderedWithinLayout 方法。别忘了也移除 testInvalidRouteDoesNotCrash 方法。

正如我们应该学到的，我们需要在 Store 模块的 module.config.php 文件中创建此路由。该文件中的 routes 键必须如下所示：

```php
'routes' => [
    'store' => [
        'type' => Segment::class,
        'options' => [
            'route' => '/store[:/controller[/:action[/:key]]]',
            'defaults' => [
                'controller' => Controller\IndexController::class,
                'action' => 'index',
            ],
        ],
    ],
],
```

太好了——暂时，我们的测试运行成功。从现在开始，我们将使用测试来实现产品的控制器层和视图层。我们的产品控制器层将有两个控制器类，一个用于接收来自 HTML 默认页面（whatstore 应用程序的一部分）的请求，另一个用于接收来自任何客户端的请求。此架构允许其他客户端（如移动应用程序）使用 whatstore 的服务。图 7.3 描绘了该面向服务的架构：

![](img/e6cf5550804ccae9d1859270da8688f5_182_0.png)

## 图 7.3 – 产品控制器架构

我们将从 ProductAPIController 开始，任何客户端都可以使用它。正如我们已定义的，我们将实现一个测试类来构建控制器类。在这样做之前，请执行以下操作：

- 1. 在 Inventory 模块的 test/Controller 文件夹中，将 IndexControllerTest.php 文件复制到 ProductAPIControllerTest.php。
- 2. 将上述文件中的类名更改为 ProductAPIControllerTest 并移除 testIndexActionCanBeAccessed 方法。

现在，让我们为 ProductAPIController 类实现测试方法。

## 测试产品插入

要在 ProductAPIControllerTest 中创建的第一个测试方法将是 testProductInsert。代码如下：

```php
public function testProductInsert() : void
{
    $discount = new Discount();
    $data = [
        'name' => 'no discount',
        'operator' => '-',
        'factor' => 0
    ];
    $discount->exchangeArray($data);
    $discountTable = $this->getApplication()->getServiceManager()->get('DiscountTable');
    $discountTable->save($discount);
    $discount = $discountTable->getByField('name', 'no discount');

    $_POST = [
        'name' => 'Cosmic Cube',
        'price' => 5642,
        'code_discount' => $discount->code
    ];

    $this->dispatch('/inventoryapi/productapi', 'POST', $_POST);
    $this->assertResponseStatusCode(200);
    $this->assertModuleName('inventory');
    $this->assertControllerName('productapi'); // as specified in router's controller name alias
    $this->assertControllerClass('ProductAPIController');
    $this->assertMatchedRouteName('inventoryapi');
}
```

testProductInsert 方法执行以下操作：

- 创建并持久化一个 Discount 对象
- 向 /inventoryapi/productapi 端点发送 HTTP POST 请求
- 检查响应是否具有 200 状态（成功）
- 检查被调用的模块是否为 Inventory
- 检查被调用控制器名称的别名是否为 productapi
- 检查被调用的控制器名称是否为 ProductAPIController
- 检查路由名称是否为 inventoryapi

我们需要定义重定向到 ProductAPIController 的 inventoryapi 路由。我们还需要为 ProductController 定义另一个路由。因此，在 Inventory 模块的 module.config.php 文件中，我们将以下路由添加到 routes 键：

```php
'routes' => [
    'inventory' => [
        'type'     => Segment::class,
        'options'  => [
            'route'    => '/inventory[:/controller[/:action[/:key]]]',
            'defaults' => [
                'controller' => Controller\IndexController::class,
                'action'     => 'index'
            ],
        ],
    ],
    'inventoryapi' => [
        'type'     => Segment::class,
        'options'  => [
            'route'    => '/inventoryapi[:/controller[/:key]]'
        ],
    ],
],
```

> **重要提示：RESTful 控制器不得具有默认动作**
>
> 当我们使用 RESTful 控制器时，不得定义默认动作。原因是默认动作会覆盖 HTTP 方法的推断。

在实现产品插入的测试后，我们将接下来实现产品恢复的测试。

## 测试产品恢复

我们将在 ProductAPIControllerTest 类中实现 testProductRecover 方法，代码应如下所示：

```php
public function testProductRecover() : void
{
    $name = 'Cosmic Cube';
    $this->dispatch('/inventoryapi/productapi/' . $name, 'GET');
    $this->assertResponseStatusCode(200);
    $this->assertModuleName('inventory');
    $this->assertControllerName('productapi'); // as
    $this->assertControllerClass('ProductAPIController');
    $this->assertMatchedRouteName('inventoryapi');
    $body = $this->getResponse()->getBody();
    $this->assertStringContainsString($name, $body);
}
```

testProductRecover 方法执行以下操作：

- 向 /inventoryapi/productapi 端点发送 HTTP GET 请求（注意搜索参数为产品名称）
- 检查响应是否具有 200 状态（成功）
- 检查被调用的模块是否为 Inventory
- 检查被调用控制器名称的别名是否为 productapi
- 检查被调用的控制器名称是否为 ProductAPIController
- 检查路由名称是否为 inventoryapi
- 检查响应体是否包含产品名称

在实现产品恢复的测试后，我们将实现产品更新的测试。

## 测试产品更新

现在，我们将在 ProductAPIControllerTest 类中实现 testProductUpdate 方法，代码如下所示：

```php
public function testProductUpdate() : void
{
    $productTable = $this->getApplication()->getServiceManager()->get('ProductTable');
    $product = $productTable->getByField('name', 'Cosmic Cube');
    $product->name = 'Infinity Gauntlet';
    $product->price = 9654;
    $_POST = $product->toArray();
    $this->dispatch('/inventoryapi/productapi/' . $product->code, 'PUT', $_POST);
    $this->assertResponseStatusCode(200);
    $this->assertModuleName('inventory');
    $this->assertControllerName('productapi'); // as specified in router's controller name alias
    $this->assertControllerClass('ProductAPIController');
    $this->assertMatchedRouteName('inventoryapi');
}
```

testProductUpdate 方法执行以下操作：

- 获取 ProductTable 的实例
- 恢复之前插入的产品
- 将产品数据提取到 $_POST 变量中
- 向 /inventoryapi/productapi 端点发送 HTTP PUT 请求（注意搜索参数为产品代码）
- 检查响应是否具有 200 状态（成功）
- 检查被调用的模块是否为 Inventory
- 检查被调用控制器名称的别名是否为 productapi

