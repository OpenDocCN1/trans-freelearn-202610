```
```php
$results = $statement->execute();

foreach($results as $row) {
    $sqlscript = <<<SQL
    INSERT INTO `resources_role`(`code`, `code_role`,
    `code_resource`) VALUES (NULL, (SELECT code FROM `roles`
    WHERE name = 'ADMIN'),{$row['code']});
    SQL;

    $adapter->query($sqlscript, Adapter::
    QUERY_MODE_EXECUTE);
}

echo "Permissions were granted.\n";
}
```
```

These functions are orchestrated by the following script:

```
```php
// main program

$config = getConfig();
$db = $config['db'];
unset($db['database']); // remove database key because it
causes error in connection when database does not exist
$adapter = new Adapter($db);

try {
    recreateDatabase($adapter);
    $db = $config['db'];
    $adapter = new Adapter($db);
    addUserAndRole($adapter);
    addResources($adapter);
    grantPermissionsToRole($adapter);
} catch (Exception $e) {
    echo 'Fail to recreate the database.' .
    $e->getMessage() . "\n";
}
```
```