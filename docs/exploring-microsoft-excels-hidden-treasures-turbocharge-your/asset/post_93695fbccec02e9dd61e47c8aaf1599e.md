

## Excluding weekend dates from charts

Fortunately, there’s an easy fix, although many of us would be hard-pressed to figure it out on our own. Check this out:

1. Right-click on the Horizontal (Category) axis and then choose Format Axis.
2. Choose Axis Options | Axis Type | Text axis to close the gap. The settings are illustrated in Figure 9.12:

Figure 9.12 – Choosing Text axis to remove phantom dates from charts

As you can see, when the Chart Axis options are set to Automatically select based on data or Date axis, Excel interpolates any missing dates to display every day of the week. Choosing Text axis instructs Excel to only display dates that are present in the underlying chart data. The good news is this is an easy fix, but the bad news is it can mean yet another repetitive task in Excel—unless you create a chart template that has this setting in place.

Tip
The first step in creating a chart template is to right-click on a chart that has all of the settings you want to use going forward, and then choose **Save as Template**. Assign a name for your template, and then click on **Save**. To utilize your template, click on a dataset that you wish to chart, and then choose **Insert | Recommended Charts | All Charts | Templates**. Double-click on your template of choice to skip the **OK** button.

## Sparklines

Traditional charts in Excel may take up a lot of space on the screen or printed page, but are not the only game in town. **Sparklines** are tiny charts that appear within worksheet cells, as shown in cells B4:B8 in Figure 9.13:

Figure 9.13 – Sparklines

For instance, you might use **Sparklines** as part of a dashboard, which is a spreadsheet that communicates a large amount of information in compact form. Let’s first walk through the process of creating each type of **Sparkline** and then I’ll show you an odd quirk. Follow the next steps:

1. Activate the **Sparklines** worksheet in the example workbook for this chapter.
2. Select cells **F4:I4** and then **Insert | Line** within the **Sparklines** group to display the **Create Sparklines** dialog box.
3. As shown in Figure 9.14, the **Data Range** setting is prefilled with the range of cells you selected:

Figure 9.14 – Create Sparklines dialog box

Nuance
Often in Excel, we select the data that we want to work with before initiating a command, as described previously. Although you _can_ do that with **Sparklines**, it’s best to do the opposite, and _first_ select the cell or cells where you want **Sparklines** to appear, as you’ll see later in this chapter.

4. Click in the **Location Range** field, then click on cell E4 or type `E4`, and then click **OK** to make a **Line Sparkline** appear in cell E4.

Tip
The size of a **Sparkline** chart correlates with the height and width of a cell. Increase the row height and/or column width of the underlying cell if you wish to make **Sparklines** larger.

5. Select cell E5, and then choose **Insert | Column** from within the **Sparklines** group.
6. The **Location Range** setting is set to E5, so specify cells F5:I5 for the **Data Range** field, and then click **OK** to create a **Column Sparkline**.
7. Select cell E6, and then choose **Insert | Win/Loss**.
8. **Location Range** is set to E6, so specify F6:I6 for the **Data Range** field, and then click **OK** to create a **Win/Loss Sparkline**.

Tip
**Line** and **Column Sparklines** offer a sense of scale, while a **Win/Loss Sparkline** simply gives an indication of which amounts are positive and which are negative.

9. Now that you’ve created one of each type of **Sparkline**, let’s get into the quirk. Activate the **Enter Mode Versus Edit Mode** worksheet.

10. Select cells C4:F5 and then **Insert | Column**.

11. Click in the Location Range field, and then try to click on the Sparklines worksheet. Inexplicably, you can only select cells on the active worksheet from the Location Range field. Click **Cancel** to close the Create Sparklines dialog box.

12. Select cells E7:E8 on the Sparklines worksheet, and then choose **Insert | Column**.

13. Specify cells C4:F5 on the Enter Mode Versus Edit Mode worksheet in the Data Range field, and then click OK. Notice that now you _can_ add Sparklines to multiple cells if you wish.

> Tip
> You can create self-expanding sparklines by turning the source data into a Table. I discuss Tables in more detail in _Chapter 7, Automating Tasks with the Table Feature_. In this case, click on cell **B3** of the Enter Mode Versus Edit Mode worksheet, choose **Insert | Table**, and then click OK. New periods that you add to column F and beyond will now automatically appear in the Sparklines in cells E7:E8 of the Sparklines worksheet.

Notice that a **Sparkline** tab appears in Excel’s Ribbon when you click on a cell that contains a **Sparkline**. This tab enables you to do the following:

1. Edit the data range that a **Sparkline** refers to, including controlling the settings for hidden and empty cells in a similar fashion to what I described with charts in the previous section.
2. Toggle to a different type of **Sparkline**.
3. Identify various points within **Sparkline**—for instance, the minimum and maximum values.
4. Change the style and color of **Sparklines**.
5. Control axis settings.
6. Clear **Sparklines** from one or more cells.

> Nuance
> You should choose **Sparkline | Clear** or **Home | Clear | Clear All** to remove **Sparklines**. Pressing the **Delete** key in a cell that contains a **Sparkline** will erase the cell contents (if any) but will leave the **Sparkline** intact, which means you can type other information in a cell that contains a **sparkline** if you wish. The **Clear All** command will erase not only the **Sparkline** but any other cell formatting or contents as well.

Let’s move on from working with charts and see how to resolve circular references that can arise within your spreadsheets.

## Circular references

**Circular references** are formulas that refer to themselves, like a snake trying to eat its own tail. Let’s say that you entered the formula =SUM(B3:B5) in cell B5 of the **Circular References** worksheet, as shown in _Figure 9.15_. Typically, circular references are created accidentally. Excel alerts you with the prompt shown in _Figure 9.15_.

Figure 9.15 – Circular reference

If you click OK on this prompt, the formula may return an amount, or it may return zero. Excel displays a **Circular References** message on the **Status Bar** informing you of the cell that contains a circular reference, as shown in _Figure 9.16_:

Figure 9.16 – Circular reference Status Bar message

Conversely, if a circular reference exists on another worksheet or even another open workbook, no cell reference will appear, and the status bar will simply report **Circular References**. Fortunately, there’s a hidden menu command that you can use to navigate directly to circular references by following these steps:

1. Choose the **Formulas** | **Error Checking** dropdown | **Circular References**, and then choose a cell reference, as shown in **Figure 9.17**:

Figure 9.17 – Circular References menu command

> **Nuance**
> The **Circular References** command is unavailable when no open workbooks contain circular references, or if the **Enable iterative calculation** option discussed in the next section is enabled.

2. Excel activates the worksheet cell that you selected.
3. Edit the formula to eliminate the circular reference, meaning adjust the cell reference so that it does not include the cell that the formula is in.
4. Repeat _steps 1 through 3_ as needed until you eliminate all circular references.

Although most circular references are accidental, let's see what to do when you purposefully want to create a circular reference.

### Enable iterative calculation option

You might occasionally run across a situation where you purposefully need to use a circular reference in a spreadsheet. Let's say that a real-estate developer has been tasked with constructing a new building that costs 100 million **United States dollars (USD)**, for which the developer will earn a 3% development fee. If the developer agrees to the fee being 3% of the _building cost_, they will earn $3 million.

