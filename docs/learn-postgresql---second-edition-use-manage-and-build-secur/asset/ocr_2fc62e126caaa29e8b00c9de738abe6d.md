| 2 | 6 | 6 | 3 |
| 3 | 7 | 10 | 4 |
| 3 | 8 | 10 | 4 |

As we can see, there are four frames in the last table, so internally, PostgreSQL works in this way: first, PostgreSQL splits the window function into frames using the order by clause and then aggregates the data among the frames; for example:

+   - The sum of row number 3 is the result of the sum of row number 1 + row number 2 + row number 3 + row number 4: 0+0+1+1=2.
+   - The sum of row number 4 is the result of the sum of row number 1 + row number 2 + row number 3 + row number 4: 0+0+1+1=2.
+   - The sum of row number 5 is the result of the sum of row number 3 + row number 4 + row number 5 + row number 6: 1+1+2+2=6.
+   - The sum of row number 6 is the result of the sum of row number 3 + row number 4 + row number 5 + row number 6: 1+1+2+2=6.

In the preceding example, we have considered a partition ordered in an ascending way. In the next example, the partition is sorted in a descending way and we will see the difference between ROWS and RANGE in this scenario.

This is the query for the RANGE clause:

```
forumdb=> SELECT x,row_number() OVER w, dense_rank() OVER w,sum(x) OVER w
FROM (select generate_series(1,8) % 4 as x) V
WINDOW w AS (ORDER BY x desc RANGE BETWEEN 1 PRECEDING AND CURRENT ROW);
 x | row_number | dense_rank | sum
----+-------------+------------+-----
 3 |           1 |          1 |   6
 3 |           2 |          1 |   6
 2 |           3 |          2 |  10
 2 |           4 |          2 |  10
 1 |           5 |          3 |   6
 1 |           6 |          3 |   6
 0 |           7 |          4 |   2
 0 |           8 |          4 |   2
(8 rows)
```