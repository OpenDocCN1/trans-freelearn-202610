To the project libraries add the directory containing the hibernate.properties file. Also add the Commons lang JAR file from the Hibernate 2.1.6 directory.

![](bbox=[0.175, 0.19, 0.825, 0.545])

In the Hibernate Java application import the Hibernate API classes. Import the Java class that was generated from the mapping file.

# Adding Data to the Database Table

Create a Catalog class object and set values for the different fields of the Java class with the setter methods.

```
```
Catalog catalog=new Catalog();
catalog.setId("catalog 1");
catalog.setJournal("Oracle Magazine");
catalog.setPublisher("Oracle Publishing");
catalog.setEdition("Jan-Feb 2004");
catalog.setTitle("Understanding Optimization");
catalog.setAuthor("Kimberly Floss");
```
```