And this is the query for the ROWS clause. As we can see, things work exactly as in the previous example without the ORDER BY DESC option:

```
forumdb=> SELECT x,row_number() OVER w, dense_rank() OVER w,sum(x) OVER w
FROM (select generate_series(1,8) % 4 as x) V
WINDOW w AS (ORDER BY x desc ROWS BETWEEN 1 PRECEDING AND CURRENT ROW);
 x | row_number | dense_rank | sum
---+------------+------------+-----
 3 |          1 |          1 |   3
 3 |          2 |          1 |   6
 2 |          3 |          2 |   5
 2 |          4 |          2 |   4
 1 |          5 |          3 |   3
 1 |          6 |          3 |   2
 0 |          7 |          4 |   1
 0 |          8 |          4 |   0
(8 rows)
```

In this example, using the sum function, we can better understand the difference between the RANGE and ROWS options. As we can see, the RANGE option aggregates data by frame(RANGE) while the ROWS option aggregates data by rows. The main difference between the ROWS clause and the RANGE clause is that ROWS operates on individual rows, while RANGE operates on groups. That concludes our chapter on window functions.

# Summary

In this chapter, we explored how to use window functions. We have seen that by using window functions, we can create more complex aggregates compared to those made with the GROUP BY statement, which we saw in Chapter 5, Advanced Statements. We learned how to use the ROW_NUMBER (), FIRST_VALUE (), LAST_VALUE (), RANK DENSE_RANK(), LAG (), LEAD (), CUME_DIST (), and NTILE () functions. We have also seen the difference between creating aggregates with the ROWS BETWEEN and RANGE BETWEEN clauses. You can use what you have learned in this chapter in data mining operations to make your work much easier.

For more information on window functions, you can consult the official documentation: https://www.postgresql.org/docs/current/functions-window.html.