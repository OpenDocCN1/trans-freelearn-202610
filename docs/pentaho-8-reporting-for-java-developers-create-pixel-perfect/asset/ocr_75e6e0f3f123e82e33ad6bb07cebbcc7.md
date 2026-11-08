# StaticConnectionProvider

The first implementation of `ConnectionProvider` is the `StaticConnectionProvider` class, which takes in an existing `java.sql.Connection` object as part of its constructor:

```
ConnectionProvider conn = new StaticConnectionProvider(sqlConn);
```