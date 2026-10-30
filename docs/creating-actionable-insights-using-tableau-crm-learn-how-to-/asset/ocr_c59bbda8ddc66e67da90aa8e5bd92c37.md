+   - rank—Returns the rank of each record in the window based on order, while repeating the rank when the values are the same, and skips as many on the next non-match
+   - row_number—Returns the row number in the window, incrementing by 1 for every row in the window
+   - sum—Returns the cumulative sum from groups of value

An example of a window function would be to rank the deals per salesperson by the deal amount, as seen in the following screenshot:

![](bbox=[0.12, 0.3, 0.87, 0.59])

# Additional functions

Two additional functions are available—the case function and the coalesce function.

# The case function

The case function handles if/then/else logic for formulas. This function can be used with TEXT, NUMBER, DATE_ONLY, and DATETIME data. The syntax for the case function is shown here:

```
case primary_expr(optional)
when condition
then result_expr
...
```