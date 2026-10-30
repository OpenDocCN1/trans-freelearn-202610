
To the project libraries add the directory containing the hibernate.properties file. Also add the Commons lang JAR file from the Hibernate 2.1.6 directory.

![](img/553e44bc30e70d0d8e58278a7ece8da8_423_0.png)

In the Hibernate Java application import the Hibernate API classes. Import the Java class that was generated from the mapping file.

## Adding Data to the Database Table

Create a Catalog class object and set values for the different fields of the Java class with the setter methods.

```
Catalog catalog=new Catalog();
catalog.setId("catalog 1");
catalog.setJournal("Oracle Magazine");
catalog.setPublisher("Oracle Publishing");
catalog.setEdition("Jan-Feb 2004");
catalog.setTitle("Understanding Optimization");
catalog.setAuthor("Kimberly Floss");
```

Interface org.hibernate.Session, the main runtime interface between a Java application and Hibernate, is used to create, update, and delete data in a database. A Session object is obtained from a SessionFactory. The SessionFactory interface provides openSession() methods to create a database connection and open a session on the connection, or open a session on a specified connection. The org.hibernate.cfg.Configuration class is used to specify configuration properties, JavaBean persistence class and mapping files to create a SessionFactory object. Create a Configuration object:

```
Configuration config=new Configuration();
```

Add the JavaBean persistence class, Catalog.class to the Configuration object using the addClass() method.

```
config.addClass(example.hibernate.Catalog.class);
```

The mapping file, catalog.hbm.xml, which is copied to the same directory as the mapped Java class is configured with the Configuration object. The hibernate.properties file in the Class path of the Hibernate application gets configured as the properties file for the Configuration object. Create a SessionFactory object from the Configuration object using the buildSessionFactory() method.

```
SessionFactory sessionFactory=config.buildSessionFactory();
```

The SessionFactory creates and pools JDBC connections for the Hibernate application. First, we will add data to the database table that was created with the hibernate mapping file and the hibernate properties file. Obtain a Session object from the SessionFactory object using the openSession() method.

```
Session sess = sessionFactory.openSession();
```

A JDBC connection gets obtained from the connection pool when a Session is opened. Hibernate obtains and pools connections using java.sql.DriverManager. Obtain a Transaction object from the Session object using the beginTransaction() method to add data to the database table.

```
org.hibernate.Transaction tx = sess.beginTransaction();
```

Store the JavaBean object, created earlier, in the database with the save() method and commit the transaction using the commit() method:

```
sess.save(catalog);
tx.commit();
```

The values specified in the Catalog object get stored in the database table. Close the Session object using the close() method:

```
sess.close();
```

## Retrieving Data from the Database Table

Next, we will retrieve the data stored in the database table. Create a query to select data from the table. The query is defined in Hibernate Query Language (HQL) syntax which is similar to SQL.

```
String hqlQuery ="from example.hibernate.Catalog";
```

If the Select clause is not specified in the query all of the fields selected in the From clause are selected from the mapped class. The From clause is specified with the mapped Java class, example.hibernate.Catalog, not the database table. The Java class object to database mapping is performed by the mapping file and the properties file. Open a Session object from the SessionFactory object using the openSession() method:

```
Session sess = sessionFactory.openSession();
```

Create a Query object with the createQuery(hqlQuery) method of the Session object:

```
Query query = sess.createQuery(hqlQuery);
```

Obtain a List from the HQL query with the list() method of the Query object:

```
List list = query.list();
```

Iterate over the List and output values for the specified HQL query. For example the Journal column value is output as follows:

```
for (int i = 0; i < list.size(); i++) {
Catalog catalog = (Catalog) list.get(i);
    System.out.println("CatalogId " + catalog.getId() +
    " Journal: " + catalog.getJournal());
}
```

## Updating the Database Table

Next, we will update table values with the Hibernate Java application. Create a query to select data, which is to be modified, from the table

```
String hqlQuery="from Catalog";
```

Obtain a Session object from the SessionFactory object.

```
Session sess = sessionFactory.openSession();
```

Obtain a Transaction object from the Session object.

```
Transaction tx = sess.beginTransaction();
```

Create a Query object from the HQL query using the createQuery() method of the Session object. Obtain a List result set with the list() method. Obtain the JavaBean object to be modified.

```
Query query = sess.createQuery(hqlQuery);
List list = query.list();
Catalog catalog = (Catalog) list.get(0);
```

As an example, set the value of the publisher field to "Oracle Magazine".

```
catalog.setPublisher("Oracle Magazine");
```

Begin a Session transaction using the beginTransaction() method of the Session object.

```
Transaction tx = sess.beginTransaction();
```

Update the database table with the saveOrUpdate() method of the Session object and commit the transaction:

```
sess.saveOrUpdate(catalog);
tx.commit();
```

## Deleting Data

Next, delete a table row with the Hibernate API. As an example, delete the table row for edition "March-April 2005". Create a HQL query which selects a database table row to delete.

```
String hqlQuery="from Catalog as catalog where catalog.edition='March-April 2005'";
```

Open a database session with the openSession() method of the SessionFactory object:

```
Session sess = sessionFactory.openSession();
```

Create a Query object with the HQL query. Obtain the result set List for the HQL query. Obtain the result set item to be deleted:

```
Query query = sess.createQuery(hqlQuery);
List list = query.list();
Catalog catalog = (Catalog) list.get(0);
```

Begin a session transaction using the beginTransaction() method:

```
Transaction tx = sess.beginTransaction();
```

Delete the row specified in the HQL query with the `delete()` method of the `Session` object and commit the transaction:

```
sess.delete(catalog);
 tx.commit();
```

The example Java application, `HibernateDB.java`, has the `addToCatalog` method to add data, `retrieveFromCatalog` method to retrieve data, `updateCatalog` method to update data, and `deleteFromCatalog` method to delete data. The following list contains `HibernateDB.java`:

```
package hibernate;
import hibernate.Catalog;
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;
import org.hibernate.Query;
import java.util.List;

public class HibernateDB {
Transaction tx;
Session sess;
Configuration config;
SessionFactory sessionFactory;
public void addToCatalog() {
    try {
        Catalog catalog = new Catalog();
        catalog.setId("catalog 1");
        catalog.setJournal("Oracle Magazine");
        catalog.setPublisher("Oracle Publishing");
        catalog.setEdition("Jan-Feb 2004");
        catalog.setTitle("Understanding Optimization");
        catalog.setAuthor("Kimberly Floss");

        Catalog catalog2 = new Catalog();
        catalog2.setId("catalog 2");
        catalog2.setJournal("Oracle Magazine");
        catalog2.setPublisher("Oracle Publishing");
        catalog2.setEdition("March-April 2005");
        catalog2.setTitle("Starting with Oracle ADF");
        catalog2.setAuthor("Steve Muench");

        config = new Configuration();
        config.addClass(Catalog.class);
        sessionFactory = config.buildSessionFactory();
        sess = sessionFactory.openSession();
        tx = sess.beginTransaction();
```
