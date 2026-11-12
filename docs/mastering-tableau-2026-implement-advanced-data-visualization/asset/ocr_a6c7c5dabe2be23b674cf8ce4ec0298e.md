# 4. Observe the results in the following screenshot:

Fixed and Exclude: 1st - Region; 2nd - Category; 3rd - Segment; 4th - Null

| 1st Dim | 2nd Dim | 3rd Dim | 4th Dim | Sales | Case Fixed | Case Exclude 1 | Case Exclude 2 |
|---|---|---|---|---|---|---|---|
| Central | Furniture | Consumer | | $5,444 | 1,161,401 | 26,738 | 98,554 |
| | | Corporate | | $2,779 | 706,146 | 13,050 | 60,994 |
| | | Home Office | | $1,849 | 429,653 | 7,592 | 35,228 |
| | Office Supplies | Consumer | | $9,085 | 1,161,401 | 31,861 | 98,554 |
| | | Corporate | | $3,462 | 706,146 | 24,179 | 60,994 |
| | | Home Office | | $3,793 | 429,653 | 14,556 | 35,228 |
| | Technology | Consumer | | $5,144 | 1,161,401 | 39,955 | 98,554 |
| | | Corporate | | $3,848 | 706,146 | 23,766 | 60,994 |
| | | Home Office | | $5,003 | 429,653 | 13,081 | 35,228 |
| East | Furniture | Consumer | | $6,941 | 1,161,401 | 26,738 | 98,554 |
| | | Corporate | | $4,415 | 706,146 | 13,050 | 60,994 |
| | | Home Office | | $2,893 | 429,653 | 7,592 | 35,228 |
| | Office Supplies | Consumer | | $11,361 | 1,161,401 | 31,861 | 98,554 |
| | | Corporate | | $7,217 | 706,146 | 24,179 | 60,994 |
| | | Home Office | | $2,982 | 429,653 | 14,556 | 35,228 |
| | Technology | Consumer | | $12,459 | 1,161,401 | 39,955 | 98,554 |
| | | Corporate | | $8,011 | 706,146 | 23,766 | 60,994 |
| | | Home Office | | $176 | 429,653 | 13,081 | 35,228 |
| South | Furniture | Consumer | | $2,052 | 1,161,401 | 26,738 | 98,554 |
| | | Corporate | | $2,193 | 706,146 | 13,050 | 60,994 |
| | | Home Office | | $249 | 429,653 | 7,592 | 35,228 |
| | Office Supplies | Consumer | | $3,393 | 1,161,401 | 31,861 | 98,554 |
| | | Corporate | | $2,349 | 706,146 | 24,179 | 60,994 |
| | | Home Office | | $2,978 | 429,653 | 14,556 | 35,228 |
| | Technology | Consumer | | $10,024 | 1,161,401 | 39,955 | 98,554 |
| | | Corporate | | $6,046 | 706,146 | 23,766 | 60,994 |
| | | Home Office | | $1,381 | 429,653 | 13,081 | 35,228 |
| West | Furniture | Consumer | | $12,301 | 1,161,401 | 26,738 | 98,554 |
| | | Corporate | | $3,663 | 706,146 | 13,050 | 60,994 |
| | | Home Office | | $2,600 | 429,653 | 7,592 | 35,228 |
| | Office Supplies | Consumer | | $8,022 | 1,161,401 | 31,861 | 98,554 |

Figure 7.27: Without context filter

# 5. Case Exclude 2 and Case Fixed are no longer identical. Case Fixed is no longer impacted by the Product Name filter because the context was removed.

The behavior difference observed between EXCLUDE and FIXED in the preceding exercise reflects the underlying filter order of operations. As shown in Figure 7.27, Context Filters will impact FIXED, EXCLUDE, and INCLUDE calculations because the Context Filter is being applied first to the dataset, then the LOD. Dimension Filters, however, will only impact EXCLUDE and INCLUDE LODs because the FIXED LOD will be applied to the dataset first and then the dimension filter, followed by the EXCLUDE and INCLUDE LODs.