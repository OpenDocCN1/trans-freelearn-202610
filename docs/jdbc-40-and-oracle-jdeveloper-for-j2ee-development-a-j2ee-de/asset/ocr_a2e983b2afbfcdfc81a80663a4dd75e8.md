The `setQueryTimeout (int)` method specifies the timeout, in seconds, for a `Statement` object to execute. The `executeQuery(String sql)` executes an SQL query and returns a single `ResultSet` object. The `executeUpdate(String sql)` method executes an SQL statement, which is either a **DML** (INSERT, UPDATE, or DELETE) statement or a **DDL** statement. If the SQL string is a DML statement, the `executeUpdate(String)` method returns the number of rows modified. If the SQL string is a DDL statement, the method returns the value, "0". SQL statements can also be run in a batch with the `executeBatch()` method. Add SQL commands to run a batch with the `addBatch(String sql)` method:

```
```
stmt.addBatch("SQL command");
stmt.executeBatch();
```
```

The `executeBatch()` method returns an `int []` value of update counts. The batch SQL commands can be cleared with the `clearBatch()` method. If a `Statement` object is not being used, it is closed automatically. It is recommended to close the `Statement` object with the `close()` method:

```
```
stmt.close();
```
```

When a `Statement` object is closed, the database and the JDBC resources associated with that object are also closed. Further, the `ResultSet` object associated with the `Statement` object is also closed.

In JDBC 4.0, the new methods discussed in following table have been added to the Statement interface:

| Method | Description |
|---|---|
| `isClosed()` | Tests, if the `Statement` object has been closed. |
| `isPoolable()` | Tests, if the `Statement` object is poolable. |
| `setPoolable()` | Sets the `Statement` object as poolable. By default, a `Statement` object is not set to poolable. The method is only a hint to the statement pooling implementation. Statement pooling provides a better management for statement pooling resources. |