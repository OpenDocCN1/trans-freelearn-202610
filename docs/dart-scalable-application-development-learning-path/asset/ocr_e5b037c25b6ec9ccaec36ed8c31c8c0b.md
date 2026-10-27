```
}

Future<String> getItem(key) {
  var sql = 'SELECT value FROM $PRODUCT WHERE id = ?';
  var completer = new Completer();
  _getDatabase(SHOPPING, PRODUCT).then((SqlDatabase database) {
    database.readTransaction((SqlTransaction tx) {
      tx.executeSql(sql, [key],
        (SqlTransaction txn, SqlResultSet result) {
          if (result.rows.isEmpty) {
            return completer.complete(null);
          } else {
            Map row = result.rows.first;
            return completer.complete(row['value']);
          }
        }, (SqlTransaction transaction, SqlError error) {
          completer.completeError(error);
        });
      });
    });
  });
  return completer.future;
}

```

For the create and update operations, we use the following transaction and specify the special SQL statement, and we need to specify the SQL parameters key and value as well:

```
Future setItem(key, value) {
  var sql = 'INSERT OR REPLACE INTO $PRODUCT (id, value) ' +
      'VALUES (?, ?)';
  var completer = new Completer();
  _getDatabase(SHOPPING, PRODUCT).then((SqlDatabase database) {
    database.transaction((SqlTransaction tx) {
      tx.executeSql(sql, [key, value],
        (SqlTransaction txn, SqlResultSet result) {
          return completer.complete(value);
        }, (SqlTransaction transaction, SqlError error) {
          completer.completeError(error);
        });
      }, (error) => completer.completeError(error));
    });
  return completer.future;
}

```