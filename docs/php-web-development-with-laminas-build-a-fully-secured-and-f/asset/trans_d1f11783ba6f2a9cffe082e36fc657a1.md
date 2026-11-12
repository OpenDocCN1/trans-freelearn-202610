


- 5. 单击 **编辑...** 按钮并选择 `tests` 文件夹。单击 **确定**，返回 **编辑命名空间** 窗口后，单击 **确定**。

- 6. 我们做了什么？我们已将 `SchoolTest` 命名空间关联到 `tests` 文件夹。这意味着使用 `SchoolTest` 命名空间声明的类和接口将在 `tests` 文件夹中搜索。

- 7. 为了使这些关联正常工作，需要重新生成自动加载文件。

- 8. 按 `CTRL` + `S` 保存更改，并单击 **安装依赖项** 按钮。

- 9. 不会安装任何依赖项，但由于自动加载配置发生了更改，Composer 将重新生成自动加载文件。

现在，我们可以在 `SchoolClass` 中使用 `laminas-db`，而无需使用 PHP 命令来导入 Laminas 类，例如 `include` 或 `require`。

在下一节中，我们将实现包含数据库连接数据的配置文件。

## 创建 config.php 文件并重新运行 SchoolClassTest 测试用例

嗯，根据我们的组件图，`SchoolClass` 依赖于一个名为 `config.php` 的文件。我们将在名为 `config` 的文件夹中创建此文件。在 `school1` 项目的根目录下创建此 `config` 文件夹。然后，在 `config` 文件夹中，创建一个 `config.php` 文件。此文件的内容如下：

### config.php

```php
<?php
return [
    'db' => [
        'driver' => 'Pdo_Mysql',
        'database' => 'school',
        'username' => 'root',
        'password' => '',
        'hostname' => '127.0.0.1'
    ]
];
```

此文件中的数组包含 laminas-db 文档中定义的数据库连接参数：https://docs.laminas.dev/laminas-db/adapter/#creating-an-adapter-using-configuration。config.php 文件减少了与数据库相关的代码更改。您可以更改数据库服务器甚至数据库驱动程序，而无需更改其他文件的源代码。config.php 中定义的数组将由 Laminas 的 Adapter 类使用。使用此类，我们可以建立到数据库的连接并执行 SQL 语句。此类实现了由四人帮（Erich Gamma、Richard Helm、Ralph Johnson 和 John Vlissides）在经典著作《设计模式：可复用面向对象软件的基础》中编目的适配器设计模式。该模式有助于在具有不同接口的组件之间轻松切换，例如使用不同驱动程序的数据库。因此，我们将编写 SchoolClass::getAll 方法的内容：

### SchoolClass::getAll

```php
public static function getAll() : iterable
{
    $config = require (realpath(__DIR__ . '/../config') .
        'config.php');
    $adapter = new Adapter($config['db']);
    $statement = $adapter->query('SELECT * from classes');
    $resultSet = $statement->execute();
    return $resultSet;
}
```

SchoolClass::getAll 方法从 config.php 文件读取数组，并将 db 键注入 Adapter 类的实例中。使用此实例，我们准备一条 SQL SELECT 语句并执行它。

> **注意：自动代码补全**
>
> 在 Eclipse 代码编辑器中输入类名时，可用类列表将变得可见。因此，如果从自动补全列表中选择一个类，Eclipse 将自动添加命名空间行。但如果您不这样做，请检查命名空间是否存在于文件顶部。例如，Laminas 数据库适配器的命名空间是 Laminas\\Db\\Adapter\\Adapter。

现在，我们可以再次运行 `SchoolClassTest` 测试用例。您将在 **PHPUnit** 选项卡下（参见 _图 3.22_）看到 `SchoolClass::getAll` 方法的代码已通过测试：

![](img/e6cf5550804ccae9d1859270da8688f5_78_0.png)

图 3.22 – 显示 testListing 的 PHPUnit 选项卡

## 创建新的 testInserting 测试方法

