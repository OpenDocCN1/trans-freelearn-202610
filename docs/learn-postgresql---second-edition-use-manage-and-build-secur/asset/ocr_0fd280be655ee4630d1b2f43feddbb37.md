4;A.I;Machine Learning discussions

As you can see from the above example, all the fields are now separated by a semicolon, and the first row in the file is the table column list. It is worth noting that COPY already comes with a defined CSV format, which can be specified with the option FORMAT csv:

```
```sql
forumdb=# COPY forum.categories TO '/tmp/categories.csv'
WITH ( FORMAT csv );
COPY 5
```
```

Having a CSV file containing the data means that COPY FROM can load the data into a table. In other words, COPY is not only an efficient bulk loader; it is also a useful tool to load data into the database from external resources like spreadsheets.

Imagine we need to load the data contained in the categories.csv file into another table, named categories_reloaded:

```
```sql
forumdb=# CREATE TABLE forum.categories_reloaded( LIKE forum.categories );
CREATE TABLE
forumdb=# COPY forum.categories_reloaded
             FROM '/tmp/categories.csv'
             WITH (FORMAT csv);
COPY 5
```
```

It is also possible to specify a WHERE clause, in order to filter what is going to be loaded; as an example, imagine we want to load only odd rows:

```
```sql
forumdb=# COPY forum.categories_reloaded
             FROM '/tmp/categories.csv'
             WITH (FORMAT csv)
             WHERE pk % 2 = 1;
```
```

The COPY TO does not allow for a WHERE clause, but it is possible to copy from a query, which does the trick of filtering tuples:

```
```sql
forumdb=# COPY
    ( SELECT * FROM forum.categories
      WHERE pk % 2 = 1 )
    TO '/tmp/categories.odd.csv'
    WITH (FORMAT csv);
```
```