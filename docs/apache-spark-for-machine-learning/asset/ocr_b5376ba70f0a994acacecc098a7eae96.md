+   - Window types in ML and AI:

+   - **Sliding windows**: An overlap between successive windows, commonly used in speech and audio processing

+   - **Tumbling windows**: Non-overlapping, sequential windows, often used in real-time analytics

+   - **Expanding windows**: Start from a fixed point and grow over time, useful in cumulative metrics calculation

# # Why windowing is required and its examples in Spark

Let's explore some key reasons why windowing is necessary and beneficial:

+   - **Performing calculations across rows**: Window functions perform calculations across a set of rows related to the current row without collapsing them into a single output row, unlike traditional aggregate functions. This is essential for analyses where you need to retain individual row details.

+   - **Data partitioning**: Window functions enable you to partition data into groups without altering the row structure of the output. This partitioning allows for comparative and aggregate analysis within subsets of data, such as calculating sums, averages, or other metrics within each group.

+   - **Ranking and row numbering**: They provide the ability to rank items within a dataset without the need for complex subqueries. Functions such as `ROW_NUMBER()`, `RANK()`, and `DENSE_RANK()` are invaluable for tasks such as ranking sales data by region or department, identifying top-performing products, or assigning unique identifiers to rows based on a specific order.

+   - **Running totals and moving averages**: Window functions are particularly useful for financial and statistical analyses, such as calculating running totals, moving averages, or cumulative sums. This capability is essential for time series analysis, budgeting, financial reporting, and trend analysis.

+   - **Accessing previous and next row values**: Functions such as `LEAD()` and `LAG()` allow you to access data from preceding or following rows within the same result set. This feature is crucial for comparing current row data with previous or next rows, such as calculating period-over-period changes in sales or stock movements, or detecting sequence patterns.

Let's go through some example problems to understand several `groupby` operations.

# ## Problem 1

**Problem statement**: How do we calculate a new column for each group whose row value is equal to the sum of the current row and the previous two rows?

As shown in *Figure 2.10*, the dataset consists of three columns – namely, `dept_name`, `emp_id`, and `salary`. We create a new column, `sum`. Each row value in this new `sum` column is a sum of the current row value, plus two previous row values in the `salary` column.