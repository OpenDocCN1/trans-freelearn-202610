# Querying a Database Table

In this section, the database table created and modified in the previous sections will be queried, and the result is displayed in a HTML table. Create a JSP, catalogQuery.jsp. Select the JSTL Core 1.1 and JSTL SQL 1.1 tag libraries for the JSP. As in the previous sections, add a setDataSource tag for obtaining a connection with the Oracle database. The SQL taglib tag to query a database is sql:query. Position the cursor in the JSP page, and select Query tag in the SQL Component Palette.

![](bbox=[0.177, 0.283, 0.822, 0.614])

In the Insert Query window, specify the var attribute, which is a required attribute. The var attribute specifies the variable for the result set returned by the Query tag. Select the Advanced Properties tab.

![](bbox=[0.343, 0.694, 0.657, 0.827])