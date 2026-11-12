```
autofocus="autofocus"
value="<?=$schoolClass->name?>"><br/>
<input type="hidden" name="code" value="<?=
    $schoolClass->code?>"><br/>
<input type="submit" value="save">
</form>
<p>
<a href="index.php">Homepage</a>
</p>
</body>
</html>
```

What is the difference between the previous version and the current version of this file? The answer is the PHP block code, with the import of the autoload.php files, the reading of the parameter code (sent via HTTP GET), and the recovery of an instance of SchoolClass from the code. Wait a moment! Where did this SchoolClass->getByCode method come from? You are very smart! It doesn't exist yet! We have to implement it! Let's go:

```
public static function getByCode($code = null): SchoolClass
{
    $adapter = self::getAdapter();
    $qi = function ($name) use ($adapter) {
        return $adapter->platform->quoteIdentifier($name);
    };
    $fp = function ($name) use ($adapter) {
        return $adapter->driver->formatParameterName($name);
    };
    $statement = $adapter->query('SELECT * from classes
        WHERE ' . $qi('code') . '=' . $fp('code'));
    $resultSet = $statement->execute(['code' => $code]);
    if ($resultSet->count() == 0){
        return new SchoolClass();
    }
    $row = $resultSet->current();
    return new SchoolClass((int)$row['code'],$row['name']);
}
```