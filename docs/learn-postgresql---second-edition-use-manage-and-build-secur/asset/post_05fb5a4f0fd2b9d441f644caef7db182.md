
## Using advanced statement window functions

In this section, we will discuss advanced window functions in detail, and we will explore some techniques that may be useful for carrying out more detailed data analysis.

Let’s start with a query that we saw at the start of this chapter:

```sql
forumdb=> select distinct category, count(*) over (partition by category)
from posts order by category;
  category | count
-----------+-------
         1 |     2
         3 |     1
(2 rows)
```

Here, below, there is another way to write the same aggregate that we described before:

```sql
forumdb=> select distinct category, count(*) over w1
from posts WINDOW w1 as (partition by category RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
order by category;
  category | count
-----------+-------
         1 |     2
         3 |     1
(2 rows)
```

What does RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW mean? They are the default conditions, known as the frame clause. This means that the data is partitioned, first by category, and then within the partition, and the count is calculated by resetting the count every time the frame is changed.

## The frame clause

In this section, we’ll talk about the frame clause, which allows us to manage partitions in a different way. The frame clause has two forms:

- Rows between start_point and end_point
- Range between start_point and end_point

It only makes sense to use the frame clause if the order by clause is also present. We will use the ROWS BETWEEN clause when we are going to consider a specific set of records relative to the current row. We will use the RANGE BETWEEN clause when we are going to consider a range of values in a specific column relative to the value in the current row.

### ROWS BETWEEN start_point and end_point

Now we will look at some simple examples to try to better explain the frame clauses. These are typically used to do in-depth data analysis and data mining, among other tasks. Let’s start with some examples, beginning here:

```sql
forumdb=> select x from (select generate_series(1,5) as x) V WINDOW w as (order by x);
 x
---
1
2
3
4
5
(5 rows)
```

Suppose that we want to have an incremental sum row by row. The goal that we want to reach is as follows:

| x | sum(x) |
|---|--------|
| 1 | 1 |
| 2 | 3 |
| 3 | 6 |
| 4 | 10 |
| 5 | 15 |

This can be achieved using the following query:

```sql
forumdb=> select x, sum(x) over (order by x) from generate_series(1,5) as x;
 x | sum
----+-----
 1 |  1
 2 |  3
 3 |  6
 4 | 10
 5 | 15
(5 rows)
```

The same query can be written in this way:

```sql
forumdb=> SELECT x, SUM(x) OVER w
  FROM (select generate_series(1,5) as x) V
  WINDOW w AS (ORDER BY x ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW);
  x | sum
  ----+-----
  1 | 1
  2 | 3
  3 | 6
  4 | 10
  5 | 15
  (5 rows)
```

Now, let's imagine that the query was executed in successive steps, one for each row of the table. In the following diagrams, we will simulate the internal behavior of PostgreSQL, to better understand how the clause ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW works:

1. First, PostgreSQL uses the order_by_clause condition to order the data inside the window, as shown by the blue arrow in the following diagram:

   ![](img/bbe5644feba3d6a2ab994dc7e6cecbf1_201_0.png)

   As we can see, on the right of the image, we have two further pointers: a green one for the **UNBOUNDED PRECEDING** clause and an orange pointer for the **CURRENT ROW** clause. The result is 1, so in the first step both point to the first row. Now, let’s see what happens in the next steps.

2. In this step, the **UNBOUNDED PRECEDING** pointer still points to the first row, whereas the **CURRENT ROW** pointer now points to the second row, and the result of the sum is 1+2 = 3:

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

3. Next, the **UNBOUNDED PRECEDING** pointer still points to the first row, whereas the **CURRENT ROW** pointer points to the third row, and the result of the sum is 1+2+3 = 6:

   STEP 3

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

   STEP 4

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

That is how a frame clause works!

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

The query that we have to perform is described in the following example:

```sql
forumdb=> SELECT x, SUM(x) OVER w
    FROM (select generate_series(1,5) as x) V
    WINDOW w AS (ORDER BY x RANGE BETWEEN 1 PRECEDING AND CURRENT ROW);
    x | sum
    ----+-----
    1 |  1
    2 |  3
    3 |  5
    4 |  7
    5 |  9
    (5 rows)
```

The preceding query works similarly to what we saw before. The only difference is that now the calculation range is between the first row and the current row of the partition, as written in the statement BETWEEN 1 PRECEDING AND CURRENT ROW. In this example, only two lines are used to calculate the sum. The same mechanism can be used to perform an incremental sum, as we can see in the following example:

```sql
forumdb=> SELECT x, SUM(x) OVER w
    FROM (select generate_series(1,5) as x) V
    WINDOW w AS (ORDER by x ROWS UNBOUNDED PRECEDING);
    x | sum
    ----+-----
    1 |  1
    2 |  3
    3 |  6
    4 | 10
    5 | 15
    (5 rows)
```

Now the only difference is that the calculation range is by ROWS UNBOUNDED PRECEDING and not BETWEEN 1 PRECEDING AND CURRENT ROW.

Let’s look at another example where window functions simplify our work. Always starting from the series that we’ve seen before, we know that the total sum is 1+2+3+4+5 = 15. So now suppose that we want to do a reverse sum starting from the max value of the table, that is, 5.

In this example, we want the result to be as follows:

| x | sum(x) |
|---|--------|
| 1 | 15 |
| 2 | 14 |
| 3 | 12 |
| 4 | 9 |
| 5 | 5 |

The query that makes this possible is the following:

```sql
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

- In the first row, all values are added: 1+2+3+4+5 = 15.
- In the second row, these values are added: 2+3+4+5 = 14.
- In the third row, these values are added: 3+4+5 = 12.

### RANGE BETWEEN start_point and end_point

As discussed earlier, when we use RANGE BETWEEN, we will consider a RANGE of values with respect to the value in the current row. The difference when it comes to the ROWS clause is that if the field that we use for ORDER BY does not contain unique values for each row, then RANGE will combine all the rows it comes across with non-unique values, rather than processing them one at a time.
