# ALTER ROLE

And now, every time the luca role connects to the forumdb database, the SET command is automatically executed:

```
$ psql -U luca forumdb

forumdb=> SHOW client_min_messages;

client_min_messages
----------------------
debug
(1 row)
```

The general syntax for changing runtime parameters for a role is as follows:

```
ALTER ROLE name IN DATABASE dbname SET parameter_name TO parameter_value
```

Here, you have to specify the role name or the special keyword ALL for every existing role, the database name, and the name and value of the parameter you want to change.

It is also possible to discard any per-role configuration with the RESET ALL clause, as in the following example:

```
forumdb=# ALTER ROLE luca
    IN DATABASE forumdb
    RESET ALL;
ALTER ROLE
```

# Inspecting roles

There are different ways to inspect existing roles and get information about their properties. One quick approach, as already seen in Chapter 3, Managing Users and Connections, is to use the \du command in psql:

```
forumdb=> \du

                         List of roles
 Role name |          Attributes
-----------+----------------------------
--
book_authors | Cannot login
enrico     |
```