```
$config = require __DIR__ . '../config/autoload/
global.php';

if (file_exists(__DIR__ . '../config/autoload/
local.php')) {
    $override = require __DIR__ . '../config/autoload/
    local.php';
    $config = ArrayUtils::merge($config, $override);
}

return $config;
}
```

This getConfig function has the same code lines used in the index.php file in the public folder. This function partially emulates the application initialization. In sequence, we will declare another function (recreateDatabase) to drop the database and recreate it with some essential data:

```
function recreateDatabase(AdapterInterface $adapter) : void
{
    echo "Dropping database...\n";
    $adapter->query('DROP DATABASE IF EXISTS whatstore',
    Adapter::QUERY_MODE_EXECUTE);
    $sqlscript = file_get_contents(__DIR__ .
    '/whatstore.sql');
    echo "Creating database and tables...\n";
    $adapter->query($sqlscript, Adapter::
    QUERY_MODE_EXECUTE);
    echo "Whatstore database has been created.\n";
}
```

The recreateDatabase function uses an AdapterInterface implementation to drop the database and recreate it in sequence using a SQL script (which can be found in the GitHub repository). In sequence, we declare a function (addUserAndRole) to create and record in the employee table and another one in the roles table:

```
function addUserAndRole($adapter) : void
{
    $password = Employee::encrypt('moonknight');
    $sqlscript = <<<SQL
    INSERT INTO `employees`(`ID`, `name`, `nickname`,
```