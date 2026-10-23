Aggregate functions are used in conjunction with the group by clause. A group by clause splits a resultset into groups of rows and aggregate functions perform calculations on them. For example, if we wanted to count how many records there are for each category, PostgreSQL first groups the data and then counts it. The following diagram illustrates the process:

![](bbox=[0.22, 0.22, 0.75, 0.5])

Figure 5.6: Group by aggregation

This diagram illustrates that PostgreSQL, before grouping the data, sorts it internally. Therefore, we must remember that a grouping operation always implies an ordering operation; this will become more clear when we discuss performance later on.

Now that we have understood the theory, let’s address how to actually calculate how many records there are for each category:

```
forumdb=> select category,count(*) from posts group by category;
 category | count
----------+-------
        3 |     1
        1 |     2
(2 rows)
```

The preceding query counts how many records there are for each category in the posts table.

Another way to write the same query is as follows:

```
forumdb=> select category,count(*) from posts group by 1;
 category | count
```