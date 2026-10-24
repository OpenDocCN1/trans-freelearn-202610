

## Excel Quirks and Nuances

2. Position your mouse over the middle of cell **B4** and notice how your cursor presents as a white cross, and then click once to select cell **B4**.

3. Position your mouse over the bottom border of cell **B4** and notice how your cursor changes to a four-pointed arrow, as shown in Figure 9.6. Now, here’s the nuance: double-click on the bottom border of cell **B4** and notice how your cursor moves down to cell **B13**, which is the last filled cell in this area of the worksheet.

This is a hidden double-click trick in Excel that can make you feel like Excel is running amok if you’re not aware of the technique. You can also use keyboard shortcuts to navigate in a similar fashion, as outlined here:

- Ctrl + down arrow key moves the cursor down to the next empty or filled cell—depending upon where you’re starting from—or the bottom of the worksheet. This means row 1048576 if you carry this out in a cell outside of the area of the worksheet where your data resides. The mouse equivalent is to double-click the bottom edge of a cell.

- Ctrl + up arrow key moves the cursor up the column to the next empty or filled cell, again depending upon where you’re starting from. The mouse equivalent is to double-click the top edge of a cell.

- Ctrl + right arrow key moves your cursor across the current row to the next empty or filled cell, again depending upon the type of cell that you start from. If you’re working to the right of your data, pressing Ctrl + right arrow key will take you to column **XFD**, which is the last column to the right. The mouse equivalent is to double-click the right-hand border of a cell.

- Ctrl + left arrow key moves your cursor left across the current row to the next empty or filled cell. The mouse equivalent is to double-click on the left-hand border of a cell.

> **Nuance**
> Ctrl + arrow keyboard shortcuts work everywhere in Excel, meaning that if your cursor is in cell **B13** and you press Ctrl + down, you’ll end up at cell **B1048576**, meaning the very last row of the spreadsheet. Conversely, the mouse double-click trick only works when navigating within or to filled areas of the spreadsheet. If you click on cell **B16** and then double-click on the bottom border, nothing will happen. However, if you double-click on the top border of cell **B16**, your cursor will move to cell **B14**, which is the empty cell adjacent to your data. This means you can double-click within or toward your data, but you can’t inadvertently double-click your way to a far edge of the worksheet the way that Ctrl + arrow shortcuts can.

There isn’t a mouse equivalent for pressing the Home key (Fn + left arrow in Excel for Mac) to move your cursor to column **A** of the current row that you’re on or pressing Ctrl + Home (Fn + ⌃ + left arrow in Excel for Mac).

Let’s now explore an even trickier nuance in Excel, which is the difference between **Enter** and **Edit** modes.

## Enter Mode Versus Edit Mode

Often, users add data to a chart and then must manually resize the chart to include the new data. I show how to eliminate this repetitive task in _Chapter 7, Automating Tasks with the Table Feature_, but I want to compare two manual approaches here. Let’s jump in:

1. Activate the **Enter Mode Versus Edit Mode** worksheet in the example workbook for this chapter.

2. Notice that the chart does not reflect the **March** values. The first way to resolve this is to do the following:

   I. Click once on the chart.

   II. Drag the **Selection Handle** shown in cell D5 in _Figure 9.7_ across to cell E5:

   ![](img/0c7a357df7e57e06b6d9f631c219040c_291_0.png)

   Figure 9.7 – Using the Selection Handle to resize charts

   III. At this point, your chart should reflect the month of **March**.

> **Nuance**
> If you don’t see the **Selection Handle** in _Figure 9.7_, choose **File | Options | Advanced**, and then turn on the **Enable fill handle and cell drag-and-drop** setting.

3. Now, let’s add **April** and resize the chart using a second approach so that I can show you the difference between **Enter** mode and **Edit** mode. To begin, enter these values:

   - Cell F3: `April`
   - Cell E4: 700
   - Cell E5: 800

4. Once again, **April** does not appear on the chart, so click once on the chart and then this time choose **Chart Design | Select Data**.

