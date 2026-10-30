
The data for the stylesheet is retrieved from an Oracle database table. Obtain a JDBC connection from the database. First, create an InitialContext object. Subsequently, create a DataSource object using the lookup() method of the InitialContext class to lookup the data source JNDI. Obtain a connection from the DataSource object using the getConnection() method:

```
InitialContext initialContext = new InitialContext();
javax.sql.DataSource ds = (javax.sql.DataSource)
initialContext.lookup("java:comp/env/jdbc/ApachePOIConnectionDS");
java.sql.Connection conn = ds.getConnection();
```

Create a Statement object using the createStatement() method of the Connection interface. Run an SQL query on the Catalog table using the executeQuery() method of the Statement interface to generate a ResultSet object.

```
Statement stmt=conn.createStatement();
ResultSet resultSet=stmt.executeQuery("Select * from Catalog");
```

Create a header row for the Excel spreadsheet using the createRow() method of the HSSFRow class. The rows in an Excel spreadsheet are '0' based.

```
HSSFRow row=sheet1.createRow(0);
```

Set the header row cell values corresponding to the table columns. Create a cell with the createCell() method of the HSSFRow class and set the cell value using the setCellValue() method. For example, the value for the first cell in the row is set to CatalogId as follows:

```
row.createCell((short)0).setCellValue("CatalogId");
```

To add rows to the spreadsheet, iterate over the result set and add a row for each of the table rows. Retrieve the column values from the ResultSet and set the values in the row cells:

```
for (int i=1;resultSet.next(); i++){
    row=sheet1.createRow(i);
    row.createCell((short)0).setCellValue(resultSet.getString(1));
    row.createCell((short)1).setCellValue(resultSet.getString(2));
    row.createCell((short)2).setCellValue(resultSet.getString(3));
    row.createCell((short)3).setCellValue(resultSet.getString(4));
    row.createCell((short)4).setCellValue(resultSet.getString(5));
}
```

Create a directory C:/excel. Create a FileOutputStream to output the Excel spreadsheet to an XLS file. An XLS file represents an excel spreadsheet.

```
FileOutputStream output=new FileOutputStream(new File("c:/excel/catalog.xls"));
```

Output the Excel spreadsheet to a XLS file using the write() method of the HSSFWorkbook class.

```
wb.write(output);
```

The catalog.jsp used to generate an Excel spreadsheet is listed below.

```jsp
<%@ page contentType="text/html;charset=windows-1252"%>
<%@ page import="org.apache.poi.hssf.usermodel.*, java.sql.*, java.io.*,javax.naming.InitialContext"%>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=windows-1252">
<title>untitled</title>
</head>
<body><%HSSFWorkbook wb=new HSSFWorkbook();
HSSFSheet sheet1=wb.createSheet("sheet1");
InitialContext initialContext = new InitialContext();
javax.sql.DataSource ds = (javax.sql.DataSource)
initialContext.lookup("java:comp/env/jdbc/ApachePOIConnectionDS");
java.sql.Connection conn = ds.getConnection();
Statement stmt=conn.createStatement();
ResultSet resultSet=stmt.executeQuery("Select * from Catalog");
HSSFRow row=sheet1.createRow(0);
row.createCell((short)0).setCellValue("CatalogId");
row.createCell((short)1).setCellValue("Journal");
row.createCell((short)2).setCellValue("Publisher");
row.createCell((short)3).setCellValue("Edition");
row.createCell((short)4).setCellValue("Title");
row.createCell((short)5).setCellValue("Author");
for (int i=1;resultSet.next(); i++)
{
row=sheet1.createRow(i);
row.createCell((short)0).setCellValue(resultSet.getString(1));
row.createCell((short)1).setCellValue(resultSet.getString(2));
row.createCell((short)2).setCellValue(resultSet.getString(3));
row.createCell((short)3).setCellValue(resultSet.getString(4));
row.createCell((short)4).setCellValue(resultSet.getString(5));
row.createCell((short)5).setCellValue(resultSet.getString(6));
}
FileOutputStream output=new FileOutputStream(new File("c:/excel/catalog.xls"));
wb.write(output);
output.close();
resultSet.close();
stmt.close();
conn.close();
%>
</body>
</html>
```

To run the catalog.jsp JSP in the JDeveloper embedded OC4J server, right-click on the JSP node in the Applications-Navigator and select Run.

![](img/553e44bc30e70d0d8e58278a7ece8da8_373_0.png)

