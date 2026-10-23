```
pg_stat_statements | 1.5
pg_stat_statements | 1.6
pg_stat_statements | 1.8
pg_stat_statements | 1.9
pg_stat_statements | 1.7
pg_stat_statements | 1.10
```

It is useful to know that the pg_stat_statements extension can be installed in a version between 1.4 and 1.10 depending on your needs.

> You should always install the latest version of an extension, that is, the one with the highest version number, unless you are forced to install a specific version for backward compatibility.

# Altering an existing extension

The ALTER EXTENSION statement is very rich and complex and allows you to fully modify an existing extension. The statement allows four main changes to an existing extension:

+   - Upgrading the extension to a new version
+   - Setting the schema of a relocatable extension
+   - Adding a database object to the extension
+   - Removing a database object from the extension

In order to upgrade an already installed extension, you must specify the UPDATE clause, specifying the target version number. As an example, consider the pg_stat_statements extension presented before, and assume we install version 1.6 of it for the sake of upgrading it. In order to update the extension to version 1.10, it is possible to issue an ALTER EXTENSION statement as the following example shows:

```
forumdb=# CREATE EXTENSION
    pg_stat_statements WITH VERSION '1.6';
CREATE EXTENSION

forumdb=# ALTER EXTENSION pg_stat_statements
    UPDATE TO '1.10';
ALTER EXTENSION
```