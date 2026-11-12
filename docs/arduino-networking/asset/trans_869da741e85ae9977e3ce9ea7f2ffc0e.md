

Serial.print("IP address: ");
Serial.println(Ethernet.localIP());

在草图的 loop() 函数中，第一步是从 DHT11 传感器获取测量值：

```
float h = dht.readHumidity();
float t = dht.readTemperature();
```

将这些测量值转换为字符串：

```
String temp = String((int) t);
String hum = String((int) h);
```

出于调试目的，我们也会在串口上打印这些值。在测试草图时，我们稍后会检查这些值是否正确：

```
Serial.println("Temperature: " + temp);
Serial.println("Humidity: " + hum);
```

现在，我们将实际将数据发送到服务器。暂时不必担心服务器端代码的作用，稍后我们会处理。首先，我们需要连接到运行在你电脑上的服务器：

```
if (client.connect(server, 80)) {
 if (client.connected()) {
 Serial.println("connected");
}
}
```

如果成功，我们可以发送请求。如同第 1 章《发现 Arduino 以太网扩展板》中所述，我们将使用标准 GET 请求，并将温度和湿度测量值作为参数传递。此时，你还需要输入你电脑的 IP 地址。所有这些都通过以下代码片段完成：

```
client.println("GET /datalogger/datalogger.php?temp=" + temp + "&hum=" + hum + " HTTP/1.1");
 client.println("Host: 192.168.1.100");
 client.println("Connection: close");
 client.println();
```

你可以看到代码调用了名为 datalogger.php 的文件，我们将在下一节中对其进行检查。

然后，在发出请求后，我们可以读取服务器的响应：

```
while (client.connected()) {
 while (client.available()) {
 char c = client.read();
 Serial.print(c);
 }
}
```

接下来，如果客户端不再连接到服务器，我们可以关闭连接：

```
if (!client.connected()) {
 Serial.println();
 Serial.println("disconnecting.");
 client.stop();
}
```

```
delay(1000);
```

## 注意

本节的所有代码都可以在此章节的 GitHub 仓库中找到：

https://github.com/openhomeautomation/arduino-networking/tree/master/chapter2

现在，如果我们只是将代码上传到 Arduino 开发板，不会发生什么大事，因为我们在服务器端没有做任何事情。Arduino 开发板会发出请求，但由于请求文件在服务器上不存在，服务器将返回错误代码。

因此，首先我们需要在服务器上构建一个文件来处理开发板发来的请求，并将数据记录到某个地方。接下来我们要做的正是此事。

## 将传入数据记录到数据库

在本节中，我们将使用 PHP 构建项目的服务器端部分。如果你是 PHP 的完全初学者，我推荐以下资源来学习该语言的基础知识：

