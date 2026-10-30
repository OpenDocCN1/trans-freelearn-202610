```
<h:column binding="#{backing_catalog.column4}"/>
<h:column binding="#{backing_catalog.column5}"/>
<h:column binding="#{backing_catalog.column6}"/>
</h:dataTable>
```

Next, bind the command button to a backing bean method in which the Data Table will be generated. Double-click on the command button. In the Bind Action Property window, select the default action method in the backing bean for the command button and click on OK. The backing bean class for the JSF page, Catalog.java gets displayed. Add UIColumn component variables for the Data Table columns and add getter/setter methods for the UIColumn components. Modify the commandButton1_action method, which is called when the Data Table button is clicked. In the commandButton1_action method, obtain a JDBC connection with the database. Load the driver class oracle.jdbc.OracleDriver using the Class.forName method. Create a Connection object using the getConnection method of the DriverManager class:

```
Class.forName("oracle.jdbc.driver.OracleDriver");
String url="jdbc:oracle:thin:@<host>:1521:<database>";
Connection connection = DriverManager.getConnection(url,
    "OE", "<password>");
```

![](bbox=[0.19, 0.47, 0.26, 0.51])

> In the connection URL <host> is the database Host and <database> is the database instance.

Create a Statement object using createStatement method. Specify result set type as scrollable and result set concurrency as read-only.

```
Statement
stmt=connection.createStatement(ResultSet.TYPE_SCROLL_INSENSITIVE,
ResultSet.CONCUR_READ_ONLY);
```

Query the database with the SQL Select statement specified in the Input Text field of the JSF page. The Input Text value is retrieved by invoking the getValue method of the backing bean property for the Input Text. The SQL query is run using the executeQuery method of Statement object. The executeQuery method generates a ResultSet object.

```
ResultSet rs=stmt.executeQuery((String)inputText1.getValue());
```

Using the backing bean property for the Data Table, set the border and cell padding on the Data Table. The border is set using the setBorder method and the cell padding is set using the setCellPadding method:

```
dataTable1.setBorder(5);
dataTable1.setCellPadding("1");
```