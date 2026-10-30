```
connection.setClientInfo("ApplicationName","WebSphereApp");
connection.setClientInfo("ClientUser","WebSphereUser");
connection.setClientInfo("ClientHostname","WebSphereHost");
```

PreparedStatements are pooled by default and provide recycling of PreparedStatement objects instead of creating new PreparedStatement objects. PreparedStatement objects that are frequently used do not have to be opened, initialized, and closed for each client request. JDBC 4.0 has also introduced Statement pooling in which frequently used Statement objects are not closed, but are returned to a statement pool from which other clients can use these objects. To add statement pooling to a connection pool: First, test if the database supports statement pooling using the supportsStatementPooling() method of the DatabaseMetaData interface. Second, test if the Statement object is poolable using the isPoolable() method. If it is poolable, the Statement object can be pooled using the setPoolable(true) method:

```
java.sql.Statement stmt = conn.createStatement();
DatabaseMetaData metaData=connection.getMetaData();
if(metaData.supportsStatementPooling())
{
if(stmt.isPoolable())
stmt.setPoolable(true);
}
```

JDBC 4.0 has introduced support for the wrapper pattern using which non-standard vendor-specific JDBC resources can be accessed. The wrapper pattern is implemented in the Wrapper interface. JDBC interfaces that are to be made available as extensions to the JDBC API should extend the Wrapper interface. In the IBM Data Server Driver for JDBC and SQLJ, the Wrapper interface is extended by the following interfaces in the com.ibm.db2.jcc package:

+   - DB2Connection.
+   - DB2BaseDataSource.
+   - DB2SimpleDataSource.
+   - DB2Statement.
+   - DB2ResultSet.
+   - DB2DatabaseMetaData.