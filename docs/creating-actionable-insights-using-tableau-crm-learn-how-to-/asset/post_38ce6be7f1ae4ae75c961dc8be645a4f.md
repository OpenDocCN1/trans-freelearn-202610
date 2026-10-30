
This is what my example lens looks like in chart mode:

![](img/06379e43783b9ddfee45e9da5192efe1_245_0.png)

Figure 9.10 – A sample opportunity lens in chart mode

My example SAQL looks like this in query mode:

Learn how to write and edit SAQL queries ▶

```
q = load "opportunity1";
q = filter q by (!('StageName' in ["Closed Lost", "Closed Won"]) || 'StageName' is null);
q = filter q by 'Account.Industry' in ["Banking", "Biotechnology", "Communications", "Education", "Electronics"];
q = group q by 'Account.Name';
q = foreach q generate 'Account.Name' as 'Account.Name', count() as 'count', sum('Amount') as 'sum_Amount';
q = order q by 'Account.Name' asc;
q = limit q 2000;
```

| Account Name ↑ | Count of Rows | Sum of Amount |
|---|---|---|
| Becker755 Inc | 1 | $3,526,355 |
| Brewer763 Inc | 1 | $1,422,600 |
| Bryant456 Inc | 1 | $1,269,390 |
| Burton810 Inc | 1 | $136,800 |
| Carlson744 Inc | 1 | $788,800 |

Figure 9.11 – A sample opportunity lens with SAQL revealed in query mode

This is what the code looks like:

```
q = load "opportunity1";
q = filter q by (!('StageName' in ["Closed Lost", "Closed Won"]) || 'StageName' is null);
q = filter q by 'Account.Industry' in ["Banking", "Biotechnology", "Communications", "Education", "Electronics"];
q = group q by 'Account.Name';
q = foreach q generate 'Account.Name' as 'Account.Name', count() as 'count', sum('Amount') as 'sum_Amount';
q = order q by 'Account.Name' asc;
q = limit q 2000;
```

The best way to learn how SAQL works is to get your hands dirty, so let's dive into some example use cases.

## Example use cases

Here are some example use cases where SAQL is helpful.

## Union use case

Follow these steps to create and edit a Union SAQL query:

- 1. Create a lens from the **Opportunities** dataset with no grouping, using **Sum of Amount** as the measure.
- 2. Switch to **Compare Table** mode.
- 3. Filter **Sum of Amount** as follows:
  - **Opportunity Type = New Business**
  - **Account Type = Customer**
  - **Industry ≠ Agriculture, Apparel**
- 4. Clone the column and filter by selecting **Existing Business** under **Opportunity Type**.
- 5. Edit the column names as follows:
  - New Business
  - Existing Business
- 6. **Save** the lens as **Union Use Case**.

Your progress so far will look like this:

![](img/06379e43783b9ddfee45e9da5192efe1_247_0.png)

Figure 9.12 – Table view of the Union Use Case lens

- 7. Change the lens to a Funnel chart and notice that it cannot be used correctly; it shows one value at a time only. Switch to **Query Mode** to view and edit the SAQL content.

This is your starting point with the SAQL content:

```
q = load "opportunity1";
q_A = filter q by 'Type' == "New Business" && 'Account.Type' == "Customer" && (!('Account.Industry' in ["Agriculture", "Apparel"]) || 'Account.Industry' is null);
q_B = filter q by 'Type' == "Existing Business" && 'Account.Type' == "Customer" && (!('Account.Industry' in ["Agriculture", "Apparel"]) || 'Account.Industry' is null);
result = group q_A by all full, q_B by all;
result = foreach result generate sum(q_A.'Amount') as 'A', sum(q_B.'Amount') as 'B';
result = limit result 2000;
```

Notice that you have three data streams in your query: q_A, q_B, and result.

- 8. Update your code to remove the query limit and union your two columns, as follows:

```
q = load "opportunity1";
q_A = filter q by 'Type' == "New Business" && 'Account.Type' == "Customer" && (!('Account.Industry' in ["Agriculture", "Apparel"]) || 'Account.Industry' is null);
q_B = filter q by 'Type' == "Existing Business" && 'Account.Type' == "Customer" && (!('Account.Industry' in ["Agriculture", "Apparel"]) || 'Account.Industry' is null);
result = group q_A by all full, q_B by all;
result = foreach result generate sum(q_A.'Amount') as 'A', sum(q_B.'Amount') as 'B';
final1 = foreach result generate "New Business" as 'Description', sum('A') as 'Sum';
final2 = foreach result generate "Existing Business" as 'Description', sum('B') as 'Sum';
final = union final1,final2;
```

