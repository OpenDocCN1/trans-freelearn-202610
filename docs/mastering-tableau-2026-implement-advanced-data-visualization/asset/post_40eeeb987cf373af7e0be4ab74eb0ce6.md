
In this section, we'll watch what happens when these generated fields are used independently, then observe how they work elegantly together to create a view. Let's explore this with an exercise:

- 1. In the workbook associated with this chapter, navigate to the MeasureNames/Values worksheet and make sure that the Sample - Superstore data source is selected.
- 2. Drag Measure Values to the Text shelf and observe the results; all four values are printed on top of each other:

![](img/66cf6b89349fda3581a4f196cb3c98a4_53_0.png)

Figure 1.33: Measure Values

- 3. Clear the worksheet by clicking on the Clear Sheet icon on the toolbar:

![](img/66cf6b89349fda3581a4f196cb3c98a4_53_1.png)

Figure 1.34: Clear Sheet

- 4. Now, drag **Measure Names** to the **Rows** shelf and observe that the view merely displays **No Measure Values.**

- 5. Drag **Measure Values** to the **Text** shelf. Note the list of measures and associated values; each of the four values is printed in the respective row:

![](img/66cf6b89349fda3581a4f196cb3c98a4_54_0.png)

Figure 1.35: List of measure values

Perhaps the relationship between **Measure Names** and **Measure Values** is best explained by an analogy. Consider several pairs of socks and a partitioned sock drawer. *Step 2* is the equivalent of throwing the socks into a pile. The results are, well, disorganized. *Step 4* is the equivalent of an empty sock drawer with partitions. The partitions are all in place, but where are the socks? *Step 5* is a partitioned drawer full of nicely organized socks. **Measure Names** is like the sock drawer. **Measure Values** is like the socks. Independent of one another, they aren't of much use. Used together, they can be applied in many different ways.

## Measure Names and Measure Values shortcuts

Tableau provides various shortcuts to quickly create the desired visualization. If you're new to the software, this shortcut behavior may not seem intuitive, but you'll gain an understanding of it. Let's use the following exercise to explore how you can use a shortcut to rapidly deploy Measure Names and Measure Values:

- 1. In the workbook associated with this chapter, navigate to the MeasureNames/Values ShrtCts worksheet.

- 2. Drag Profit onto Text in the Marks card.

- 3. Drag Sales directly on top of Profit in the view (Show Me appears):

![](img/66cf6b89349fda3581a4f196cb3c98a4_55_0.png)

Figure 1.36: Show Me and measures

- 4. Observe the results, including the appearance of the **Measure Values** shelf, the deployment of **Measure Names** on the **Rows** and **Filters** shelves, and **Measure Values** on the **Text** shelf:

![](img/66cf6b89349fda3581a4f196cb3c98a4_56_0.png)

Figure 1.37: Measure Values appearance

Several things happened in step 2 of this exercise. After placing the Sales number on top of the Profit number in the view, Tableau did the following:

- 1. **Deployed Measure Names on the Filters shelf**: Open the **Measure Names** filter by right-clicking and hit **Edit Filter...**. Observe that only **Sales** and **Profit** are selected. This limits the view to display only those two measures.

- 2. **Deployed Measure Names on the Rows shelf**: **Measure Names** is acting like a partitioned container—that is, like the sock drawer in the analogy. Because of the filter, the only rows that display are for **Sales** and **Profit**.

- 3. **Displayed the Measure Values shelf**: The **Measure Values** shelf is somewhat redundant. Although it clearly shows measures that are displayed in the view, it essentially acts as an easy way to access the filter. You can simply drag measures on and off the **Measure Values** shelf to adjust the filter and thus display/hide additional measure values. You can also change the order within the **Measure Values** shelf to change the order of measures in the view.

- 4. **Deployed Measure Values on the Text shelf:** Measure Values is simply defining numbers that will display for each row—in this case, numbers associated with **Sales** and **Profit**.

