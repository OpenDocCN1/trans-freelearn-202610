In this chapter, you will learn how to:

+   - Create a Data Source in WebLogic Server.
+   - Configure Connection Pool Properties for a Data Source.
+   - Create a multi data source in WebLogic Server.
+   - Performance Tune WebLogic Server Connections.
+   - Deploy a Web Application to WebLogic Server from JDeveloper.
+   - JDBC 4.0 version of the Web Application.

# Setting the Environment

Install the database that requires a JDBC connection. The procedure to install the databases is discussed in Chapter 1. Download the JDBC driver for the database, if WebLogic does not have a driver for the DBMS. We will configure the JDBC connectivity with the Oracle database 10g. Copy the Oracle database JDBC driver JAR file `ojdbc14.jar` from `<Oracle>\jdbc\lib` directory to the `<weblogic91>\samples\domains\wl_server\lib` directory, `<Oracle>` being the Oracle database 10g installation directory and `<weblogic91>` being the WebLogic Server 9.1 installation directory. JAR files in the server `lib` directory get automatically added to the classpath, on server startup. If a later version is required, replace it in the `lib` directory. Alternatively, you can edit the `<weblogic91>\samples\domains\wl_server\bin\startWebLogic` script by adding a driver JAR file to the classpath argument constructed by the script. Put the JAR file ahead of `weblogic.jar` in the classpath. Double-click on the `<weblogic91>\samples\domains\wl_server\startWebLogicEx` command script to start the WebLogic examples server.

# Creating a Data Source

A data source is a pool of JDBC connections from which a connection can be obtained using the `getConnection()` method of a `DataSource` object. In this section, we will create a data source in the WebLogic Server Administration Console. Access the Administration Console with the URL `http://localhost:7001/console`, and in the Administration Console, select the node **Services | JDBC | DataSources**.