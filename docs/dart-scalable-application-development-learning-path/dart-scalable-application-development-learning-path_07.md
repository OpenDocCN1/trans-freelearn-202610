## 实时数据收集

> “没有数据就进行理论推导是一个重大错误。”
> 
> —— 夏洛克·福尔摩斯

IT 界的大多数人都熟悉**垃圾进，垃圾出（GIGO）**这个说法。如果错误的数据进入系统，输出也将是错误的。应该始终使用好的数据源，并且系统需要足够健壮，以处理用户或其他系统的“垃圾”。规范和标准可以提供帮助，但总会有例外和灰色地带。

在我的职业生涯中，听过最令人困惑的话之一来自一位遇到软件数据输入问题的客户：“我们有我们自己的 ISO 标准”！

### 启动地震监测系统

本书接下来的系列章节将围绕一个单一项目，从多个不同角度展开。我们将跟踪、记录和展示一个包含我们居住星球地震信息的真实世界数据源。

这个项目相当宏大，过程中会涉及大量 Dart 代码。令人惊讶的是，地壳中有如此多的活动——也许你可以利用我们迄今为止项目的输出，写一篇关于地震学的博客或演示文稿！

本节的项目名为 `QuakeMonitorFS`——请确保选择名称以 FS 结尾而不是 DB 的项目。FS 版本保存到文件系统，而 DB 版本保存到数据库。我们将首先查看文件系统存储版本，然后再看数据库版本。

### 介绍数据源

该应用程序的数据源是由美国地质调查局（USGS）地震灾害计划提供的 Web 服务。他们的网站 http://earthquake.usgs.gov/ 上有大量信息，包括来自世界各地持续更新的数据源。

对 Dart 程序员最感兴趣的数据源采用一种特定的 JSON 格式。

### 探索 GeoJSON 格式

GeoJSON 是一种用于记录地理数据结构的格式，它支持几何定义，例如点和多边形。例如，地震震中的位置（点）和受影响区域（多边形）。GeoJSON 的一部分如下所示：

```
{
  "type":"Point",
  "coordinates":[122.3818333,45.0686667,12.9]
},
"id":"uw61022191"
```

> 前面的示例是报告过去一小时所有地震的数据源（http://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_hour.geojson）中的一小段摘录。花点时间查看此数据源，感受一下这种格式包含的数据类型。

为了启动这个项目，我们将创建一个数据监控应用程序，用于记录来自此数据源的信息。由于该程序将长时间运行，我们需要添加日志记录功能，以确保其平稳运行。

### 获取并记录数据

bin/main.dart 中的 main 函数将使用 DataMonitor 类的实例提示每分钟下载数据。一天有 1440 分钟，数据文件平均大小约为 2k，因此每天 3 兆字节的磁盘空间是可行的：

```
main() async {
  setupLogging();

  DataMonitor quakeMon = new DataMonitor();
```


```dart
Duration updateInterval = new Duration(seconds: 60);

    new Timer.periodic(updateInterval, quakeMon.fetchData);
}
```

主函数被标记为 async，这表明实现将使用 await 特性。对 setupLogging 的调用将在本章下一节中介绍。

DataMonitor 的实现在 lib/quakemonitorfs.dart 中，其中包含关键方法 fetchData：

```dart
fetchData(Timer timerFetch) async {
    try {
        var outDir = new Directory('data');
        if (!await outDir.exists()) {
            await outDir.create();
        }

        log.info("Calling web service...");
        HttpClientRequest req = await geoClient.getUrl(Uri.parse(dataUrl));
        HttpClientResponse resp = await req.close();
        String latestFilePath = path.join('data', latestFilename);
        await resp.pipe(new File(latestFilePath).openWrite());
        String fileContents = await new File(latestFilePath).readAsString();

        var dataset = await JSON.decode(fileContents);

        int newId = int.parse(dataset['metadata']['generated'].toString());

        if (idPreviousFetch != newId) {
            idPreviousFetch = newId;
            var f = new File(path.join('data', '${newId}.txt'));
            await f.writeAsString(dataset.toString());
            log.fine("Saved $newId - ${dataset.toString()}");
        }
    } catch (exception, stacktrace) {
        log.severe("Exception fetching JSON.", exception, stacktrace);
    }
}
```