我们已创建自动化测试，但它不是单元测试。由于我们正在连接真实数据库，`SchoolClassTest->testListing` 方法是一个集成测试：它不仅测试 `SchoolClass` 的接口，还测试与数据库的连接。

现在可以通过浏览器请求 `index.php` 页面来测试用户界面吗？嗯，虽然我们有从数据库读取数据的代码，但 `classes` 表中没有记录。因此，列表中没有学校班级。在测试列表页面之前，最好先实现插入逻辑。

我们将按照以下代码片段，向 `SchoolClassTest` 添加一个名为 `testInserting` 的新测试方法：

```php
public function testInserting()
{
    $schoolClass = new SchoolClass(0, '1st year');
    $schoolClass->save();
    $config = require (realpath(__DIR__ . './../config').
        '/config.php');
    $adapter = new Adapter($config['db']);
    $qi = function ($name) use ($adapter) {
        return $adapter->platform->quoteIdentifier($name);
    };
    $fp = function ($name) use ($adapter) {
        return $adapter->driver->formatParameterName($name);
    };
    $statement = $adapter->query('SELECT name from classes where ' . $qi('name') . ' = ' . $fp('name'));
    $resultsSet = $statement->execute(['name' => '1st year']);
    $this->assertCount(1, $resultsSet);
    $statement = $adapter->query('DELETE from classes where ' . $qi('name') . ' = ' . $fp('name'));
    $resultsSet = $statement->execute(['name' => '1st year']);
}
```

SchoolClassTest->testInserting 方法检查 SchoolClass->save 是否为 classes 表生成一条新记录。该测试方法会删除记录以重置数据状态。当然，此测试方法假设 classes 表的 name 字段禁止重复值。因此，必须只有一个名为 '1st year' 的记录。作为 TDD 第一步的一部分，如果您现在运行此测试，它将失败。原因很明显：SchoolClass->save 方法不存在。让我们来实现它！

> **注意：用于处理字段名和值的闭包**
>
> 在 SchoolClassTest->testInserting 方法中，我们使用 $qi 和 $fp 闭包分别对字段名进行引用和对值进行格式化。因此，例如，如果我们将数据库从 MySQL 更改为 PostgreSQL，则无需更改类中的代码，只需更改 config.php 文件中的连接参数即可。此外，适当格式化值可以避免 SQL 注入攻击。

根据我们的类图，SchoolClass->save 方法返回一个布尔值。如果方法成功插入，则返回 true；否则返回 false。此方法的代码如下：

```php
public function save() : bool
{
	$adapter = self::getAdapter();

	$fp = function ($name) use ($adapter) {
		return $adapter->driver->formatParameterName($name);
	};

	$statement = $adapter->query('INSERT INTO
		classes(name) VALUES ('. $fp('name') . ')');

	try {
		$statement->execute(['name' => $this->name]);
	} catch (\Exception $e) {
		error_log($e->getMessage());
		return false;
	}

	return true;
}
```

要插入新记录，我们需要连接到数据库。因此，我们将数据库适配器创建代码提取到单独的 SchoolClassTest::getAdapter 静态方法中：

```php
private static function getAdapter(): AdapterInterface
{
    $config = require (realpath(__DIR__ . '/../config') .
        '/config.php');

    $adapter = new Adapter($config['db']);

    return $adapter;
}
```

这样，我们就可以避免为每个需要连接数据库的方法重复此代码。当然，我们必须重构 SchoolClassTest::getAll 方法以使用 SchoolClassTest::getAdapter：

```php
public static function getAll(): iterable
{
    $adapter = self::getAdapter();

    $statement = $adapter->query('SELECT * from classes');
    $resultSet = $statement->execute();

    return $resultSet;
}
```

回到 SchoolClassTest->save 方法，您应该注意，它使用 $this->name 属性执行 SQL INSERT。因此，必须定义该属性并赋值。实际上，我们需要向类中添加两个属性，分别对应 classes 表中的每个相关字段：

```php
public int $code = 0;
public string $name = '';
```

