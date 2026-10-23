As you can see, all description values are sorted and NULL values are positioned at the end of the result set. The same thing can be achieved by running the following:

```
forumdb=> select * from categories order by description NULLS last;
pk | title | description
----+---------------+-----------------------------
  3 | Programming Languages | All about programming languages
  1 | Database | Database related discussions
  4 | C Language | Languages
  5 | Python Language | Languages
  2 | Unix | Unix and Linux discussions
  6 | A new discussion | NULL
(6 rows)
```

2. If we want to place NULL values at the beginning, we have to perform the following:

```
forumdb=> select * from categories order by description NULLS first;
pk | title | description
----+---------------+-----------------------------
  6 | A new discussion | NULL
  3 | Programming Languages | All about programming languages
  1 | Database | Database related discussions
  4 | C Language | Languages
  5 | Python Language | Languages
  2 | Unix | Unix and Linux discussions
(6 rows)
```

> If not specified, the following are the default actions for ORDER BY type queries:
> 
> ORDER BY NULLS LAST is the default for ASC (which is also the default) and NULLS FIRST is the default for DESC.

# Creating a table starting from another table

We will now examine how to create a new table using data from another table. To do this, you need to create a temporary table with the data present in the categories table as follows:

```
forumdb=> create temp table temp_categories as select * from categories;
SELECT 6
```