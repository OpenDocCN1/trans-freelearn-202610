
# Advanced Report Configuration

| CLOSE DATE | OPPORTUNITY OWNER | OPPORTUNITY NAME | AMOUNT Sum | AMOUNT BY CLOSE DATE | AMOUNT BY GRAND TOTAL |
|---|---|---|---|---:|---:|---:|
| June 2016<br>(3 records) | Alvin Lee<br>(1 record) | Pyramid Emergency Generators | $100,000.00 |  |  |
|  |  | Subtotal | $100,000.00 | $265,000 | - |
|  | Johan Yu<br>(2 records) | Element Stage 1A | $85,000.00 |  |  |
|  |  | Element Stage 1B | $80,000.00 |  |  |
|  |  | Subtotal | $165,000.00 | $265,000 | - |
| Subtotal |  |  | $265,000.00 | - | $478,000.00 |
| July 2016<br>(3 records) | Alvin Lee<br>(1 record) | Element Phase IIa | $38,000.00 |  |  |
|  |  | Subtotal | $38,000.00 | $213,000 | - |
|  | Johan Yu<br>(2 records) | Project Motor Head | $100,000.00 |  |  |
|  |  | Project Motor Links in Kentica | $75,000.00 |  |  |
|  |  | Subtotal | $175,000.00 | $213,000 | - |
| Subtotal |  |  | $213,000.00 | - | $478,000.00 |
| GRAND TOTAL<br>(6 RECORDS) |  |  | $478,000.00 |  |  |

From the preceding examples, we can easily understand that the `PARENTGROUPVAL()` function gets the summary value from one or more levels above, and that's why it is called **parent group value**.

Next, let's use the `PARENTGROUPVAL()` function in real actions; we would like to know the percentage of total opportunity for each month compared to the total. For sure, you can't do this without a custom summary formula, and here is the report result:

| CLOSE DATE | OPPORTUNITY OWNER | RECORD COUNT | AMOUNT Sum | % OF TOTAL |
|---|---|---:|---:|---:|
| June 2016 | Alvin Lee | 1 | $100,000.00 | - |
|  | Johan Yu | 2 | $165,000.00 | - |
| Subtotal |  | 3 | $265,000.00 | 55.44% |
| July 2016 | Alvin Lee | 1 | $38,000.00 | - |
|  | Johan Yu | 2 | $175,000.00 | - |
| Subtotal |  | 3 | $213,000.00 | 44.56% |
| GRAND TOTAL |  | 6 | $478,000.00 | - |

The subtotal for **June 2016** is **55.44%**; this is calculated by *265,000 / 478,000*100*. The same is for **July 2016** which is **44.56%**, which is calculated by *213,000 / 478,000*100*. The formula behind this is pretty simple and not much different from the one we used earlier: *AMOUNT:SUM / PARENTGROUPVAL(AMOUNT:SUM, GRAND_SUMMARY)*; in short, this formula will divide the monthly subtotal by the **GRAND TOTAL**.

# Using the PARENTGROUPVAL() function in the Matrix report

In the **Matrix** report, the syntax for this function is *PARENTGROUPVAL(summary_field, parent_row_grouping, parent_column_grouping)*.

The same is true with the **Summary** report; the `PARENTGROUPVAL()` function is not applicable when you select the formula displayed as **All summary levels**. This function is applicable only when you select **At a specific row/column grouping level...**.

The grouping level parameter in the **Summary** report is split into two parameters in the **Matrix** report: **parent_row_grouping and parent_column_grouping**.

Let's create a new Opportunity report with the **Matrix** format, filter the report by **Close Date** of 3 months, and then group by **Calendar Month (column)** and **Stage (row)**. Refer the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_190_0.png)

Let's consider a use case: we would like to know the percentage of each Stage compared to the GRAND TOTAL:

- 1. Create a formula summary called Total Amount and show it in Column Grand Summary. Refer the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_191_0.png)

Do not worry about how to get the keyword for the parameters; refer to the hands-on exercise in PARENTGROUPVAL () for the preceding Summary report; when you click on the < Insert button, the keyword will be automatically populated; then, add AMOUNT:SUM as summary_field. Run the report and you should see your report as something similar to what is shown in the following screenshot:

| CLOSE DATE | APRIL 2016 |  | MAY 2016 |  | JUNE 2016 |  | Total |  |
| STAGE | AMOUNT Sum | TOTAL AMOUNT | AMOUNT Sum | TOTAL AMOUNT | AMOUNT Sum | TOTAL AMOUNT | AMOUNT Sum | TOTAL AMOUNT |
| Prospecting | $0.00 |  | $0.00 |  | $80,000.00 |  | $80,000.00 | $645,000.00 |
| Qualification | $0.00 |  | $0.00 |  | $100,000.00 |  | $100,000.00 | $645,000.00 |
| Id. Decision Makers | $0.00 |  | $50,000.00 |  | $0.00 |  | $50,000.00 | $645,000.00 |
| Closed Won | $60,000.00 |  | $0.00 |  | $85,000.00 |  | $145,000.00 | $645,000.00 |
| Closed Lost | $270,000.00 |  | $0.00 |  | $0.00 |  | $270,000.00 | $645,000.00 |
| Total | $330,000.00 |  | $50,000.00 |  | $265,000.00 |  | $645,000.00 |  |

