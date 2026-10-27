## 9 使用数据库

在本章中，我们将介绍以下小节：

- 使用 IndexedDB 在本地存储数据
- 使用 Lawndart 编写离线 Web 应用
- 在 MySQL 中存储数据
- 在 PostgreSQL 中存储数据
- 在 Oracle 中存储数据
- 在 MongoDB 中存储数据
- 在 RethinkDB 中存储数据

### 简介

数据之于应用，就像食物之于人；没有数据，应用就毫无意义。此外，数据必须持久化；数据可以存储在客户端、服务器端，或两端都存。在客户端，我们介绍 IndexedDB 和 Lawndart 数据管理器，它们提供离线数据存储，无需担心 IndexedDB 是否受支持。然后，我们研究如何将数据存储在服务器端的 SQL 和 NoSQL 数据库系统中。到本章结束时，你将拥有一系列广泛的选择，来决定将应用数据存储到哪个数据库。

### 使用 IndexedDB 在本地存储数据

```
import 'dart:indexed_db';
import 'job.dart';

class JobStore {
  static const String JOB_STORE = 'jobStore';
  static const String TYPE_INDEX = 'type_index';
  Database _db;
  final List<Job> jobs = new List();

  Future openDB() {
    return window.indexedDB
      .open('JobDB', version: 1, onUpgradeNeeded: _initDb)
      .then(_loadDB)
      .catchError(print);
  }

  void _initDb(VersionChangeEvent e) {
    _db = (e.target as Request).result;
    var store = _db.createObjectStore(JOB_STORE, autoIncrement: true);
    store.createIndex(TYPE_INDEX, 'type', unique: false);
  }

  Future add(Job job) {
    // create transaction and get object store:
    var trans = _db.transaction(JOB_STORE, 'readwrite');
    var store = trans.objectStore(JOB_STORE);
    store.add(job.toMap())
    // called when add completes
    .then((addedKey) {
      print(addedKey);
      job.dbKey = addedKey;
    });
    return trans.completed.then((_) {
      // called when transaction completes
      jobs.add(job);
      return job;
    });
  }

  Future _loadDB(Database db) {
    _db = db;
    var trans = db.transaction(JOB_STORE, 'readonly');
    var store = trans.objectStore(JOB_STORE);
```

以下是截图以及 Chrome 开发者工具中 IndexedDB 数据库的视图：

![](img/52c05b53a56abd53584e4fed738ed509_785_0.png)

### 工作原理...

与 IndexedDB 的交互是在 `dart:indexed_db` 库中实现的，因此我们需要在任何需要的地方导入它。测试浏览器是否能在 IndexedDB 中存储数据总是个好主意。要判断是否支持 IndexedDB，可以使用 `IdbFactory` 类的 `supported` getter。与 IndexedDB 的所有交互都是异步的，并返回 `Future`。

如第二个注释所示，调用了 `openDB()`。`window.indexedDB.open` 方法用于打开或创建新数据库。如果给定的数据库名称和版本已存在，则会打开该数据库。对于新的数据库名称或更高的版本号，会触发 `upgrade needed` 事件。在其事件处理器中，会在 `JobDB` 数据库内为 `Job` 类型创建一个对象存储（object store），记录会自动递增键。我们还展示了如何使用 `createIndex` 方法在对象存储中基于 `Job` 类的 `type` 字段创建索引。IndexedDB 数据库可以包含多个对象存储，每个存储可以有多个索引。使用 `.catchError` 处理打开数据库时可能出现的错误非常重要。我们还保留了一个核心的 `Database` 对象 `_db`，供数据库方法使用。





一个常见的场景是，将某个存储中的所有记录都读入应用，以便在打开数据库后显示它们。这是在 _loadDB 中完成的。如果我们查看其他方法（add、update、remove 和 clear），我们会看到一个共同的模式，即所有数据库操作都必须在 Transaction 对象内执行。它以 object store 名称 JOB_STORE 和某种模式（如 readonly 或 readwrite）作为参数进行检索。读取多条记录，例如在 _loadDB 中，是通过打开游标然后监听它（使用 listen）来实现的。每次 listen 事件都通过 cursor.value 读取一条新记录，该记录被转换为 Job 对象并添加到列表中。

要存储的对象必须以 map 格式提供给存储的 add 方法。当为此存储将 autoincrement 设置为 true 时，生成的键号将作为 addedKey 返回。当事务完成（trans.completed.then）时，我们将新创建的 job 添加到我们的列表中。update、remove 和 clear 方法遵循相同的模式，它们分别在 object store 中调用 put、delete 和 clear 方法。

