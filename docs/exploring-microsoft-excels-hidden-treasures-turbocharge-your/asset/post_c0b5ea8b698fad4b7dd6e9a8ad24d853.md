

## Names, LET, and LAMBDA

6. Accept the default cell reference of = 'Names and Formulas'!$C$8 in the Refers To field unless you wish to assign the name to a different cell. You do not need to add an equals sign (=) or a dollar sign ($) as Excel will do so automatically when you click OK.

7. Click OK to create the name.

8. Type =Annual in any cell and notice how the AutoComplete menu displays the full name, along with the comment that you entered. Then, press Escape to discard the entry.

**Nuance**
You can also enter numbers, text, or formulas in the Refers To field. Text must be enclosed in double quotes, and formulas must begin with an equals sign (=). This approach offers a way to make it harder for others to alter key inputs in your workbook because the information won’t appear in a worksheet cell, but can be referenced by its name as if it were. The data associated with such Names can be edited in the Name Manager. Formulas that you enter in the Refers to field should have dollar signs ($) before the column and row; otherwise, you may find your name referring to random cells as you move your cursor from cell to cell.

Now that we’ve created Names in three different ways, let’s use Name Manager to edit or delete them.

## Name Manager

Every name that’s created within an Excel workbook will appear in the Name Manager, along with any Tables in the workbook. The Name Manager allows you to do the following task:

- Add or remove comments for Names or Tables
- Change Names and Table Names
- Change the Refers to field for Names, and Delete Names.

**Tip**
In Chapter 7, Automating Tasks with the Table Feature, I showed you how to edit and rename Tables, as well as how to convert a Tables back into a normal range of cells.

Let’s walk through the Name Manager:

1. Choose Formulas | Name Manager to display the dialog box shown in Figure 11.10:

![](img/0c7a357df7e57e06b6d9f631c219040c_355_0.png)

Figure 11.10 – Name Manager

2. Click **New** to display the **New Name** dialog box, which is an alternative to choosing **Formulas** | **Define Name**. Press **Escape** or click **Cancel** to close the **New Name** dialog box.

3. Choose any name on the list and then click **Edit** to display the **Edit Name** dialog box, as shown in Figure 11.11, which is identical to the **New Name** dialog box. You can edit any field except for **Scope** for **Names** – you can only edit the **Name** and **Comment** fields for **Tables**. Press **Escape** (or click **Cancel**) to close the **Edit Name** dialog box without making any changes; otherwise, press **Enter** (or click **OK**). You must delete and then recreate a name to change its scope:

![](img/0c7a357df7e57e06b6d9f631c219040c_355_1.png)

Figure 11.11 – Edit Name dialog box

**Nuance**
Fields such as **Refers to** default to **Enter** mode, which means that touching the left or right arrow keys will insert an often-unwanted cell reference instead of moving your cursor left or right. Press the _F2_ (_Fn_ + _F2_) key to switch to **Edit** mode to enable navigation within the field. I discuss this concept in more detail in _Chapter 9, Excel Quirks and Nuances_.

4. Choose **Height** from the list, click the **Delete** button, and then click **Close**.

5. Notice how the formula in cell **F4** of the `Names and Formulas` worksheet returns `#NAME?`. This error arose because we deleted the **Height** range, which the formula still refers to. If you didn’t enter a formula in cell **F4** yet, type `=Height` and press **Enter**, which will also return `#NAME?`.

**Tip**
**Names** referred to within formulas do not revert to cell references when the name is deleted. To resolve a `#NAME?` error that arises from deleted **Names**, you must either manually update any formulas to use cell references instead or recreate the **Names**. Note that the `#NAME?` error can also arise when you misspell a name or worksheet function name, or when you refer to a non-existent name or worksheet function.

6. Click on cell **C5**, type `Height` in the **Name Box** area, and then press **Enter**. Notice how the formula in cell **F4** now returns a proper result.

**Tip**
Any of the methods we’ve used to create **Names** can also be used to recreate deleted **Names**.

Now, let’s return to the `Names Introduction` worksheet and see what happens when named cells are deleted:

1. Double-click cell **F3**, press _Ctrl_ + _A_ (`⌘` + _A_) to select the entire formula, press _Ctrl_ + _C_ (`⌘` + _C_) to copy the formula, and then press **Escape** so that you don’t accidentally modify the formula.

