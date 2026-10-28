![](bbox=[0.213, 0.082, 0.799, 0.335])

Figure 12.16: The results of Group By columns

We now have all product numbers with their latest reporting dates. The only remaining piece of the puzzle is to join the results of the Grouped Rows step with the data from the previous step. For that, we use the Merge Queries option, which runs the Table.NestedJoin() function in the background. Follow these steps:

+   24. Select the Grouped Rows step.
+   25. Click the Merge Queries button from the Home tab.
+   26. Select the Product (Current) table from the dropdown. Note that we are selecting the current query (Product).
+   27. On the top table, press the Ctrl button on your keyboard and select the ProductNumber and the Reporting Date columns sequentially.
+   28. Do the same for the bottom table. Note that the sequence of selecting the columns is important.
+   29. Ensure that the Join Kind is Left Outer (all from first, matching from second).
+   30. Click OK.