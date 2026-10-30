```
q2 = foreach q2 generate 'CloseDate_Year' as 'CloseDate_Year', 'CloseDate_Month' as 'CloseDate_Month', sum('Amount') as 'sum_Amount';
q1 = load "opportunity";
q1 = filter q1 by date('CloseDate_Year', 'CloseDate_Month', 'CloseDate_Day') in ["1 months ago".."1 months ago"]; q1 = group q1 by ('CloseDate_Year', 'CloseDate_Month');
q1 = foreach q1 generate 'CloseDate_Year' as 'CloseDate_Year', 'CloseDate_Month' as 'CloseDate_Month', sum('Amount') as 'sum_Amount';
final = union q3, q2, q1;
```

# Creating buckets using the case statement

Follow these steps to try this use case for yourself:

+   1. Create a new lens from the **Opportunities** dataset.
+   2. Group by **Opportunity Name** and measure **Sum of Amount**.
+   3. Filter by **Won deals**.
+   4. Change to **Query Mode**.
+   5. Remove the `limit` statement.
+   6. Click **Save** to save your lens.

Now that you have your basic SAQL prepared, add the following case statement:

```
result = foreach q generate 'Name' as 'Name', 'sum_Amount' as 'Amount', (case when 'sum_Amount' > 2000000 then "Big" when 'sum_Amount' > 500000 then "Medium" else "Small" end) as 'Size';
```

The finished SAQL code should look like this:

```
q = load "opportunity1";
q = filter q by 'IsWon' == "true";
q = group q by 'Name';
q = foreach q generate 'Name' as 'Name', sum('Amount') as 'sum_Amount';
q = order q by 'Name' asc;
```