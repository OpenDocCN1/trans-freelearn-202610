# Oracle Web RowSet

```
<td>Select Query:</td>
</tr><tr><td>
<textarea name="selectQuery" rows="5"
cols="50"></textarea>
</td>
</tr><tr><td>
<input class="Submit" type="submit" value="Apply"/>
</td>
</tr>
</table>
</form>
</body>
</html>
```

Right-click on the GenerateWebRowSet.jsp and select Run to run the JSP.

![](bbox=[0.175, 0.361, 0.825, 0.678])

In the JSP page displayed, specify the SQL query from which a Web RowSet is to be generated. For example, specify SQL Query:

```
SELECT JOURNAL, PUBLISHER, EDITION, TITLE, AUTHOR FROM OE.Catalog
```