几乎每一行都有一个 await！程序只需要每隔一分钟左右处理一次信息，因此它不需要响应式结构，更线性的方式就足够了，尽管代码仍然是异步的。

远程资源首先被处理，然后响应被关闭。返回的 JSON 字符串可以直接写入文件。完成后，fileContents 变量会填充来自文件的数据，然后由 JSON 解析以将对象返回到 dataset。

检查响应的 id 以防止重复保存相同的响应。请注意，生成的 id 可能不同，而内容相同。该 feed 更像是一个快照，而不是一个稳定的流。

数据使用获取的 id 作为文本（.txt）文件保存到 data 文件夹中。为了提供一些反馈，id 和 JSON 会通过对 log.fine 的调用写入日志文件。

## Logging

我在早期软件项目中遇到了许多困难点，唯一能做的就是求助于日志记录，以弄清楚发生了什么。不久后我开始喜欢上日志记录，并在开始新项目时将其作为重点。它是帮助你测试的另一双眼睛——尤其是在你不查看时，例如在持续运行数据收集器时。

Dart 包 logging 提供了日志记录功能，但它对输出没有做任何有用的事情，或者换句话说，日志存储的处理完全由应用程序负责。

### A simple example of logging

打开本章的 Logging 文件夹，它是一个独立的示例，包含各种日志记录：

```dart
Logger log = new Logger('DataMonitor');

Logger.root.level = Level.ALL;
Logger.root.onRecord.listen((LogRecord rec) {
  String logMessage = '${rec.level.name}\t${rec.time}\t${rec.message}';
  String exceptionMessage = '';
  if (rec.error != null) {
    print("$logMessage \t${rec.error.message}");
    print(exceptionMessage);
  }
});
```

```dart
log = new Logger('DataMonitor');
Logger.root.level = Level.ALL;
Logger.root.onRecord.listen((LogRecord rec) {
  var entry = '${rec.level.name}\t${rec.time}\t\t${rec.message}\n';
  File logStore = new File("datamonlog.txt");
  logStore.writeAsStringSync(entry, mode: FileMode.APPEND);
});
log.info('Earthquake Monitor - Data fetcher. Starting...');
}
```

日志记录在 bin/main.dart 中设置，输出附加到文本文件。请注意，这不会保存到 data 子文件夹中，因为它可能会与所有数据文件一起丢失。

在 DataMonitor 类的 fetchData 方法中，Logger 对象在构造函数中设置，关键事件将被记录。使用不同的日志级别（info、fine、severe 等），这将允许程序的操作员将日志级别调整到所需级别。

## Saving to the database

数据收集应用程序目前写入文件系统。虽然这对于归档很有用，但它使得访问和查询任何类型的分析或显示变得困难且缓慢。

![](img/52c05b53a56abd53584e4fed738ed509_162_0.png)

> 如果你对数据库不熟悉，这里有一个非常快速的介绍！数据库是以结构化形式存储的数据集合。这些数据通过数据库管理系统进行访问和管理。你可能听说过 SQL Server、MySQL、Hadoop 或 Oracle。
>
> 数据库通常使用一种称为 SQL 的语言进行访问，以从数据库读取和写入记录。数据通常组织成表，每个表由记录组成。每个表有一组定义为数据类型的列——这类似于编程语言中的变量。SQL 在排序、分组和处理数据集方面很强。
>
> SQL 可以以多种形式存储在数据库中，最常见的是存储过程，可以添加/删除和更新数据，并执行一些处理。
>
> 连接到数据库几乎总是需要登录，并且可能在远程计算机上。这有时被称为连接字符串。它类似于网络连接。

### Installing a database system

有许多数据库系统可用，你可能已经有自己喜欢的了！就本书而言，所使用的数据库将是 PostgreSQL，因为它是免费的、开源的，并且适用于所有主要操作系统。而且它是最容易安装的数据库之一，因此我们可以花更多时间编写 Dart。

PostgreSQL 是一个功能强大的开源对象关系数据库系统，拥有 15 年的历史。

它是一款一流的产品，被 Yahoo 和联合国等行业和政府的大公司使用。它严格遵守 SQL 语言标准，可以处理最大尺寸达 32 TB 的数据库表。

