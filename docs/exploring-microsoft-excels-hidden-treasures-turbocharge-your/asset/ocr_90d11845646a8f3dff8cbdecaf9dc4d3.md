# # LAMBDA conflicts and errors

The LAMBDA function opens an endless array of possibilities in Excel, but only for users with Microsoft 365. If someone opens a workbook that uses one or more LAMBDA functions in Excel 2021 or earlier, the formulas will return #NAME? because other Excel versions cannot recognize your LAMBDA as a valid function.

> > **Tip**
> > LAMBDA does work in Excel for the web (http://www.office.com), which only requires a free Microsoft account for access. At the time of writing, the Define Name and Name Manager commands are not offered in Excel for the web, so you cannot use that platform to create LAMBDA functions. However, you can use existing functions.

Let’s formalize the LAMBDA function in cell F11 into a worksheet function named PRICE to see if a conflict arises with the built-in PRICE function in Excel, which is used to calculate prices for securities:

+   1. Double-click on cell F11 and press Ctrl + A (⌘ + A) to select the entire formula. Then, press Ctrl + C (⌘ + C) (or right-click on the selection and choose Copy).

+   2. Choose Insert | Define Name (or Formulas | Name Manager | New).

+   3. Enter PRICE in the Name field, as shown in Figure 11.23:

![](bbox=[0.183, 0.508, 0.835, 0.785])

Figure 11.23 – Creating a LAMBDA function named PRICE

+   4. Important: Make sure that the Scope field is set to Workbook.

+   5. Enter Creates extended price calculation in the Comment field.

+   6. Erase the contents of the Refers to field and paste in the formula that you copied in step 1.