
4. Create the calculated fields, as shown in the following table:

| Name | Calculation | Notes |
|---|---|---|
| Lookup | LOOKUP(SUM([Sales]), -1) | Notice the -1 included in this calculation. This instructs Tableau to retrieve the value from the previous row. |
| Size | SIZE() | The SIZE function returns the number of rows in the partition. Therefore, as can be seen in the following screenshot, the size equals the total number of rows. |
| Window Sum | WINDOW_SUM(SUM([Sales])) | The Window sum functions (WINDOW_SUM(expression, [start, end]) can operate either directionally or non-directionally. Since this example does not include the [start, end] option, it operates non-directionally. |
| Window Sum w/ Start&End | WINDOW_SUM(SUM([Sales]), 0,1) | This example of a window function is operating directionally, as can be seen by the inclusion of the [start, end] option. |
| Running Sum | RUNNING_SUM(SUM([Sales])) | By their nature, RUNNING functions operate directionally since they consider previous rows in order to compute. |

5. Place Category and Ship Mode on the Rows shelf.

6. Double-click on Sales, Lookup, Size, Window Sum, Window Sum w/ Start&End, and Running Sum to populate the view as shown in the following screenshot:

| Category | Ship Mode | Lookup along Table (Down) | Running Sum along Table (Down) | Sales | Size along Table (Down) | Window Sum along Table (Down) | Window Sum w/ Start&End |
|---|---|---|---|---|---|---|---|
| Furniture | First Class | 110,761 | 110,761 | 110,761 | 12 | 2,326,534 | 149,909 |
| | Same Day | | 149,909 | 39,149 | 12 | 2,326,534 | 200,655 |
| | Second Class | 39,149 | 311,416 | 161,506 | 12 | 2,326,534 | 604,838 |
| | Standard Class | 161,506 | 754,748 | 443,332 | 12 | 2,326,534 | 544,957 |
| Office Supplies | First Class | 443,332 | 856,372 | 101,625 | 12 | 2,326,534 | 130,715 |
| | Same Day | 101,625 | 885,463 | 29,090 | 12 | 2,326,534 | 192,034 |
| | Second Class | 29,090 | 1,048,406 | 162,943 | 12 | 2,326,534 | 601,178 |
| | Standard Class | 162,943 | 1,486,641 | 438,235 | 12 | 2,326,534 | 577,601 |
| Technology | First Class | 438,235 | 1,626,007 | 139,366 | 12 | 2,326,534 | 200,398 |
| | Same Day | 139,366 | 1,687,039 | 61,033 | 12 | 2,326,534 | 203,254 |
| | Second Class | 61,033 | 1,829,261 | 142,222 | 12 | 2,326,534 | 639,495 |
| | Standard Class | 142,222 | 2,326,534 | 497,273 | 12 | 2,326,534 | 497,273 |

Figure 5.1: Table calculation

7. The table in Figure 5.1 shows you that the Size function and the Window Sum function are the only two non-directional functions. All others present a number depending on the direction. For example, Lookup moves down and prints the value prior to the actual field. You can change the direction by clicking on the triangle of any table calculation field and selecting Edit Table Calculation.

This first exercise was an example of two table calculation categories: directional and non-directional. Now, we will move on to considering the table calculation functions individually. Regretfully, space does not allow us to explore all table calculations; however, to gain a working understanding, it should suffice to consider all but one of the unique groups of options. The four table calculations that begin with Script_ as well as the six Model_ functions, will be covered in Chapter 14, Integrating Programming Languages.

Although I added a real-world scenario at the end of each section in writing, the follow-along exercises focus on demonstrating table calculation functionality and considering how each interacts directionally or non-directionally with the dataset in the cache (also known as the partition).

The following exercises will show us each table calculation individually; at the end of each section, you will find my thoughts on a real-world scenario.

## Lookup and Total

The objectives of the following worksheet are to display those customers who made purchases in the last two days of 2013 and the associated absolute, as well as relative sales for the month of December.

In the following table, LOOKUP behaves directionally, whereas TOTAL is non-directional. This behavior is easy to notice in the results. Follow these steps in order to do so:

- 1. In the workbook associated with this chapter, navigate to the Lookup/Total worksheet.
- 2. Drag Customer Name and Order Date onto the Rows shelf. Set Order Date to Month/Day/Year discrete by right-clicking and selecting Day as well as Discrete.
- 3. Place Order Date on the Filters shelf, choose to view only December 2025, and select Month/Year as the date format.
- 4. Create the following calculated fields:

| Name | Calculation |
| :--- | :--- |
| Lookup Intervening Time | DATEDIFF('day', LOOKUP(MAX([Order Date]), 0), MAKEDATE(2025, 12, 31)) |
| % Total Sales | SUM(Sales)/TOTAL(SUM([Sales])) |

- 5. Right-click on % Total Sales and select Default Properties | Number format to set the number format to percentages with two decimal places.
- 6. Place Lookup Intervening Time on the Filters shelf and choose a range from 0 to 1.
- 7. Double-click on Sales and % Total Sales to place both fields on the view.
- 8. Format as desired:

| Customer Name | Day of Order Date | % Total Sales along Table (Down) | Sales |
|---|---|---|---|
| Chuck Clark | 30 December 2025 | 0.25% | 209.3 |
| Erica Bern | 30 December 2025 | 0.04% | 34.6 |
| Harry Olson | 30 December 2025 | 0.00% | 3.0 |
| Jill Matthias | 30 December 2025 | 0.00% | 3.0 |
| Patrick O'Donnell | 30 December 2025 | 0.55% | 466.8 |

Figure 5.2: Total Sales

Let's consider how the preceding worksheet functions:

- The filter on Order Date ensures that the dataset returned to Tableau only includes data from the month of December 2025.
- The % Total Sales SUM([Sales])/TOTAL(SUM([Sales]))) includes the TOTAL(SUM([Sales])) calculated field, which returns the total sales for the entire dataset. Dividing SUM([Sales]) by this total returns the percentage of the total.
- Lookup Intervening Time: DATEDIFF('day', LOOKUP(MAX([Order Date]), 0), MAKEDATE(2025, 12, 31)) will return an integer that reflects the difference between the date returned by the LOOKUP function and 12/31/2025. Note that the LOOKUP function has an offset of zero. This results in each row returning the date associated with that row. This differs from directional and non-directional table calculations, which include a LOOKUP function with an offset of -1, which causes each row in the view to return data associated with the previous row.

