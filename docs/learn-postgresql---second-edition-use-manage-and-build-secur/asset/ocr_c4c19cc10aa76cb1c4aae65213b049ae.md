As we can see, the lag function returns a result set with an offset value equal to 1. If we introduce an offset parameter, the lag function will return a result set with an offset equal to the number that we have passed as input, as can be seen in the next example:

```
forumdb=> select x,lag(x,2) over w from (select generate_series(1,5) as x)
V WINDOW w as (order by x) ;
 x | lag
----+-----
 1 |
 2 |
 3 | 1
 4 | 2
 5 | 3
(5 rows)
```

The lead function is the opposite of the lag function, as described in the official documentation:

> “The LEAD function returns the value evaluated at the row that is offset rows after the current row within the partition; if there is no such row, it instead returns the default (which must be of the same type as the mentioned value). Both the offset and default are evaluated with respect to the current row. If omitted, the offset defaults to 1 and the default becomes null.”

Here are a couple of examples where we can see how it works. In the first example, we will use the lead function without any parameters:

```
forumdb=# select x,lead(x) over w from (select generate_series(1,5) as x)
V WINDOW w as (order by x) ;
 x | lead
----+------
 1 | 2
 2 | 3
 3 | 4
 4 | 5
 5 |
(5 rows)
```

As we can see in the lead function, the offset starts from the bottom.