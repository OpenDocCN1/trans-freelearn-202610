

## Logical tests

The IF function in Excel has three arguments, **logical_test**, **value_if_true**, and **value_if_false**. The latter two arguments are used to display information or perform calculations. In the case of **Conditional Formatting**, you’ll only need the equivalent of the _logical_test_ argument. The custom rule you create will be applied when the test returns TRUE and will not be applied when your test returns FALSE.

> **Nuance**
> It’s easy to think that you need to enter an IF formula into the **Format values where this formula is true** field, but remember that **Conditional Formatting** needs is a logical test.

Let’s say that you want to create a rule where you format cells where an amount is greater than or equal to a particular value. Here are the steps to take:

1. Activate the **Custom Rule-Based on Input Cell** worksheet within this chapter’s example workbook.

2. Designate a cell as the input to drive the **Conditional Formatting**, such as cell H3 in **Figure 4.18**, and enter an amount, such as 7,500.

![](img/0c7a357df7e57e06b6d9f631c219040c_125_0.png)

3. Select the range of cells that you wish to format, such as cells **F6:H15**.

## Conditional Formatting

4. Choose **Home | Conditional Formatting | New Rule... | Use a formula to determine which cells to format.**

5. Enter `=$H6>$H$3` in the **Format values where this formula is true** field.

> Nuance
> The $ signs can make or break a Conditional Formatting rule. In this case, you _do not_ want dollar signs around H6 because you want the formatting to be based upon the contents of cells F6:H15. If you used $H$6, then the rule would format rows 6 through 15 based upon the contents of cell H6 alone. Conversely, you _do_ want $ around H3 to create an absolute reference to column H and row 3. If you omit the $, then your Conditional Formatting will be based upon the location of other cells relative to their position from cell H3.

6. Click the **Format** button and choose a color from the **Fill** tab, such as light green.

7. Click **OK** twice to close the dialog boxes.

8. Enter `5,000` in cell **H3** and note that more rows become highlighted.

Now that we’ve laid our hands on many aspects of applying or creating **Conditional Formatting** rules, let’s now see how to manage the rules that we’ve applied to our spreadsheet.

## Managing rules

Nothing about **Conditional Formatting** is set in stone, so you can always make changes as needed. I’ll discuss a few ways that you can manage existing rules in this section and provide even more options in the upcoming **Troubleshooting conditional formatting** section. In this section, I’ll show you how to change the color applied to a rule and adjust the range of cells that **Conditional Formatting** is applied to. I’ll also show you how to create a legend that can document the color scheme in use, and then we’ll look at how to remove rules from a range of cells all the way through an entire worksheet.

## Editing existing rules

Sometimes, you may decide that you want to change a color that a **Conditional Formatting** rule applies. Let’s change **Fill Color** for the first rule that we created at the start of this chapter:

1. Activate the **Greater Than-Less Than-Between** worksheet, and then click on cell **D3**.

2. Choose **Home | Conditional Formatting | Manage Rules**.

3. Double-click on the **Cell Value > 8000** rule, or click once on the rule and then click **Edit** to display the **Edit Formatting Rule** dialog box.

4. This dialog box allows you to change any aspect of the **Conditional Formatting** rule except for the range of cells that the rule applies to. Click the **Format** button and choose a shade of blue from the **Fill** tab.

5. Click **OK** to close the **Format Cells** dialog box.

6. Note that you now have a blue fill with red text. Click the **Format** button again, activate the **Font** tab, and then choose **Automatic** from the **Color** dropdown.

7. Click **OK** twice to close the dialog boxes.

As I mentioned, the **Edit Formatting** dialog box enables you to change every aspect of a rule other than which cells the rule applies to. We’ll use the **Conditional Formatting Rules Manager** to adjust the cells to be formatted.

## Applies to ranges

As shown in _Figure 4.19_, the **Applies to** column of the **Conditional Formatting Rules Manager** dialog box enables you to see which cells are being formatted. You can review this column to make sure that **Conditional Formatting** is being applied to the cells that you expect.