If the visualization has an axis, the shortcut to deploy **Measure Names** and **Measure Values** requires the placement of a second measure on top of the axis of an initial measure. In *Figure 1.38*, **Order Date** is located on the **Columns** shelf and **Sales** on the **Rows** shelf. Note that the screenshot was taken while **Profit** was placed on top of the y axis:

![](img/66cf6b89349fda3581a4f196cb3c98a4_57_0.png)

Figure 1.38: Axis shortcut

The resulting worksheet can be seen in Figure 1.39:

![](img/66cf6b89349fda3581a4f196cb3c98a4_58_0.png)

Figure 1.39: Two-line chart

This section gave us a better understanding of **Measure Names** and **Measure Values**. You will come across those concepts more in your Tableau career, but now that you've successfully mastered the basics, let's move on to three other concepts that will be beneficial for your learning curve.

## Three essential Tableau concepts

An important stop on the road to mastering Tableau involves three essential concepts. In this section, we'll discuss each of them:

- Dimensions and measures
- Row-level, aggregate-level, and table-level calculations
- Continuous and discrete values

We'll start by defining dimensions and measures.

### Dimensions and measures

Tableau categorizes every field from an underlying data source as either a **dimension** or a **measure**. A dimension is qualitative or, to use another word, categorical. A measure is quantitative or aggregable. A measure is usually a number but may be an aggregated, non-numeric field, such as MAX ( Date ). A dimension is usually a text, Boolean, or date field, but may also be a number, such as Count. Dimensions provide meaning to numbers by slicing those numbers into separate parts/categories. Measures without dimensions are mostly meaningless.

Let's look at an example to understand better:

- 1. In the workbook associated with this chapter, navigate to the **Dimensions and Measures** worksheet.

- 2. Drag **Orders (Count)** to the **Text** shelf on the **Marks** card. The result is mostly meaningless. The result is **10,194**, but without the context supplied by slicing the measure across one or more dimensions, there is really no way to understand what it means:

![](img/66cf6b89349fda3581a4f196cb3c98a4_59_0.png)

Figure 1.40: Worksheet without context

- 3. Place **Region** on the **Columns** shelf and **Order Date** on the **Rows** shelf:

![](img/66cf6b89349fda3581a4f196cb3c98a4_60_0.png)

| Year of Order Date | Central | East | South | West |
| :--- | :--- | :--- | :--- | :--- |
| 2022 | 466 | 560 | 349 | 676 |
| 2023 | 476 | 664 | 340 | 650 |
| 2024 | 609 | 797 | 413 | 815 |
| 2025 | 784 | 965 | 518 | 1,112 |

Figure 1.41: Dimensions and Measures

As shown in Figure 1.41, step 3 brings meaning. Placing Region and Order Date on the Columns and Rows shelves provides context, which imparts meaning to the visualization.

### Row-level, aggregate-level, and table-level calculations

There are three levels of calculations in Tableau: row, aggregate, and table. To understand how these three levels function, it’s important to understand the Tableau processes. We’ll do so with an example that considers the Number of Records and Quantity fields from the Orders dataset.

In earlier versions of Tableau, Number of Records was a field created automatically. This is not the case anymore; therefore, please create a calculated field called Number of Records by selecting Analysis | Create Calculated Field and typing 1 inside it:

![](img/66cf6b89349fda3581a4f196cb3c98a4_61_0.png)

Figure 1.42: Number of Records

Consider the following calculation types: calculated fields and queries. A comparison to the commonly used language SQL will give us a better understanding of how to interpret Tableau calculations. Note that the SQL is slightly simplified for the sake of this example.

Let's take a deeper look at the three levels of calculations and consider the example in the following table:

| Calculation type | Calculated field in Tableau | Query passed to data source |
| --- | --- | --- |
| Row-level | Number of Records / Quantity | SELECT Number of Records / Quantity FROM Orders |
| Aggregate-level | [Number of Records] / SUM(Quantity) | SELECT [Number of Records], SUM(Quantity) FROM [Orders] |
