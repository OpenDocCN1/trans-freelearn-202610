# Auditing by session

The first way of using PgAudit (and the most simple to understand and try) is by session.

As with other configuration settings, you can also set the PgAudit configuration by means of the SET SQL statement. This is useful to test the configuration before applying it to the whole cluster. Try setting pgaudit.log directly in your interactive session and perform some actions to see what happens. As an example, suppose we want to audit any changes to the data:

```
forumdb=# SET pgaudit.log TO 'write, ddl';
SET
forumdb=# SELECT count(*) FROM forum.categories;
 count
-------
     3
(1 row)

forumdb=# INSERT INTO forum.categories( description, title ) VALUES(
 'Fake', 'A Malicious Category' );
INSERT 0 1

forumdb=# SELECT count(*) FROM forum.categories;
 count
-------
     4
(1 row)

forumdb=# INSERT INTO forum.categories( description, title ) VALUES(
 'Fake2', 'Another Malicious Category' );
INSERT 0 1
```

The pgaudit.log parameter can be set only by superusers, therefore if you want to try it dynamically in an interactive session, you need to connect as a database administrator. You can, of course, set this for all users at a cluster-wide level by setting the parameter in the postgresql.conf configuration file.