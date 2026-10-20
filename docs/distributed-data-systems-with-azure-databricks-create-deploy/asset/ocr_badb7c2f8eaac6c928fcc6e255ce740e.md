# Relationships and columns

Skew join optimization is a computationally intensive operation that must be run only when it is required. That is why the argument of SKEW is column names. When running joins, it is useful to then just run SKEW on the column that we are using to do this.

In the following code example, we apply the skew hint to a single column, which is user_id:

```
SELECT /*+ SKEW('orders', user_id) */ *
FROM orders, users
WHERE user_id = our_condition
```

We can also apply the hint to multiple columns, as follows:

```
SELECT /*+ SKEW('orders', ('user_id', 'o_region_id')) */ *
FROM orders, users
WHERE user_id = our_condition AND o_region_id= c_region_id
```

You can also specify skew values in the hint. Depending on the query and data, the skew values might be known (for example, because they never change) or might be easy to find out. Doing this reduces the overhead of skew join optimization. Otherwise, Delta Lake detects them automatically.

Values can be also specified on the hints, depending on the type of query. The more specific the parameters passed to SKEW, the less overhead there will be for the optimization task.

In the following code example, we run a query on a single column specifying a single skew value:

```
SELECT /*+ SKEW('orders', 'user_id', 0) */ *
FROM orders, users
WHERE user_id = our_condition
```

We can also pass several skew values, as follows:

```
SELECT /*+ SKEW('orders', 'user_id', (0, 1, 2)) */ *
FROM orders, users
WHERE user_id = our_condition
```