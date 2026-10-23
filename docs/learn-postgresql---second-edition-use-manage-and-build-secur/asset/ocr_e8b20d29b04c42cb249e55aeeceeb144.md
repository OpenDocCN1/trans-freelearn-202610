```
------------------+-------
Data types in C++ | 0
Indexing PostgreSQL | 3
Indexing Mysql | 3
(3 rows)
```

Now let’s suppose that we want to search for all users that have posts with likes greater than 2; a query that solves this problem is:

```
forumdb=> select u.* from users u where exists (select 1 from posts p where u.pk=p.author and likes > 2 );
pk | username | gecos | email
----+----------+-------+-------------
1 | luca_ferrari | | luca@pgtraining.com
(1 row)
```

Let’s suppose now that we want the value of the likes field too. A simple way to solve this problem is using the lateral join:

```
forumdb=> select u.username,q.* from users u join lateral (select author, title,likes from posts p where u.pk=p.author and likes > 2 ) as q on true;
username | author | title | likes
---------+--------+---------------+-------
luca_ferrari | 1 | Indexing PostgreSQL | 3
luca_ferrari | 1 | Indexing Mysql | 3
(2 rows)
```

This query is very similar to the EXISTS query, except the fact that, in the main query, we can have all the values that are in the subquery and we can use them in the main part of the query.

# Aggregate functions

Aggregate functions perform a calculation on a set of rows and return a single row. PostgreSQL provides all the standard SQL aggregate functions:

+   - AVG(): This function returns the average value.
+   - COUNT(): This function returns the number of values.
+   - MAX(): This function returns the maximum value.
+   - MIN(): This function returns the minimum value.
+   - SUM(): This function returns the sum of values.