The TOTAL AMOUNT column here does not have much meaning; it just copies GRAND TOTAL to TOTAL AMOUNT for each stage, while we would like to know the percentage of each Stage compared to GRAND TOTAL.

- 2. Edit the report and change the summary formula:
  - Change the format to Percent
  - Change the formula to AMOUNT:SUM / PARENTGROUPVAL (AMOUNT:SUM, ROW_GRAND_SUMMARY, COLUMN_GRAND_SUMMARY)

So, we are dividing Column Total to GRAND TOTAL and showing the percentage. Here is the report:

| CLOSE DATE | APRIL 2016 |  | MAY 2016 |  | JUNE 2016 |  | Total |  |
| STAGE | AMOUNT Sum | TOTAL AMOUNT | AMOUNT Sum | TOTAL AMOUNT | AMOUNT Sum | TOTAL AMOUNT | AMOUNT Sum | TOTAL AMOUNT |
| Prospecting | $0.00 |  | $0.00 |  | $80,000.00 |  | $80,000.00 | 12.40% |
| Qualification | $0.00 |  | $0.00 |  | $100,000.00 |  | $100,000.00 | 15.50% |
| Id. Decision Makers | $0.00 |  | $50,000.00 |  | $0.00 |  | $50,000.00 | 7.75% |
| Closed Won | $60,000.00 |  | $0.00 |  | $85,000.00 |  | $145,000.00 | 22.48% |
| Closed Lost | $270,000.00 |  | $0.00 |  | $0.00 |  | $270,000.00 | 41.86% |
| Total | $330,000.00 |  | $50,000.00 |  | $265,000.00 |  | $645,000.00 |  |

With a simple change to **Custom Summary Formula**, you can move the formula to **Row Grand Summary** to get a percentage for each month by comparing it to **GRAND TOTAL**:

# Custom Summary Formula

Column Name: Total Amount

Description:

Format: Percent    Decimal Places: 2

# Where will this formula be displayed?

This formula calculation will be displayed in the report at the level you select.

At all summary levels

At a specific row/column grouping level...

A Row Grand Summary

B Close Date (by Calendar Month

# Formula

# Functions

# Tips

Summary Fields  Operators  Check Syntax

All  ABS

```
AMOUNT:SUM /
PARENTGROUPVAL(AMOUNT:SUM,
ROW_GRAND_SUMMARY,
COLUMN_GRAND_SUMMARY)
```

ABS(number)
Returns the absolute value of a number, a number without its sign

< Insert

Help on this function

# Advanced Report Configuration

Notice that A and B have changed; the formula stays the same, and here is the report:

| STAGE | APRIL 2016 AMOUNT | APRIL 2016 TOTAL AMOUNT | MAY 2016 AMOUNT | MAY 2016 TOTAL AMOUNT | JUNE 2016 AMOUNT | JUNE 2016 TOTAL AMOUNT | Total AMOUNT | Total TOTAL AMOUNT |
|---|---|---:|---:|---:|---:|---:|---:|---:|
| Prospecting | $0.00 |  | $0.00 |  | $80,000.00 |  | $80,000.00 |  |
| Qualification | $0.00 |  | $0.00 |  | $100,000.00 |  | $100,000.00 |  |
| Id. Decision Makers | $0.00 |  | $50,000.00 |  | $0.00 |  | $50,000.00 |  |
| Closed Won | $60,000.00 |  | $0.00 |  | $85,000.00 |  | $145,000.00 |  |
| Closed Lost | $270,000.00 |  | $0.00 |  | $0.00 |  | $270,000.00 |  |
| Total | $330,000.00 | 51.16% | $50,000.00 | 7.75% | $265,000.00 | 41.09% | $645,000.00 |  |

This report shows the percentage of the total amount for each month compared to the **GRAND TOTAL**.

# The PREVGROUPVAL() function

The PREVGROUPVAL() function returns the value of the previous grouping on the same hierarchy level. This allows you to prepare reports that show, for example, the month-to-month opportunity comparison; the main difference with the PARENTGROUPVAL() function is the return value from the higher hierarchy level.

The formula syntax for the PARENTGROUPVAL() function is the same for all report formats:

*PREVGROUPVAL(summary_field, grouping_level [, increment]).*

There is not much difference in the navigation and tips to get the formula parameters for the PARENTGROUPVAL() function compared to the PARENTGROUPVAL() function. The only difference in the syntax is an additional optional parameter **incremental**. If we skip this parameter, it will use the default value, which is 1. The increment is the number of columns or rows before the current summary; the **minimum** and the default value is 1, while the maximum is 12.

Although the syntax is almost similar to the PARENTGROUPVAL() function, the functionality is totally different. Both functions serve a different purpose to make our report powerful.

The same is the case with the PARENTGROUPVAL() function; this function is not applicable when you opt to display this formula: At **All summary levels, Grand summary only**. It will work only for field level groupings for the **Summary** report and **specific row/column grouping level** for the **Matrix** report.
