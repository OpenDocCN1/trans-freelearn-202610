If running the preceding code results in the following output printed to the console, then we are good:

```
MySQL_connect.py

<terminated> C:\Eclipse_NEON_workspace\2nd Edition Python GUI Programming Cookbook\Ch07_Code\MySQL_connect.py
<_mysql.connection open to '127.0.0.1' at 605769b8>
```

If you are not able to connect to the MySQL server via the *Command Line Client* or the Python *mysqlclient*, then something probably went wrong during the installation. If this is the case, try uninstalling, rebooting your PC, and then running the installation again.

# How it works...

In order to connect our GUI to a MySQL server, we need to be able to connect to the server with administrative privileges if we want to create our own database. If the database already exists, then we just need the authorization rights to connect, insert, update, and delete data. We will create a new database on a MySQL server in the next recipe.

# Configuring the MySQL database connection

In the previous recipe, we used the shortest way to connect to a MySQL server by hardcoding the credentials required for authentication into the `connection` method. While this is a fast approach for early development, we definitely do not want to expose our MySQL server credentials to anyone. Instead, we *grant* permission to access databases, tables, views, and related database commands to specific users.

A much safer way to get authenticated by a MySQL server is by storing the credentials in a configuration file, which is what we will do in this recipe. We will use our configuration file to connect to the MySQL server and then create our own database on the MySQL server.

![](bbox=[0.079, 0.787, 0.157, 0.85])

We will use this database in all of the following recipes.