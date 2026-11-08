# An example of subreport using Java

To show in practice an example of subreport using Java, let's use the https://github.com/fcorti/pentaho-8-reporting-for-java-developers repository; specifically, the project `pentaho-reporting-subreport`, stored in the `Chapter 13 - Internationalization, sub-reports and cross-tabs using Java` folder.

As you can see from the `PentahoServlet` class, `DataFactory` declares the `default` query and the `default2` query using the following piece of code:

```
// Defining the queries.
SQLReportDataFactory dataFactory = new SQLReportDataFactory(provider);
dataFactory.setQuery(
"default",
"SELECT ORDERNUMBER, ORDERDATE FROM ORDERS");
dataFactory.setQuery(
"default2",
"SELECT PRODUCTCODE, QUANTITYORDERED, PRICEEACH FROM ORDERDETAILS " +
"WHERE ORDERNUMBER=${ORDERNUMBER} ORDER BY ORDERLINENUMBER ASC LIMIT 5");
report.setDataFactory(dataFactory);
```

![](bbox=[0.143, 0.505, 0.208, 0.555])

> *Pay a lot of attention to giving the queries declared in `DataFactory` unique names. If you don't do that, the behavior of your application will be unpredictable, and you will experience a painful waste of time understanding where exactly the problem is.*

As described previously in the chapter, `MasterReport` and `SubReport` declare the query they are going to use with the following code:

```
...
report.setQuery("default");
...
subReport.setQuery("default2");
...
```

In the following piece of Java code, you can find the declaration of the `SubReport` object, an example of addition of elements to the subreport's band, and the inclusion of the subreport in the main report:

```
...
// Creating the subreport.
SubReport subReport = new SubReport();
subReport.setQuery("default2");
subReport.addInputParameter("ORDERNUMBER", "ORDERNUMBER");
```