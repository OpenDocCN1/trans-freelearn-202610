DriverConnectionProvider class, with the following source code:

```
```
...
// Defining the connection provider.
DriverConnectionProvider provider = new DriverConnectionProvider();
provider.setDriver("org.hsqldb.jdbcDriver");
provider.setProperty("user", "pentaho_user");
provider.setProperty("password", "password");
provider.setUrl("jdbc:hsqldb:./resources/sampledata/sampledata");

// Defining the query.
SQLReportDataFactory dataFactory = new SQLReportDataFactory(provider);
String sqlQuery = "SELECT PRODUCTNAME, QUANTITYINSTOCK FROM PRODUCTS";
dataFactory.setQuery("default", sqlQuery);
report.setDataFactory(dataFactory);
...
```
```

To run the project, execute exactly the same tasks as described previously; download the pentaho-reporting-data-source-jdbc folder to your local machine, open a terminal, move into this folder, and execute the following commands in the given sequence:

```
```
mvn clean install
mvn package
java -jar target/dependency/jetty-runner.jar target/*.war
```
```

Then open a browser at http://localhost:8080, and you should see the Pentaho report in PDF format as a result.

As you can see, inside the source code of both the projects, SQLReportDataFactory is used in exactly the same way and the resulting report is the same in terms of layout.