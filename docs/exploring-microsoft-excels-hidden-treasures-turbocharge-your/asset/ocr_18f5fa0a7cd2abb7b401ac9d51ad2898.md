![](bbox=[0.216, 0.108, 0.767, 0.573])

Figure 6.24 – The Solver Parameters dialog box

Let’s say that you wanted to solve for a payment amount in the same fashion that we did for **Goal
Seek**. You can use the same example that we used for **Goal Seek** if you change the interest rate back
to 5.25% or any other static amount. Here are the steps for using **Solver** to determine the amount
that you can borrow based on a particular interest rate, term, or payment:

+   1. Activate the **Solver** worksheet of this chapter’s example workbook.
+   2. Choose **Data | Solver**.
+   3. Enter C4 in the **Set Objective** field of the **Solver Parameters** dialog box, as shown in **Figure 6.21**.
+   4. Choose **Value Of**.
+   5. Enter a payment amount in the field adjacent to **Value Of**, such as 350.
+   6. Set the **By Changing Variable Cells** field to C5.
+   7. Click on the **Solve** button.