+   testAutomatically: It is an optional argument, which instructs Roo to create integration tests for the JPA entities created via the database reverse engineer command.

+   enableViews: It is an optional argument, which instructs Roo to reverse the engineer database views along with database tables. If specified, Roo creates a JPA entity corresponding to each database view.

+   includeTables: It is an optional argument, which specifies the tables for which JPA entities are created by Roo. By default the JPA entities corresponding to all the tables in the database are created by Roo. The value of the includeTables argument specifies a space-separated list of tables enclosed within double quotes, as shown here:
+   
+   .. roo> database reverse engineer --schema no-schema-required
+   --includeTables "FLIGHT_TBL FLIGHT_DESC_TBL"

+   excludeTables: It is an optional argument, which specifies the tables for which the JPA entities must not be created by Roo. The value of the excludeTables argument specifies a space-separated list of tables enclosed within double quotes, as shown here:
+   
+   .. roo> database reverse engineer --schema no-schema-required
+   --excludeTables "FLIGHT_TBL FLIGHT_DESC_TBL"

Let's now look at some of the interesting things that happened when we executed the database reverse engineer command:

+   A dbre.xml file is generated in the SRC_MAIN_RESOURCES directory of the Roo project. This file contains database metadata in XML format, as described in the Generating database metadata recipe.

+   A Java class is generated corresponding to each of the tables and views in the database. The name of the Java source file is <table-name>.java, where the table-name is the name of the database table in the camel-case with underscores in the table name removed. Each Java class represents a JPA entity corresponding to a database table or view.

+   The META-INF/persistence.xml file is updated. Roo modifies (in case you are using Hibernate as the JPA provider) the value of the property hibernate.hbm2ddl.auto from create to validate, so that the database is not modified when Hibernate SessionFactory is created. This makes sense because JPA entities in the application have been created from the database and not otherwise.

+   A * .Roo_Db_Managed.aj ITD file is created corresponding to each JPA entity created by Roo. It defines fields and relationships derived for the JPA entity from the database metadata. As this ITD file contains fields and relationship information, if you reverse engineer the database multiple times, the changes corresponding to the database are restricted to this ITD file, making incremental database reverse engineering possible in Roo applications.