PostgreSQL 可以从 http://www.postgresql.org/ 下载，适用于 Mac、Linux 和 Windows，并且可通过大多数 Linux 发行版的软件包管理器获得。此外，在 PostgreSQL 下载页面上，还有许多虚拟机镜像可供下载并直接使用。

本书的示例假设数据库安装与运行软件的机器相同。调整连接字符串将允许连接到远程机器。

### Using PostgreSQL from Dart

包 postgresql 提供了非常强大的 PostgreSQL 数据库驱动程序，可从 Dart 程序中使用。它是一个备受推崇的第三方包，并得到了 Dart 团队的贡献。

本节的项目称为 QuakeMonitorDB，其获取方式与文件系统版本相同。

**File** 菜单有 **Add Server...** 选项，可在其中建立到数据库服务器的连接。

![](img/52c05b53a56abd53584e4fed738ed509_165_0.png)

## Live Data Collection

一旦连接可用，就可以在左侧窗格中的树中选择它。要向数据库发出命令，请从该服务器的数据库列表中选择它，并从 Tools 菜单中选择 Query tool...。

![](img/52c05b53a56abd53584e4fed738ed509_166_0.png)

查询工具允许对已连接的数据库运行 SQL 查询，并显示输出和结果。输入查询，例如 select 'Dart is awesome!'; 然后点击 Query 菜单中的 Execute。

如果你愿意，PostgreSQL 还有一个功能强大的内置命令行客户端，称为 psql。





## 创建数据库和登录

应用程序的数据库将被称为 `quakedata`。使用的登录名为 `webquakeuser`，密码为 `Coco99nut`（区分大小写且不带引号）：

```
CREATE DATABASE quakedata
    WITH OWNER = postgres
        ENCODING = 'UTF8'
        TABLESPACE = pg_default
        LC_COLLATE = 'en_GB.UTF-8'
        LC_CTYPE = 'en_GB.UTF-8'
        CONNECTION LIMIT = -1;
```

创建数据库的 SQL 脚本名为 `CreateDatabase.sql`，可在本章示例代码的 SQL 子文件夹中找到：

```
CREATE ROLE webquakeuser LOGIN
    ENCRYPTED PASSWORD 'md504a29d543c1492922e76af320cae3190'
    SUPERUSER INHERIT CREATEDB NOCREATEROLE NOREPLICATION;
```

创建用户的 SQL 脚本名为 `CreateUser.sql`。

## 定义表

主表将是对 JSON 字段的简单存储，加上两个元数据字段，其创建 SQL 可在 `CreateTable.sql` 中找到：

```
CREATE TABLE dm_quakefeed (
    quake_id serial PRIMARY KEY,
    geojson text NOT NULL,
    modified_date TIMESTAMP default CURRENT_TIMESTAMP
);
```

## 实时数据采集

此 SQL 定义了一个包含三个字段的表。`quake_id` 字段将作为每条记录的唯一标识符，`geojson` 将存储从服务检索到的数据，而 `modified_date` 将记录插入数据库的时间。让我们看一下以下截图：

![](img/52c05b53a56abd53584e4fed738ed509_168_0.png)

空数据库现已配置完成，准备连接到 Dart 以保存数据。表可在左侧面板的树形结构中找到 **quakedata | Schemas | public | Tables。**

## 插入数据

第一步是创建连接字符串以连接到 PostgreSQL 服务器。类 `DaoQuake` 在 `lib/quakedao.dart` 中处理数据库访问。这由修订后的 `DataMonitor` 类的 `fetchData` 方法调用，如下所示：

```
if (idPreviousFetch != newId) {
    idPreviousFetch = newId;

    //Save to database.
    await daoGeoJson.storeJson(dataset.toString());

    log.fine("Saved to db $newId - ${dataset.toString()}");
}
```

daoGeoJson 实例处理数据库交互。其余的数据获取，例如访问 Web 服务，与基于文件版本相同：

