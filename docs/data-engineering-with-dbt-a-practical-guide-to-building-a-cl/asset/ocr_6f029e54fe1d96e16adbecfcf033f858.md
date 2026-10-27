A **cumulative window frame** is a subset of the window that is either growing or shrinking based on the current row inside the window. The frame contains the rows of the window either before or after the current row. The syntax of the cumulative frame definition is the two following lines:

```
```sql
{ROWS | RANGE} BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
| {ROWS | RANGE} BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING
```
```

The first expression takes all the rows in the partition or all the rows in the same order range up to the current one, while the second takes from the current row to the end of the partition or range. A better description of ROWS and RANGE follows.

A **sliding window frame** is a subset of rows that extends from a number of N rows before the current row to a number of M rows after the current row. One or both sides can be fixed to the start or end of the partition.

The following are the three possible forms of the syntax:

```
```sql
ROWS BETWEEN <N> { PRECEDING | FOLLOWING }
          AND <M> { PRECEDING | FOLLOWING }
| ROWS BETWEEN UNBOUNDED PRECEDING AND <M> { PRECEDING | FOLLOWING }
| ROWS BETWEEN <N> { PRECEDING | FOLLOWING } AND UNBOUNDED FOLLOWING
```
```

The first syntax is when you want to create a frame that extends from N rows before to M rows after the current rows, while the other two are the cases when one of the two extremities is fixed to be the start or end of the window.

# ROWS/RANGE in a cumulative window definition

When specifying a cumulative window frame, we can use either ROWS or RANGE, but what is the difference?

+   - **ROWS:** Indicates that the fixed side of the frame extends to the limit of the partition: from the start of the partition to the current row or from the current row to the end of the partition.
+   - **RANGE:** Indicates that the fixed side of the frame extends to the previous/following rows that have the same value for the ORDER BY value. The frame operates on the preceding/following rows that are in the same position in the ordering. As an example, you can think of stepping through many teams that have the same number of points in a championship list.

> **Important note**
> 
> Providing extensive information on window functions is beyond the scope of this introduction to SQL used in analytical operations. It is also one area where SQL is not always the same in all databases. We suggest searching for “window functions” in the documentation of your database.