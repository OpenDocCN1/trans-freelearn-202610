```
/* Set the connection to null, so that the finally block below knows that it has been taken care of.*/
}
catch (Exception e )
{
/* Do whatever, according to your needs. You would not have a catch block, if it is not needed.*/
}
finally
{
/* Always have the above finally block. A finally block is crucial to ensure that the connection is closed and returned to the pool, and is not leaked.*/
/* Failsafe: Do every individual thing you want to do in the finally block in it's own try block-catch-ignore so everything is attempted.*/
try
{
if (c != null) c.close();
}
catch (Exception ignore)
{
}
}
}
```

# Deploying a Web Application to WebLogic Server from JDeveloper

We will develop a web application to retrieve data from an Oracle database table with the data source configured in the Configuring a Data Source section. Create an example database table in the Oracle database. Create a table, Catalog, using a SQL Client tool such as SQL * Plus, or a command-line utility with the following QL script:

```
CREATE TABLE Catalog (CatalogId INTEGER PRIMARY KEY, Journal VARCHAR(25), Publisher VARCHAR(25), Edition VARCHAR(25), Title Varchar(45), Author Varchar(25));
INSERT INTO Catalog VALUES ('1', 'Oracle Magazine', 'Oracle Publishing', 'Nov-Dec 2004', 'Database Resource Manager', 'Kimberly Floss');
INSERT INTO Catalog VALUES ('2', 'Oracle Magazine', 'Oracle Publishing', 'Nov-Dec 2004', 'From ADF UIX to JSF', 'Jonas Jacobi');
INSERT INTO Catalog VALUES ('3', 'Oracle Magazine', 'Oracle Publishing', 'March-April 2005', 'Starting with Oracle ADF ', 'Steve Muench');
```