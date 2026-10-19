# Boolean data types

You’ve probably used Boolean data types in Excel – they are either True or False. Like in Excel, there is a third Boolean type of empty, where no value has been supplied.

Sometimes, the impact of an empty value shows up as a Required Dependency ... not found message, which disappears when we pass in a value. Keep this in mind as we run the sample.

Now that we are supplying additional information to our decision node, we want to update our decision to take advantage of it. To do this, follow these steps:

+   1. Select the greeting Decision node in our Decision Model.

+   2. Click on the Edit button that appears on the middle left as a context menu.

+   3. Using the boxed editor, update the expression to if AM then "Good Morning "+name else "Good Afternoon "+name.

+   4. The result should look like Figure 3.25 – note that the editor has captured the text, but won’t show all of it on screen unless we drag the right-hand side of the boxed editor to expand it:

![](bbox=[0.33, 0.41, 0.68, 0.54])

Figure 3.25 – Updating the decision expression

+   5. Click on the Problems tab at the bottom left of the screen to see whether KIE has highlighted any issues. A major source of errors is getting the characters for the quotes around the text wrong – it’s the same double-quote character you use in Excel.

No doubt you’re keen to try out these changes. Like a previous example, click on the Run … as Form menu at the top right of the screen and enter some values like in Figure 3.26:

![](bbox=[0.27, 0.7, 0.74, 0.81])

Figure 3.26 – Updated input/output form