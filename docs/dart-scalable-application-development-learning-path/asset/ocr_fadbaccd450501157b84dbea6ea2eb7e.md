```
pool.prepare('delete from jobs where dbKey = ?').then((query) {
  var params = new List();
  params.add(job.dbKey);
  return query.execute(params);
}).then((_) {
}).catchError(print);
}
```

# How it works...

The connection information for the database is stored in the `connection.options` file. In the fifth comment, a connection to the database is opened by calling `getPool`, which returns a `ConnectionPool` object that maintains a pool of database connections. The `getPool` option takes as argument an `OptionsFile` object, from the `options_file` package, and reads the information from `connection.options`:

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

All interactions with the database work via queries. If there is a free connection in `ConnectionPool` when queries are executed, it will be used; otherwise, the query is queued until there is a free connection. As we can see in `readData`, a select query is made via `pool.query(selectStr)`. This returns a `Future` object, so the processing of the results takes place in the then section. This can be done using the following code:

```
results.forEach((row) {
  print('dbKey: ${row.dbKey}, type : ${row.type}, …');
```

The preceding line can also be written as follows:

```
results.forEach((row) {
  print('dbKey: ${row[0]}, type : ${row[1]}, …');
```