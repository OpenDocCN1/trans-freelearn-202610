# Configuring JDBC in Oracle JDeveloper

```
while (resultSet.next())
{
%>
<tr>
<td><%=out.println(resultSet.getString(1));%></td>
<td><%=out.println(resultSet.getString(2));%></td>
<td><%=out.println(resultSet.getString(3));%></td>
<td><%=out.println(resultSet.getString(4));%></td>
<td><%=out.println(resultSet.getString(5));%></td>
<td><%=out.println(resultSet.getString(6));%></td>
</tr>
<%
}
%>
</table>
<%
resultSet.close();
stmt.close();
if(!connection.isClosed())
connection.close();
%>
```

Run the JSP, by right-clicking on the JSP node in the **Applications Navigator** and selecting **Run**.

![](bbox=[0.188, 0.489, 0.81, 0.828])