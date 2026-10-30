Configure a connection with the MySQL database in JBoss application server as explained in the Configuring JBoss Server with MySQL Database section. We will obtain a connection with the MySQL database in catalog.jsp. Create an InitialContext object, and create a DataSource object using JNDI lookup on the MySQL data source, configured in JBoss application server.

```
InitialContext initialContext = new InitialContext();
DataSource ds = (DataSource)
initialContext.lookup("java:/MySqlDS");
```

Create a Connection object from the DataSource object using the getConnection() method and create a Statement object from the Connection object using the createStatement() method. Run an SQL query on the Catalog table using the executeQuery() method of the Statement interface. Iterate over the ResultSet and create an HTML table. The catalog.jsp file is listed below:

```
<%@ page contentType="text/html;charset=windows-1252"%>
<%@ page language="java" import="java.sql.*, javax.naming.*, javax.sql.*" %>
<%
InitialContext initialContext = new InitialContext();
DataSource ds = (DataSource)
initialContext.lookup("java:/MySqlDS");
java.sql.Connection connection = ds.getConnection();
Statement stmt=connection.createStatement();
ResultSet resultSet=stmt.executeQuery("Select * from Catalog");
%>
<table border="1" cellspacing="0">
<tr>
<th>CatalogId</th>
<th>Journal</th>
<th>Publisher</th>
<th>Edition</th>
<th>Title</th>
<th>Author</th>
</tr>
<%
while (resultSet.next())
{
%>
<tr>
<td><%=out.println(resultSet.getString(1));%></td>
<td><%=out.println(resultSet.getString(2));%></td>
<td><%=out.println(resultSet.getString(3));%></td>
<td><%=out.println(resultSet.getString(4));%></td>
```