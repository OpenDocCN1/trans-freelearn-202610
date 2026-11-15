
Real-world scenario: why would you want to use a window average? Because you want to smoothen the outliers, for example. A window average provides a steady representation of your data. Single highs or lows will be removed. If you analyze stock prices, you will notice that prices can differ throughout the year! By using the window averages or rather window standard deviations as an early warning indicator, you will be able to see price jumps more clearly and can, as an investor, act upon it. Steady growth or decline will, however, fall into the moving bands. This is not achievable by using fixed bands.

## First and Last

The objective of the following worksheet is to display the first and last instance of the best-selling item in the `Superstore` dataset. Notice how the following example demonstrates that the `FIRST()` and `LAST()` functions behave directionally.

Take the following steps:

1. In the workbook associated with this chapter, navigate to the `First/Last` worksheet.
2. Create the following calculated fields:

| Name | Calculation |
| --- | --- |
| `First` | `FIRST()` |
| `Last` | `LAST()` |
| `First or Last` | `FIRST() = 0 OR LAST() = 0` |

3. Place `Product Name` on the `Filters` shelf, select the `Top` tab, and choose `Top` `1` by `Sum` of `Sales`, as shown in the following screenshot:

![](Images/B18435_05_05.png)

Figure 5.5: Top filter

4. Place `First or Last` on the `Filters` shelf and select `True` in the resulting dialog box.
5. Drag `Product Name`, `Order Date`, and `Row ID` onto the `Rows` shelf.
6. Set `Order Date` to `Month/Day/Year` discrete.
7. Double-click on `Sales`, `First`, and `Last` to place each on the view.
8. Right-click on `First`, `Last`, and `First or Last` and select `Compute Using` | `Table (down)`.
9. Format as desired:

![](Images/B18435_05_06.png)

Figure 5.6: First and Last

Let’s consider how the preceding worksheet functions:

* **First**: `FIRST()` starts at `0` and counts down to the last row of the dataset in the cache. In the preceding screenshot, note that the first instance of `Canon imageCLASS` occurs on `05/24/13`. The fact that `FIRST()` ranges from `0` to `-4` communicates that there are five instances of `Canon imageCLASS` in the dataset in the cache or within the window cache.
* **Last**: `LAST()` starts at the last row of the dataset in the cache and counts down to 0. In the preceding screenshot, note that the last instance of `Canon imageCLASS` occurs on `11/18/14`. The fact that `LAST()` ranges from `4` to `0` communicates that there are five instances of `Canon imageCLASS` in the dataset in the cache.
* **First or Last**: `FIRST() = 0 OR LAST() = 0`, when placed on the `Filters` shelf and set to `True`, hides all instances of matching rows except the first and last.
* The `Row ID` field is included in the view to make sure that the very first and last instances of `Canon imageCLASS` display. Otherwise, if there are multiple instances of `Canon imageCLASS` on the first or last date, the sales numbers will reflect multiple values. It’s important to set `Compute Using` to `Table (down)` for each table calculation in the view. `Compute Using` is the same as `Addressing`, which will be discussed in detail in the `Application of functions` section.

Real-world scenario: you could use this type of calculation for the time between the last 5 orders per quarter, to see whether the time between orders is increasing or decreasing during the day. This can then help to improve marketing or schedule help desk hours more efficiently.

## Index

The objective of the following worksheet is to list those states in the USA with over 50 postal codes represented in the underlying dataset.

Notice how the following example demonstrates that the `INDEX()` function behaves directionally and can be seen as a count of rows:

1. In the workbook associated with this chapter, navigate to the `Index` worksheet.
2. Set the `Marks` card to `Circle`.
3. Place `State` on the `Rows` shelf and `Postal Code` on the `Detail` shelf.
4. Create a calculated field named `Index` with the code: `INDEX()`. Drag `Index` onto the `Filters` shelf and select the `Range of values` filter. Choose to view only values that are 50 or greater by moving the slider to a minimum value of `50`.
5. Right-click on `Index` and select `Edit Table Calculation`. Select `Specific Dimensions` and check `Postal Code`:

![](Images/B18435_05_07.png)

Figure 5.7: Index

6. Select `Analysis` | `Stack Marks` | `Off`, then review:

![](Images/B18435_05_08.png)

Figure 5.8: Final index

Let’s consider how the preceding worksheet functions:

**Index**: `INDEX()` counts from 1 to *n*. As such, it behaves directionally. In this case, as a result of the partitioning and addressing settings, `Index` is counting postal codes. (Partitioning and addressing will be discussed in detail in the `Application of functions` section.) Setting the `Index` filter to display only values of 50 or more ensures that only those states with 50 or more postal codes are in the partition/dataset in the cache display.

Real-world scenarios: most often, I use `Index` interchangeably with `First` and `Last`.

## Rank

The objective of the following worksheet is to display the top three selling items in each region. This example will demonstrate how `RANK()` interacts directionally with the dataset in the cache:

1. In the workbook associated with this chapter, navigate to the `Rank` worksheet.
2. Place `Product Name` on the `Filters` shelf, select the `Top` tab, and choose `Top 3` by `Sum` of `Sales`:

![](Images/B18435_05_09.png)

Figure 5.9: Filter

3. Place `Region` and `Product Name` on the `Rows` shelf and `Sales` on the `Columns` shelf.
4. Note that only two items display for each region:

![](Images/B18435_05_10.png)

Figure 5.10: Reviewing the top two items

5. Create a calculated field named `Rank` with the code `RANK(SUM(Sales))`. Drag `Rank` between `Region` and `Product Name` onto the `Rows` shelf.
6. Note that before you can place `Rank` between the two pills on the `Rows` shelf, you have to cast it as discrete. One way to accomplish this is by placing `Rank` on the `Detail` shelf, right-clicking on the pill, and selecting `Discrete`.
7. Right-click on `Rank` and select `Compute Using` | `Product Name`.
8. Remove `Product Name` from the `Filters` shelf.
9. Press `Ctrl` (or press `Option` on Mac), and right-click and drag `Rank` from the `Rows` shelf onto the `Filters` shelf. Pressing the `Ctrl` key while dragging a pill from one shelf to another will create a copy of that pill. Failing to press the `Ctrl` key will, of course, simply result in moving the pill. In the resulting dialog box, select `1`, `2`, and `3`.

Format as desired:

![](Images/B18435_05_11.png)

Figure 5.11: Rank

Let’s consider how the preceding worksheet functions:

* If you followed the step-by-step instructions for this exercise, you will have noticed that after placing `Region` and `Product Name` on the `Rows` shelf and filtering to show only the top three product names, the resulting visualization only showed two products in each region. This is because the filter on `Product Name` showed the top three products overall, but it so happened that two out of the three were always present in each region. But we actually wanted to show the top three per region rather than overall. To fix this issue, we employed the `RANK()` table calculation function.