## See also

- 如果你想将本地存储与 IndexedDB 进行比较，请参阅第 5 章 Handling Web Applications 中的 Using Browser Local Storage 配方

## Using Lawndart to write offline web apps

如果主要要求是你的应用可以脱机工作，提供通用的离线键值存储，无论客户端浏览器是否支持 IndexedDB 还是不支持？那么，Lawndart 就来救援了。

Lawndart（https://github.com/sethladd/lawndart, 但可通过 pub 包获取）并不是一个新数据库，而是一个管理器，它会自动选择客户端上可用的最佳本地存储机制。它由 Seth Ladd 开发，是 Lawnchair（http://brian.io/lawnchair/）的 Dart 重写版。你可以在项目 using_lawndart 中看到它的实际效果。

## How to do it...

通过在 pubspec.yaml 文件中添加 lawndart:any 来导入 Lawndart 包。以下内容来自启动脚本 using_lawndart.dart 的相关代码：

```
void main() {
  js = new JobStore();
  // 1- creating and opening the database:
```
```
```dart
js.open();
querySelector("#store").onClick.listen(storeData);
}

storeData(Event e) {
  var job = _jobData();
  // 2- writing data to storage
  js.add(job);
}
```
```

数据库特定的代码被隔离在 jobstore_lawndart.dart 中：

```
```dart
library store;

import 'dart:async';
import 'package:lawndart/lawndart.dart';
import 'job.dart';

class JobStore {
  static const String JOB_DB = 'jobDb';
  static const String JOB_STORE = 'jobStore';
  final List<Job> jobs = new List();
  // 3- making a Store object:
  var _store = new Store(JOB_DB, JOB_STORE);

  Future open() {
    // 4- opening storage and retrieving all records:
    return _store.open().then(_loadDB).catchError(print);
  }

  _loadDB(_) {
    Stream dataStream = _store.all();
    return dataStream.forEach((dbjob) {
      var job = new Job.fromJson(dbjob);
      jobs.add(job);
    });
  }

  add(Job job) {
    // 5- storing data:
    _store.save(job.toMap(), job.dbKey.toString()).then((addedKey) {
      jobs.add(job);
    });
  }
}
```
```

```
Future update(Job job) {
  return _store.save(job.toMap(), job.dbKey.toString());
}

Future remove(Job job) {
return _store.removeByKey(job.dbKey.toString());
}

clear() {
  _store.nuke().then((_) {
    jobs.clear();
  });
}
```

## How it works...

Lawndart 为本地存储、IndexedDB 和 Web SQL 提供异步但一致的接口。你的应用只需使用 Store 类的实例工作，工厂构造函数将尝试 IndexedDB、Web SQL，最后是本地存储。这在第三条注释中显示，其中创建了 Store 类的对象；该对象还构建了所需的本地数据库。在第一条注释中，JobStore 对象中的 open 方法触发 Store 对象上的 open 方法（第四条注释）。_loadDB 选项随后读取整个存储；这是通过 all 方法实现的，该方法返回 Stream of values。添加和更新数据是通过在存储对象上调用 save 方法完成的（第五条 5）。删除记录是通过 removeByKey 完成的；清除整个数据库需要 nuke 方法。请注意，使用 Lawndart 的代码比上一配方中显示的 IndexedDB 要干净得多。

## See also

- Cargo 是 Joris Hermans 开发的另一个 pub 包，实现了类似的目标。它是一个存储包，抽象了客户端上的本地存储、客户端以及服务器上的存储，并在磁盘上存储 JSON 文件。有关更多信息，请参阅 https://pub.dartlang.org/packages/cargo。
- 完整的 API 文档可以在 http://www.dartdocs.org/documentation/lawndart/ 找到。

## Storing data in MySQL

MySQL 无可否认是最受欢迎的开源 SQL 数据库。Dart 可以使用 James Ots 的 pub 包 `sqljocky`（https://github.com/jamesots/sqljocky）与 MySQL 通信。在本节中，我们将逐步演示如何使用此驱动程序。你可以在 `using_mysql` 项目中看到它的实际效果。

### Getting ready

