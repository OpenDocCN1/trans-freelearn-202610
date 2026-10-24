+   5. Enter or select K6 in the Cell link field.
+   6. Optional: Adjust the Drop down lines field if needed.
+   7. Click on OK.
+   8. Click on any cell other than D6 to deselect the Combo Box, and then notice how a position number appears in cell K6 when you make a choice from the Combo Box.
+   9. Enter =@INDEX (H5 : H11 , K6 ) in cell L6 to return the name of the day of the week that you chose.

> Tip
> It’s typically best to store cell links on a separate worksheet from the Form Controls so that you don’t inadvertently compromise the cells when making edits to your form. If the cell links reside on a worksheet that you will be protecting, make sure to unprotect the cell link cells; otherwise, you will encounter an error when you attempt to use your form control because Excel cannot update locked cells. See the Protecting Data Validation cells section if you’re unclear as to how to unlock cells.

As you can see, Combo Box controls are like Data Validation lists but cannot be overridden by pasting data into them. Let’s now see how to add Checkboxes and Option Buttons to a worksheet.

# # Checkboxes and Option Buttons

Checkboxes and Option Buttons enable you to allow the user to make choices within a spreadsheet. Users can tick as many Checkboxes as they would like, whereas Option Buttons (sometimes referred to as radio buttons) allow a single choice within a group. Let’s compare the differences between Checkboxes and Option Buttons.

# # Creating Checkboxes

Cell D7 of the Form Controls worksheet contains a Checkbox that is linked to cell K7. When you click the Checkbox, Excel puts the word TRUE in cell K7. If you click the Checkbox again, Excel puts the word FALSE. If you wish to reset the Checkbox, simply erase cell K7.

Now let’s see how to add a Checkbox to cell D8 as follows:

+   1. Choose Developer | Insert and then click the Checkbox command, which is the third command on the first row under Form Controls, as shown in Figure 5.31.
+   2. Click in cell D8 to create the Checkbox.
+   3. If needed, right-click on the Checkbox to display the handles around the outside, and then use the arrow keys to nudge the Checkbox into the desired position.