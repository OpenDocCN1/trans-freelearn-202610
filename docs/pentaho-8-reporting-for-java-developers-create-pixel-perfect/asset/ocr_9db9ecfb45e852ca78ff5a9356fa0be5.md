details on the bottom line. In the connection canvas, you will find five different types of associations to the `SampleData` schema. Take your time to examine each connection and their differences.

By selecting one connection and clicking on the pencil icon (✏️), you will have access to the connection parameters and settings. Even if all the connections point to the `SampleData` schema, the type of access is different, and the different parameters and settings. We won't detail the differences between the types of connections, but will wait for the chapter dedicated to the data sources to discover further details.

![](bbox=[0.14, 0.35, 0.21, 0.41])

> *It's important to note that the default installation of Pentaho Report Designer does not provide all the JDBC drivers to the sources, even if they are listed in this panel. To install a JDBC driver, simply copy the JAR file implementing the driver into the `<report-designer>/lib/jdbc` folder and restart.*

Even if you are going to use an existing connection, it is very simple to create a new one by using the green plus icon (➕). As an exercise, try to click the green plus icon (➕) to see what the window looks like.

After having introduced the connection canvas, select `SampleData (local)` and continue defining a query over the connection. To define a query, press the green plus icon (➕) on the right of the `Available Queries` panel. Once clicked, a new named `Query 1` will be defined. To change its name to be meaningful, select it and change the value in the `Query Name` field. In our case, we are going to call it `Actuals`.

Now that the query is available but should be defined, let's focus on the bottom panel containing the query definition. At the bottom right of the canvas, digit the query or compose it using **SQLLeonardo Query Builder**. The simple query builder is accessible through the pencil icon (✏️) on the right. In both cases, you can select a single table or define a more complex query over multiple tables. Once finished, click on Preview to see the resulting data set.

In our example, let's define the query as described in the following screenshot: