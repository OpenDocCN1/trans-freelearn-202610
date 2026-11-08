# HQLDataFactory

The `org.pentaho.reporting.engine.classic.extensions.datasources.hibernate.HQLDataFactory` class allows you to populate your report based on a Hibernate query. **Hibernate** is an open source relational persistence engine. It allows you to easily persist your Java objects to a database.

Much like many of the other `DataFactory` implementations, `HQLDataFactory` is configured with information to connect to Hibernate, along with the ability to specify named queries. The `HQLDataFactory` API defines an interface called `SessionProvider`, which it uses to gain access to the current hibernate `Session` object. `SessionProvider` contains a single API method, `getSession()`, which returns an instance of `org.hibernate.Session`. There are two implemented versions of this session provider: `StaticSessionProvider` and `DefaultSessionProvider`.

At this time, `HQLDataFactory` is not available as a data source within Pentaho Report Designer, so code is required to utilize Hibernate as a data source.

![](bbox=[0.143, 0.57, 0.207, 0.62])

> *HSQLDataFactory is documented here because it is quoted in the official Javadoc website (http://javadoc.pentaho.com/reporting/). As you can see from Pentaho's artifact repository regarding the hibernate extension, found at this link:http://repository.pentaho.org/artifactory/repo/pentaho-reporting-engine/pentaho-reporting-engine-classic-extensions-hibernate/, the library is not supported anymore and the latest update is on the 3.9.2-GA release.*