# STEP 3

| x | sum(x) |
|---|--------|
| 1 | 1 |
| 2 | 3 |
| 3 | 6 |
| 4 |  |
| 5 |  |

UNBOUNDED PRECEDING

CURRENT ROW

ORDER BY

Figure 6.5: The unbounded preceding and current row (2)

4. In the fourth step, the UNBOUNDED PRECEDING pointer still points to the first row, whereas the CURRENT ROW pointer now points to the fourth row, and the result of the sum is 1+2+3+4 = 10:

# STEP 4

| x | sum(x) |
|---|--------|
| 1 | 1 |
| 2 | 3 |
| 3 | 6 |
| 4 | 10 |
| 5 |  |

UNBOUNDED PRECEDING

CURRENT ROW

ORDER BY

Figure 6.6: The unbounded preceding and current row (3)