- 要获取数据库软件，请从 http://dev.mysql.com/downloads/mysql/ 下载并安装 MySQL Community Server 安装程序。这很简单。但是，如果你需要安装帮助，请访问 http://dev.mysql.com/doc/refman/5.7/en/installing.html。
- 通过从 MySQL 安装的 bin 文件夹中的命令提示符启动 `mysqld` 来运行 MySQL 数据库系统。我们需要创建一个数据库和表来存储数据。最简单的方法是启动 MySQL Workbench 程序，建立连接，然后单击按钮 **Create a new schema in the connected server**，将其命名为 `jobsdb`，然后单击 **apply**。
- 通过双击它选择架构，然后单击右侧的按钮到之前的按钮 **Create a new table in the active schema**。将表命名为 `jobs`，并创建 `dbKey`、`type`、`salary`、`company`、`posted` 和 `open` 列；`dbKey` 是主键。要将驱动程序导入到你的应用程序，请将 `sqljocky` 添加到 `pubspec.yaml` 并保存它。然后会自动执行 `pub get` 命令。

## How to do it...

应用程序从 `using_mysql.dart` 开始，其中创建了 JobStore 对象，打开数据库，将记录写入 jobs 表，然后检索这些记录，如以下代码所示：

```
```dart
import 'job.dart';
import 'jobstore_mysql.dart';

Job job;
JobStore js;

void main() {
  js = new JobStore();
  // 1- create some jobs:
  job = new Job("Web Developer", 7500, "Google", new DateTime.now());
  js.jobs.add(job);
  job = new Job("Software Engineer", 5500, "Microsoft",
    new DateTime.now());
```
```

## Working with Databases

```
```java
js.jobs.add(job);
job = new Job("Tester", 4500, "Mozilla", new DateTime.now());
js.jobs.add(job);
// 2- opening the database:
js.open();
// 3- storing data in database:
js.storeData();
// 4- retrieving and displaying data from database:
js.readData();
}
```
```

运行上述代码后，我们可以在 MySQL Workbench 中的 `jobs` 表中验证插入是否成功：

![](img/52c05b53a56abd53584e4fed738ed509_790_0.png)

readData 方法从 **jobs** 表读取数据并将其打印到控制台：

**dbKey: 1, type : Web Developer, salary: 7500, company: Google, posted: 2014-07-02 14:34:12.000, open: 1**

**dbKey: 2, type : Software Engineer, salary: 5500, company: Microsoft, posted: 2014-07-02 14:34:12.000, open: 1**

**dbKey: 3, type : Tester, salary: 4500, company: Mozilla, posted: 2014-07-02 14:34:12.000, open: 1**

第一次运行后，readData 不会显示任何输出，因为它在数据库中的插入完成之前就被执行了；请通过第二次运行验证输出。

通过 jobstore_mysql.dart 中的 JobStore 类进行与 MySQL 的交互：

```
import 'package:sqljockey/sqljockey.dart';
import 'package:options_file/options_file.dart';
import 'job.dart';

ConnectionPool pool;

class JobStore {
  final List<Job> jobs = new List();
  Job job;

  // 5- opening a connection to the database:
  open() {
    pool = getPool(new OptionsFile('connection.options'));
  }

  ConnectionPool getPool(OptionsFile options) {
    String user = options.getString('user');
    String password = options.getString('password');
    int port = options.getInt('port', 3306);
    String db = options.getString('db');
    String host = options.getString('host', 'localhost');
    return new ConnectionPool(host: host, port: port, user: user, password: password, db: db);
  }

  storeData() {
    for (job in jobs) {
      insert(job);
    }
  }

  // 6- inserting a record in a table:
  insert(Job job) {
    var jobMap = job.toMap();
    pool.prepare('insert into jobs (dbKey, type, salary, company, posted, open) values (?, ?, ?, ?, ?, ?') .then((query) {
      var params = new List();
      params.add(job.dbKey);
```





```
pool.prepare('delete from jobs where dbKey = ?').then((query) {
  var params = new List();
  params.add(job.dbKey);
  return query.execute(params);
}).then((_) {
}).catchError(print);
}
```

## 工作原理...

数据库的连接信息存储在 `connection.options` 文件中。在第五个注释中，通过调用 `getPool` 打开到数据库的连接，它返回一个 `ConnectionPool` 对象，该对象维护一个数据库连接池。`getPool` 选项接受来自 `options_file` 包的 `OptionsFile` 对象作为参数，并读取 `connection.options` 中的信息：

```
# connection.options to define how to connect to a mysql db
user=root
password=?????  # substitute your password here
# port defaults to 3306
port=3306
db=jobsdb
# host defaults to localhost
host=localhost
```

