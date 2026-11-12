![](bbox=[0.13, 0.075, 0.92, 0.365])

# Figure 7.14: Case Fixed II

2. This time, we fix the calculation in the Case Fixed column to only take the Region into account but ignore the Category. Check for yourself and do the math for Central:

10,072 + 16,340 + 13,995 = 40,406.

Change the Choose Fixed Dims parameter to Fixed: Nothing and note that the amount reflects the total SUM(Sales):

# Fixed and Exclude: 1st - Region; 2nd - Category; 3rd - Null; 4th - Null

| 1st Dim | 2nd Dim | 3rd Dim | 4th Dim | Sales | Case Fixed | Case Exclude 1 | Case Exclude 2 |
|---|---|---|---|---|---|---|---|
| Central | Furniture | | | $10,072 | 194,776 | 0 | 0 |
| | Office Supplies | | | $16,340 | 194,776 | 0 | 0 |
| | Technology | | | $13,995 | 194,776 | 0 | 0 |
| East | Furniture | | | $14,249 | 194,776 | 0 | 0 |
| | Office Supplies | | | $21,560 | 194,776 | 0 | 0 |
| | Technology | | | $20,646 | 194,776 | 0 | 0 |
| South | Furniture | | | $4,494 | 194,776 | 0 | 0 |
| | Office Supplies | | | $8,721 | 194,776 | 0 | 0 |
| | Technology | | | $17,451 | 194,776 | 0 | 0 |
| West | Furniture | | | $18,564 | 194,776 | 0 | 0 |
| | Office Supplies | | | $23,975 | 194,776 | 0 | 0 |
| | Technology | | | $24,710 | 194,776 | 0 | 0 |

# Figure 7.15: Fixed: Nothing