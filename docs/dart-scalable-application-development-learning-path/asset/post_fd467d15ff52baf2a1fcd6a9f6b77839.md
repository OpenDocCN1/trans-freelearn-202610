
A common scenario is that all the records from a certain store are read into the app to show them after opening the database. This is done in _loadDB. If we look at the other methods (add, update, remove, and clear), we will see a common pattern, which all database operations must perform within a Transaction object. This retrieves as parameters the object store name JOB_STORE and a certain mode such as readonly or readwrite. Reading a number of records, such as in _loadDB, works through the opening cursor and then listening to it (using listen). Each listen event reads a new record through cursor.value, which is transformed into a Job object and added to the list.

An object to be stored must be given in the map format to the store's add method. When autoincrement is set to true for this store, the generated key number is returned as addedKey. When the transaction is complete (trans.completed.then), we add the newly created job to our list. The same pattern is followed in the update, remove, and clear methods, which call the methods put, delete, and clear, respectively, in the object store.

## See also

- Refer to the Using Browser Local Storage recipe in Chapter 5, Handling Web Applications, if you want to compare local storage with IndexedDB

## Using Lawndart to write offline web apps

What if the main requirement is that your app can work detached, providing universal offline key-value storage, whether IndexedDB is supported by your client's browsers or not? Then, Lawndart comes to the rescue.

Lawndart (https://github.com/sethladd/lawndart, but available via pub package) is not a new database, but rather a manager, which automatically chooses the best local storage mechanism available on the client. It was developed by Seth Ladd as a Dart rework of Lawnchair (http://brian.io/lawnchair/). You can see it in action in the project using_lawndart.

## How to do it...

Import the Lawndart package by adding lawndart:any to your pubspec.yaml file. The following is the relevant code from the startup script using_lawndart.dart:

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

The database specific code is isolated in jobstore_lawndart.dart:

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

Lawndart presents an asynchronous, but consistent, interface to the local storage, IndexedDB, and Web SQL. Your app simply works with an instance of the class Store, and the factory constructor will try IndexedDB, Web SQL, and finally, local storage. This is shown in the third comment, where an object of class Store is made; the object also constructs the local database needed. In the first comment, the open method in the JobStore object triggers open method on the Store object (fourth comment). The _loadDB option then reads the entire store; this is accomplished through the all method, which returns Stream of values. Adding and updating data is done by calling the save method on the store object (fifth 5). Deleting a record is done through removeByKey; clearing an entire database needs the nuke method. Notice that the code that uses Lawndart is much cleaner than IndexedDB, as shown in the previous recipe.

## See also

- Cargo is another pub package developed by Joris Hermans, which accomplishes a similar goal. It is a storage package that abstracts local storage, storage on the client as well as on the server, and stores JSON files on the disk. For more information, refer to https://pub.dartlang.org/packages/cargo.
- The complete API documents can be found at http://www.dartdocs.org/documentation/lawndart/.

## Storing data in MySQL

MySQL is undeniably the most popular open source SQL database. Dart can talk to MySQL using the pub package `sqljocky` by James Ots (https://github.com/jamesots/sqljocky). In this section, we will demonstrate how to use this driver step by step. You can see it in action in the `using_mysql` project.

### Getting ready

- To get the database software, download and install the MySQL Community Server installer from http://dev.mysql.com/downloads/mysql/. This is straightforward. However, if you need any help with the installation, visit http://dev.mysql.com/doc/refman/5.7/en/installing.html.
- Run the MySQL database system by starting `mysqld` on a command prompt from the bin folder of the MySQL installation. We need to create a database and table to store data. The easiest way is to start the MySQL Workbench program, make a connection, and then click on the button **Create a new schema in the connected server**, name it `jobsdb`, and click on **apply**.
- Select the schema by double-clicking on it, and then clicking on the button on the right to the previous button **Create a new table in the active schema**. Name the table `jobs`, and create the `dbKey`, `type`, `salary`, `company`, `posted` and `open` columns; `dbKey` is the primary key. To import the driver to your application, add `sqljocky` to `pubspec.yaml` and save it. A `pub get` command is then done automatically.

## How to do it...

The application starts with `using_mysql.dart`, where a JobStore object is created, the database is opened, records are written to the jobs table, and then these records are retrieved, as shown in the following code:

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

After running the preceding code, we can verify that the insertions succeeded in MySQL Workbench in the `jobs` table:

![](img/52c05b53a56abd53584e4fed738ed509_790_0.png)

The readData method reads the data from the **jobs** table and prints it to the console:

**dbKey: 1, type : Web Developer, salary: 7500, company: Google, posted: 2014-07-02 14:34:12.000, open: 1**

**dbKey: 2, type : Software Engineer, salary: 5500, company: Microsoft, posted: 2014-07-02 14:34:12.000, open: 1**

**dbKey: 3, type : Tester, salary: 4500, company: Mozilla, posted: 2014-07-02 14:34:12.000, open: 1**

After the first run, readData will not show any output because it gets executed before the insertions in the database are complete; verify the output through a second run.

The interaction with MySQL through sqljockey takes place in the JobStore class in jobstore_mysql.dart:

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