```
storeJson(String jsonString) async {
var dbConn;
try {
    dbConn = await connect(uri);
    await dbConn.execute(
        'insert into dm_quakefeed (geojson) values (@geojson)', {
        'geojson': jsonString
    });
} catch (exception, stacktrace) {
    log.severe("Exception storing JSON.", exception, stacktrace);
    print(exception);
    print(stacktrace);
} finally {
    dbConn.close();
}
}
```

首先需要注意的是，实现被包裹在 try{}、catch{} 和 finally{} 中。这是因为我们希望即使在错误条件下也能获得稳健的解决方案。数据库会失败，连接也会失败，因此插入可能不会进行，所以我们捕获异常。一旦 dbConn 对象连接到数据库服务器，它就会运行 SQL 语句来存储 jsonString。

应用程序还需要管理系统资源，因此我们始终希望关闭数据库连接，就像内存和磁盘空间一样，它们是有限资源。finally 子句总是在 try、catch 和 except 块完成后执行：

```
String uri = 'postgres://webquakeuser:Coco99nut@localhost:5432/quakedata';
```

数据库连接在 uri 连接字符串中定义，它标识服务器、用户名、密码和数据库。通常这会存储在配置文件中。

## 运行程序

程序运行几分钟后，数据将开始在数据库中累积。这可以在 pgAdmin 的 SQL 编辑器中查看，可通过单击工具栏上的 SQL 图标运行：

```
select * from dm_quakefeed;
```

## 实时数据采集

此简单的 `select` 语句将显示表内容：

![](img/52c05b53a56abd53584e4fed738ed509_170_0.png)

## 维护数据库

设置数据库并编写相应的应用程序只是运行数据库的一小部分。就像花园一样，它们需要照料和修剪才能获得最佳效果。

我们将研究为数据监控应用程序编写维护脚本，以报告数据并清除数据。

在编辑器中打开 `bin/maintenance.dart` 文件。

## 管理命令行参数

为了在这些任务之间切换，将使用命令行参数。Dart `main` 函数接收其参数的字符串列表：

```
```dart
import 'package:QuakeMonitorDB/quakedao.dart';

main(List<String> arguments) {
  if (arguments.length == 0 || arguments.length > 1) {
    print("Please use either -info or -delete.");
    return 0;
  }

  if (arguments[0] == "-info") {
    var daoGeoJson = new DaoQuake();
    daoGeoJson.displayInfo();
  } else if (arguments[0] == "-delete") {
    var daoGeoJson = new DaoQuake();
    daoGeoJson.deleteRecords();
  }
}
```
```

结果集从 PostgreSQL 返回并转换为列表。返回的每条记录都是该记录列值的列表：

```
Count  : 269
MinDate : 2015-05-26 00:00:00.000
MaxDate : 2015-05-27 00:00:00.000
```

在此实例中，我们返回了一条记录（result[0]），其中包含三个列（通过索引访问）的列表，其中包含我们需要的信息。

## 删除数据

此非常严重但有用的操作实现起来相当简单：

```
deleteRecords() async {
  var dbConn;
  try {
    dbConn = await connect(uri);

    var query = "delete from dm_quakefeed";
    await dbConn.execute(query);
  } catch (exception, stacktrace) {
    log.severe("Exception getting info.", exception, stacktrace);
    print(exception);
  } finally {
    dbConn.close();
  }
}
```

如果在此运行后使用 -info 选项重新运行维护程序，结果如下所示，其中 null 表示没有可用数据：

```
Count  : 0
MinDate : null
MaxDate : null
```

请务必非常、非常小心地使用它！

## 观察 Dart VM 内部

如果你在启动 Dart 项目时密切关注输出窗口，你会注意到对 Observatory 的引用。此工具（是 Dart SDK 的一部分）允许开发人员查看正在运行的 Dart 虚拟机内部，并分析正在进行的活动。这对于了解应用程序正在做什么以及瓶颈所在非常有用。

![](img/52c05b53a56abd53584e4fed738ed509_173_0.png)

Observatory 支持命令行应用程序和 Web 应用程序（Dart 版本，而不是编译为 JavaScript 的输出）在 Dartium 中运行。要启用它，只需传递众多 Observatory 命令行参数之一 — 请参阅 SDK 文档（https://www.dartlang.org/tools/dart-vm/）获取完整列表：

```
dart --enable-vm-service bin/main.dart
```

