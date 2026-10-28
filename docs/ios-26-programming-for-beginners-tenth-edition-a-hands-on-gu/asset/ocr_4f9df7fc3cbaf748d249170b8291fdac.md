# Embed user interface elements in a stack view

The New Entry scene now has all the required user interface elements, but the position of the elements relative to the enclosing view is ambiguous. You'll embed all the elements in a vertical stack view and use constraints to resolve the positioning issues. Follow these steps:

+   1. In the document outline, select all the user interface elements that you added earlier, as shown here:

![](bbox=[0.34, 0.26, 0.72, 0.57])

Figure 13.35: Document outline showing selected elements

+   2. Choose Embed In | Stack View from the Editor menu.

+   3. Select Stack View in the document outline. In the Attributes inspector, under Stack View, set Alignment to Center and Spacing to 8.

+   4. With the stack view selected, click the Add New Constraints button. Enter the following values to set the constraints for the stack view:

+   - Top: 20
+   - Left: 20
+   - Right: 20