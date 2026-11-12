

| Calculation type | Calculated field in Tableau | Query passed to data source |
| :--- | :--- | :--- |
| Table-level | WINDOW_SUM([Number of Records])/Quantity)) | SELECT [Number of Records], SUM(Quantity) FROM [Orders] |

Table 1.1: Levels of calculation

For the row- and aggregate-level calculations, the computation is actually completed by the data source engine, as Tableau is an in-memory tool. Tableau merely displays the results. This, however, isn't the case for the table-level calculation. Although the query passed to the data source for the table-level calculation is identical to the query for the aggregate-level calculation, Tableau performs additional computations on the returned results. Let's explore this further with an exercise using the same calculated fields.

Let's look at the following steps and begin our exercise:

- 1. In the workbook associated with this chapter, navigate to the Row_Agg_Tbl worksheet.
- 2. Connect to the Orders data source by clicking on Orders in the Data section in the top-left corner.

![](img/66cf6b89349fda3581a4f196cb3c98a4_62_0.png)

Figure 1.43: Connecting to Sample - Superstore dataset

- 3. Select Analysis | Create Calculated Field to create the following calculated fields. Note that each must be created separately; that is, it isn't possible in this context to create a single calculated field that contains all three calculations:
- o. Name the first calculation Lev–Row and enter the following code: [Number of Records]/[Quantity].
- o. Next, name the second calculation Lev–Agg and enter the following code: SUM([Number of Records])/SUM([Quantity]).
- o. Lastly, name the third calculation Lev–Tab and enter WINDOW_AVG([Lev–Agg]).
- 4. In the Data pane, right-click on the three calculated fields you just created and select Default Properties | Number format.
- 5. In the resulting dialog box, select Percentage and click OK.
- 6. Place Order Date on the Columns shelf.
- 7. Place Measure Names on the Rows shelf and Measure Values on Text on the Marks card.
- 8. Exclude all values except for Lev–Row, Lev–Agg, and Lev–Tab by dragging them off the Measure Values shelf or removing them from the Measure Names filter:

![](img/66cf6b89349fda3581a4f196cb3c98a4_63_0.png)

Figure 1.44: Level of calculations

Let's break the values in detail:

- **Lev–Agg** is an aggregate-level calculation. The computation is completed by the data source engine. The sum of [Number of Records] is divided by the sum of [Quantity]. The results of the calculation are likely useful for the Tableau author.

- **Lev–Row** is a row-level calculation. The computation is completed by the data source engine. The [Number of Records] field is divided by the [Quantity] field for each row of the underlying data. The results are then summed across all rows. Of course, in this case, the row-level calculation doesn't provide useful results; however, since a new Tableau author may mistakenly create a row-level calculation when an aggregate-level calculation is what's really needed, the example is included here.

- **Lev–Tab** is a table calculation. Some of the computation is completed by the data source engine, that is, the aggregation. Tableau completes additional computation on the results returned from the data source engine based on the dimensions and level of detail in the data displayed in the sheet. Specifically, the results of **Lev–Agg** are summed and then divided by the number of members in the dimension. For the preceding example, this is the following:

(26.29% + 26.34% + 26.3% + 26.55%)
4

Once again, the results in this case aren't particularly helpful but do demonstrate knowledge that a budding Tableau author should possess.

## Continuous and discrete values

Continuous and discrete aren't concepts that are unique to Tableau. Indeed, both can be observed in many arenas. Consider the following example of two rivers: River-Liquid and River-Ice. Water is flowing in River-Liquid. River-Ice is composed of ice cubes. Could you theoretically sort the ice cubes in River-Ice? Yes! Is there any way to sort the water in River-Liquid? In other words, could you take buckets of water from the bottom of the river, cart those buckets upstream, and pour the water back into River-Liquid, and thereby say "I have sorted the water in the river?" No.

The H²O in River-Liquid is in a continuous form; that is, water. The H²O in River-Ice is in a discrete form; that is, ice.

Having considered continuous and discrete examples in nature, let's turn our attention back to Tableau. Continuous and discrete in Tableau can be more clearly understood with the following seven considerations:

- **Continuous is green and discrete is blue**: Select any field in the **Data** pane or place any field on a shelf, and you'll note that it's either green or blue. Also, the icons associated with fields are either green or blue.

- Continuous is always numeric. Discrete may be a string.

- Continuous and discrete aren't synonymous with dimensions and measures:

- It's common for new Tableau authors to conflate continuous with measures and discrete with dimensions. They aren't synonymous. A measure may be either discrete or continuous.

- Also, a dimension, if it's a number, may be discrete or continuous. To prove this point, right-click on any numeric or date field in Tableau and note that you can convert it:

![](img/66cf6b89349fda3581a4f196cb3c98a4_65_0.png)

Figure 1.45: Converting between discrete and continuous

- Discrete values can be sorted. Continuous values can't: Sortable/not sortable behavior is most easily observed with dates and a measure like Sum(Sales), as shown in the following example:

![](img/66cf6b89349fda3581a4f196cb3c98a4_65_1.png)

Figure 1.46: Left: continuous (not sortable); right: discrete (sortable)

- **Continuous colors are gradients. Discrete colors are distinct:** The following example shows **Sales** as continuous and then as discrete. Note the difference in how colors are rendered. The left portion of the screenshot demonstrates the continuous results in gradients, and the right portion demonstrates the discrete results in distinctly colored categories:

![](img/66cf6b89349fda3581a4f196cb3c98a4_66_0.png)

Figure 1.47: Sales as continuous (left) and discrete (right)

- **Continuous pills can be placed to the right of discrete pills, but not to the left because the discrete pills define the aggregation level:**
  - The Tableau author is able to place **Region** to the right of **Year** when **Year** is discrete
  - The Tableau author is unable to place **Region** to the right of **Year** when **Year** is continuous

- **Continuous creates axes and discrete creates headers:** Note in the right portion of the following screenshot that **Order Date** is continuous and the **Year of Order Date** axis is selected. Since **Year of Order Date** is an axis, the entire x-plane is selected. In the left portion, however, **Year(Order Date)** is discrete, and 2014 is selected. Since 2014 is a header only, it's selected and not the entire x-plane. **Figure 1.48** shows the discrete dates.

### Sheet 12

Pages Columns YEAR(Ship Date) Rows SUM(Sales)

Filters Marks Bar Color Size Label Detail Tooltip SUM(Sales)

![](img/66cf6b89349fda3581a4f196cb3c98a4_67_0.png)

Figure 1.48: Discrete dates

Figure 1.49 shows the continuous dates.

![](img/66cf6b89349fda3581a4f196cb3c98a4_68_0.png)

Figure 1.49: Continuous dates

Congratulations—you just mastered three important concepts in Tableau: measures and dimensions, the level of calculations, and discrete and continuous values. With this knowledge alone, you will be able to create your first dashboards. I want to highly encourage you to do so; the learning curve when playing around with these tools will be very steep, and you will gain confidence quickly with practice! It will also help you to follow-along in the upcoming chapters.

To conclude the basics, we will cover the export of dashboards to other devices next. Even if you don’t feel ready to do so yet, it will round up the basic Tableau Desktop tour and will provide the raw diamond as a whole. We can then continue improving all bits and pieces in future chapters!

## Exporting data to other devices

Once a dashboard looks as it’s expected to, the developer has different choices for sharing the work. An upload to Tableau Server is the most likely option. The end user might not look at the results on just a laptop; they could use a tablet or cellphone, too.

