# Getting ready

To get the database software, download and execute the Oracle installer from http://www.oracle.com/technetwork/database/enterprise-edition/downloads/index-092322.html.

+   - The database server is started through the menu option **Start Database**.
+   - We need to create a table to store data. Start the SQL command-line terminal and paste the contents of the script `jobs.txt`. This creates the table `jobs`, together with the `dbKey`, `type`, `salary`, `company`, `posted`, and `open` columns; `dbKey` is the primary key.
+   - To import the driver to your application, add `oracledart` to `pubspec.yaml` and save it. A `pub get` command is then done automatically.

# How to do it...

The application starts from `using_oracle.dart`, where a `JobStore` object is created, the database is opened, records are written to the `jobs` table, and then these records are retrieved. The code from `using_oracle.dart` is identical to the code from `using_postgresql.dart`, except that we now import `jobstore_oracle.dart`; so please refer to the previous recipe. The `jobstore_oracle.dart` file contains the code to talk to the database driver:

```
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