[PHP 语言基础教程](http://php.net/manual/en/tutorial.php)

首先，我们将查看 datalogger.php 文件的内容。该文件将处理来自 Arduino 以太网扩展板的请求，在数据库中记录数据，并返回一条简单的消息。请注意，该文件必须位于你 Web 服务器上的名为 datalogger 的文件夹中。我们将查看代码的重要部分。要获取本节的完整代码，请参阅本章的 GitHub 仓库。请注意，所有 PHP 代码都应位于 <?php ... ?> 标签之间。

该文件首先接收来自 Arduino 以太网扩展板发送的 GET 请求中的数据：

```
$temperature = intval($_GET["temp"]);
$humidity = intval($_GET["hum"]);
```

我们还实例化与 SQLite 数据库的连接：

```
$db = new SQLite3('database.db');
```

然后，如果数据库文件是全新的，我们需要为数据库赋予一些结构。如果你不熟悉 SQL 命令，我邀请你访问以下链接：

[SQL 命令参考](http://www.cs.utexas.edu/~mitra/csFall2013/cs329/lectures/sql.html)

我们将在数据库中创建四个不同的列：一个由 SQLite 自动递增的唯一 ID，一个用于记录测量时间的时间戳，以及温度和湿度数据。这是通过以下代码片段完成的：

```
$db->exec('CREATE TABLE IF NOT EXISTS measurements (id INTEGER PRIMARY KEY,
timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP NOT NULL, temperature
INTEGER, humidity INTEGER);');
```

请注意，如果你使用更多传感器（例如，如果你也在测量环境光照度），可以在此处添加更多字段。只需向列表中添加一个新字段，例如：pressure INTEGER。

现在，我们可以将数据作为新行插入数据库。由于 SQLite 会自动添加 ID 和时间戳字段，我们只需插有关温度和湿度的数据：

```
$db->exec("INSERT INTO measurements (temperature, humidity) VALUES
('$temperature', '$humidity')");
```

如果你需要向数据库中插入更多数据，例如，如果你有另一项需要记录的测量值，可以简单地扩展此命令以将更多数据插入 SQLite 数据库。

## 提示

这段简单的代码说明将数据插入 SQLite 数据库是多么容易。然而，它使我们的服务器暴露于一种名为 SQL 注入的安全问题中。由于我们的项目仅用于你自己的本地网络，这里这并非真正的问题。然而，如果你计划将项目部署到网上，我建议修改代码以解决此安全问题。以下链接将为你提供关于此问题的更多信息 [SQL 注入](http://www.tutorialspoint.com/sqlite/sqlite_injection.htm)。

最后，我们用一条简单的消息回复 Arduino 开发板：

```
echo "Data received";
```

现在，我们尚未真正讨论数据库本身。本章代码中包含一个名为 database.db 的示例文件，但我建议你直接删除它并尝试创建自己的数据库。你可以直接在终端中进入存放所有项目文件的目录并输入：

```
sqlite3 database.db
```

## 提示

如果你使用 Windows，我建议将 Console 用作终端。你可以通过以下链接获取它 [Console 终端](http://sourceforge.net/projects/console/)。

此命令将创建你的数据库文件。你可以再次通过输入以下命令退出数据库文件：

```
.exit
```

请注意，如果你在 Linux 下，可能需要更改数据库及包含项目的文件夹的权限。为此，请通过终端进入项目文件夹并输入：

```
chmod 777 database.db
```

现在是时候对项目进行首次测试了。请确保服务器的所有文件都位于 Web 服务器上名为 datalogger 的文件夹中。你现在可以将 Arduino 草图上传到开发板并打开串行监视器。你首先应该看到草图正在连接到你的网络并接收 IP 地址，如下所示：

```
IP address: 192.168.1.103
```

然后，它应该进入 loop() 函数的第一次迭代，并打印出温度和湿度测量值：

```
Temperature: 21
Humidity: 37
```

此步骤之后，你应该看到草图已连接到你的服务器，并且服务器正在返回标准 200 OK 响应：

```
connected
HTTP/1.1 200 OK
```

```
Date: Mon, 19 May 2014 08:09:57 GMT
Server: Apache/2.2.23 (Unix) mod_ssl/2.2.23 OpenSSL/0.9.8y DAV/2 PHP/5.4.10
X-Powered-By: PHP/5.4.10
Content-Length: 13
Connection: close
Content-Type: text/html
```

此外，在该响应的末尾，你应该看到我们在 PHP 文件中定义的确认消息：

```
Data received
disconnecting.
```

如果你看到了这些消息，恭喜！这意味着你的 Arduino 以太网扩展板已成功与运行在你电脑上的 Web 服务器进行通信，并确实在记录一些数据。

如果情况并非如此，请首先检查硬件连接。确保 DHT11 传感器已正确连接到 Arduino 开发板，并且以太网电缆已插入扩展板和你的 Internet 路由器。不要犹豫，重新学习第 1 章《发现 Arduino 以太网扩展板》，以确保你的以太网扩展板正常工作。

此外，请确保 Web 服务器在你的电脑上处于活动状态，并且项目的所有文件（尤其是 `datalogger.php` 文件）都位于 Web 服务器文件夹内名为 `datalogger` 的文件夹中。你可以独立于 Arduino 以太网扩展板进行此测试。只需打开你喜欢的 Web 浏览器，并输入：

```
localhost/folder_of_the_project/datalogger.php?temp=20&hum=40
```

这将在数据库中记录一些虚拟数据，因此你可以确信 Web 服务器正常运行。

我们现在要检查服务器端是否正确记录了数据。而且我们将一举两得，因为这段代码还将在下一节中用于绘制数据。

要检查数据是否正确记录，你可以简单地创建一个名为 `readout.php` 的文件，它将从数据库读取数据、对其格式化并打印格式化后的数据，以便你查看。首先，我们需要访问 SQLite 数据库：

```
$db = new SQLite3('database.db');
```

然后，我们需要向数据库发出查询，以便它返回我们想要的数据。在此请求中，我们将获取名为 measurements 的表中的所有字段。这是通过以下代码片段完成的：

```
$results = $db->query('SELECT id, timestamp, temperature, humidity FROM measurements');
```

现在，我们需要使用 PHP 来解析包含查询所有结果的变量。我们只需为此使用一个 while 语句：

```
while($row = $results->fetchArray())
{