所有与数据库的交互都通过查询进行。当执行查询时，如果 `ConnectionPool` 中有空闲连接，将使用该连接；否则，查询会排队，直到有空闲连接。正如我们在 `readData` 中看到的，select 查询通过 `pool.query(selectStr)` 进行。这会返回一个 `Future` 对象，因此结果的处理发生在 then 部分。可以使用以下代码完成：

```
results.forEach((row) {
  print('dbKey: ${row.dbKey}, type : ${row.type}, …');
```

前面的代码行也可以写成如下形式：

```
results.forEach((row) {
  print('dbKey: ${row[0]}, type : ${row[1]}, …');
```

## 数据库操作

插入、更新和删除查询（在同名方法中编码）必须首先经历准备阶段，然后执行阶段。让我们看几个示例。对于插入，该查询如下所示（参考第六个注释）：

```
pool.prepare('insert into jobs (dbKey, type, salary, company,\n  posted, open) values (?, ?, ?, ?, ?, ?)').then((query) {})
```

对包含自增字段的表执行插入查询时，将在 ${result.insertId} 中返回该字段的值。对于更新，该查询如下所示（参考第七个注释）：

```
pool.prepare('update jobs set type = ?, salary = ?, company = ?,\n  posted = ?, open = ? where dbKey = ?').then((query) {})
```

删除查询如下所示（参考注释 8）：

```
pool.prepare('delete from jobs where dbKey = ?').then((query) {})
```

? 字符表示要在 SQL 字符串中替换的值。这些值按指定顺序放在 params 列表中，该列表作为参数传递给 query.execute。具有多个参数集的查询可以使用 query.executeMulti() 执行。如果需要将多个查询作为一个整体执行（全部成功或全部失败），请使用 sqljockey 中的 Transaction 类，格式如下：

```
pool.startTransaction().then((trans) {\n  trans.query('...').then((result) {\n    trans.commit().then(() {...});\n  });\n});
```

## 更多内容...

完整的 API 文档可在 http://jamesots.github.io/sqljockey/docs/ 找到。

## 在 PostgreSQL 中存储数据

