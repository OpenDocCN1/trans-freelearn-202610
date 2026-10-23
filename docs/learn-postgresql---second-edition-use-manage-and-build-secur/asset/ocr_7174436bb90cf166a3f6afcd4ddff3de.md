As we can see, on the right of the image, we have two further pointers: a green one for the **UNBOUNDED PRECEDING** clause and an orange pointer for the **CURRENT ROW** clause. The result is 1, so in the first step both point to the first row. Now, let’s see what happens in the next steps.

+   2. In this step, the **UNBOUNDED PRECEDING** pointer still points to the first row, whereas the **CURRENT ROW** pointer now points to the second row, and the result of the sum is 1+2 = 3:

| x | sum(x) |
|---|--------|
| 1 | 1      |
| 2 | 3      |
| 3 |        |
| 4 |        |
| 5 |        |

STEP 2

UNBOUNDED PRECEDING

CURRENT ROW

ORDER BY

Figure 6.4: The unbounded preceding and current row (1)

+   3. Next, the **UNBOUNDED PRECEDING** pointer still points to the first row, whereas the **CURRENT ROW** pointer points to the third row, and the result of the sum is 1+2+3 = 6: