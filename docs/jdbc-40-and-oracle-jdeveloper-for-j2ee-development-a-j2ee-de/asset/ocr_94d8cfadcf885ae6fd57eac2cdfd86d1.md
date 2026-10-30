The structure of the OE.Catalog table generated is as follows:

```
SQL> DESC OE.CATALOG;
Name                          Null?    Type
------------------------------ -------- ----------------------------
ID                            NOT NULL VARCHAR2(255)
JOURNAL                                VARCHAR2(255)
PUBLISHER                              VARCHAR2(255)
EDITION                                VARCHAR2(255)
TITLE                                  VARCHAR2(255)
AUTHOR                                 VARCHAR2(255)
SQL>
```

In the following section table data will be added, retrieved, updated, and deleted with the Java class generated from the mapping file and the hibernate.properties properties file.

# Modifying Table Data with Hibernate

In this section a middle-tier Hibernate Java application is developed to add/update and delete data to the table generated from the mapping file, catalog.hbm.xml. The Java application is not the JavaBean class generated from the mapping file. The Java application integrates the JavaBean class generated from the mapping file, the mapping file, and the properties file to provide a database persistence and query service.

Select File | New and in the New Gallery window select General | Java Class. In the Create Java Class window, specify a class name, HibernateDB.java, and a class package, hibernate. A Java class is added to the Hibernate project. Next, copy the mapping file catalog.hbm.xml and the C:/Hibernate/src/hibernate/Catalog.java java bean class to the hibernate directory. The directory structure of the Hibernate application is shown in following figure.

![](bbox=[0.38, 0.645, 0.62, 0.822])