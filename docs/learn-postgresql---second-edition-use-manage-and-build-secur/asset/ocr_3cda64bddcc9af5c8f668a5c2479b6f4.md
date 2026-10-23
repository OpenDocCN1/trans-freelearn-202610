If we only want the second record that was inserted, we have to perform the following query:

```
forumdb=> select * from categories order by pk offset 1 limit 1;
 pk | title |        description
-----+-------+----------------------------
  2 | Unix  | Unix and Linux discussions
(1 row)
```

offset and limit are very useful when we want to return data in a paged way.

Another valuable function of limit is that it can create a new table from an existing table. For example, if we want to create a table called new_categories starting from the categories table, we have to execute the following statement:

```
forumdb=> create table new_categories as select * from categories limit 0;
SELECT 0
```

This statement will copy into the new_categories table only the data structure of the table categories.

The SELECT 0 clause means that no data has been copied into the new_categories table; only the data structure has been replicated, as we can see here:

```
forumdb=> \d new_categories
               Table "forum.new_categories"
   Column    |  Type   | Collation | Nullable | Default
---------------+---------+-----------+----------+---------
 pk           | integer |           |          |
 title        | text    |           |          |
 description  | text    |           |          |
```

# Using subqueries

In this section, we will talk about subqueries. Subqueries can be described as nested queries – we can nest a query inside another query using parentheses. Subqueries can return a single value or a recordset, just like regular queries. We will start by introducing subqueries using the IN/ NOT IN operator.