Run a SQL query with the executeQuery() method to return a ResultSet object. Specify a SQL query, which selects all the columns of the example database table, Catalog:

```
ResultSet resultSet=stmt.executeQuery("Select * from Catalog");
```

Create a HTML table with a row that corresponds to each row in the result set, and a column that corresponds to each column in the result set. Add a header row to the html table. Iterate over the result set, and add row values to the html table:

```
while (resultSet.next())
{
<tr>
<td><%=out.println(resultSet.getString(1));%></td>
</tr>
}
```

The JSP page, catalog.jsp, which is used to generate a HTML table from the example database table with the data source configured in the WebLogic server, is listed below:

```
<%@ page contentType="text/html"%>
<%@ page import="java.sql.*,javax.sql.*,javax.naming.*"%>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html">
<title>WebLogic Application</title>
</head>
<body>
<%
InitialContext ctx=new InitialContext();
DataSource ds=(DataSource)ctx.lookup("jdbc/OracleDS");
Connection connection=ds.getConnection();
Statement stmt=connection.createStatement();
ResultSet resultSet=stmt.executeQuery("Select * from Catalog");
%>
<table border="1" cellspacing="0">
<tr>
<th>CatalogId</th>
```