
The preceding figure represents the join types that we have seen in two forms:

1. Using only the ON subclause, showing the results of the pure join
2. Using a WHERE clause on the column used in the ON subclause

In the figure, this information is used to exclude from the outer joins the rows where the match happens, where A and B overlap, which are the rows returned by an inner join.

This type of query is useful, and often used, for example, to see whether we have orders where the referenced customer does not exist in the customer table. This would be called an orphan key in the order table.

Let’s see an example using Snowflake sample data:

```sql
SELECT *
FROM "SNOWFLAKE_SAMPLE_DATA"."TPCH_SF1"."ORDERS"
LEFT OUTER JOIN "SNOWFLAKE_SAMPLE_DATA"."TPCH_SF1"."CUSTOMER"
    ON C_CUSTKEY = O_CUSTKEY
WHERE C_CUSTKEY is NULL;
```

This query should return no rows, as all the customers referenced by the orders should exist in the customer table. Now that we have covered all the basic functions in SQL, let us check out an advanced feature: window functions.

## Advanced – introducing window functions

A window function is a SQL function that operates on a set of rows, called a window of rows.

These functions are also called analytic functions, as they are mostly used in analytic queries.

A window is a set of correlated rows, containing any amount from 0 to all the rows resulting from a query.

Generally, the windows divide the resulting rows into disjoint partitions.

Some window functions can also operate on a window frame, which is a dynamic subset of the rows defined by the window, based on the current row.

You can define a cumulative window frame, taking the rows of the window before or after the current row, or a sliding window frame, going from some rows before the current row to some rows after the current row.

Window functions have a lot in common with aggregate functions, the ones that use GROUP BY, as they both divide the results of a query into groups, that is, partitions, and then calculate the function on the rows in each group.

To use a window function, you use this syntax:

```
<function_name> ([<arguments>])
    OVER ( [PARTITION BY <part_expr>] [ORDER BY <order_expr>])
```

The presence of the OVER clause specifies that the function is applied to the windows defined inside the OVER clause. Of course, the function can also take its own arguments.

The PARTITION BY clause defines how the rows are partitioned and `<part_expr>` is a comma-separated list of columns or expressions calculated on the query data, in a similar way to what we have seen for the group by expressions and shown in the previous example.

The ORDER BY clause specifies the ordering of the rows to be used to calculate the function and it is not visible outside of the window function. The order by clause and `<order_expr>` work pretty much as they do in the SELECT statement to order the results of the query.

At the syntax level, both the PARTITION BY and ORDER BY clauses are optional, even if some functions require or forbid one of the two. Look at each function's documentation for more details.

The ORDER BY clause has an effect on the order-sensitive window function, which comes in two types:

- **Rank-related functions**: These functions calculate values based on the rank of the row, which is the order of the row in the window. They are used to make explicit, that is, crystallized into a value that can be used later, one of many possible orderings of a row in a window.

  One example is the ordering of teams in a championship based on points or medals in a sport: you can have different styles of ordering based on what is decided when you have the same value.

  As an example, if two teams (A and B) have the same score, after another team (C) with an higher score, you can say that A and B are both second or you can say that one is second and the other one third.

- **Window frame functions**: These functions work on a window frame, which is a dynamic subset of rows of a window. As we saw previously, they can use cumulative or sliding window frames. The next section will delve into these.

## Window frame definition

Some functions can use a dynamic window frame instead of a fixed window.

You specify the window frame inside the OVER clause, after the ORDER BY clause of the window:

```
<function_name> ([<arguments>])
    OVER ( [PARTITION BY <part_expr>]
            ORDER BY <order_expr>
            <cumulative_frame_def> | <sliding_frame_def>
    )
```

A **cumulative window frame** is a subset of the window that is either growing or shrinking based on the current row inside the window. The frame contains the rows of the window either before or after the current row. The syntax of the cumulative frame definition is the two following lines:

```sql
{ROWS | RANGE} BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
| {ROWS | RANGE} BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING
```

The first expression takes all the rows in the partition or all the rows in the same order range up to the current one, while the second takes from the current row to the end of the partition or range. A better description of ROWS and RANGE follows.

A **sliding window frame** is a subset of rows that extends from a number of N rows before the current row to a number of M rows after the current row. One or both sides can be fixed to the start or end of the partition.

The following are the three possible forms of the syntax:

```sql
ROWS BETWEEN <N> { PRECEDING | FOLLOWING }
          AND <M> { PRECEDING | FOLLOWING }
| ROWS BETWEEN UNBOUNDED PRECEDING AND <M> { PRECEDING | FOLLOWING }
| ROWS BETWEEN <N> { PRECEDING | FOLLOWING } AND UNBOUNDED FOLLOWING
```

The first syntax is when you want to create a frame that extends from N rows before to M rows after the current rows, while the other two are the cases when one of the two extremities is fixed to be the start or end of the window.

### ROWS/RANGE in a cumulative window definition

When specifying a cumulative window frame, we can use either ROWS or RANGE, but what is the difference?

- **ROWS:** Indicates that the fixed side of the frame extends to the limit of the partition: from the start of the partition to the current row or from the current row to the end of the partition.
- **RANGE:** Indicates that the fixed side of the frame extends to the previous/following rows that have the same value for the ORDER BY value. The frame operates on the preceding/following rows that are in the same position in the ordering. As an example, you can think of stepping through many teams that have the same number of points in a championship list.

> **Important note**
>
> Providing extensive information on window functions is beyond the scope of this introduction to SQL used in analytical operations. It is also one area where SQL is not always the same in all databases. We suggest searching for “window functions” in the documentation of your database.

## Summary

In this chapter, you were introduced to SQL, starting with the basics and working up to some quite advanced topics. You also got hands-on with working with Snowflake in the cloud.

You learned about the different SQL commands and categories, opened an account in Snowflake, and created your first database with users and roles. You then got into the details of the possible clauses and operators used in writing SELECT queries and tried them with Snowflake sample data. You then analyzed in more detail how to use the JOIN clause to blend data from multiple tables. We closed the chapter by presenting the window functions, which open up new avenues for our queries and make easy and efficient many complex calculations.

In the next chapter, *Setting Up Your dbt Cloud Development Environment*, we will create our first dbt models, setting up dbt and GitHub in the cloud.

## Further reading

In this chapter, we have described the most important SQL topics, but there is much more that you can read if you want to look deeper and become proficient with SQL.

The Snowflake documentation is free to use and provides good explanations and examples, and of course, is the reference source of knowledge for the Snowflake dialect of SQL.

You can start with these pages:

- SQL command reference: https://docs.snowflake.com/en/sql-reference-commands.html
- Summary of commands: https://docs.snowflake.com/en/sql-reference/intro-summary-sql.html
- Queries: https://docs.snowflake.com/en/user-guide/querying.html
