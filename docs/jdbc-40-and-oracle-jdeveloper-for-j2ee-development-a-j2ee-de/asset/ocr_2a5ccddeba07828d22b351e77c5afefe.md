```
Page</a></td>
    </tr>
</table>
    </form>
    <%
        webrowset.WebRowSetQuery query=null;
        query=(
            webrowset.WebRowSetQuery)session.getAttribute("query");
        String deleteRow=request.getParameter("deleteRow");
        if((deleteRow!=null)){
        int delete_Row=Integer.parseInt(deleteRow);
        query.deleteRow(delete_Row);
        }
    %>
    <form name="query" action="DeleteRow.jsp" method="post">
        <table>
            <tr>
                <td><h4>Delete Row</h4></td>
            </tr>
            <tr>
                <td>
                    <input name="deleteRow" type="text" size="25"
                        maxlength="50"/>
                </td>
            </tr>
            <tr>
                <td>
                    <input class="Submit" type="submit" value="Apply"/>
                </td>
            </tr>
        </table>
    </form>
    </body>
</html>
```

# Updating Database Table

Next, we will update the database table with the modified OracleWebRowSet object. Click on the **Modify Web RowSet** link in the DeleteRow JSP. In the ModifyWebRowSet JSP, click on the **Update Database** link. In the UpdateDatabase.jsp, click on **Apply**.