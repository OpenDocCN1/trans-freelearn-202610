The output of this is shown in the following screenshot:

![](bbox=[0.105, 0.09, 0.845, 0.52])

In this example, you measure the difference between sorting data using if statements and switch statements:

+   1. You first start by leveraging the measure-command cmdlet to measure the duration of sorting using if statements.
+   2. You continue to use the foreach loop to evaluate each $extension in the $extensions array.
+   3. You declare the statement of if the $extension is -like "*.xml". If this evaluates to true, it will add one value to the $xmls variable leveraging the += assignment operator.
+   4. You then evaluate the statement of if the $extension is -like "*.exe". If this evaluates to true, it will add one value to the $exes variable leveraging the += assignment operator.