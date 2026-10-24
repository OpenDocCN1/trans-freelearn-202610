# # Introducing Data Validation 127

Use the following steps to recreate the rule in cell C8:

+   1. Select cell D8 and then choose Data | Data Validation.
+   2. Select Whole Number from the Allow list and Greater than from the Data list.
+   3. Enter 0 (zero) in the Minimum field.
+   4. Activate the Input Message tab, type Numeric Value in the Title field, and then type Enter a number greater than zero. in the Input message field.
+   5. Activate the Error Alert tab, type Invalid Input in the Title field, and then type You must enter a number greater than zero. in the Error message field.
+   6. Accept the default style of Stop and then click OK to close the Data Validation dialog box.
+   7. Enter 1 .5 into cell D8 and then press Enter. Notice how the prompt shown in Figure 5.8 can cause one problem and create a new one. The value 1 .5 is a number greater than zero, but our rule requires a whole number. Click Cancel or press Esc to close the error prompt.
+   8. Click on cell D8 and then choose Data | Data Validation.
+   9. Activate the Input Message tab and update the Title and Input message fields to read Whole Number and Enter a whole number greater than zero., respectively.
+   10. Activate the Error Alert tab and update the Error alert field to read You must enter a WHOLE number greater than zero.

> > Tip
> > The only formatting option available in the Error alert field is to type certain words in upper case, so entering You must enter a WHOLE number greater than zero. is a way to emphasize the rule.

+   11. Click OK to close the Data Validation dialog box.
+   12. Enter 1 .5 into cell D8 and press Enter to see how the documentation communicates the rule more clearly.

Now let’s look at the more permissive Warning style.

# ## Warning style

Type 1 .5 in cell C9 to trigger the prompt shown in Figure 5.9, which gives the user the option to override the Data Validation rule by choosing between the following buttons:

+   • Yes: Allows the user to bypass the Data Validation rule
+   • No: Rejects the invalid input and enables the user to try again, in the same fashion as how the Retry button works when the Stop style is in use