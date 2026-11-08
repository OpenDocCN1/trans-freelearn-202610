# # About the JNDI access

The **Java Naming and Directory Interface (JNDI)** is a Java API for a directory service that allows Java software clients to discover and look up data and objects using a name. Because of its nature, the settings requested for the JNDI connection is a name only. This unique name should match an existing JNDI connection available in the Pentaho Reporting environment.

To set up a JNDI data source in Pentaho Reporting, edit the `.pentaho/simple-jndi/default.properties` file. The `.pentaho` directory is in the home or user directory of the user account that runs Pentaho Report Designer. If you have multiple copies of Pentaho Report Designer installed in multiple user accounts, each `default.properties` file should be edited.

The following example shows text lines to be added/edited relation to an existing JNDI connection; the JNDI connection to `SampleData` on HyperSQL:

```
SampleData/type=javax.sql.DataSource
SampleData/driver=org.hsqldb.jdbcDriver
SampleData/url=jdbc:hsqldb:hsql://localhost/sampledata
SampleData/user=pentaho_user
SampleData/password=password
```

As you can see, the name of the JNDI connection is used as a prefix for all the settings in the properties file.