## Creating a Spreadsheet with Apache POI

An Excel spreadsheet which may be opened in MS Excel or the Excel Viewer tool is generated:

![](img/553e44bc30e70d0d8e58278a7ece8da8_374_0.png)

## JDBC 4.0 Version

In the JDBC 4.0 version of the Apache POI application, we need to create a JDBC connection with Oracle database using the Oracle database 11g JDBC 4.0 drivers, which may be obtained from: http://www.oracle.com/technology/software/tech/java/sqlj_jdbc/htdocs/jdbc_111060.html. Use the ojdbc6.jar JDBC JAR file to configure a JDBC connection and data source with Oracle data source. As JDBC 4.0 drivers required JDK 6.0, set the J2SE Version to JDK 6.0 by selecting Tools | Project Properties. In the Project Properties window, select the Libraries node and select a JDK 6.0 Java executable in the J2SE Version field.

We will add the following JDBC 4.0 features to the Apache POI application:

- Connection state tracking
- Connection client identification using client info properties
- Statement pooling
- Wrapper pattern
- Java SE chained exception facility

Connection state tracking is implemented by the connection pool manager and is used to identify connections that have become unusable in the connection pool. JDBC 4.0 provides the isValid() method in the Connection interface for connection state tracking. If the isValid() method returns false the connection pool manager closes the connection.

```
if (!connection.isValid())
connection.close();
```

Connection client identification is used to identify connections that might cause reduced performance due to excessive CPU usage. Connections are associated with application clients using client info properties. The Connection interface in JDBC 4.0 includes two new method setClientInfo() and getClientInfo() to set and get client info properties. Client info properties supported by a database may be retrieved using the getClientInfoProperties() method of the DatabaseMetaData interface as follows:

```
DatabaseMetaData metaData=connection.getMetaData();
ResultSet rsClientInfo=metaData.getClientInfoProperties();
```

The standard client info properties ApplicationName, ClientUser, and ClientHostname may be set on the Connection object as follows:

```
conn.setClientInfo("ApplicationName","OracleApp");
conn.setClientInfo("ClientUser","OracleUser");
conn.setClientInfo("ClientHostname","OracleHost");
```

PreparedStatement objects are pooled by default if the database supports statement pooling. JDBC 4.0 has also added the provision to pool Statement objects. Frequently used Statement objects are recommended to be pooled to avoid the overhead of closing and opening new Statement objects. Statement pooling is implemented using the isPoolable() and setPoolable() methods of the Statement object as follows:

```
if (stmt.isPoolable())
stmt.setPoolable(true);
```

The Wrapper interface in JDBC 4.0 provides access to nonstandard JDBC methods in vendor-specific extensions to JDBC API using the wrapper pattern. The Wrapper interface is extended by the following JDBC interfaces:

- CachedRowSet
- CallableStatement
- Connection
- DatabaseMetaData
- DataSource
- FilteredRowSet
- JdbcRowSet
- JoinRowSet
- ParameterMetaData
- PreparedStatement
- ResultSet
- ResultSetMetaData
- RowSet
- RowSetMetaData
- Statement
- WebRowSet

Any vendor-specific interface extending these interfaces implement the wrapper pattern as it also extends the Wrapper interface. For example, the OracleStatement interface in Oracle's extension to the JDBC API extends the Statement interface and thus extends the Wrapper interface. The OracleStatement interface provides methods that are not defined in the Statement interface. We will use the OracleStatement interface instead of the Statement interface to run SQL queries. To create an OracleStatement object, first determine if the Statement object is a wrapper for the OracleStatement interface using the isWrapperFor() method. If a wrapper, unwrap the OracleStatement interface to create an OracleStatement object using unwrap() method in the Wrapper interface. Subsequently invoke the defineColumnType() method of OracleStatement object to set column types.

JDBC 4.0 supports the Java SE chained exceptions facility to retrieve chained exceptions and chained causes. Chained exceptions are exceptions that are linked to an exception. JDBC 4.0 supports the enhanced for loop introduced in J2SE 5.0 to iterate over chained exceptions as follows:

```
for(Throwable e : exception ) {
    out.println("Error encountered: " + e);
}
```

The JDBC 4.0 version of the Apache POI application is listed as follows.

```
<%@ page contentType="text/html;charset=windows-1252"%>
<%@ page import="org.apache.poi.hssf.usermodel.*, java.sql.*, java.io.*,javax.naming.InitialContext, oracle.jdbc.OracleStatement"%>
```
