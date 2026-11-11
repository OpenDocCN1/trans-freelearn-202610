
我们有这些对象，只需使用 ID 获取方法来获取该车站或线路的数据库主键 ID 即可。这些 ID 用于插入的 SQL 语句中。

在你的网页浏览器中运行此文件一次，你将拥有一个完全填充了伦敦地铁车站信息的数据库。是时候创建我们 mashup 的网页前端了。

## TubeSource 数据库接口类

此 mashup 将始终具有所有车站的下拉菜单。一旦用户选择一条线路，页面将自行刷新，并且该线路的车站将被标记。这意味着两件事：

- 1. 我们需要一个函数从数据库中提取地铁线路的名称。
- 2. 我们需要一个函数根据线路从数据库中提取车站名称。

我们将为此创建一个数据库接口类。它将是来自数据库的所有地铁信息的来源。在示例中，此文件位于 classes 目录中，名为 clsTubeSource.php。所有与数据库交互的操作都将在此类中进行。

```php
class TubeSource {
    private $dbConn;
    public function getAllLines() {
        $returnArray = array();
        $sql = 'SELECT LineID, LineName FROM line';
        $e = mysql_query($sql, $this->dbConn);
        while ($row = mysql_fetch_array($e)) {
            $lineObj = new Line();
            $lineObj->setLineId($row['LineID']);
            $lineObj->setLineName($row['LineName']);
            array_push($returnArray, $lineObj);
        }
        return $returnArray;
    }
    public function getStationsByLine($lineid) {
        $returnArray = array();
        $sql = 'SELECT S.StationName, S.Latitude, S.Longitude FROM
            stationtoline AS SL
            INNER JOIN station AS S
            ON SL.StationID = S.StationID
            WHERE SL.LineID = ' . $lineid;
        $e = mysql_query($sql, $this->dbConn);
        while ($row = mysql_fetch_array($e)) {
            $stationObj = new Station();
            $stationObj->setStationName($row['StationName']);
            $stationObj->setLat($row['Latitude']);
            $stationObj->setLong($row['Longitude']);
            array_push($returnArray, $stationObj);
        }
        return $returnArray;
    }
    public function __construct(&$dbConn) {
        $this->dbConn = $dbConn;
    }
}
?>
```

此类在构造函数中接收一个数据库连接对象。其两个方法 getAllLines() 和 getAllStationsByLine() 分别返回 Line 对象和 Station 对象的数组。它们以与 SPARQL 查询类似的方式处理并填充模型类。getAllStationsByLine() 将线路的主键 ID 作为参数，并将其用于 WHERE 子句中。

## 主用户界面

在此阶段，我们可以创建主用户界面页面，以查看我们的 mashup 的进展情况。让我们创建绘制 Google 地图并在用户选择线路时绘制标记的功能。此页面需要执行以下操作：

- 1. 创建并显示 Google 地图。
- 2. 包含用于显示车站标记的 JavaScript。
- 3. 调用 TubeSource 数据库类。
- 4. 为用户提供一个下拉菜单，其中填充了来自 TubeSource 的数据。

主页的这种基本形式名为 index-Basic.php。我们将逐步介绍处理所有列出功能的页面部分。稍后，我们将修改页面以添加 Flickr 调用以获取照片。

```php
<?php
$googleKey = 'YOUR GOOGLE API KEY';
require_once('classes/models/clsLine.php');
require_once('classes/models/clsStation.php');
require_once('classes/clsTubeSource.php');
```
```

此页面以一些初步初始化开始。Google API 密钥被设置在一个变量中。我们的所有模型类以及 TubeSource 类都被包含在内。

```php
//Create MySQL Client
$mySQLConn = @mysql_connect("127.0.0.1", "tubeapp", "tubular") or die("Couldn't connect to the MySQL server.");
$db = mysql_select_db("londontube", $mySQLConn) or die("Couldn't connect to the londontube database.");
//Create a DB abstraction object
$tubeSourceObj = new TubeSource($mySQLConn);
```

我们需要创建数据库代码。在这里，数据库客户端被创建，并且 TubeSource 使用该客户端进行实例化。

```php
$linesArr = $tubeSourceObj->getAllLines();
if ($_GET['line']) {
  $stationsArr = $tubeSourceObj->getStationsByLine($_GET['line']);
}
?>
```

接下来的几行结束了初步的 PHP 代码。第一行调用 TubeSource 的 getAllLines() 来获取所有线路。返回的 Line 对象数组 $linesArr 将用于创建下拉菜单。

如果向此页面传递了 GET 参数，我们将调用 TubeSource 的另一个方法 getStationsByLine()。这将为我们获取存储在数组中的线路的 Station 对象。

接下来，我们开始我们的 HTML 和 JavaScript。

```html
<html>
  <head>
    <title>London Tube Stations</title>
