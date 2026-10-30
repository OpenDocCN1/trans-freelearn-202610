add/retrieve/update/delete data in the database tables. In this chapter, an example mapping file, (`catalog.hbm.xml`), consisting of properties for a journal catalog, is mapped to a database table, `OE.Catalog`, and to a Java class, `Catalog.java`.

# Hibernate and JDBC

Hibernate is a JDBC-based database persistence framework. A Hibernate application's Java to SQL mappings are defined in the `org.hibernate.cfg.Configuration` object. The mappings are compiled from XML mapping files. A XML mapping file is added to a `Configuration` object as follows:

```
```java
Configuration cfg = new
    Configuration().addResource("Catalog.hbm.xml");
```
```

Hibernate configuration properties, which include JDBC properties, are set using one of the following methods:

+   1. Set the configuration properties using `setProperties()` method of the `Configuration` object.
+   2. Specify the configuration properties in the `hibernate.properties` file.
+   3. Specify the properties as System properties.
+   4. Specify the properties in the `hibernate.cfg.xml` file.

A `SessionFactory` is used to create and pool connections. A `SessionFactory` is created from a `Configuration` object as follows:

```
```java
SessionFactory sessionFactory = cfg.buildSessionFactory();
```
```

When a `Session` object is created a JDBC connection is obtained from the connection pool.

```
```java
Session session = sessionFactory.openSession();
```
```

Hibernate creates and pools connections either using `java.sql.DriverManager` or a data source. If connections are to be created and pooled using a `DriverManager`, the JDBC connection properties discussed in the following table need to be specified.

| Property | Description |
|---|---|
| `hibernate.connection.driver_class` | JDBC Driver Class |
| `hibernate.connection.url` | JDBC Connection URL |
| `hibernate.connection.username` | Database User Name |