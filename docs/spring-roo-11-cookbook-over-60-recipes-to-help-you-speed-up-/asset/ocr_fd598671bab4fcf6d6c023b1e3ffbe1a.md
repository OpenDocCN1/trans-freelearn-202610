# Getting ready

Refer to the Setting up a JPA provider for your project recipe to create the flight-app Roo project and to set up a persistence provider using the persistence setup command.

Start the Roo shell from the C:\\roo-cookbook\\ch02-recipes directory.

> You won't need this recipe if you're using JNDI-bound data source
> in your Roo project.

# How to do it...

The following steps will show you how to add, modify, or remove properties:

+   1. The database properties set command is useful if you want to modify properties
+   defined in the database.properties file, as shown here:
+   roo> database properties set --key database.username --value root
+   roo> database properties set --key database.password --value
+   asarin

+   2. The database properties set command can also be used to add new properties
+   to the database.properties file, as shown here:
+   roo> database properties set --key database.modified.url --value
+   jdbc:mysql://localhost:3406/myFlightAppDB
+   roo> database properties set --key database.initialPoolSize
+   --value 10

+   3. The database properties remove command is for removing an existing property
+   from the database.properties file, as shown here:
+   roo> database properties remove --key database.url

# How it works...

The database properties set and database properties remove commands are
processed by JPA add-on. Instead of using these commands you can use the properties
set and properties remove command also, as shown in the Managing properties defined
in a properties file recipe in Chapter 1, Getting Started with Spring Roo

It is important to note that if you modify the names of properties in the database.
properties file, then these modifications must be reflected in the dataSource bean
defined in the applicationContext.xml file of your Roo project.