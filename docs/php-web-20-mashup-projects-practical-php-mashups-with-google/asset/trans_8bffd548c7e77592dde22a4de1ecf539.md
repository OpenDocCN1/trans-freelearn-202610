
这些“普通旧 PHP 对象”通用性足够，可以在我们应用程序中稍后重用。

基于我们的数据库模式，我们的 `populateDB.php` 需要执行以下步骤：

- 1. 从 RDF 文件中获取所有线路。
- 2. 将所有线路插入表中。
- 3. 记住插入时生成的表主键。
- 4. 从 RDF 文件中获取所有车站。
- 5. 将所有车站插入表中。
- 6. 记住插入时生成的表主键。
- 7. 从 RDF 文件中获取线路中的所有车站。
- 8. 使用插入时生成的键，并根据 RDF 文件中的查询将它们正确插入到车站-线路连接表中。

我们的脚本以 RAP 所要求的标准初始化和准备代码开始。此外，我们还包含了两个模型对象定义。

```php
define("RDFAPI_INCLUDE_DIR", "Absolute/Path/To/rdfapi-php/api/");
require_once(RDFAPI_INCLUDE_DIR . "RdfAPI.php");
require_once('classes/models/clsLine.php');
require_once('classes/models/clsStation.php');
```

接下来，创建 SPARQL 客户端。我们将 tube 文档的 URL 传递给 load() 方法。

```php
//Create SPARQL Client
$sparqlClient = ModelFactory::getDefaultModel();
$sparqlClient->load('http://space.frot.org/rdf/tube_model2.rdf');
```

我们需要创建数据库连接。如果您不使用 MySQL，请根据需要修改此部分，并根据用户进行自定义。

```php
//Create MySQL Client
$mySQLConn = @mysql_connect("127.0.0.1", "DB USER NAME", "DB USER PASSWORD") or die("Couldn't connect to the MySQL server.");
$db = mysql_select_db("londontube", $mySQLConn) or die("Couldn't connect to the londontube database.");
```

现在是创建一些将查询 RDF 文档的函数的时候了。

第一个是 getAllStations() 函数。此函数将查询 RDF 文档并返回一个车站对象数组。

```php
function getAllStations(&$sparqlClient) {
    $returnArray = array();
    $query = '
PREFIX : <http://xmlns.com/foaf/0.1/>
PREFIX geo: <http://www.w3.org/2003/01/geo/wgs84_pos#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT DISTINCT ?stationName ?lat ?long
FROM <tube_model2.rdf>
WHERE {
    ?type rdfs:type
    <http://www.w3.org/2003/01/geo/wgs84_pos#SpatialThing> .
    ?type :name ?stationName .
    ?type geo:lat ?lat .
    ?type geo:long ?long
}
ORDER BY ?stationName';
    $result = $sparqlClient->sparqlQuery($query);
    if ($result != "false") {
        foreach ($result as $station) {
            if ($station != "") {
                $stationObj = new Station();
                $stationObj->setStationName($station['?stationName']->getLabel());
                $stationObj->setLat($station['?lat']->getLabel());
                $stationObj->setLong($station['?long']->getLabel());
                $returnArray[$station['?stationName']->getLabel()] = $stationObj;
            }
        }
    }
    return $returnArray;
}
```

此函数以我们之前构建的 SPARQL 查询开始，并使用传递给它的 SPARQL 客户端将其对加载的 RDF 文档执行。请记住，查询获取名称、纬度和经度。结果集包含每个车站的一行。foreach 循环遍历此结果集。它将每个结果对象放入名为 $station 的 RAP 资源对象中。对于结果集中的每个车站，都会实例化一个新的车站。使用 setter 方法，$station 中的结果填充每个车站对象的名称、纬度和经度。然后它将此对象放入要返回的数组中，以车站名称作为键。由于 RDF 中没有整数标识符，我们将不得不使用下一个最好的方法。车站和线路的名称将不得不作为键。

同样的原则适用于 getAllLines()，它从 RDF 文档中获取所有车站线路。