使用 Observatory 启动 main.dart 程序，并浏览地址 localhost:8181（请注意，此端口号可能会更改，但可以在启动命令参数 --enable-vm-service=<port>/<IP address> 中指定）：

```
chromium.exe index.html
```

使用 Observatory 启动 index.html。将要输入到 Web 浏览器的地址将会显示：

```
Observatory listening on http://127.0.0.1:34935
```

URL 可以在任何 Web 浏览器中打开，最重要的功能是刷新按钮，它会重新采样正在显示的数据。

在等待数据监控收集数据时，花点时间探索 Dart VM 在运行代码时的内部工作原理。

## 单元测试

如果你是测试驱动开发的倡导者，你可能有点担心我们到目前为止还没有编写任何单元测试。情况即将改变，因为我们将介绍 Dart 单元测试包，其名称简单地称为 test（你可能会在网上看到使用旧的 unittest 包的旧示例代码）。

在本章的示例代码中，有一个名为 Unittest 的子文件夹，其中包含 bin/main.dart 中的单元测试：

```
library Unittestdemo.test;

import 'package:test/test.dart';

void main() {

  test('HelloWorldTest', () {
    expect(1+1, 2);
  });

}
```

这定义了一个名为 HelloWorldTest 的新测试，实际测试可以通过过去在函数中进行。测试结果可以使用 expect 方法进行验证。库包含大量匹配器来检查结果，例如 isResult、isList、isNull、isTrue 和 isNonPositive。




单元测试侧重于一次测试应用程序的一小部分。测试在代码中定义，以便可以轻松快速地运行。为了确保它们快速运行，真正的单元测试不应使用外部资源（如网络或文件）。

要使应用程序背后的代码易于进行单元测试，设计必须允许对象被隔离测试。例如，所有 IO 都可以包装到一个简单的类中，这样就可以编写一个使用 IO 类的假（或模拟）版本的单元测试。

这会影响设计，但确保代码在整个开发过程中都经过测试。测试甚至可以在开发开始之前编写。

## 运行单元测试

单元测试的运行方式与任何其他 Dart 命令行程序一样，可以从您选择的 IDE/编辑器或从命令行运行。例如，从 Unittest 文件夹中，可以通过 `dart bin/main.dart` 运行测试：

```
00:00 +0: HelloWorldTest
00:00 +1: All tests passed!
```

测试运行器的默认输出包含颜色字符（您的 IDE 或 shell 可能不支持）：

![](img/52c05b53a56abd53584e4fed738ed509_175_0.png)

## 实时数据收集

```dart
  o = new GeoUpdate(sampleJson);
});
test('Test 1', () => expect(o.features.length, equals(5)));
test('Test 2', () => expect(1, equals(1)));
});
```

在项目开发过程中，开发测试和实现通常是一个迭代的过程。如这组中的第二个测试所示，很容易放入一个占位测试。测试有助于确保代码的最终实现具有预先设计、松耦合，并且在更改时提供一种简单的方法来自动重新测试一系列功能。

对比一下：运行一组单元测试只需几秒钟即可覆盖许多测试用例，而运行一个完整的 Web 应用程序则需要几分钟，浏览页面、输入数据、检查结果，而且只覆盖了一个测试用例。

## 检查测试结果

运行单元测试产生的结果显示所有测试都失败。这是预期的，因为我们还没有编写实现。

我们不会以令人沮丧的方式结束本章！我们所做的是考虑设计，为对象定义了一些需求，并创建了一个测试套件来在我们创建解决方案时验证它。

## 小结

严肃的应用程序需要健壮性，现在您将掌握 Dart 中的单元测试，以帮助快速生产高质量软件。我们还介绍了如何记录关键操作细节，并将其归类到正确的级别，以便长期监控系统。

将 Dart 连接到严肃的数据库对许多应用程序至关重要，您现在可以使用工业级数据库系统来实现这一点。您现在已经具备了连接到远程 Web 服务以收集真实数据的技能。

在下一章中，我们将探索数据库系统，包括将数据提取到我们的应用程序中，我们将挖掘这个不断更新的数据存储库，以填充传入数据的丰富数据网格视图，该视图将实时更新。
