

## Top and bottom rules

The top and bottom rules allow you to identify values based on their relative positions within a list. Four out of the six reference 10 Items or 10%, but you can enter any number or percentage that you wish. To apply any of these rules, do the following:

- Select a range of cells that you wish to format, such as cells H3:H22, of the Top and Bottom Rules worksheet in this chapter’s example workbook.
- Choose Home | Conditional Formatting | Top/Bottom Rules and then select a rule:
  - Top 10 Items: Formats the 10 largest values in the list unless you specify a different value
  - Top 10%: Formats 10% of the largest values in the list unless you specify a different value
  - Bottom 10 Items: Formats the 10 smallest values in the list unless you specify a different value
  - Bottom 10%: Formats 10% of the largest values in the list unless you specify a different value
  - Above Average: Formats cells that exceed the average, meaning that in a list that contains the numbers 1 to 10 that amounts 6 through 10 would be formatted
  - Below Average: Formats cells that are less the average, meaning that in a list that contains the numbers 1 to 10 that amounts 1 through 5 would be formatted
- Optional: Enter a number or percentage if the rule allows.
- Optional: Assign formatting of your choice.
- Click OK to close the Conditional Formatting dialog box.

> Nuance
> The percentage is based on the number of items on your list. If your list has 90 items and you specify Top 10%, the Conditional Formatting feature will multiply 90 by 10 to determine that the 9 largest values should be formatted.

Now, let’s look at using Data Bars to illustrate your data with in-cell charts.

## Data Bars

Data Bars are charts that appear within cells to provide a sense of proportionate scale within a column of numbers. As you’ll see, numbers can appear in the cell with the charts, or you can hide the numbers. Cells D3:D22 of the Data Bars worksheet in the example workbook for this chapter show how numbers and Data Bars can coexist in the same cell. To create this type of formatting, do the following:

- Select a range of cells that contain numbers, such as K3:K22 of the Data Bars worksheet.
- Choose Home | Conditional Formatting | Data Bars and select from the Gradient Fill or Solid Fill sections.

> Tip
> Most Conditional Formatting rules in the desktop versions of Excel offer live previews as you craft a rule, or when you hover your mouse over a particular data bar, color scale, or icon set.

3. As shown in Figure 4.10, the Data Bars are commensurate with the size of the numbers in a cell in comparison to the other numbers in the list. Thus, the largest values will have the longest Data Bars, and the smallest values will have the shortest Data Bars.

![](img/0c7a357df7e57e06b6d9f631c219040c_116_0.png)

Figure 4.10 – Data Bars Conditional Formatting

Alternatively, you can show amounts in one column and data bars in the second column:

- Select cells L3:L22 of the Data Bars worksheet, which contains the same numbers present in cells K3:K22.
- Choose Home | Conditional Formatting | Data Bars | More Rules...
- Click the Show Bar Only checkbox and then click OK.

Data Bars are one of many tools you can use in Excel to create a dashboard, which is a spreadsheet that presents a large amount of information in as small a space as possible, typically with graphics that can be comprehended immediately. For instance, cells O3:P14 illustrate how a mix of positive and negative values are presented, as shown in Figure 4.11.

![](img/0c7a357df7e57e06b6d9f631c219040c_117_0.png)

Figure 4.11 – Data Bars presenting negative and positive amounts

Next, let’s look at how to apply color scales by way of Conditional Formatting.

## Color Scales

Color Scales are an easy way to create a heat map within your data, and assign colors based on a number’s proportionate scale within a list. Here are the steps:

- Optional: Sort the amounts in ascending or descending order.
- Select a range of cells that contain numbers, such as cells I3:I22 in the Color Scales worksheet of this chapter’s example workbook.
- Choose Home | Conditional Formatting | Color Scales, and then choose a Color Scale grouping to create a heat map, such as cells D3:D22 in Figure 4.12.

![](img/0c7a357df7e57e06b6d9f631c219040c_117_1.png)

Figure 4.12 – Color scales Conditional Formatting

## Icon Sets

Icon Sets, also known as Cell Icons, are in-cell graphical indicators that provide a sense of scale or value relative to other numbers in your list. As with Data Bars, you can choose to show numbers and icons in the same cell or hide the numbers and only show the icons. By default, Icon Sets group amounts based upon percentages, but you can edit the rule to use percentiles instead. Here are the default percentages that Excel uses and how they’re calculated:

- 67th percent: This calculation takes the form smallest value from the list plus .67 multiplied by (largest value minus smallest value). This is illustrated by the formula =MIN(D8:D27)+0.67*(MAX(D8:D27)-MIN(D8:D27)) in cell C2 of the Cell Icons – Percent worksheet. The MIN function returns the smallest value in a range, while the MAX function returns the largest. As shown in cell D2, this calculates as 72.03%, which means any amounts that are greater than the largest value in the list multiplied by 72.03% will be formatted with the first icon in the set.
- 33rd percent: This calculation takes the form smallest value from the list plus .33 multiplied by (largest value minus smallest value). This is illustrated by the formula =MIN(D8:D27)+0.33*(MAX(D8:D27)-MIN(D8:D27)) in cell C3. As shown in cell D3, this calculates out to 43.21%, which means any amounts that are less than the largest value in the list multiplied by 43.21% will be formatted with the third icon in the set.
- 34th through 66th percent: In this case, any amounts greater than 43.21% of the largest value and less than 72.03% of the largest value are formatted with the second icon in the set.

> Nuance
> Icon Sets that have four icons base these calculations on 25%, 50%, and 75%, while Icon Sets that have five icons use 20%, 40%, 60%, and 80% to determine the brackets.

> Nuance
> As with data bars, you can choose More Rules to fine-tune the presentation of the formatting.

Now, let’s look at the Icon Sets rule, which offers yet another way to graphically present your data.