```php
function getAllLines(&$sparqlClient) {
    $returnArray = array();
    $query = '
PREFIX dc: <http://purl.org/dc/elements/1.1/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
FROM <tube_model2.rdf>
SELECT DISTINCT ?lineName
WHERE {
    ?type rdfs:type
        <http://space.frot.org/rdf/space.owl#Tube_Line> .
    ?type dc:title ?lineName
}
ORDER BY ?lineName';
    $result = $sparqlClient->sparqlQuery($query);
    if ($result != "false") {
        foreach ($result as $line) {
            if ($line != "") {
                $lineObj = new Line();
                $lineObj->setLineName($line['?lineName']->getLabel());
                $returnArray[$line['?lineName']->getLabel()] = $lineObj;
            }
        }
    }
    return $returnArray;
}
```

同样的原则适用于 getAllLines()，它从 RDF 文档中获取所有车站线路。同样，线路名称是此数组中的键。

最后，我们创建一个查找车站到线路关系的函数。

```php
function getLinesAndStations(&$sparqlClient) {
    $returnArray = array();
    $i = 0;
    $query = '
PREFIX : <http://xmlns.com/foaf/0.1/>
PREFIX dc: <http://purl.org/dc/elements/1.1/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT DISTINCT ?lineName ?stationName
FROM <tube_model2.rdf>
WHERE {
    ?line rdfs:type <http://space.frot.org/rdf/space.owl#Tube_Line>
    ?line dc:title ?lineName .
    ?line rdf:subject ?infourl .
    ?infourl dc:title ?stationName
}
ORDER BY ?lineName ?stationName';
    $result = $sparqlClient->sparqlQuery($query);
    if ($result != "false") {
        foreach ($result as $relationship) {
            if ($relationship != "") {
                $returnArray[$i]['line'] = $relationship['?lineName']->getLabel();
                $returnArray[$i]['station'] = $relationship['?stationName']->getLabel();
                $i++;
            }
        }
    }
    return $returnArray;
}
```

这个数组与另外两个一样，首先使用 SPARQL 查询加载的 RDF 文档。然而，返回的数组与另外两个不同。我们没有创建任何模型对象来保存关系，因此没有使用类似的东西。相反，我们返回一个多维数组。整数是索引，每个值内部是一个关联数组。关联数组将线路和车站名称组合在一起。

现在我们有三个返回三个数组的函数。让我们调用它们并开始处理数组。

```php
$linesArr = getAllLines($sparqlClient);
$stationsArr = getAllStations($sparqlClient);
$joinArr = getLinesAndStations($sparqlClient);
```

此块将数组存储在 $linesArr、$stationsArr 和 $joinArr 中。首先，我们将对 $linesArr 数组进行操作。

```php
foreach ($linesArr as $line) {
    $sql = 'INSERT INTO line (LineName) VALUES (\'' . addslashes(
        $line->getLineName()) . '\'\');
    $e = mysql_query($sql, $mySQLConn);
    $line->setLineId(mysql_insert_id($mySQLConn));
}
```

此 foreach 循环将 $linesArr 数组中的每个 Line 对象插入数据库。代码中的最后一句话将从插入中获取新的 ID 号并将其存储在对象属性中。另一个 foreach 循环对车站执行相同的操作。

```php
foreach ($stationsArr as $station) {
    $sql = 'INSERT INTO station (StationName, Latitude, Longitude)
    VALUES (\'' . addslashes($station->getStationName()) . '\', \''
    . addslashes($station->getLat()) . '\', \'' . addslashes($station->getLong()) . '\'\');
    $e = mysql_query($sql, $mySQLConn);
    $station->setStationId(mysql_insert_id($mySQLConn));
}
```

完成此操作后，我们仍然拥有线路和车站的数组。然而，每个对象的 ID 属性现在都已使用数据库分配的主键编号设置。我们需要在填充连接表时使用此属性。

```php
foreach ($joinArr as $key => $value) {
    $sql = 'INSERT INTO stationtoline (LineID, StationID) VALUES (' .
    $lines[$value['line']]->getLineId() . ',' . $stations[$value[
    'station']]->getStationId() . ')';
    $e = mysql_query($sql, $mySQLConn);
}
```

请记住，$join 是一个多变量数组，'line' 是关联数组中具有线路名称的键，而 'station' 是具有车站名称的键。我们使用这些键来获取 $linesArr 和 $stationsArr 中的对象。一旦
