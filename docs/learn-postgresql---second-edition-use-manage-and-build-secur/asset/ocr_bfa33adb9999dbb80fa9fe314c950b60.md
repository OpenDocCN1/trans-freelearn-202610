# # Migrating from MySQL/MariaDB to PostgreSQL using pgloader

In this section, we will see how to migrate a database from the MySQL/MariaDB world to the PostgreSQL world in a very simple way. The tool we will use is called pgloader. The references for further information on this section can be found at https://pgloader.io.

Two Docker containers are available for this section, one with a mariadb server inside and another with a postgresql server inside. The mariadb server, called mariadb-source, contains a copy of the forumdb database used in *Chapter 4*; the postgresql server called pg-destination contains an empty database called forumdb. Our goal will be to migrate all the contents of the forumdb database from the mariadb-source server to the postgresql-destination server using the pgloader tool.

Let’s open two Bash terminals, and in the first one, let’s execute:

```
```bash
chapter19$ bash run-pg-docker-mariadb.sh
```
```

Once the container has started, let’s execute the statement below, using LearnPostgreSQL as the password:

```
```text
root@mariadb-source:~# mysql -D forumdb -p
[...]
Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [forumdb]>
```
```

Now we are inside the forumdb database and we can make a query on it:

```
```text
MariaDB [forumdb]> show tables;
+------------------+
| Tables_in_forumdb |
+------------------+
| categories       |
| j_posts_tags     |
| posts            |
```
```