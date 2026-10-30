```
```sql
CREATE TABLE OE.Catalog(CatalogId VARCHAR(25), Journal VARCHAR(25),
    Publisher Varchar(25),Edition VARCHAR(25), Title Varchar(255),
    Author Varchar(25));

INSERT INTO OE.Catalog VALUES('catalog1', 'Oracle Magazine',
    'Oracle Publishing', 'March-April 2005', 'Starting with Oracle ADF'
    , 'Steve Muench');

INSERT INTO OE.Catalog VALUES('catalog2', 'Oracle Magazine',
    'Oracle Publishing','Jan-Feb 2005', 'Understanding Optimization',
    'Kimberly Floss');
```
```

Next, we will create a JDeveloper project for generating an Excel spreadsheet and add a library for the Apache POI JAR file to the project. Also, we will create a JDBC connection with the Oracle database for retrieving the data for the Excel spreadsheet.

First, create a JDBC connection to the Oracle database from the JDeveloper IDE. To create a connection select the Connections tab, select the Database node in the Connections Navigator, right click on the Database node, and select New Database Connection. In the Create Database Connection (Type) window displayed, specify a Connection Name and set Connection Type to Oracle(JDBC).

![](bbox=[0.18, 0.47, 0.82, 0.81])