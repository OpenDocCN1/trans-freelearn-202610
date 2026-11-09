
即使经过我的重新格式化，这个 JSON 也不太美观，而且其中许多名称-值对对我们来说并不有趣。尽管如此，我们还是成功生成了一个功能性的 JSON。如果我们使用程序来消费这个 JSON，我们将忽略有关电子表格本身的所有多余信息，只关注标题和内容实体以及 $t 值（在本例中为 Yale University 和 _cokwr: 26）。这些值在前一个示例中显示的 JSON 中被突出显示。如果你想知道是否可以将电子表格转换为 CSV 再转换为 JSON，答案是肯定的。我们将在本章后面的“使用 PHP 转换”和“使用 Python 转换”部分详细介绍如何做到这一点。

## 使用 phpMyAdmin 将 SQL 转换为 CSV 或 JSON

在本节中，我们将讨论两种无需编程即可直接从数据库写入 JSON 和 CSV 的选项，MySQL 在我们的例子中。

首先，phpMyAdmin 是一个非常常见的 MySQL 数据库基于 Web 的前端。如果您使用的是该工具的现代版本，您将能够导出整个表或查询结果为 CSV 或 JSON 文件。使用我们在第 1 章 Why Do You Need Clean Data? 中首次访问的同一个 enron 数据库，请考虑以下导出选项卡的屏幕截图，其中 JSON 被选为整个 employeelist 表的目标格式（CSV 也可在该选择框中使用）：

![](img/9aa003afabe05fdb07da46ad71325de6_107_0.png)

PhpMyAdmin JSON export for entire tables

导出查询结果的过程非常相似，只是不要使用屏幕顶部的导出选项卡，而是运行 SQL 查询，然后在页面底部的“查询结果操作”下使用导出选项，如下所示：

![](img/9aa003afabe05fdb07da46ad71325de6_108_0.png)

PhpMyAdmin can export the results of a query as well

以下是我们可以在 employeelist 表上运行以测试此过程的简单查询：

```sql
SELECT concat(firstName, " ", lastName) as name, email_id
FROM employeelist
ORDER BY lastName;
```

当我们将结果导出为 JSON 时，phpMyAdmin 会向我们显示 151 个值，格式如下：

```json
{
  "name": "Lysa Akin",
  "email_id": "lysa.akin@enron.com"
}
```

phpMyAdmin 工具很好，并且在转换存储在 MySQL 中的中等数量的数据时非常有效，特别是作为查询的结果。如果您使用的是不同的 RDBMS，您的 SQL 接口很可能也有一些自己的格式选项，您应该探索一下。

另一种策略是完全绕过 phpMyAdmin，只使用 MySQL 命令行来写出您想要格式化的 CSV 文件：

```sql
SELECT concat(firstName, " ", lastName) as name, email_id
INTO OUTFILE 'enronEmployees.csv'
FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"'
LINES TERMINATED BY '\n'
FROM employeelist;
```

这将使用指定的名称写入一个逗号分隔的文件（employees.csv）。它将被写入当前目录。

那 JSON 呢？使用这种策略没有非常干净的方式输出 JSON，因此您应该使用前面显示的 phpMyAdmin 解决方案，或者使用用 PHP 或 Python 编写的更强大的解决方案。这些编程解决方案将在后面的部分中介绍，所以请继续阅读。

## 使用 PHP 转换

在我们的第 2 章 Fundamentals – Formats, Types, and Encodings 中，在关于 JSON 数值格式的讨论中，我们简要展示了如何使用 PHP 连接到数据库、运行查询、从结果构建 PHP 数组，然后将 JSON 结果打印到屏幕上。在这里，我们首先将此示例扩展为写入文件而不是打印到屏幕，并写入 CSV 文件。接下来，我们将展示如何使用 PHP 读取 JSON 文件并转换为 CSV 文件，反之亦然。

### 使用 PHP 将 SQL 转换为 JSON

在本节中，我们将编写一个 PHP 脚本来连接到 enron 数据库，运行 SQL 查询，并将其导出为 JSON 格式的文件。为什么要为此编写 PHP 脚本而不是使用 phpMyAdmin？嗯，这种策略在我们需要在导出数据之前对数据进行额外处理，或者我们怀疑数据量超过基于 Web 的应用程序（如 phpMyAdmin）所能处理的情况时很有用：

