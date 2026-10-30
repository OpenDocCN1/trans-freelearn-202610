supported by a JDBC driver can be obtained using the `getClientInfoProperties()` method. When one or more connections bog down the application, the `getClientInfo()` method can be used to identify which connections could be causing the reduction in performance. Some standard client info properties that a JDBC driver can support are discussed in the following table:

| Client Info Property | Description |
|---|---|
| ApplicationName. | The name of the application that is using the connection. |
| ClientUser. | The name of the user. |
| ClientHostname. | The hostname on which the application is running. |

Similar to connection pooling, JDBC 4.0 provides `Statement` pooling to reduce the overheads of opening, initiating, and closing `Statement` objects. Frequently used `Statement` objects can be pooled using the `setPoolable(boolean poolable)` method. The `isPoolable()` method is used to check if a `Statement` object is poolable. The `Statement` object, `stmt` can be pooled, if poolable:

```
if(stmt.isPoolable())
stmt.setPoolable(true);
```

# Scalar Functions

Most databases support numeric, string, time, date, system, and conversion functions on the scalar values. SQL statements run using the `Statement` object, and can include the scalar functions using the JDBC escape syntax. JDBC 4.0 provides some new scalar function, which are discussed in the following table:

| Scalar Function | Description |
|---|---|
| CHAR_LENGTH | Returns the length of a string expression. |
| CHARACTER_LENGTH | |
| CURRENT_DATE | Returns the current date. |
| CURRENT_TIME | Returns the current time. |
| CURRENT_TIMESTAMP | Returns the current timestamp. |
| EXTRACT | Extracts a field from a datetime value. |
| OCTET_LENGTH | Returns the length of a string expression in octets (bytes). |