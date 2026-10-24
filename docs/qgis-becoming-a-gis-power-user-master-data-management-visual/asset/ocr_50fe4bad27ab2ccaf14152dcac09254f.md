# # Creating a new Postgres database

Now that we installed the Postgres database server with PostGIS, the pgRouting extensions, and the pgAdmin III client program, we want to create a new database where we can work. Perform the following steps:

+   1. Open the pgAdmin III program.
+   2. Right-click on the **Databases** section of the **Servers** tree and click on **New Database...** to create the database, as shown in the following screenshot:

![](bbox=[0.352, 0.298, 0.649, 0.457])

+   3. Enter a name for the new database, **packt**, and click on **OK**.

# # Registering the PostGIS and pgRouting extensions

Next, we need to tell Postgres that we want to use the PostGIS and pgRouting extensions with our new database. Perform the following steps:

+   1. Open pgAdmin III if you haven't already done so.
+   2. Navigate to **Tools | Query**.
+   3. Enter the following SQL in the **SQL Editor** area:

```
CREATE EXTENSION postgis;
CREATE EXTENSION pgrouting;
```

+   4. From the **Query** menu, choose **Execute**.