Create an object of type, java.sql.PreparedStatement and check if the object is a wrapper for the Oracle JDBC specific interface, oracle.jdbc.OraclePreparedStatement using the isWrapperFor() method. If the object is a wrapper for the interface, create an instance of the oracle.jdbc.OraclePreparedStatement JDBC interface using the unwrap() method:

```
String url="jdbc:oracle:thin:@localhost:1521:ORCL";
Connection connection = DriverManager.getConnection(url,"oe", "pw");
String sql="INSERT INTO CATALOG(catalogId, journal) VALUES(?,?)"
java.sql.PreparedStatement stmt = connection.prepareStatement(sql);
Class class = Class.forName("oracle.jdbc.OraclePreparedStatement");
if(stmt.isWrapperFor(class))
{
OraclePreparedStatement ops = (OraclePreparedStatement)stmt.
unwrap(class);
ops.defineColumnType(2, oracle.jdbc.OracleTypes.VARCHAR,4000);
}
```

# Enhancements in SQLException

An error in interaction with the datasource is represented with the SQLException class. JDBC 4.0 has enhanced support for navigation of chained SQLExceptions with the iterator() method in the SQLException class. A chained SQLException is an Exception that is linked with other Exceptions. The iterator() method iterates over the chained exceptions and the chained causes. Chained exceptions can be retrieved and iterated over (without having to invoke the getNextException() and getCause() methods recursively) using the enhanced For-Each loop introduced in J2SE 5. When an SQLException is generated using the For-Each loop, the chained exceptions can be output as shown below:

```
catch(SQLException sqlException)
{
for(Throwable e : sqlException )
{
System.out.println("Error encountered: " + e);
}
}
```

In JDBC 4.0, four constructors have been added to the SQLException class with the Throwable cause as one of the parameters. The getCause() method can return non-SQLExceptions. In JDBC 4.0, three new categories of SQLExceptions have been added, which are as follows:

+   - SQLTransientException
+   - SQLNonTransientException
+   - SQLRecoverableException