We retrieve the results by calling the `fetchall` method on the cursor object.

Running this code shows us which databases currently exist in our MySQL server instance. As we can see from the output, MySQL ships with several built-in databases such as `information_schema`, and so on. We have successfully created our own `guidb` database, which is shown in the output. All other databases illustrated come shipped with MySQL:

```
MySQL_show_DBS.py
```

![](bbox=[0.046, 0.329, 0.904, 0.4])

Note how, even though we specified the database when we created it in mixed-case letters as GuiDB, the `SHOW DATABASES` command shows all existing databases in MySQL in lower-case and displays our database as `guidb`.

# How it works…

In order to connect our Python GUI to a MySQL database, we first have to know how to connect to the MySQL server. This requires establishing a connection, and this connection will only be accepted by MySQL if we are able to provide the required credentials.

While it is easy to place strings into one line of Python code, when we deal with databases, we have to be really thoughtful because today's personal sandbox development environment could easily end up being accessible on the World Wide Web by tomorrow.

You do not want to compromise database security, and the first part of this recipe showed ways to be more secure by placing the connection credentials to the MySQL server into a separate file and, by placing this file into a location where it is not accessible from the outside world, our database system will become more secure.

In a real-world production environment, the MySQL server installation, connection credentials, and this `dbConfig` file would be handled by IT system administrators who would enable you to import the `dbConfig` file to connect to the MySQL server without you knowing what the actual credentials are. Unpacking `dbConfig` would not expose the credentials as it does in our code.