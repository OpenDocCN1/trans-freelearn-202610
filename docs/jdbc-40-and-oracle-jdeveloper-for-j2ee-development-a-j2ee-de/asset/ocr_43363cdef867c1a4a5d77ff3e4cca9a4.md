Specify the SQL query in the sql attribute field and click on the OK button.

![](bbox=[0.339, 0.175, 0.661, 0.353])

The sql:query tag gets added to the JSP page:

```
<sql:query  var="catalog" sql="SELECT * FROM CATALOG"/>
```

Add a <table> to display the result set of the Query tag. Add the table headers. A row will be added corresponding to each row in the result set. Select the JSTL 1.1 Core component library and add a ForEach tag to the <table>.

![](bbox=[0.178, 0.485, 0.822, 0.817])