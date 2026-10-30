```
<table border="1" cellspacing="0">
<tr>
<th>RowId</th>
<th>CatalogId</th>
<th>Journal</th>
<th>Section</th>
<th>Edition</th>
<th>Title</th>
<th>Author</th>
</tr>
<%
while (resultSet.next())
{
%>
<tr>
<td><%=out.println(resultSet.getRowId("ROWID").toString());%></td>
<td><%=out.println(resultSet.getString(1));%></td>
<td><%=out.println(resultSet.getString(2));%></td>
<td><%=out.println(resultSet.getString(3));%></td>
<td><%=out.println(resultSet.getString(4));%></td>
<td><%=out.println(resultSet.getString(5));%></td>
<td><%=out.println(resultSet.getString(6));%></td>
</tr>
<%
}
resultSet.close();
stmt.close();
connection.close();
}
%>
</table>
</body>
</html>
```

# Summary

WebSphere 6.1 application server has the provision to create a JDBC Provider and a JDBC data source with a relational data source. In this chapter, we configured a JDBC connection in Web Sphere with DB2 9 database. The combination of the IBM's DB2 database and IBM's WebSphere application server is suitable for the development of JDBC applications. DB2 V9 supports the SQL XML data type that can be used with a new Java data type in JDBC 4.0, called SQLXML. IBM Data Server Driver for JDBC and SQLJ Version 4.0 supports JDBC 4.0 specification.