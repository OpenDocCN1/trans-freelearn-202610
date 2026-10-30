# Connecting to a Database with JSTL SQL Tag Library

```
password="pw"/><sql:query var="catalog" sql="SELECT * FROM
OE.CATALOG"/>
<table border>
<tr>
    <td><b>CatalogId</b></td>
    <td><b>Journal</b></td>
    <td><b>Publisher</b></td>
    <td><b>Edition</b></td>
    <td><b>Title</b></td>
    <td><b>Author</b></td>
</tr>
<c:forEach var="journal" items="${catalog.rows}">
<tr>
    <td>
        <c:out value="${journal.CatalogId}"/>
    </td>
    <td><c:out value="${journal.Journal}"/></td>
    <td><c:out value="${journal.Publisher}"/></td>
    <td><c:out value="${journal.Edition}"/></td>
    <td><c:out value="${journal.Title}"/></td>
    <td><c:out value="${journal.Author}"/></td>
</tr>
</c:forEach>
</table>
</body>
</html>
```

To run the **catalogQuery.jsp**, right-click on the JSP and select **Run**:

![](bbox=[0.22, 0.538, 0.78, 0.828])