2. Click on cell **F7** and then press _Ctrl_ + _V_ (`⌘` + _V_) or choose **Home | Paste** to paste the formula, which should return `3, 456`.

**Nuance**
Copying formulas from the **Formula Bar** area, as opposed to from the cell, prevents Excel from changing the column letters and/or row numbers. Pressing _Ctrl_ + _A_ (`⌘` + _A_) in the **Formula Bar** area is an easy way to select an entire formula.

3. Enter the =L*W*H formula in cell F8 if needed. This formula should also return 3 , 456.

4. Select rows 2:5 on the Names Introduction worksheet and choose Home | Delete (or right-click on the selection and choose Delete).

5. The formulas that were in F7:F8 are now in cells F4:F5 and return #REF! because the underlying cells being referenced were deleted.

6. Choose Formulas | Name Manager and notice how the Refers to column now returns #REF! for the L, H, and W Names, as shown in Figure 11.12:

![](img/0c7a357df7e57e06b6d9f631c219040c_357_0.png)

Figure 11.12 – Deleted cells cause Names to return #REF!

7. Deleting the rows removed the cells, but not the Names. To resolve a #REF ! error related to a named cell, you can do the following:

- Use the Undo command to restore the deleted cells.
- Recreate the Names in a new location.
- Delete the Names by way of Name Manager:
  - Click on a name and then click Delete.
  - Click the Value heading to group the deleted Names, click on the first name that you wish to remove, hold down the Shift key while you click on the last name, and then click Delete.
  - Hold down Ctrl (⌘), select two or more Names, and then click Delete.

8. While we’re here, let’s finish up our tour of Name Manager: click the Name column if you have sorted on another column and wish to arrange the Names back into alphabetical order. You can sort Names based on any column heading.

9. Click the Filter button to filter Names based on their scope, error status, and type of name. Choose Clear from the top of this list at any point to redisplay all Names.

10. The Refers to field at the bottom of Name Manager enables you to edit the cell reference or data associated with a name without displaying the Edit Name dialog box. Click the checkmark button if you make any changes or click the X button to discard any edits that you have embarked on and then wish to discard.

11. Press Escape (or click Close) to dismiss the Name Manager dialog box.

## Names, LET, and LAMBDA

![](img/0c7a357df7e57e06b6d9f631c219040c_358_0.png)

Now, let’s take another look at using **Names** within formulas.

### Using Names within formulas

Naming cells can make formulas easier to write, as you don’t necessarily have to navigate to cells to use them in formulas: you can just type the **Names** instead. Let’s look at a few examples of utilizing **Names** in formulas:

1. Enter `=PRODUCT(Length:Height)` into cell F5 of the `Names and Formulas` worksheet. This illustrates that you can specify **Names** for cells that bookend a range that you wish to refer to. The formula will return `3,456` if you enter it correctly.

2. Select cells `C3:C5`, type `Dimensions` into the **Name Box** area, and then press `Enter`. Now, a total of four **Names** have been assigned to cells `C3:C5`: **Length**, **Width**, **Height**, and **Dimension**.

3. In cell F6, type `=PRODUCT(Dimensions)` and press `Enter`. This illustrates that you can refer to a named range of two or more cells within a formula. This formula should also return `3,456`.

4. In cell F7, type `=PRODUCT(` and then click on cell `C3`, type `:`; click on cell `C5`, and then type `)`) and press `Enter` – the resulting formula should be `=PRODUCT(Length:Height)`, which shows how Excel automatically incorporates **Names** into formulas from cells that you select. Choosing or typing Names in cells eliminates switching between worksheets. The formula should return `3,456`.

5. If you’re using Excel for Windows, select cell F8, and then choose **Formulas | Use in Formula** and select **Height**, type `*`, choose **Use in Formula** and select **Length**, type `*`, choose **Use in Formula** and select **Width**, and then press `Enter`. The formula should return `3,456`.

![](img/0c7a357df7e57e06b6d9f631c219040c_358_1.png)

As we discussed, you can create **Names** that are scoped to an entire workbook or a single worksheet. The `LET` function goes further by enabling us to create **Names** that are scoped to a single cell.

