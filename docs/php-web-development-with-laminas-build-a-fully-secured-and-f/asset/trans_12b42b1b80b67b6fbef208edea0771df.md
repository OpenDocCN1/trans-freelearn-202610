

- 7. 删除 SchoolClassTable 的 getInstance 方法，并将该类的构造函数改为 public。这意味着你需要找到以下行：

```
private function __construct (TableGatewayInterface
$tableGateway)
```

将其替换为以下行：

```
public function __construct (TableGatewayInterface
$tableGateway)
```

控制器工厂类依赖于特定配置来查找数据表网关工厂类。

$container 对象的 get 方法使用的键是一个别名。此别名必须在配置文件中定义。由于我们在 School 模块中使用 SchoolClassTable，我们将在 School 模块的 module.config.php 文件中定义此配置。此文件位于模块的 config 文件夹中，这很合理。

- 8. 将 module.config.php 的命名空间改为 School。此文件实际上是带有模块配置的 PHP 数组。你将找到一个名为 controllers 的键，如下所示：

```
'controllers' => [
    'factories' => [
        Controller\IndexController::class =>
            InvokableFactory::class,
    ],
],
```

此代码片段显示 IndexController 类与 InvokableFactory 类相关联。InvokableFactory 是 laminas-mvc 提供的控制器工厂。此基本工厂在不进行依赖注入的情况下创建关联类的实例。我们不使用 IndexController。

- 9. 从 src/Controller 文件夹中删除 IndexController.php 文件。必须按如下更改上述代码片段：

```
'controllers' => [
    'aliases' => [
        'schoolclass' => Controller\
            SchoolClassController::class
    ],
    'factories' => [
        Controller\SchoolClassController::class =>
Controller\SchoolClassControllerFactory::class
],
],
```

是的，除了更改 factories 中的关联外，我们还添加了将 "schoolclass" 字符串与 SchoolClassController 关联的键别名。此关联起什么作用？它有助于将路由与实际控制器名称解耦。

- 10. 更改 router 键内部的 routes 键。删除 router 的当前值，并将其替换为以下内容：

```
'routes' => [
    'schoolclass' => [
        'type' => Segment::class,
        'options' => [
            'route' => '/schoolclass[/:action'
            [/:code]]',
            'defaults' => [
                'controller' => 'schoolclass',
                'action' => 'index',
            ],
        ],
    ],
],
],
```

路由是 URL 模式与控制器类之间的关联。laminas-router 组件中有几种可用的路由类型。在此示例中，我们使用允许参数的 Segment 路由。在 '/schoolclass[/:action[/:code]]' 模式中，括号之间的部分是可选的，冒号后面的单词是参数。defaults 键定义省略参数的假设值。请注意，我们为 controller 和 action 定义了默认值。controller 键的值是 SchoolClassController 的别名。

你可能忘了，但别名的值是传递给 SchoolClassControllerTest 的 $this->assertControllerName 方法的值。事实上，你可以使用别名或类的完整名称。

现在可以成功运行测试用例了吗？嗯，测试仍然会失败，因为我们需要配置 SchoolClassTable 的别名。

- 11. 通过向 module.config.php 的数组中添加 service_manager 键来配置别名。此键位于以下数组的第一级：

```
'service_manager' => [
    'factories' => [
        'SchoolClassTable' => Model \
            SchoolClassTableFactory::class
    ]
],
```

依赖注入容器将使用 factories 内部定义的键来调用声明为值的工厂。

现在可以运行测试了吗？不行。

- 12. 将 School/src 文件夹中 Module 类的命名空间改为 School。Module 类加载 module.config.php 文件，因此如果找不到，模块配置将被忽略。

做出此更改后，我们可以运行测试了，对吗？不，因为我们需要配置数据库连接。保持冷静并进行配置。我们将继续使用 school 数据库，即 school11 和 school12 项目使用的同一数据库。

- 13. 安装 laminas-db。幸运的是，你知道如何操作。但这次，由于你在带有 laminas-mvc 的项目中安装 laminas-db，你将在 Console 选项卡中看到以下提示：

```
Please select which config file you wish to inject
'Laminas\Db' into:
[0] Do not inject
[1] config/modules.config.php
[2] config/development.config.php.dist
Make your selection (default is 1)
```

这允许你（选择性地）将 laminas-db 注入到应用程序中作为模块。如前所述，Laminas 中的模块是最大的复用单元。你可以仅为开发环境注入组件（选项 2）。但我们将 laminas-db 注入为生产环境的组件，因此请选择选项 1。

- 14. Composer 接下来会询问以下内容：

```
Remember this option for other packages of the same type?
(Y/n)
```

回答 Y，以便避免在接下来安装的 Laminas 组件中再次出现上述提示。请记住，laminas-db 需要连接参数。根据 Laminas 的最佳实践，我们必须将全局配置（适用于任何环境）与本地配置（适用于特定环境）分开。

- 15. 将全局配置放在项目根目录下 config/autoload 文件夹中的 global.php 文件中。school 项目的内容必须如下：

# global.php

```
<?php

use Laminas\Db\Adapter\AdapterServiceFactory;
return [
    'db' => [
        'driver' => 'Pdo_Mysql',
        'database' => 'school'
    ],
    'service_manager' => [
        'factories' => [
            'DbAdapter' => AdapterServiceFactory::class
        ]
    ]
];
```

与之前项目的数据库配置不同之处在于使用了 AdapterServiceFactory，其昵称为 DbAdapter。此类使用 db 键的数据来创建我们老朋友 Adapter 类的一个实例。对吧，但其他参数呢？嗯，其他参数可能因环境而异。因此，我们必须将它们放入 local.php 文件。虽然 global.php 对所有环境都相同，但每个环境都有不同的 local.php。因此，local.php 文件不得成为版本控制的一部分。骨架附带一个名为 local.php.dist 的模板。

- 16. 从 `config/autoload` 文件夹复制 `local.php.dist` 到同一文件夹中的 `local.php`，并用以下代码填充：

# local.php

```
<?php
return [
    'db' => [
        'hostname' => 'localhost',
        'username' => 'root',
        'password' => ''
    ]
];
```

我们已拥有所有必要数据。你一定很想知道测试现在是否能成功运行。

- 17. 运行测试：

```
1) SchoolTest\\Controller\\SchoolClassControllerTest::testListing
Failed asserting response code "200", actual status code is "500"

Exceptions raised:
Exception 'Laminas\\View\\Exception\\RuntimeException' with message 'Laminas\\View\\Renderer\\PhpRenderer::render: Unable to render template "school/school-class/index"; resolver could not resolve to a file' in /opt/lampp/htdocs/school3/vendor/laminas/laminas-view/src/Renderer/PhpRenderer.php:492
```

运行时，你会收到上述错误消息。此消息出现的原因是控制器操作没有对应的视图。

请注意，错误消息明确指出应用程序期望 `school/school-class/index` 模板。Laminas 控制器层和视图层之间存在一种约定。从路由接收 HTTP 请求的控制器方法带有 `Action` 后缀。当 `Action` 方法结束并返回 `ViewModel` 对象时，框架会查找与方法名相同但不带 `Action` 后缀的文件。因此，`indexAction` 方法对应名为 `index` 的文件。视图文件的默认扩展名是 `.phtml`。

