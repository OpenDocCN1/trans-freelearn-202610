+   3. Select Analysis | Create Calculated Field to create the following calculated fields. Note that each must be created separately; that is, it isn't possible in this context to create a single calculated field that contains all three calculations:
+      o. Name the first calculation Lev–Row and enter the following code: [Number of Records]/[Quantity].
+      o. Next, name the second calculation Lev–Agg and enter the following code: SUM ([Number of Records])/SUM (Quantity).
+      o. Lastly, name the third calculation Lev–Tab and enter WINDOW_AVG ([Lev – Agg]).
+   4. In the Data pane, right-click on the three calculated fields you just created and select Default Properties | Number format.
+   5. In the resulting dialog box, select Percentage and click OK.
+   6. Place Order Date on the Columns shelf.
+   7. Place Measure Names on the Rows shelf and Measure Values on Text on the Marks card.
+   8. Exclude all values except for Lev–Row, Lev–Agg, and Lev–Tab by dragging them off the Measure Values shelf or removing them from the Measure Names filter:

![](bbox=[0.22, 0.42, 0.79, 0.8])

Figure 1.44: Level of calculations