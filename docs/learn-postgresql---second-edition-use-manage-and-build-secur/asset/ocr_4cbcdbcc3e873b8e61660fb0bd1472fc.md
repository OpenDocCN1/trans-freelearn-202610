In this example, we want the result to be as follows:

| x | sum(x) |
|---|--------|
| 1 | 15 |
| 2 | 14 |
| 3 | 12 |
| 4 | 9 |
| 5 | 5 |

The query that makes this possible is the following:

```
forumdb=> SELECT x, SUM(x) OVER w
FROM (select generate_series(1,5) as x) V
WINDOW w AS (ORDER BY X ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING);
x | sum
----+-----
1 | 15
2 | 14
3 | 12
4 | 9
5 | 5
(5 rows)
```

What makes this possible is the UNBOUNDED FOLLOWING clause, which works the opposite way to UNBOUNDED PRECEDING. This happens because of the following:

+   - In the first row, all values are added: 1+2+3+4+5 = 15.
+   - In the second row, these values are added: 2+3+4+5 = 14.
+   - In the third row, these values are added: 3+4+5 = 12.

# RANGE BETWEEN start_point and end_point

As discussed earlier, when we use RANGE BETWEEN, we will consider a RANGE of values with respect to the value in the current row. The difference when it comes to the ROWS clause is that if the field that we use for ORDER BY does not contain unique values for each row, then RANGE will combine all the rows it comes across with non-unique values, rather than processing them one at a time.