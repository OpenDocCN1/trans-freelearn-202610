

## 3. Click on the **Solver Add-In** checkbox, as shown in **Figure 6.23**:

![](img/0c7a357df7e57e06b6d9f631c219040c_206_0.png)

Figure 6.23 – Enabling the Solver Add-in checkbox

- 4. Click on **OK**.
- 5. Close the **Options** dialog box if needed.

At this point, a **Solver** command will appear on the **Data** tab of Excel's ribbon. Click on this button to display the **Solver** dialog box, as shown in **Figure 6.24**:

![](img/0c7a357df7e57e06b6d9f631c219040c_207_0.png)

Figure 6.24 – The Solver Parameters dialog box

Let’s say that you wanted to solve for a payment amount in the same fashion that we did for **Goal Seek**. You can use the same example that we used for **Goal Seek** if you change the interest rate back to 5.25% or any other static amount. Here are the steps for using **Solver** to determine the amount that you can borrow based on a particular interest rate, term, or payment:

- 1. Activate the **Solver** worksheet of this chapter’s example workbook.
- 2. Choose **Data | Solver**.
- 3. Enter C4 in the **Set Objective** field of the **Solver Parameters** dialog box, as shown in **Figure 6.21**.
- 4. Choose **Value Of**.
- 5. Enter a payment amount in the field adjacent to **Value Of**, such as 350.
- 6. Set the **By Changing Variable Cells** field to C5.
- 7. Click on the **Solve** button.

8. Excel will display a **Solver Results** dialog box, as shown in Figure 6.25. Optionally, you can click on the **Return to Solver Parameters Dialog** checkbox if you want to change this payment amount or return any of the other variables that you might have included in a scenario that has more moving parts to it.

9. Click on **OK** to return to the worksheet or the **Solver Parameters** dialog box based upon your choice in the previous step:

![](img/0c7a357df7e57e06b6d9f631c219040c_208_0.png)

Figure 6.25 – The Solver Results dialog box

**Nuance**

One benefit of using **Solver** versus **Goal Seek** is that the **Solver** feature remembers the choices that you make, whereas the **Goal Seek** dialog box resets itself every time. You can prefill the **Set Cell** field in the **Goal Seek** dialog box if you click on the formula cell before initiating the **Goal Seek** command. In short, **Solver** is more efficient if you are carrying out repeated goal-seeking actions.

As you can see in Figure 6.25, you can also store **Solver** results in Excel’s **Scenario Manager**, which I discussed in the _Understanding the Scenario Manager feature_ section earlier.

## Summary

Often, Excel’s **What-If Analysis** features are overlooked as a means to automate repetitive tasks in Excel. In this chapter, you learned about the `PMT`, `CUMIPMT`, and `CUMPRINC` functions, which can be used to calculate loan payments and total interest for one or more periods in a loan. We used those functions as the basis for walking through the **What-If Analysis** features in Excel. For instance, the **Scenario Manager** feature enables you to swap different sets of inputs into a worksheet and compare the results by way of scenario reports. Alternatively, **Goal Seek** allows you to solve for a single missing input. This eliminates the manual guesswork that can otherwise be involved in trying to determine the missing amount.

Then, our trip through the Data | What-If Analysis menu took us to the **Data Table** feature. **Data Tables** can solve multiple equations at once based on swapping out one or more inputs. This gives your formulas better integrity because you don’t have to try to link the inputs within the formula; you simply anchor the **Data Table** with a single formula and Excel swaps the inputs out.

If you want to project a series of numbers into the future, you can create a **Forecast Sheet** that will automate your projection. Such sheets cannot be created in **Excel for macOS** but can be accessed in that version. The chapter finished with an introduction to the hidden **Solver** feature that can be used to solve two or more missing inputs. I didn’t have space to delve very far into **Solver**, but you saw how **Solver** can streamline tasks that you might normally carry out in **Goal Seek** because **Solver** remembers the most recent settings, whereas the **Goal Seek** dialog box resets itself each time you access it.

In Chapter 7, *Automating Tasks with the Table Feature*, you’ll learn about numerous ways to automate Excel features by simply converting the underlying data into a Table. Additionally, I’ll draw a distinction between the similarly named **Data Table**, **Table**, and **PivotTable** features.

## Automating Tasks with the Table Feature

I consider the **Table** feature to be a superpower in Excel that unlocks tremendous automation capabilities with just a keyboard shortcut or a couple of mouse clicks. For instance, formulas and features that reference data in **Tables**, versus normal ranges of cells, automatically “see” any new data that you add to the **Table**. This eliminates having to rewrite formulas to expand the cell range and circle back to features such as **PivotTables** to expand the source data. **Slicers** eliminate the need to navigate drop-down menus when filtering, while enabling the **Total Row** option only tallies visible rows instead of the entire list. **Tables** allow **Charts** and **Sparklines** to be self-updating—the list truly goes on and on. In this chapter, I will cover the following main topics:

- Excel’s unwritten rule
- What is a Table?
- **Table** enhancements
- **Table** automation opportunities
- Navigation and selection nuances
- Troubleshooting **Tables**

In short, this chapter is all about showing you how to simultaneously eliminate repetitive tasks and improve data integrity in one fell swoop. This chapter also supplements _Chapter 12, Power Query_, as data you extract from other sources by way of Power Query is typically returned to a **Table**.

## Technical requirements

All the keyboard shortcuts in this chapter work in **Excel for Windows**, but only some of them will work in **Excel for macOS**. Unless otherwise noted, all features in this chapter will work the same way in **Excel for Windows** and **macOS**. A workbook with all the examples that I used in this chapter is available for download from GitHub at `https://github.com/PacktPublishing/Exploring-Microsoft-Excels-Hidden-Treasures/tree/main/Chapter07`.

## Excel's unwritten rule

Over the years, I've observed an unwritten rule that I find many users unwittingly break: the first row of _any_ list should be a _single_ row of unique titles—or **Headers** in Excel vernacular. This applies to every list that you create in Excel, no matter if you're using the **Table** feature or not. **Headers** are so crucial that the **Table** feature adds placeholder **Headers** when needed. Both **Tables** and **PivotTables** require that each **Header** be unique and will append numbers to duplicate **Headers** to enforce compliance.

> **Tip**
> An addendum to the preceding rule is that lists should not have any blank rows or columns. Most Excel features act on the **Current Region**, which is the contiguous block of non-blank cells surrounding the active cell. Blank rows and columns truncate the **Current Region**, and they require you to manually select the entire list before sorting or filtering instead of being able to choose a cell within the list and then sort or filter.

When creating new spreadsheets, try to keep **Headers** to a single row and then use **Home** | **Wrap Text** if needed for multiline display. You can also press `Alt + Enter` (`Option + Enter`) at each point within a cell where you want a line break. If you want to keep multiline **Headers** in place, consider inserting a blank row just immediately above the last **Header Row** that you can then hide.

> **Tip**
> Additionally, you can use the `TEXTJOIN` function in **Excel 2019** and later to combine text from multiple cells into one. Once you copy and paste the results as values, you can then delete the original rows.

