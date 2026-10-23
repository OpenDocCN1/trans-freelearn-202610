In order to allow the user luca to create new objects within the schema, the CREATE permission has to be granted. However, without the USAGE permission, the role will not be able to access anything in the schema, so you need to provide both permissions at the same time:

```
-- as user 'forum'
forumdb=> GRANT CREATE ON SCHEMA configuration TO luca;
GRANT

forumdb=> GRANT USAGE ON SCHEMA configuration TO luca;
GRANT
```

Therefore, the luca role can now create a new object within the schema:

```
-- as user 'luca'
forumdb=> CREATE TABLE configuration.conf( param text,
    value text,
    UNIQUE (param) );
CREATE TABLE

forumdb=> INSERT INTO configuration.conf
    VALUES( 'posts_per_page', '10' );
INSERT 0 1
```

Without the USAGE permission, a role is no longer able to access any object within the schema, even if it is the owner of the object:

```
-- as role 'forum'
forumdb=> REVOKE USAGE ON SCHEMA configuration FROM luca;
REVOKE
```

In fact, a user can no longer read their own data:

```
-- as role 'luca'
forumdb=> SELECT * FROM configuration.conf;
ERROR: permission denied for schema configuration
LINE 1: SELECT * FROM configuration.conf;
```

On the other hand, it is common to allow a role to manipulate data contained in a specific schema while not granting it the capability to create new database objects like tables.