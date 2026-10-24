# Nuance

You cannot delete a row or column that intersects with an array formula. If you try to delete row 21, Excel will return the Can’t change part of a data table error. Although true, there is an easy workaround. Erase cells D19:D23, delete row 21, and then repeat steps 1 through 3 above for cells C18:D22.

Now, let’s create a Data Table that has two inputs.

# Creating a Data Table with two inputs

The Data Table shown in Figure 6.17 calculates loan payments based on five interest rates and five loan terms. I’m practically using the same inputs in cells D2:D4 of the Data Table – Two Inputs worksheet as I did for the one-input example, but this time, the = – PMT ( D2 / 12 , D3 * 12 , D4 ) formula is in cell C7:

![](bbox=[0.1328, 0.3875, 0.8555, 0.5969])

Figure 6.17 – A Data Table with two inputs

Cells D7:H7 contain five loan lengths, while cells C8:C12 contain five interest rates. Cells D8:H12 contain the = TABLE ( D3 , D2 ) formula. The = – PMT ( $C33 / 12 , D$32 * 12 , $D$30 ) formula in cell D33 is a formula-based alternative. Notice, this time, additional $ signs are required to maintain the proper integrity of the arguments. The Data Table feature allows you to put a simpler formula in cell C7 and know that the results will automatically use the correct inputs.

You can follow along in the work area of the Data Table – Two Inputs worksheet if you wish:

+   1. Select cells C20:H25.

+   2. Choose Data | What If Analysis | Data Table.

+   3. Enter D16 in the Row input cell field.

+   4. Enter D15 in the Column input cell field. Click on OK.