The union function combines multiple result sets into one result set. You need the same structure and field names in result sets. You do have a choice to either use a different dataset or the same dataset when it comes to creating result datasets.

When you click Run Query, you will see a table, as follows:

Learn how to write and edit SAQL queries

```
Query
1 q = load "opportunity1";
2 q_A = filter q by 'Type' == "New Business" && 'Account.Type' == "Customer" && (!('Account.Industry' in ["Agriculture", "Apparel"]) || 'Account.Industry' is null);
3 q_B = filter q by 'Type' == "Existing Business" && 'Account.Type' == "Customer" && (!('Account.Industry' in ["Agriculture", "Apparel"]) || 'Account.Industry' is null);
4 result = group q_A by all full, q_B by all;
5 result = foreach result generate sum(q_A.'Amount') as 'A', sum(q_B.'Amount') as 'B';
6 result = limit result 2000;
7
8 final1 = foreach result generate "New Business" as 'Description', sum('A') as 'Sum';
9 final2 = foreach result generate "Existing Business" as 'Description', sum('B') as 'Sum';
10 final = union final1,final2;
```

| # | Description | Sum |
|---|---|---|
| 1 | New Business | 257,039,003 |
| 2 | Existing Business | 109,018,414 |

Figure 9.13 – Table results from the Union function

You can now use a funnel or other chart type to visualize the sum broken up into two components: new and existing business.

Another use case for the Union function is to analyze the same metric showing a rolling 3 months, as follows:

```
q3 = load "opportunity";
```

```
q3 = filter q3 by date('CloseDate_Year', 'CloseDate_Month', 'CloseDate_Day') in ["3 months ago".."3 months ago"]; q3 = group q3 by ('CloseDate_Year', 'CloseDate_Month');
```

```
q3 = foreach q3 generate 'CloseDate_Year' as 'CloseDate_Year', 'CloseDate_Month' as 'CloseDate_Month', sum('Amount') as 'sum_Amount';
```

```
q2 = load "opportunity";
```

```
q2 = filter q2 by date('CloseDate_Year', 'CloseDate_Month', 'CloseDate_Day') in ["2 months ago".."2 months ago"]; q2 = group q2 by ('CloseDate_Year', 'CloseDate_Month');
```

```
q2 = foreach q2 generate 'CloseDate_Year' as 'CloseDate_Year', 'CloseDate_Month' as 'CloseDate_Month', sum('Amount') as 'sum_Amount';
q1 = load "opportunity";
q1 = filter q1 by date('CloseDate_Year', 'CloseDate_Month', 'CloseDate_Day') in ["1 months ago".."1 months ago"]; q1 = group q1 by ('CloseDate_Year', 'CloseDate_Month');
q1 = foreach q1 generate 'CloseDate_Year' as 'CloseDate_Year', 'CloseDate_Month' as 'CloseDate_Month', sum('Amount') as 'sum_Amount';
final = union q3, q2, q1;
```

## Creating buckets using the case statement

Follow these steps to try this use case for yourself:

- 1. Create a new lens from the **Opportunities** dataset.
- 2. Group by **Opportunity Name** and measure **Sum of Amount**.
- 3. Filter by **Won deals**.
- 4. Change to **Query Mode**.
- 5. Remove the `limit` statement.
- 6. Click **Save** to save your lens.

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

```
result = foreach q generate 'Name' as 'Name', 'sum_Amount' as 'Amount', (case when 'sum_Amount' > 2000000 then "Big" when 'sum_Amount' > 500000 then "Medium" else "Small" end) as 'Size';
```

When you click **Run Query**, you should see this:

![](img/06379e43783b9ddfee45e9da5192efe1_251_0.png)

| Opportunity Name | Amount | Size |
|---|---:|---|
| Opportunity for Abbott1184 | $2,016,250 | Big |
| Opportunity for Abbott414 | $206,587 | Small |
| Opportunity for Abbott636 | $121,490 | Small |
| Opportunity for Adkins111 | $1,402,500 | Medium |
| Opportunity for Aguilar1908 | $3,314,900 | Big |
| Opportunity for Aguilar210 | $202,174 | Small |
| Opportunity for Alexander278 | $18,960 | Small |

Figure 9.14 – Bucketed results created using the Case function

> **Tip**
>
> If you're looking to dive deep into SAQL, I can recommend these resources:
>
> https://developer.salesforce.com/docs/atlas.en-us.bi_dev_guide_saql.meta/bi_dev_guide_saql/bi_saql_reference.htm
>
> https://www.salesforceblogger.com/all-about-saql/
