5. And in the fifth and final step, we have the desired result:

| x | sum(x) |
|---|--------|
| 1 | 1 |
| 2 | 3 |
| 3 | 6 |
| 4 | 10 |
| 5 | 15 |

STEP 5

UNBOUNDED PRECEDING

CURRENT ROW

ORDER BY

Figure 6.7: The unbounded preceding and current row (4)

That is how a frameset clause works!

Let’s look at some more examples of how the frame clause works using different options. If for each row of the table we wanted to find the sum of the current row with the preceding row, we would start with the following:

| X |
|---|
| 1 |
| 2 |
| 3 |
| 4 |
| 5 |

We want to end up with the following result:

| x | sum(x) |
|---|--------|
| 1 | 1 |
| 2 | 3 |
| 3 | 5 |
| 4 | 7 |
| 5 | 9 |