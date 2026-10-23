There are also a lot of *introspection* commands, such as, for example, \d to list all user-defined tables. These special commands are, under the hood, a way to execute queries against the PostgreSQL system catalogs, which are, in turn, registries about all objects that live in a database. The introspection commands will be shown later in the book and are useful as shortcuts to get an idea of which objects are defined in the current database.

Many psql features will be detailed as you move on through the book, but it is worth spending some time trying to get used to this very efficient and rich command-line client.

# Introducing the connection string

In the previous section, you learned how to specify basic connection options, such as -d and -U for a database and user, respectively. psql also accepts a LibPQ connection string.

LibPQ is the underlying library that every application can use to connect to a PostgreSQL cluster and is, for example, used in C and C++ clients, as well as non-native connectors.

A connection string in LibPQ is a URI made up of several parts:

```
postgresql://username@host:port/database
```

Here, we have the following:

+   - postgresql is a fixed string that specifies the protocol the URI refers to.
+   - username is the PostgreSQL username to use when connecting to the database.
+   - host is the hostname (or IP address) to connect to.
+   - port is the TCP/IP port the server is listening on (by default, 5432).
+   - database is the name of the database to which you want to connect.

The username, port, and database parts can be omitted if they are set to their default (the username is the same as the operating system username).

The following connections are all equivalent:

```
$ psql -d template1 -U luca -h localhost

$ psql postgresql://luca@localhost/template1

$ psql postgresql://luca@localhost:5432/template1
```