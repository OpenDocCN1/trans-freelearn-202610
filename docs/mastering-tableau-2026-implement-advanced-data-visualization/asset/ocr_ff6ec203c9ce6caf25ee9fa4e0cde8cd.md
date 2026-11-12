+   2. The **Case Fixed** column now displays the totals for each **Category** and ignores **Region** because we told the calculation to fix the **2nd Dim (Region)** and ignore the dimensions in the view itself

+   3. The effect of this is visible by comparing the **Sales** column and the **Case Fixed** column:

Fixed and Exclude: 1st - Region; 2nd - Category; 3rd - Null; 4th - Null

| 1st Dim | 2nd Dim | 3rd Dim | 4th Dim | Sales | Case Fixed | Case Exclude 1 | Case Exclude 2 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Central | Furniture | | | $10,072 | 47,379 | 0 | 0 |
| | Office Supplies | | | $16,340 | 70,596 | 0 | 0 |
| | Technology | | | $13,985 | 76,801 | 0 | 0 |
| East | Furniture | | | $14,249 | 47,379 | 0 | 0 |
| | Office Supplies | | | $21,560 | 70,596 | 0 | 0 |
| | Technology | | | $20,646 | 76,801 | 0 | 0 |
| South | Furniture | | | $4,494 | 47,379 | 0 | 0 |
| | Office Supplies | | | $8,721 | 70,596 | 0 | 0 |
| | Technology | | | $17,451 | 76,801 | 0 | 0 |
| West | Furniture | | | $18,564 | 47,379 | 0 | 0 |
| | Office Supplies | | | $23,975 | 70,596 | 0 | 0 |
| | Technology | | | $24,710 | 76,801 | 0 | 0 |

Figure 7.13: Case Fixed I

+   4. Check for yourself and do the math for Furniture

10,072 + 14,249 + 4,494 + 18,564 = 47,379

Even though the **Region** is visible in the view, the **SUM(Sales)** in the **Case Fixed** column ignores the **Region** and shows the results as if the **Category** dimension is the only dimension in the view. We manipulated the granularity of the data. In the **Case Fixed** column, **Region** is there for informative purposes only but does not affect the calculation of **SUM(Sales)**.

+   1. Now change the **Choose Fixed Dims** parameter to **1st** and note that **Case Fixed** now displays the totals for each **Region**: