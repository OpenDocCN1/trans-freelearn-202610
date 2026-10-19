In the future, end users can use the preceding query to easily allocate the percentage of advertising and marketing budgets that are backed by user hits:

```
SELECT TOP (100) [country_name] ,[hits] ,[advtg_budget]
FROM [dbo].[ext_advertising_budgets]
ORDER BY 3 DESC
```

| country_name | hits | advtg_budget |
|---|---|---|
| United States of America | 24690 | 57 |
| China | 6936 | 16 |
| Japan | 2272 | 5 |
| Germany | 2323 | 5 |
| United Kingdom of Great Britain ... | 2029 | 4 |
| Korea (Republic of) | 1829 | 4 |
| France | 1585 | 3 |
| Italy | 1063 | 2 |

Figure 8.44 – Aggregated website hits and budget allocation

Click on Chart and change the variables provided, as per the following screenshot. By doing this, the end users at Electroniz can easily visualize the country-wise allocation of advertising and marketing dollars: