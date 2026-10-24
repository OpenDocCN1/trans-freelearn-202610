The =UNIQUE(C3:C12) formula in cell **G15** of *Figure 10.24* returns #SPILL! because I replaced **United States**, which the function returned in cell **G16**, with United States of America. If a user types over any result returned by a dynamic array function, the formula will return #SPILL! until any overwritten cells are cleared. In short, the #SPILL! and #CALC! errors are designed to give you immediate feedback when dynamic array functions cannot return any result due to the conditions within your spreadsheet.

Now, let’s see how to generate random numbers in Excel spreadsheets.

# ## The RANDARRAY function

Excel has three different functions that enable you to generate random numbers. Let’s look at the two longstanding options, and then we’ll look at RANDARRAY. The =RAND() formula in cell **B2** of *Figure 10.25* generates a random number between 0 and 1:

![](bbox=[0.1328, 0.3734, 0.8555, 0.4961])

The =RANDBETWEEN(100,500) formula in cell **D2** of *Figure 10.25* allows you to generate random integers between 100 and 500. RANDBETWEEN has two arguments:

+   - **Bottom** – The lowest possible integer you want to return, which, in this case, is 100
+   - **Top** – The highest possible integer you want to return, which, in this case, is 500

**Nuance**

All random number functions in Excel are volatile, meaning that, unlike most formulas, they recalculate every time you change any cell anywhere in the workbook. Normally, Excel only recalculates formulas when you change a value that the formula directly or indirectly references.