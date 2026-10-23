Let’s assume that we want to restore the database content to another local database, which we will name forumdb_restore. Here, the first step is to create a database, as follows:

```
$ psql -c 'CREATE DATABASE forumdb_restore WITH OWNER forum;'
CREATE DATABASE
```

Now, it is possible to connect to the target database and ask psql to execute the entire content of the backup file:

```
$ psql -U forum forumdb_restore
forumdb_restore=> \i backup_forumdb.sql
SET
SET
SET
SET
CREATE SCHEMA
ALTER SCHEMA
SET
SET
CREATE TABLE
ALTER TABLE
ALTER TABLE
CREATE TABLE
ALTER TABLE
CREATE TABLE
ALTER TABLE
CREATE TABLE
ALTER TABLE
CREATE TABLE
ALTER TABLE
ALTER TABLE
CREATE TABLE
ALTER TABLE
ALTER TABLE
CREATE TABLE
ALTER TABLE
ALTER TABLE
INSERT 0 1
```