At first glance, you might think that you could simplify this workbook by removing Lookup Intervening Time from the Filters shelf and adjusting the filter on [Order Date] to display only the last two days of December. However, if you do this, % Total Sales will add up to 100% across all rows in the view, which would not satisfy the workbook's objectives. Think of Lookup Intervening Time as not filtering but hiding all but the last two days in December. This hiding ensures that the data necessary to calculate % Total Sales is in the dataset in cache/partition.

Real-world scenario: I use Lookup more often than Total. Both are invaluable when working with Tableau. We already saw that you use Lookup for changes; you can think of a month-to-month comparison of profits, the number of employees today versus today last year, website click differences per day of the week, and so on.

For the table calculation total, you can use it every time you want to show the total number of something while still showing some level of aggregation in your dashboard. The percentage of Total in Figure 5.2 is actually the perfect example.

## Previous Value

The objectives of the following worksheet are to return the aggregate value of sales for each year and set next year's sales goal. Note that two options have been provided for determining next year's sales goal in order to demonstrate how PREVIOUS_VALUE differs from LOOKUP. Also, note that PREVIOUS_VALUE behaves directionally.

Let us have a look at the steps:

- 1. In the workbook associated with this chapter, navigate to the Previous Value worksheet.
- 2. Create the following calculated fields:

| Name | Calculation |
|---|---|
| Next Year Goal Prv_Val | PREVIOUS_VALUE(SUM([Sales])) * 1.05 |
| Next Year Goal Lkup | LOOKUP(SUM([Sales]), 0) * 1.05 |

- 3. Place Order Date on the Rows shelf.
- 4. Double-click Sales, Next Year Goal Prv_Val, and Next Year Goal Lkup to place each on the view.
- 5. Format as desired, or as seen in the following screenshot (by dragging the pills in Measure Values, the order will change accordingly in the view, as seen in the following screenshot):