```

```html
<script src="http://maps.google.com/maps?file=api&amp;v=2&amp;key<?= $googleKey ?>" type="text/javascript"></script>
<script type="text/javascript">
var g_map;
```

JavaScript 首先声明了一些全局变量，用于在整个应用程序中保存信息。

```javascript
function load() {
    if (GBrowserIsCompatible()) {
        var point = null;
        g_map = new GMap2(document.getElementById("map"));
```

load 函数将由 body onload 事件执行。此函数的目的在于创建 Google 地图并在需要时绘制标记。这将地图加载到 g_map 全局变量中。

```javascript
g_map.addControl(new GSmallMapControl());
g_map.addControl(new GMapTypeControl());
g_map.setCenter(new GLatLng(51.5099983215,
-0.134690001607), 11);
```

这三行代码操作我们的地图。前两行添加了一些控件。你可以向 Google 地图添加一系列控件。第一行添加了你在 Google 地图上看到的平移和缩放命令的小版本。第二行在地图右上角添加了地图类型控件按钮。这些按钮控制地图是典型的街道地图、卫星地图还是混合地图。

第三行将地图居中到一个位置。通过研究、试验和错误，我找到了伦敦市中心的纬度和经度。我们将坐标传递给 GLatLng 对象，设置一个不错的缩放级别 11 以显示伦敦大部分区域，并将其传递给 setCenter() 方法。

```php
<?php if ($_GET['line'] && count($stationsArr) > 0) {
    foreach ($stationsArr as $station) { ?>
        point = new GLatLng(<?= $station->getLat() ?>,
        <?= $station->getLong() ?>);
        g_map.addOverlay(createMarker(point,
        '<?= addslashes($station->getStationName()) ?>'));
<?php } } ?>
    }
}
```

本节创建标记。我们使用 PHP 来帮助我们。如果向页面传递了线路 GET 参数并且车站数组不为空，那么我们需要为每个车站创建一个标记。仍然在 PHP 中，我们使用 foreach 循环进行遍历。由 point 表示的 GLatLng 对象使用 PHP 对象的纬度和经度属性创建。如果我们只是使用此点并将其传递给地图的 addOverlay 方法，我们将在地图上创建一个标记。然而，我们希望对其做一些额外处理，例如创建事件。

我们使用此点并将其传递给另一个函数 createMarker()。此函数创建标记，为其添加事件监听器，然后返回相同的标记。

```javascript
// Creates a marker at the given point with the given number label
function createMarker(point, stationName) {
  var marker = new GMarker(point);
  GEvent.addListener(marker, "click", function() {
    marker.openInfoWindowHtml("<div style=\"width:220px;
    height:250px;\"><b>" + stationName + "</b></div>");
  });
  return marker;
}
```

在函数的第一行创建了一个标记。请记住，GEvent 对象是在你调用 Google 地图时创建的。它的工作是监视所有 Google 地图对象上的事件。我们通过 addListener() 方法告诉它监听此标记上的点击。

在回调函数的参数中，我们定义标记被点击时将发生什么。在这里，我们告诉地图使用 openInfoWindowHtml() 打开信息窗口。我们使用车站名称提供 HTML 作为参数。打开时，信息窗口将出现在标记上方。车站的名称将是窗口中唯一的內容。

```html
</script>
</head>
<body onload="load() " onunload="GUnload()">
```

在我们的 body 标签中，我们通过调用 load() 来启动地图创建。我们还在页面退出时添加对 GUnload() 的调用。GUnload() 是 Google 地图 API 的一部分。它的工作是关闭任何内存泄漏。每当你使用 Google 地图时，在 onunload 页面事件中调用它始终是一个好主意。

```html
<form name="selectionForm" action="index-Basic.php" method="get">
<select name="line">
  <option value="">Select a Line</option>
