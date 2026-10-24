+   4. Select cells C22:H27.
+   5. Choose Data | What If Analysis | Data Table.
+   6. Specify cell D17 as the Row Input Cell. There’s some nuance here as we want to pick the cell that has the combined inputs, not either of the cells where we separate the inputs.
+   7. Specify cell D16 as the Column Input Cell.
+   8. Click on OK to create the Data Table.

You could calculate these amounts without using a Data Table if you broke out the term and loan amounts into two separate rows. Alternatively, you could incorporate LEFT and MID in the PMT function, such as = –PMT ($C23 / 12, LEFT (D$22, 2) * 12, MID (D$22, 4, 10) ). In this case, the Data Table both streamlines the calculation and ensures proper data integrity in each cell.

Depending on the size and complexity of your calculations, Data Tables can, sometimes, cause your worksheets to take longer than usual to recalculate. Fortunately, there’s an easy solution if this situation arises.

# Improving calculation performance

The array formula within a can affect the calculation performance of your spreadsheet. Normally, Excel only recalculates formulas that are directly affected by a data entry change. On the other hand, Array formulas are recalculated every time you make a change anywhere in a workbook. If you find that this adversely slows down your spreadsheet, you can prevent Data Tables from constantly recalculating by choosing Formulas | Calculation Options | Automatic except for data tables.

You will now need to manually recalculate the Data Tables, as and when needed, by clicking on any cell that contains an array formula and then pressing F9 (Fn+ F9), or you can click on the Calculate Now command in the Formulas tab. Choose Formulas | Calculation Options | Automatic to restore the default calculation method.

Now that you have a sense of the Data Table feature, let’s see how you can use the Forecast Sheet feature to project time-based or date-based data into the future.