![](img/0c7a357df7e57e06b6d9f631c219040c_127_0.png)

Figure 4.19 – The Applies to column shows where Conditional Formatting is applied

Let’s see how easy it is for this range of cells to become compromised:

1. Activate the **Greater Than-Less Than-Between** worksheet.

2. Type 6450 into a blank cell, such as **M9** and then press **Enter**.

3. Copy cell **M9** and paste it into cell **J9**, which will erase the **Conditional Formatting** in that cell.

4. Choose **Home | Conditional Formatting | Manage Rules**.

5. The **Applies to** field for the rules that apply to column **D** will show two ranges, such as $D$3 : $D$8 , $D$10 : $D$22 , because cell **D9** no longer contains **Conditional Formatting**.

6. To restore your **Conditional Formatting**, click the button to the right of the **Applies to** field and reselect the range, or close the **Conditional Formatting Rules Manager** dialog box and choose **Home | Format Painter** to transfer the **Conditional Formatting** from an existing cell to any cells where the formatting has been removed. Either action will restore the contiguous range that you had previously.

Sometimes, rules can get so compromised that the path of least resistance is to remove them and start over. Or, rules get duplicated over and over, especially when applied to a single cell – I recently cleaned up a workbook for a client where a handful of **Conditional Formatting** rules had morphed into over 2,500 rules. Much more often though, I find myself using **Conditional Formatting** situationally, where I identify duplicates or unlocked cells and then remove the formatting.

## Creating a Conditional Formatting legend

Let’s see how to create the **Conditional Formatting** legend that appears in cells B2:E4 of the **Accessibility** worksheet. The first step is identifying the fill color assigned to the first level of **Conditional Formatting**:

1. Select cell **H6** of the Greater Than-Less Than-Between worksheet.
2. Choose **Home | Conditional Formatting | Manage Rules**.
3. Double-click on the **Cell Value > 8000** rule, or click once on the rule and then click **Edit**.
4. Click the **Format** button in the **Edit Formatting Rule** dialog box.
5. Activate the **Fill** tab of the **Format Cells** dialog box.
6. Click the **More Colors** button.
7. Select the characters in the **Hex** field and then press **Ctrl + C (**⌘**+ C)**, or right-click on the selection and choose **Copy**.
8. Click **OK** four times to close the series of dialog boxes.
9. Select cells **G2:I2** and then choose **Home | Format | Format Cells**, or press **Ctrl + 1 (**⌘**+ 1)**.
10. Activate the **Fill** tab and then click **More Colors**.
11. Activate the **Custom** tab and then paste the **Hex** code you copied from **Step 7**.
12. Click **OK** twice to close the **Format Cells** dialog box.
13. **Optional**: Repeat the same steps to capture the font color used by **Conditional Formatting**, as well by activating the **Font** tab instead of the **Fill** tab.

Nuance
The Hex code enables you to exactly match a color from elsewhere in the worksheet or from a color palette chart. An alternative to Hex codes is the RGB model, which is short for Red Green Blue, a series of 1-to-3-digit numbers that can be used to match colors as well.

## Making the Conditional Formatting legend accessible

In Chapter 1, Implementing Accessibility, I discuss how a good number of Excel users cannot see colors due to color blindness or other conditions. Since Conditional Formatting is mostly color based, it's easy to solve one problem, say categorizing data, while creating a new problem, such as making the data inaccessible to a subset of users. As shown in Figure 4.20, the Accessibility worksheet in the example workbook shows one approach you can take, where cells D6:D25 have both color and cell icon Conditional Formatting applied, while the legend includes words that describe the shapes. This approach combines techniques covered earlier in this chapter.

![](img/0c7a357df7e57e06b6d9f631c219040c_129_0.png)

Figure 4.20 – An accessible Conditional Formatting legend

## Removing Conditional Formatting

Let's first use the Conditional Formatting Rules Manager to remove individual rules:

1. Activate the Remove Individual Rules worksheet.
2. Choose Home | Conditional Formatting | Manage Rules.
3. Choose This Worksheet from the Show formatting rules for list if necessary.

