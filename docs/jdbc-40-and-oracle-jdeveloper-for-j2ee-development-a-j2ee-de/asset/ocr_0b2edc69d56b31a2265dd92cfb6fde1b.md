```
String rowUpdate=request.getParameter("rowUpdate");
String journalUpdate=request.getParameter("journalUpdate");
String publisherUpdate=request.getParameter("publisherUpdate");
String editionUpdate=request.getParameter("editionUpdate");
String titleUpdate=request.getParameter("titleUpdate");
String authorUpdate=request.getParameter("authorUpdate");
    if((rowUpdate!=null))
{
    System.out.println(rowUpdate +"Row to Update");
    int row_Update=Integer.parseInt(rowUpdate);
    query.updateRow(row_Update, journalUpdate, publisherUpdate,
        editionUpdate, titleUpdate, authorUpdate);
}
%>
<form name="query" action="UpdateRow.jsp" method="post">
    <table>
        <tr>
            <td>Database Row to Update:</td>
        </tr>
        <tr>
            <td>
                <input name="rowUpdate" type="text" size="25"
                    maxlength="50"/>
            </td>
        </tr>
        <tr>
            <td>Journal:</td>
        </tr>
        <tr>
            <td>
                <input name="journalUpdate" type="text" size="50"
                    maxlength="250"/>
            </td>
        </tr>
        <tr>
            <td>Publisher:</td>
        </tr>
        <tr>
            <td>
                <input name="publisherUpdate" type="text" size="50"
                    maxlength="250"/>
            </td>
        </tr>
        <tr>
            <td>Edition:</td>
        </tr>
        <tr>
            <td>
                <input name="editionUpdate" type="text" size="50"
```