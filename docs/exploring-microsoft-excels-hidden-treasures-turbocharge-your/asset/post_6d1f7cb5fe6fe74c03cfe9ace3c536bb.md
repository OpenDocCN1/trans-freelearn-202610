

## Calculated Columns

A **Calculated Column** within a **Table** consists entirely of formulas. As you’ll see, whenever you change the formula in one cell of the column, your change is applied automatically to all other cells in that column:

1. Activate the **Insert Table** worksheet. If the list is not already formatted as a **Table**, click on any cell in the list, choose **Insert** | **Table**, and then click on **OK**.

2. Type `Miles` into cell **F1**, and press **Enter** to add a new column.

3. In cell **F2**, type in `=D2/528` and then press **Enter** to create a **Calculated Column**—don’t worry, I know this should be `5280`, and we’ll correct it in the next step. Double-click on any cell in the **Miles** column, add a zero, and then press **Enter** to see how all formulas in the column update at once.

> **Tip**
> See the _Troubleshooting_ section at the end of this chapter if you encounter a situation where a **Calculated Column** does not update in this fashion.

4. Right-click on any cell in the **Miles** column, and then choose **Select** | **Entire Table Column** from the context menu.

5. Press `Ctrl + Shift + !` (`⌘ + Shift + !`) to apply the **Comma Style** number format or apply any number format of your choice.

6. Type `Kilometers` into cell **G1**, and press **Enter** to add a second column.

7. In cell **G2**, type in `=`, tap the left arrow to select cell **E2**, and then type in `/1000`.

8. Choose **Formulas | Show Formulas** to display the difference between the two columns. As shown in **Figure 7.14**, the formulas in the **Miles** column take the form of =D2/5280, while all cells in the **Kilometers** column contain the =[@Meters]/1000 formula, where =[@Meters] is a **structured reference** that refers to a **Header** instead of a cell reference:

![](img/0c7a357df7e57e06b6d9f631c219040c_226_0.png)

Figure 7.14 – Show Formulas mode

Here are a couple of things to keep in mind about structured references:

- The **Miles** column does not use structured references because we typed in the cell address as opposed to selecting the cell. Conversely, in the **Kilometers** column, we typed an = sign and then used the left arrow key or mouse to select cell G2. Excel creates structured references automatically when you click on or navigate to a cell within a **Table**.

- To disable automatic structured references, choose **File | Options | Formulas**, clear the **Use Table Names in formulas** option, and then click on **OK**.

- Structured references should return the same data as cell references, but sometimes they might return the value from the first **Body Row** of a **Table** in every cell. In such situations, I resolve the problem by replacing the **Header** with a cell reference instead.

- In this context, the @ symbol is known as the **This Row** operator and instructs the formula to refer to the cell on the same row as the formula, as opposed to the entire **Table** column.

9. Choose **Formulas | Show Formulas** to return them back to showing results instead of formulas.

**Nuance**
Microsoft’s documentation states that the keyboard shortcut for **Show Formulas** is `Ctrl + ‘` (grave accent) in Windows or Mac. The tilde (~) character resides on the same key, so users will often say press `Ctrl + ~` because many Excel users would be hard-pressed to identify the grave accent on a keyboard.

Now, let’s see how to create self-resizing formulas that automatically include new data that is added to a **Table**.

## Self-resizing formulas

The Formula References worksheet, as shown in Figure 7.15, contains a list in a normal range of cells in columns C:D and a Table in columns L:M:

![](img/0c7a357df7e57e06b6d9f631c219040c_227_0.png)

Figure 7.15 – Comparing formulas that refer to cell ranges versus Tables

Figure 7.15 also displays three types of formulas:

- Cell reference to a normal range of cells – Cells G4:G5 are typical formulas in Excel.

- Cell reference to a Table – Cells P4:B5 look like typical formulas in Excel but refer to a Table.

- Structured reference – Cells P8:P9 contain structured reference formulas. Since the formulas are not within a Table, the Table Name precedes the column name that the formula references. These formulas do not include the This Row operator (@) because we want to reference the entire column.

Let’s compare what happens when we increase the size of both datasets. There are three different types of formulas:

1. Select cells C10:C11 and then drag Fill Handle down to cell D13 to add two additional rows to the list.

> **Nuance**
> Cells D4:D11 contain a formula that calculates an amount. Instead of dragging with your mouse, as described in step 1, you might be able to type 9 into cell C12 and 1 into cell C13 and observe that Excel fills cells D12 and D13 automatically, or at least it should. Sometimes, a range of normal cells will exhibit behavior similar to a Calculated Column.

## Automating Tasks with the Table Feature

2. Notice that the formulas in cells G4:G5 do not reference the amounts in cells D12:D13, which means the formulas must be updated manually.

3. Type 9 into cell L13 and 10 into cell L14 and notice the following differences:

- The Amount column is a Calculated Column, so cells M12:M13 are filled automatically.

- The formulas in cells P4:P5 are updated automatically to include cells M12:M13, as shown in Figure 7.16:

![](img/0c7a357df7e57e06b6d9f631c219040c_228_0.png)

Now, let’s take structured references a step further by showing how to write formulas that reference data on another worksheet without switching worksheet tabs.

## Using structured references to write formulas

The Feature Comparison worksheet contains a Table Named Bridges that we’ll reference in our next example by writing a formula in segments without needing to switch between worksheets:

1. Type =COUNTIF(Bridges[ into cell V4 of the Formula References worksheet.

2. Tap the down arrow three times to choose Type of Bridge, as shown in Figure 7.17, and then press the Tab key to add the field name to your formula. Then, type in ] to close the field name:

![](img/0c7a357df7e57e06b6d9f631c219040c_229_0.png)

Figure 7.17 – Referencing a Table on the Feature Comparison worksheet without switching sheets

3. Type `,U4` ) to finish the formula, and then press Enter to create a Calculated Column.

The Cell Reference Formula column in Figure 7.17 shows what the formula looks like when cell references are used instead of structured references.

The COUNTIF function has two arguments:

- Range – A range of cells or a Table reference that you wish to search, in this case, Bridges [Type of Bridge] or `Feature Comparison'!$E$4:$E$15`.

- Criteria – This argument is the data that you want to search the range field for, in this case, U4.

Here, again, the `=COUNTIF (Bridges [Type of Bridge] )` formula does not include the This Row operator (@) because we are referencing the entire column. Figure 7.18 shows an error prompt that you might encounter when you add an extraneous This Row operator (@) to a formula, such as one referencing an entire column. Although the error prompt makes no reference to it, the true error in this is the extraneous @ symbol, which, if removed, results in a formula that Excel will accept:

![](img/0c7a357df7e57e06b6d9f631c219040c_229_1.png)

Figure 7.18 – An extraneous @ symbol creates a formula that Excel rejects