```php
<?php
// connect to db, set up query, run the query
$dbc = mysqli_connect('localhost','username','password','enron')
or die('Error connecting to database!' . mysqli_error());

$select_query = "SELECT concat(firstName, \" \", lastName) as
    name, email_id FROM employeelist ORDER BY lastName";

$select_result = mysqli_query($dbc, $select_query);

if (!$select_result)
    die ("SELECT failed! [$select_query]" . mysqli_error());

// ----JSON output----
// build a new array, suitable for json
$counts = array();
while($row = mysqli_fetch_array($select_result))
{
```
```
// add onto the json array
    array_push($counts, array('name'     => $row['name'],
    'email_id' => $row['email_id']));
}
// encode query results array as json
$json_formatted = json_encode($counts);

// write out the json file
file_put_contents("enronEmail.json", $json_formatted);
?>
```

这段代码将 JSON 格式的输出文件写入 file_put_contents() 行中指定的位置。

### 使用 PHP 将 SQL 转换为 CSV

以下代码片段展示了如何使用 PHP 文件输出流创建 SQL 查询结果的 CSV 格式文件。将此代码保存为 Web 服务器上脚本可用的目录中的 .php 文件，然后在浏览器中请求该文件。它将自动下载包含正确值的 CSV 文件：

```php
<?php
// connect to db, set up query, run the query
    $dbc = mysqli_connect('localhost','username','password','enron')
    or die('Error connecting to database!' . mysqli_error());

$select_query = "SELECT concat(firstName, \" \", lastName) as
    name, email_id FROM employeelist ORDER BY lastName";

$select_result = mysqli_query($dbc, $select_query);

if (!$select_result)
    die ("SELECT failed! [$select_query]" . mysqli_error());

// ----CSV output----
// set up a file stream
$file = fopen('php://output', 'w');
if ($file && $select_result)
{
    header('Content-Type: text/csv');
    header('Content-Disposition: attachment;
    filename="enronEmail.csv"');
    // write each result row to the file in csv format
```
```
while($row = mysqli_fetch_assoc($select_result))
{
    fputcsv($file, array_values($row));
}
?>
```

结果的格式如下（仅显示前三行）：

```
"Lysa Akin",lysa.akin@enron.com
"Phillip Allen",k..allen@enron.com
"Harry Arora",harry.arora@enron.com
```

如果你想知道 Phillip 的电子邮件是否真的应该有两个点，我们可以运行一个快速查询来找出 Enron 有多少电子邮件的格式是这样的：

```sql
SELECT CONCAT(firstName, " ", lastName) AS name, email_id
FROM employeelist
WHERE email_id LIKE "%..%"
ORDER BY name ASC;
```

结果发现 24 个电子邮件地址的格式与此类似，带有双点。

### 使用 PHP 将 JSON 转换为 CSV

在这里，我们将使用 PHP 读取 JSON 文件并将其转换为 CSV 并输出文件：

```php
<?php
// read in the file
$json = file_get_contents("outfile.json");
// convert JSON to an associative array
$array = json_decode ($json, true);
// open the file stream
$file = fopen('php://output', 'w');
header('Content-Type: text/csv');
header('Content-Disposition: attachment;
filename="enronEmail.csv"');
// loop through the array and write each item to the file
foreach ($array as $line)
{
    fputcsv($file, $line);
}
?>
```

这段代码将创建一个 CSV，其中包含每一行，就像之前的示例一样。我们应该注意 file_get_contents() 函数会将文件作为字符串读入内存，因此对于极大的文件，您可能需要改用 fread()、fgets() 和 fclose() PHP 函数的组合。

### 使用 PHP 将 CSV 转换为 JSON

另一个常见任务是读取 CSV 文件并将其写出为 JSON 文件。大多数时候，我们有一个 CSV，其第一行是标题行。标题行列出了文件中每一列的列名，我们希望标题行中的每一项都成为 JSON 格式的文件的键：

```php
<?php
$file = fopen('enronEmail.csv', 'r');
$headers = fgetcsv($file, 0, ',');
$complete = array();

while ($row = fgetcsv($file, 0, ','))
{
    $complete[] = array_combine($headers, $row);
}
fclose($file);
$json_formatted = json_encode($complete);
file_put_contents('enronEmail.json',$json_formatted);
?>
```

此代码对前面创建的带有标题行的 enronEmail.csv 文件的结果如下：

```
[{"name":"Lysa Akin","email_id":"lysa.akin@enron.com"},
{"name":"Phillip Allen","email_id":"k..allen@enron.com"},
{"name":"Harry Arora","email_id":"harry.arora@enron.com"},...]
```

对于此示例，实际 CSV 文件中的 151 个结果中，仅显示了前三行。

## 使用 Python 转换

在本节中，我们描述了多种使用 Python 将 CSV 转换为 JSON 以及反之亦然的方法。在这些示例中，我们将探索使用专门安装的库和使用更原生的 Python 代码来实现此目标的不同方法。