5. Here’s where the nuances come in. Fields such as **Chart data range** (shown in _Figure 9.8_) are **RefEdit** fields, short for **Reference Edit**, that enable you to select or specify a range of cells or sometimes enter data. **RefEdit** fields have three different modes, as outlined here:

   - **Point**: As shown in _Figure 9.8_, the word **Point** initially appears in Excel’s **Status Bar** when you open a dialog box that contains a **RefEdit** field, and a marquee border appears around the cells being referenced:

     ![](img/0c7a357df7e57e06b6d9f631c219040c_292_0.png)

     Figure 9.8 – Point mode

   - **Enter**: When you click on a **RefEdit** field in **Excel for Windows**, the word **Enter** appears in Excel’s **Status Bar**, in place of **Point**, as shown in _Figure 9.9_:

     ![](img/0c7a357df7e57e06b6d9f631c219040c_292_1.png)

     Figure 9.9 – Enter mode

Depending upon which action you choose next, tricky things may happen, as detailed here:

- You can resize the range of cells by holding down the _Shift_ key and using the arrow keys on your keyboard, which switches the field to **Point** mode. This is actually your only viable keyboard-based action in **Enter** mode unless you first clear the contents of the **RefEdit** field. If you don’t get the steps right though, you’ll end up inadvertently carrying out the technique presented next.

- If you click on the worksheet with your mouse, you will append one or more cells to the **RefEdit** field contents, which typically results in an invalid cell reference such as =’Enter Mode Versus Edit Mode’!$C$4:$E$5+’Enter Mode Versus Edit Mode’!$F$4:$F$5.

- Similarly, pressing the left or right arrow keys on your keyboard will also append an additional cell reference in the same fashion as using your mouse. However, you can select the field with your mouse and then press _Delete_ to clear the field, and then reselect the cells with your mouse.

> **Nuance**
> Excel for Mac does not use **Enter** mode, so you can only toggle between **Point** and **Edit** modes on that platform, which means that **Point** mode on the Mac functions the same as **Enter** mode in Windows.

- **Edit**: As shown in the following screenshot, **Edit** appears in the status bar when you press _F2_ after clicking into a **RefEdit** field, enabling the use of your keyboard to navigate within the field or for editing the field contents:

  ![](img/0c7a357df7e57e06b6d9f631c219040c_293_0.png)

  Figure 9.10 – Edit mode

You can use these keyboard shortcuts in **Edit** mode in **Excel for Windows**:

- Press left arrow or right arrow to navigate one character at a time in the field
- Press Ctrl + left arrow or Ctrl + right arrow to navigate one word or cell reference at a time
- Press Home or End to jump to the beginning or end of a field
- Press Shift + Home or Shift + End to select from the location of the cursor in the field to either end of the field

> **Nuance**
> Sometimes, Excel's **Formula Bar** drops into **Enter** mode, which you can toggle back to **Edit** mode by pressing F2 (Fn + F2). You can also press F2 to edit the contents of a worksheet cell or to edit a column heading in **Power Query**.

6. Use **Edit** mode to navigate to the column E reference in `= 'Enter Mode Versus Edit Mode'!$B$3:$E$5` and change it to column F.

> **Nuance**
> I'm going to drop an unrelated chart tip here: the **Hidden and Empty Cells** button shown in Figure 9.10 allows you to instruct Excel whether to treat empty cells as gaps or as zeros, to treat #N/A as an empty cell, and to show data in hidden rows and columns or not.

Now that you know the difference between **Enter** and **Edit** modes, you'll hopefully be less inclined to want to throw your keyboard or mouse out the window when you encounter a **RefEdit** field. Let's switch gears slightly by looking at another chart-related situation that might also put your mouse in peril.

## Excluding weekend dates from charts

Sometimes, Excel becomes overly helpful, leaving us having to figure out how to undo the unwanted help. For instance, as shown in Figure 9.11, the **Excluding Chart Weekend Dates** worksheet in the example workbook includes data for two Monday to Friday periods, and yet Excel insists on creating a gap for the weekend:

