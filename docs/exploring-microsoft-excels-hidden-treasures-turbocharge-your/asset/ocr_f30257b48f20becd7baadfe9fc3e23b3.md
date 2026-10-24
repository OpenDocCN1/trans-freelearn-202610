# # The Always create backup setting

You can instruct Excel for Windows to create an automatic fallback position for critical workbooks.
There are a few caveats:

+   - You must enable this setting on a workbook-by-workbook basis.
+   - Your backup copy is the most previously saved copy of your workbook.
+   - Excel creates a file that has a .XLK extension that it will not recognize when you open your
+     backup file, which we’ll discuss.
+   - The backup file must reside in the same folder as the original document, as there is no provision
+     for saving backup files elsewhere.
+   - This feature is not available in Excel for macOS.

Let’s see how to establish an automatic backup for key workbooks:

+   1. Choose File | Save As | Browse or More Options.
+   2. Click on Tools in the Save As dialog box, to the left-hand side of the Save button.
+   3. Click on General Options.
+   4. Click on the Always Create Backup checkbox, as shown in Figure 2.12, and then click on OK.
+   5. Click on Save:

![](bbox=[0.294, 0.648, 0.692, 0.846])

Figure 2.12 – The Always create backup setting