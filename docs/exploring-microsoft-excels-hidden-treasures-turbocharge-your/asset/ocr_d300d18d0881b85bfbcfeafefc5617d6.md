# # The Goal Seek feature

Excel's **Goal Seek** feature is a means of automating guesswork in Excel. Let's say that you want to buy
a car, and you know the interest rate, the term of the loan, and the payment that you're seeking, but
you don't know how much you can borrow. Here are the steps:

+   1. Activate the Goal Seek worksheet.
+   2. Choose Data | What-If Analysis | Goal Seek.
+   3. Enter or select cell C5 in the Set cell field of the Goal Seek dialog box, as shown in Figure 6.12:

![](bbox=[0.336, 0.602, 0.672, 0.788])

Figure 6.12 – The Goal Seek dialog box

+   4. Enter an amount, such as 350, into the To value field.
+   5. Enter or select cell C5 in the By Changing Cell field.

Tip
The Merge Scenarios dialog box does not allow you to choose which scenarios get copied to
your worksheet. It functions on an all-or-nothing basis, meaning you must copy all the scenarios
from a worksheet. However, you can use the Delete button within the Scenario Manager dialog
box to remove any extraneous scenarios.

As you can see, Scenario Manager is a powerful feature that can help you rein in your Excel workbooks
by being able to swap sets of inputs through a single worksheet, rather than recreating calculations
over and over. You do have to work within the 32-input constraint both for inputs and results. One
workaround is that you can create related sets of scenarios that you can apply sequentially, such as
Scenario 1 (Set 1 of 2) and Scenario 1 (Set 2 of 2). You won't be able to create a Scenario Summary
report that includes both sets of inputs, but you can apply the scenarios
sequentially and then use this naming convention to create as many scenarios as you wish.
Now, let's look at the next command in the What-If Analysis menu, the Goal Seek feature.