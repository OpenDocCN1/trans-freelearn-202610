For instance, to change the ownership of a table, you can issue the following command:

```
forumdb=# ALTER TABLE forum.categories OWNER TO luca;
ALTER TABLE
```

Whereas to change the ownership of a function, you can issue the following command:

```
-- equivalent to: ALTER FUNCTION get_max OWNER TO luca;
forumdb=# ALTER ROUTINE forum.get_max OWNER TO luca;
ALTER ROUTINE
```

Similar statements exist for all the other kinds of objects.

Superusers can alter the owner of every database object by setting it to any existing user, while normal users can change the ownership only to roles to which they belong.

# Inspecting ACLs

In order to see which permissions have been granted to roles and objects, you can use the already mentioned psql special command \dp (describe permissions), which reports the ACLs configured for a specific object (a table, for instance). The command performs a query against the special catalog pg_class, which contains a specific field named relacl – an array of ACLs. You can see this as follows:

```
forumdb=> \dp users

                         Access privileges

 Schema |   Name   | Type  |    Access privileges    | Column privileges | Policies
--------+----------+-------+-------------------------+-------------------+----------
 forum  | users    | table | forum=arwdDxt/forum     | gecos:            +
        |          |       |                         | forum_stats=w/forum |
(1 row)

forumdb=> SELECT relname, relacl
         FROM pg_class WHERE relname = 'users';
 relname |      relacl
---------+---------------
 users   | {forum=arwdDxt/forum}
```