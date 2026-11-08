# Using report parameters

As presented in Chapter 6, *Configuring JDBC and Other Data Sources*, there are many different options for providing data to Pentaho Reporting. All of them are grouped under the name *data sources* and each different type of data source has its own features, syntax, and reasons to be used.

Because of the different syntax and features, each data source type uses its underlying method for parameterizing queries. For instance, the default syntax for XPath, Kettle, and MDX are supported through their respective `DataFactory` implementations. Each data source query syntax defines how to specify parameters, and Pentaho Reporting provides those parameters via the `DataRow` API. The two exceptions from the default management of parameters include SQL and static data. When specifying a SQL statement in JDBC, you would normally specify a question mark to denote which parameters should be specified.

Pentaho Reporting requires that you specify the parameters by name, so some parsing is done on the SQL query before passing the query to JDBC. You may specify parameters as `${PARAM}`, and `SQLReportDataFactory` will replace each named parameter with a question mark (?) before making the prepared JDBC call. `NamedStaticDataFactory` allows for Java class methods with parameters to be executed. The names provided when configuring `DataFactory` are used when resolving the values from provided `DataRow`.

The following is an example of a SQL query containing one parameter named `productLine`, which filters the products' line on the `PRODUCTS` table of the `SampleData` schema:

```
```
SELECT
  *
FROM
  PRODUCTS
WHERE
  PRODUCTLINE = ${productLine}
```
```

*Be aware that the parameters are defined together with a collection of properties (you will see this better in the following sections).*