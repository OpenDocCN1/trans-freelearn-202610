# Creating a MySQL user and database

When we first installed MySQL on our development machine, it created a root user who can manage the database system. This user has complete access to the MySQL server, including all databases and tables.

Furthermore, beginning with MySQL v5.7, the root user can be authenticated via the auth socket plugin rather than a username and password, as allowed by the MySQL client used with TypeORM.

> **Important Note**
> 
> The name of the system user that launches the MySQL client and the MySQL user must be the same when using the auth socket plugin; moreover, you must execute mysql with sudo privileges to have access to your database(s).

For the reasons stated above, it is required to create a new user in MySQL and provide it with the right permissions. To do so, go through the following steps:

+   1. To begin, use the following command to connect to MySQL:
+   2. Once you've accessed the MySQL prompt, you may use SQL commands such as CREATE USER to create a new user with a password that utilizes the mysql native password plugin for authentication:
+   3. We must provide it with the necessary permissions using the following command:
+   4. Then, use the following command to reload all of the privileges:

```
sudo mysql
```

You'll be asked to enter a password. Enter your system's root password.

```
CREATE USER 'dbuser'@'localhost' IDENTIFIED WITH mysql_native_password BY 'p4ssw0rd';
```

In this case, we created a user called dbuser with the password p4ssw0rd.

```
GRANT ALL PRIVILEGES ON * . * TO dbuser@'localhost';
```

We have given the user all of the privileges (that is, full root access to everything in our database). This is not advised for security reasons. However, keep in mind that this is simply a test database.

```
FLUSH PRIVILEGES;
```