PostgreSQL 是另一个流行的开源 SQL 数据库。Dart 可以通过 Greg Lowe 提供的 pub 包 postgresql (https://github.com/xxgreg/postgresql) 与 PostgreSQL 通信。在本节中，我们将逐步演示如何使用该驱动。你可以在 using_postgresql 项目中看到它的实际应用。

## 准备工作

要获取数据库软件，请从 http://www.postgresql.org/download/ 下载并安装 PostgreSQL Server 安装程序，步骤如下：

- 1. 数据库服务器配置为自动启动。我们需要创建一个数据库和表来存储数据。最简单的方法是启动 pgAdmin 程序，新建一个连接，然后从菜单中选择 Edit、New Object 和 New Database，将其命名为 jobsdb，然后点击 OK。
- 2. 选择 public 模式，再次选择 Edit、New Object，然后选择 New Table。
- 3. 将该表命名为 jobs，并创建 dbKey、type、salary、company、posted 和 open 作为列；dbKey 是主键。
- 4. 要将驱动导入到应用程序中，请将 postgresql 添加到 pubspec.yaml 文件并保存。然后会自动执行 pub get 命令。

## 如何实现...

应用程序从 using_postgresql.dart 开始，在其中创建一个 JobStore 对象，打开数据库，将记录写入 jobs 表，然后检索这些记录：

```
import 'job.dart';
import 'jobstore_postgresql.dart';

Job job;
JobStore js;

void main() {
    js = new JobStore();
    // 1- create some jobs:
    job = new Job("Web Developer", 7500, "Google", new DateTime.now());
    js.jobs.add(job);
    job = new Job("Software Engineer", 5500, "Microsoft",
        new DateTime.now());
    js.jobs.add(job);
    job = new Job("Tester", 4500, "Mozilla", new DateTime.now());
    js.jobs.add(job);
    // 2- storing data in database:
    js.openAndStore();
    // 3- retrieving and displaying data from database:
    js.openAndRead();
}
```

## 数据库操作

运行上述代码后，我们可以通过 pgAdmin 在 jobs 表中验证插入是否成功，如下图所示：

![](img/52c05b53a56abd53584e4fed738ed509_796_0.png)

PostgreSQL 中的数据

readData 方法从 jobs 表中读取数据并将其打印到控制台：

1 - Web Developer - 7500 - Google - 2014-07-03 00:00:00.000 true

2 - Software Engineer - 5500 - Microsoft - 2014-07-03 00:00:00.000 true

3 - Tester - 4500 - Mozilla - 2014-07-03 00:00:00.000 true

通过驱动与 PostgreSQL 的交互发生在 jobstore_postgresql.dart 的 JobStore 类中：

```
library store;

import 'package:postgresql/postgresql.dart';
import 'job.dart';

class JobStore {
  final List<Job> jobs = new List();
  Job job;
  Connection conn;
  var uri = 'postgres://username:passwd@localhost:5432/jobsdb';

  // 5- opening a connection to the database:
  openAndStore() {
    connect(uri).then((_conn) {
      conn = _conn;
      storeData();
    })
    .catchError(print);
  }
```

```
storeData() {
    for (job in jobs) {
        insert(job);
    }
    // 6- close the database connection:
    close();
}

// 7- inserting a record in a table:
insert(Job job) {
    var jobMap = job.toMap();
    conn.execute('insert into jobs values (@dbKey, @type, @salary, @company, @posted, @open)',
        jobMap)
    .then((_) { print('inserted'); })
    .catchError(print);
}

openAndRead() {
    connect(uri).then((_conn) {
        conn = _conn;
        readData();
    })
    .catchError(print);
}

// 8- reading records from a table:
readData() {
    conn.query('select * from jobs').toList().then((results) {
        processJob(results);
        close();
    });
}

// 9- working with record data:
processJob(results) {
    for (var row in results) {
        // Refer to columns by nam:
        print('${row.dbKey} - ${row.type} - ${row.salary} - ${row.company} - ${row.posted} ${row.open}');
        // print(row[0]);    // Or by column index.
    }
}
```

结果的处理在回调处理器中完成：

```
for (var row in results) {
    print('${row.type} - ${row.salary}');
    print(row[1]);
```

字段可以通过名称或索引检索。与往常一样，我们使用 catchError 捕获错误，这至少会将错误打印到控制台。与 MySQL 类似，可以使用连接 Pool 对象来避免为每个请求获取连接的开销：

```
var pool = new Pool(uri, min: 2, max: 5);
pool.start().then((_) {
print('min amount connections established.');
pool.connect().then((conn) { // Obtain connection from pool
    // ... }
}
```

runInTransaction 连接方法允许需要以“全有或全无”方式执行的查询捆绑在一个事务中。

## 另请参阅

- 完整的 API 文档可在 http://www.dartdocs.org/documentation/postgresql/0.2.14/index.html#postgresql 找到。

## 在 Oracle 中存储数据

Oracle 是最流行的商业 SQL 数据库。Dart 可以通过 Alexander Aprelev 编写的 pub 包 oracledart (https://github.com/aam/oracledart) 与 Oracle 通信。这是用 C++ 编写的 Dart 原生扩展，使用 dart_api 接口集成到 Dart 中。它要求机器上存在 Oracle Instant Client，并且其 OCCI 二进制文件必须包含在 PATH 变量中。

在本节中，我们将逐步演示如何使用该驱动。你可以在 using_oracle 项目中看到它的实际应用。





## 准备工作

要获取数据库软件，请从 http://www.oracle.com/technetwork/database/enterprise-edition/downloads/index-092322.html 下载并执行 Oracle 安装程序。

- 数据库服务器通过菜单选项 **Start Database** 启动。
- 我们需要创建一个表来存储数据。启动 SQL 命令行终端并粘贴脚本 `jobs.txt` 的内容。这将创建表 `jobs`，以及 `dbKey`、`type`、`salary`、`company`、`posted` 和 `open` 列；`dbKey` 是主键。
- 要将驱动程序导入到您的应用程序中，将 `oracledart` 添加到 `pubspec.yaml` 并保存它。然后会自动执行 `pub get` 命令。

## 如何做...

应用程序从 `using_oracle.dart` 开始，其中创建了一个 `JobStore` 对象，打开数据库，将记录写入 `jobs` 表，然后检索这些记录。`using_oracle.dart` 中的代码与 `using_postgresql.dart` 中的代码相同，只是我们现在导入 `jobstore_oracle.dart`；因此请参阅之前的配方。`jobstore_oracle.dart` 文件包含与数据库驱动程序通信的代码：

// 1- importing the driver:
import 'package:oracledart/oracledart.dart';
import 'job.dart';

class JobStore {
  final List<Job> jobs = new List();
  Job job;
  OracleConnection conn; // connection object
  OracleResultset resultset;
  OracleStatement stmt;
  var connStr = ' "(DESCRIPTION=" "(ADDRESS=(PROTOCOL=TCP)
    (HOST=oracledb) (PORT=1521))"'
    '"(CONNECT_DATA=(SERVICE_NAME=XE) (SERVER=DEDICATED)))"';

  // 2- opening a connection to the database:
  openAndStore() {
    connect("SYS", "avalon", connStr).then((oracleconnection) {
      conn = oracleconnection;
      print('connected with Oracle!');
      storeData();
```
}).catchError(print);
}

storeData() {
for (job in jobs) {
insert(job);
}
}

// 3- inserting a record in a table:
insert(Job job) {
var jobMap = job.toMap();
var insertSql = 'insert into jobs values (:1, :2, :3, :4, :5, :6)';
stmt = conn.createStatement(insertSql);
stmt.setInt(1, jobMap['dbKey']);
stmt.setString(2, jobMap['type']);
stmt.setInt(3, jobMap['salary']);
stmt.setString(4, jobMap['company']);
stmt.setString(5, jobMap['posted']);
stmt.setString(6, jobMap['open']);
stmt.executeQuery();
}

openAndRead() {
connect("SYS", "avalon", connStr).then((oracleconnection) {
conn = oracleconnection;
readData();
}).catchError(print);
}

// 8- reading records from a table:
readData() {
resultset = conn.select("select * from jobs");
processJob(resultset);
}

// 9- working with record data:
processJob(results) {
while (resultset.next()) {
print('dbKey: ${resultset.getInt(0)}');
print('type:  ${resultset.getString(1)}');
print('salary: ${resultset.getInt(2)}');
print('company:  ${resultset.getString(3)}');
```
```
print('posted: ${resultset.getString(4)}');
print('open: ${resultset.getString(5)}');
}
}
}
```

## 它是如何工作的...

首先，导入 `oracledart` 包。`connect` 方法接收用户、他的/她的密码，然后是一个连接字符串。在其回调中，`OracleConnection` 对象 `conn` 可用。对表的查询使用 `conn.select(selectSql)` 完成，其中 `selectSql` 是所选的 SQL 字符串。这返回一个 `OracleResultset` 对象，您可以使用 `next()` 在其上进行迭代。字段的值只能通过索引使用 `getInt` 或 `getString` 提取。

如果您的语句需要参数，就像插入、更新或删除语句很可能需要的那样，您首先需要调用 `conn.createStatement(sqlStr)`，其中 `sqlStr` 包含 `:i` 指示符（i 等于从 0 开始的第 i 个索引）。这些 `:i` 占位符必须用 `stmt.setInt(i, value)` 或 `stmt.setString(i, value)` 填充。然后，可以使用 `stmt.executeQuery()` 执行语句。

## 还有更多...

在本配方和之前的配方中，我们讨论了 Dart 可以使用专用驱动程序与之通信的 SQL 数据库服务器。然而，例如，对于流行的 Microsoft SQL Server 尚无驱动程序。在这种情况下，我们可以使用 Juan Mellado 的 pub `odbc` 包（https://code.google.com/p/dart-odbc/）。ODBC 绑定是通过 Dart 原生扩展实现的，这使得使用起来稍微复杂一些，目前它仅存在 32 位版本。

## 在 MongoDB 中存储数据

MongoDB，由同名公司（http://www.mongodb.org/）提供，是 NoSQL 数据库中最受欢迎的数据库。让我们看一下有关 MongoDB 的一些事实：

- MongoDB 是一个开源的、分布式的、面向文档的数据库；每个数据记录实际上是一个文档。
- 在 MongoDB 中，表称为集合。文档以称为 BSON 的类似 JSON 的格式存储。
- Dart 到 MongoDB 的最先进驱动程序是 Vadim Tsushko、Ted Sander 和 Paul Evans 的 pub 包 `mongo_dart`。本配方将向您展示如何从 Dart 应用程序在 MongoDB 数据库中创建、读取、更新和删除操作。您可以在 `using_mongodb` 项目中看到它的实际效果。

## 准备工作

从 `http://www.mongodb.org/downloads` 为您的系统安装最新的生产版本。这很简单。但是，如果您需要更多详细信息，请参阅 `http://docs.mongodb.org/manual/installation/`。在 Dart 应用程序之前启动 mongod 服务器进程（例如，在 Windows 上从 `c:\mongodb\bin`）。要创建 jobsdb 数据库，启动一个 mongo shell 并输入 `use jobsdb;` 就这么简单。或者，这也可以通过 `mongo_dart` 完成。NoSQL 数据库是无模式的，因此集合（这里称为表）在插入第一个文档（或记录）时创建。

## 如何做...

应用程序从 `using_mongodb.dart` 开始，其中创建了一个 JobStore 对象，打开数据库，将记录写入 jobs 表，然后检索这些记录。`using_mongodb.dart` 中的代码与 `using_postgresql.dart` 中的代码相同，只是我们现在导入 `jobstore_mongodb.dart`；因此请参阅 *Storing data in PostgreSQL* 配方。运行此脚本时，首先注释掉 `js.openAndRead();`。第二次运行时，取消注释此项并注释掉 `js.openAndStore();`；否则，读取会在插入完成之前进行。`jobstore_mongodb.dart` 代码包含与数据库驱动程序通信的代码：

```
import 'package:mongo_dart/mongo_dart.dart';
import 'job.dart';

const String DEFAULT_URI = 'mongodb://127.0.0.1/';
const String DB_NAME = 'jobsdb';
const String COLLECTION_NAME = 'jobs';

class JobStore {
  final List<Job> jobs = new List();
  Job job;
  Db db;
  DbCollection jobsColl;

  JobStore() {
    // 1- make a new database
    db = new Db('${DEFAULT_URI}${DB_NAME}');
    // make a new collection
    jobsColl = db.collection(COLLECTION_NAME);
  }

  openAndStore() {
    db.open().then((_) {
```

```
jobsColl.update({"dbKey": jobMap["dbKey"]},jobMap).then((_ ) {
  print('job updated');
}).catchError(print);
}

// 6- deleting a document:
delete(Job job) {
  var jobMap = job.toMap();
  jobsColl.remove(jobMap).then((_ ) {
    print('job updated');
  }).catchError(print);
}
```

运行上述脚本后，在 mongo shell 中使用 use jobsdb 和 db.jobs.find() 验证文档是否已插入，如以下屏幕截图所示：

![](img/52c05b53a56abd53584e4fed738ed509_805_0.png)

## 它是如何工作的...

mongo_dart 库使使用 MongoDB 变得非常容易。在注释 1 中，我们在 JobStore 的构造函数中创建了 Db 和 DbCollection 对象。要操作文档，您必须首先在 Db 对象上调用 open 方法。

要将文档写入集合，请使用 DbCollection 对象的 insert 方法，但其参数必须采用 Map 格式。读取文档使用 find 方法完成。返回的列表包含类型为 Map 的项，因此为了构造真正的对象，我们必须在 Job 类中创建一个新的命名构造函数 fromMap。更新文档使用同名的方法，但其第一个参数必须是选择器以找到必须更新的文档。删除文档使用 remove 方法。





## 另请参阅

- 完整的 API 文档可以在 `http://www.dartdocs.org/documentation/mongo_dart/0.1.39/index.html#mongo_dart` 找到
- Vadim Tsushko 编写的另一个 MongoDB 客户端是一个名为 Objectory 的对象-文档映射工具，它既可以在客户端使用，也可以在服务器端使用（`https://github.com/vadimtsushko/objectory`）

## 在 RethinkDB 中存储数据

RethinkDB（`http://www.rethinkdb.com/`）是一个简单的 NoSQL 数据库，用于存储 JSON 文档。它的主要优势在于易用性：对开发人员而言，它提供了一种直观的查询语言，可以模拟表连接；对管理员而言，它提供了友好的 Web 工具来监控、分片和复制。另一个优点是查询自动并行化。目前，该数据库系统支持 OS X 和多种 Linux 发行版。

我们将通过 pub 包中名为 `rethinkdb_driver` 的驱动程序来访问 RethinkDB，该驱动程序由 William Welch 开发（`https://github.com/billysomtimes/rethinkdb`）。你可以在 `using_rethinkdb` 项目中看到它的实际应用。

## 准备工作

从 `http://www.rethinkdb.com/docs/install/` 为你的系统安装最新的生产版本。然后，执行以下步骤：

- 1. 在终端中执行 `rethinkdb` 命令启动 RethinkDB 服务器。
- 2. 然后在浏览器中访问 `localhost:8080` —— 这将启动一个管理界面，你可以在其中控制数据库服务器（从单台机器到集群）。
- 3. 点击顶部的 **Tables** 选项卡，使用 **Add Database** 按钮创建数据库 `jobsdb`。
- 4. 要创建 `jobs` 表，请点击页面顶部的 **Tables** 选项卡，然后使用 **Add Table** 按钮。
- 5. `jobs` 表包含 `dbKey`、`type`、`salary`、`company`、`posted` 和 `open` 列；`dbKey` 是主键。

## 操作步骤...

应用程序从 `using_rethinkdb` 开始，在该项目中会创建一个 JobStore 对象，打开数据库，将记录写入 `jobs` 表，然后检索这些记录。`using_rethinkdb` 中的代码与 `using_postgresql.dart` 中的代码相同，只是我们现在导入的是 `jobstore_rethinkdb.dart`，因此请参考“Storing data in PostgreSQL”菜谱。`jobstore_rethinkdb.dart` 文件包含以下与数据库驱动程序通信的代码：

```dart
import 'package:rethinkdb_driver/rethinkdb_driver.dart';
import 'job.dart';

class JobStore {
  final List<Job> jobs = new List();
  Job job;
  Rethinkdb rdb = new Rethinkdb();
  Connection conn;

  // opening a connection to the database:
  openAndStore() {
    rdb.connect(db: "jobsdb", port:8000, host: "127.0.0.1").
      then((_conn) {
        conn = _conn;
        storeData(conn);
      }).catchError(print);
  }

  storeData(conn) {
    List jobsMap = new List();
    for (job in jobs) {
      var jobMap = job.toMap();
      jobsMap.add(jobMap);
    }
    // storing data:
    rdb.table("jobs").insert(jobsMap).run(conn)
      .then((response) => print('documents inserted'))
      .catchError(print);
    // close the database connection:
    close();
  }

  openAndRead() {
    rdb.connect(db: "jobsdb", port:8000, host: "127.0.0.1").then((_conn) {
      conn = _conn;
```

```dart
readData(conn);
}).catchError(print);
}

// reading documents:
readData(conn) {
  rdb.table("jobs").getAll("1,2,3").run(conn).then((results) {
    processJob(results);
    close();
  });
}

// working with document data:
processJob(results) {
  for (var row in results) {
    // Refer to columns by name:
    print('${row.dbKey} - ${row.type} - ${row.salary} - ${row.company} - ${row.posted} ${row.open}');
  }
}

close() {
  conn.close();
}

// updating a document:
update(Job job) {
  rdb.table("jobs").update({"dbKey":job.dbKey})
    .run(conn).then((response)=>print('document updated')
  ).catchError(print);
}

// deleting a document:
delete(Job job) {
  rdb.table("jobs").get(job.dbKey).delete()
    .run(conn).then((response)=>print('document deleted')
  ).catchError(print);
}
```

## 工作原理...

和往常一样，我们必须导入驱动程序的代码。然后，我们定义一个 Rethinkdb 对象，所有方法都在该对象上定义，以及一个全局的 Connection 对象。使用以下命令打开连接，其中 host 也可以是服务器的真实名称。connect 选项还可以接收一个可选的授权密钥参数 authKey。在回调处理器中，所有文档操作都可以使用以下代码完成：

```dart
rdb.connect(db: "jobsdb", port:8000, host: "127.0.0.1")
```

例如，要读取文档，使用 getAll 命令，它接受文档键列表作为参数：

```dart
rdb.table("jobs").getAll(keysList).run(conn).then((results) {...}
```

要插入文档，使用 insert 命令：

```dart
rdb.table("jobs").insert(docMap).run(conn).then((results) {...}
```

insert 命令接受一个列表作为参数，其中包含文档，每个文档采用映射格式。update 命令将文档与其参数进行匹配，delete 命令需要文档键作为参数。

## 另请参阅

当然，还有许多其他 NoSQL 数据库。以下是一些其他驱动程序的参考：

- 对于 CouchDB，有 wilt 包（https://pub.dartlang.org/packages/wilt）和 couchclient 库（https://pub.dartlang.org/packages/couchclient）
- 对于 Redis，有 redis_client 包（https://pub.dartlang.org/packages/redis_client）
- 对于 Memcached，有 memcached_client 库（https://pub.dartlang.org/packages/memcached_client）
- 对于 Riak，有 riak_client 包（https://pub.dartlang.org/packages/riak_client）
