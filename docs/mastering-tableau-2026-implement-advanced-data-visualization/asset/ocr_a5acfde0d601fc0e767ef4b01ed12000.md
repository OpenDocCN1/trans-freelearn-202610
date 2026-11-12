The associated code for Case Fixed is:

```
SUM({FIXED [Segment]: SUM([Sales])})
```

Case Exclude 2 and Case Fixed now have identical results. This is because excluding the first two dimensions is the same as fixing on the third dimension. Only the third dimension Segment is considered in both cases.

You can observe the results in the following screenshot:

# Fixed and Exclude: 1st - Region; 2nd - Category; 3rd - Segment; 4th - Null

| 1st Dim | 2nd Dim | 3rd Dim | 4th Dim | Sales | Case Fixed | Case Exclude 1 | Case Exclude 2 |
|---|---|---|---|---|---|---|---|
| Central | Furniture | Consumer | | $5,444 | 98,554 | 26,738 | 98,554 |
| | | Corporate | | $2,779 | 60,994 | 13,050 | 60,994 |
| | | Home Office | | $1,849 | 35,228 | 7,592 | 35,228 |
| | Office Supplies | Consumer | | $9,085 | 98,554 | 31,861 | 98,554 |
| | | Corporate | | $3,462 | 60,994 | 24,179 | 60,994 |
| | | Home Office | | $3,793 | 35,228 | 14,556 | 35,228 |
| | Technology | Consumer | | $5,144 | 98,554 | 39,955 | 98,554 |
| | | Corporate | | $3,848 | 60,994 | 23,766 | 60,994 |
| | | Home Office | | $5,003 | 35,228 | 13,081 | 35,228 |
| East | Furniture | Consumer | | $6,941 | 98,554 | 26,738 | 98,554 |
| | | Corporate | | $4,415 | 60,994 | 13,050 | 60,994 |
| | | Home Office | | $2,893 | 35,228 | 7,592 | 35,228 |
| | Office Supplies | Consumer | | $11,361 | 98,554 | 31,861 | 98,554 |
| | | Corporate | | $7,217 | 60,994 | 24,179 | 60,994 |
| | | Home Office | | $2,982 | 35,228 | 14,556 | 35,228 |
| | Technology | Consumer | | $12,459 | 98,554 | 39,955 | 98,554 |
| | | Corporate | | $8,011 | 60,994 | 23,766 | 60,994 |
| | | Home Office | | $176 | 35,228 | 13,081 | 35,228 |
| South | Furniture | Consumer | | $2,052 | 98,554 | 26,738 | 98,554 |
| | | Corporate | | $2,193 | 60,994 | 13,050 | 60,994 |
| | | Home Office | | $249 | 35,228 | 7,592 | 35,228 |
| | Office Supplies | Consumer | | $3,393 | 98,554 | 31,861 | 98,554 |
| | | Corporate | | $2,349 | 60,994 | 24,179 | 60,994 |
| | | Home Office | | $2,978 | 35,228 | 14,556 | 35,228 |
| | Technology | Consumer | | $10,024 | 98,554 | 39,955 | 98,554 |
| | | Corporate | | $6,046 | 60,994 | 23,766 | 60,994 |
| | | Home Office | | $1,381 | 35,228 | 13,081 | 35,228 |
| West | Furniture | Consumer | | $12,301 | 98,554 | 26,738 | 98,554 |
| | | Corporate | | $3,663 | 60,994 | 13,050 | 60,994 |
| | | Home Office | | $2,600 | 35,228 | 7,592 | 35,228 |
| | Office Supplies | Consumer | | $8,022 | 98,554 | 31,861 | 98,554 |

Figure 7.21: Exclude II table