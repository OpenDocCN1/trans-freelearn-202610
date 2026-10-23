For further information about partitioning see https://www.postgresql.org/docs/current/ddl-partitioning.html.

# Table inheritance

Another topic that we must look at is the inheritance of tables. PostgreSQL employs the concept of inheritance from databases to objects. The concept is very simple and can be summarized as follows: suppose we have two tables, TABLE A and TABLE B. If we define TABLE A as a parent table and TABLE B as the child table, this means that all the records in TABLE B will be accessible from TABLE A.

Let’s now try to give an example of what we have just described:

+   1. Let’s define two tables.

The first table, the parent table, is defined as follows:

```
forumdb=> create table table_a (
 pk integer not null primary key,
 tag text,
 parent integer);
CREATE TABLE
```

And the second table, the child table, is defined as follows:

```
forumdb=> create table table_b () inherits (table_a);
CREATE TABLE

forumdb=> alter table table_b add constraint table_b_pk primary key(pk);
ALTER TABLE
```

+   2. The child table inherits all the fields from the parent table. The parent table is as seen here:

```
forumdb=> \d table_a;
               Table "forum.table_a"
 Column | Type | Collation | Nullable | Default
--------+----------+------------+----------+---------
 pk     | integer |            | not null |
```