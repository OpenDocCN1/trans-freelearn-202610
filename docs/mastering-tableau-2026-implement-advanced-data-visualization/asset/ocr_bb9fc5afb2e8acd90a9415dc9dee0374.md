To better understand this exercise, consider the values associated with 2025 in the preceding screenshot. Next Year Goal Prv_Val is calculated via 2024 Next Year Goal Prv_Val; that is: $613,934 x 1.05. On the other hand, Next Year Goal Lkup is calculated via the 2024 sales ; that is: $745,568 x 1.05.

Real-world scenario: Imagine you want to extrapolate a data point and keep extrapolating from that new value; for this, you use Previous_Value. One example is a bond that pays a frequent coupon. If your bond has a face value of 100 and it pays a 4% semi-annual coupon, you can calculate the final worth of the bond by using Previous_Value, just like we did with Next Year Goal Prv_Val in the last exercise.

# # Running

The objective of the following worksheet is to display the running minimum profit, running average profit, and running maximum profit compared with SUM(Profit) for each month in the dataset.

The following example demonstrates how the Running functions behave directionally:

+   1. In the workbook associated with this chapter, navigate to the Running worksheet.

+   2. Create the following calculated fields:

| Name | Calculation |
| :--- | :--- |
| Running Min | RUNNING_MIN(SUM([Profit])) |
| Running Max | RUNNING_MAX(SUM([Profit])) |
| Running Avg | RUNNING_AVG(SUM([Profit])) |

+   3. Place Order Date on the Columns shelf and set it to Month/Year continuous. Place Measure Values on the Rows shelf.

+   4. Remove all instances of measures from the Measure Values shelf except for Running Min, Running Max, Running Avg, and Profit.

+   5. Move SUM(Profit) from the Measure Values shelf to the Rows shelf. Right-click on SUM(Profit) and select Dual Axis.

+   6. Format as desired.

Let's consider how the preceding worksheet functions:

+   • Running Min: RUNNING_MIN(SUM([Profit])) is visible in the preceding screenshot, which compares the current SUM(Profit) with the least SUM(Profit) recorded to that point in time. If the current SUM(Profit) is less than the least SUM(Profit) recorded to date, the current SUM(Profit) replaces the least SUM(Profit).