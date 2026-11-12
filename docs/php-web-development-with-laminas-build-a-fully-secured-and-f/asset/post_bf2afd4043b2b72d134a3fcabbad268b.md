

- 5. Click on the **Edit...** button and select the `tests` folder. Click **OK** and, back in the **Edit Namespace** window, click **OK**.

- 6. What did we do? We have associated the `SchoolTest` namespace to the `tests` folder. This means classes and interfaces declared with the `SchoolTest` namespace will be searched in the `tests` folder.

- 7. For these associations to work, it is necessary to regenerate the autoload files.

- 8. Press `CTRL` + `S` to save the changes and click on the **Install dependencies** button.

- 9. No dependency will be installed, but as there was a change in the autoloading configuration, Composer will regenerate the autoload files.

Now, we can use `laminas-db` inside `SchoolClass` without using PHP commands for importing the Laminas classes, such as `include` or `require`.

In the next section, we will implement the configuration file with the database connection data.

## Creating the config.php file and rerunning the SchoolClassTest test case

Well, according to our component diagram, `SchoolClass` depends on a file named `config.php`. We will create this file inside a folder named `config`. Create this `config` folder in the root of the `school1` project. Then, inside the `config` folder, create a `config.php` file. The following will be the content of this file:

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

The array inside this file contains the database connection parameters defined in the documentation for laminas-db: https://docs.laminas.dev/laminas-db/adapter/#creating-an-adapter-using-configuration. The config.php file reduces the code changes related to the database. You can change the database server and even the database driver without needing to change the source code of other files. The array defined in config.php will be used by the Adapter class of Laminas. With this class, we can establish a connection to the database and execute SQL statements. This class implements the Adapter design pattern, cataloged by the Gang of Four (Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides) in the classic book Design Patterns: Elements of Reusable Object-Oriented Software. This pattern is useful to allow easy changes between components with different interfaces, such as databases with different drivers. So, we will write the content of the SchoolClass::getAll method:

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

The SchoolClass::getAll method reads the array from the config.php file and injects the db key into an instance of the Adapter class. With this instance, we prepare a SQL SELECT statement and execute it.

> **Note: Automatic code completion**
>
> As you type a class name in the Eclipse code editor, a list of available classes becomes visible. So, if you choose a class from the autocomplete list, Eclipse automatically adds the line of the namespace. But if you don't do that, check whether the namespace is present at the top of the file. For example, the namespace for the Laminas Database Adapter is Laminas\\Db\\Adapter\\Adapter.

Now, we can run the `SchoolClassTest` test case again. You will see under the **PHPUnit** tab (see _Figure 3.22_) that the code of the `SchoolClass::getAll` method has passed the test:

![](img/e6cf5550804ccae9d1859270da8688f5_78_0.png)

Figure 3.22 – The PHPUnit tab showing testListing

## Creating a new testInserting test method

We have created an automated test, but it isn’t a unit test. As we are connecting to a real database, the `SchoolClassTest->testListing` method is an integrated test: it tests not only the interface of `SchoolClass` but also the connectivity with the database.

Can we test the user interface now, requesting the `index.php` page through the browser? Well, although we have the code to read from the database, there are no records in the `classes` table. So, we don’t have school classes in the list. It’s better to implement the inserting logic before class creation to test the listing page.

We will add a new test method into `SchoolClassTest`, named `testInserting`, according to the following code snippet:

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

The SchoolClassTest->testInserting method checks whether SchoolClass->save produces a new record for the classes table. The test method deletes the record for restarting the data state. This test method presumes, of course, that the name field of the table classes prevents duplicate values. So, there must be only one record called '1st year'. As part of the first step of TDD, if you run this test now, it will fail. The reason is obvious: the SchoolClass->save method doesn't exist. Let us implement it!

> **Note: Closures for treating field names and values**
>
> In the SchoolClassTest->testInserting method, we use the $qi and $fp closures for quoting the field name and formatting the value respectively. So, if we change our database from MySQL to PostgreSQL, for instance, it won't be necessary to change the code into a class but only to change the connection parameter in the config.php file. In addition, formatting the values appropriately avoids SQL injection attacks.

According to our class diagram, the SchoolClass->save method returns a Boolean value. If the method inserts successfully, it returns true – otherwise, it returns false. The code of this method is as follows:

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

To insert a new record, we need to connect to a database. So, we have extracted the database adapter creation code to a separate SchoolClassTest::getAdapter static method:

```php
private static function getAdapter(): AdapterInterface
{
    $config = require (realpath(__DIR__ . '/../config') .
        '/config.php');

    $adapter = new Adapter($config['db']);

    return $adapter;
}
```

This way, we can avoid repeating this code for every method that needs to connect to the database. Of course, we had to refactor the SchoolClassTest::getAll method to use SchoolClassTest::getAdapter:

```php
public static function getAll(): iterable
{
    $adapter = self::getAdapter();

    $statement = $adapter->query('SELECT * from classes');
    $resultSet = $statement->execute();

    return $resultSet;
}
```

Coming back to the SchoolClassTest->save method, you should notice that it executes SQL INSERT using an $this->name attribute. So, this attribute must be defined and have a value. In fact, we need to add two attributes to the class, one for each related field from the classes table:

```php
public int $code = 0;
public string $name = '';
```

