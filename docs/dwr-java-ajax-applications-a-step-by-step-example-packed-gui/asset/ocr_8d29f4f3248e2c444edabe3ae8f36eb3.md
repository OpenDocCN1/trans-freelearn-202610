# 6

# Backend Integration

This chapter is on integrating web applications with various backend services. Almost all applications involve a database, so the first example in this chapter shows one way of accessing database from the DWR application. The second example integrates with a standard web service while the third example uses JMS messaging to access a backend service. The JMS example also shows how to do Reverse AJAX (Comet) using DWR.

This chapter includes the following sections:

+   - Integrating a Database with DWR — shows how DWR is used to access a database
+   - Integrating with web services — uses Eclipse tooling to generate a web service client for our DWR application
+   - Integrating with a Messaging System — integrates our DWR application with the Python-based "order system" using Active MQ and JMS

# Integrating a Database with DWR

Here, we integrate a database with DWR by exposing a database table via a remoted Java class. Our Java class connects directly to the database using JDBC, and related SQL statements are included in the class. Other ways to access databases include EJBs, Hibernate, and numerous others.

The example here replaces the first example in Chapter 4, Tables and Lists, so that country data comes from a database instead of a CSV file. From the application point of view there is only one change required in the dwr.xml file and that is changing the entry to point to the Java class that handles database access. Before we implement the Java class we have to set up and populate the database. Here we use Apache Derby database, which comes bundled with the Geronimo application server.