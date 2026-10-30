Vendor-specific interfaces provide additional methods in the JDBC API. For example, create DB2Statement object using the wrapper pattern to use the setDB2ClientProgramId() method, which sets the program Id from a connection in the DB2Statement interface. Test if the Statement object is a wrapper for the DB2Statement interface using the isWrapperFor() method. If the Statement object is a wrapper, unwrap the DB2Statement interface using the unwrap() method to create a DB2Statement object. Also, invoke the setDB2ClientProgramId() method of the DB2Statement object:

```
Class class = Class.forName("com.ibm.db2.jcc.DB2Statement");
if(stmt.isWrapperFor(class))
{
com.ibm.db2.jcc.DB2Statement db2Stmt = (com.ibm.db2.jcc.
DB2Statement) stmt.unwrap(class);
db2Stmt.setDB2ClientProgramId("WebSphereprogram");
}
```

JDBC 4.0 has introduced support for the SQL ROWID data type. ROWID type column values can be retrieved using the getRowId() method. ROWID identifies a row uniquely in a database table and is fastest method to access a row. As the DB2 database supports the ROWID data type, add a column for ROWID in the SELECT query:

```
ResultSet resultSet=db2Stmt.executeQuery("Select ROWID, CATALOGID,
JOURNAL, PUBLISHER, EDITION, TITLE, AUTHOR from Catalog");
```

Retrieve ROWID column values using the getRowId() method of the ResultSet interface:

```
<td><%=out.println(resultSet.getRowId("ROWID").toString());%></td>
```

Prior to JDK 5.0, collections had to be iterated using the iterators and the index variables. JDK 5.0 introduced an enhanced FOR loop using which collections can be iterated without iterators and index variables. For example, if a java.util. Collection of String objects is to be iterated, then the String objects in the Collection are to be retrieved. An Iterator would be created using for loop. The String objects in the Collection are retrieved using the next() method:

```
for(Iterator i = c.iterator(); i.hasNext(); )
{
String str = (String) i.next();
}
```

The String objects in a Collection of Strings are retrieved using the enhanced for loop, introduced in JDK 5.0:

```
for(String string : c)
{
String str =string
}
```