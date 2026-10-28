![](bbox=[0.178, 0.083, 0.822, 0.219])

Figure 5.14: Split column creates two new columns called OrderDateTime.1 and OrderDateTime.2

The new columns’ names are not user-friendly, so we should rename them.

Note

# GOOD PRACTICE

Avoid creating excessive transformation steps when possible. More transformation steps translate into more data processing time and extra load on the Power Query engine.

With the preceding note in mind, we do not rename the two new columns as a new step. Instead, we change the Power Query expression of the Split Column by Delimiter step. There are two ways to do so.

The first option is to change the expressions from Advanced Editor, as shown in the following image:

+   1. Click Advanced Editor from the Home tab on the ribbon bar.
+   2. Find the #"Split Column by Delimiter" step.
+   3. Change the column names.
+   4. Click Done: