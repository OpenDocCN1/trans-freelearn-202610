3. Enter the =L*W*H formula in cell F8 if needed. This formula should also return 3 , 456.
4. Select rows 2:5 on the Names Introduction worksheet and choose Home | Delete (or right-click on the selection and choose Delete).
5. The formulas that were in F7:F8 are now in cells F4:F5 and return #REF! because the underlying cells being referenced were deleted.
6. Choose Formulas | Name Manager and notice how the Refers to column now returns #REF! for the L, H, and W Names, as shown in Figure 11.12:

![](bbox=[0.135, 0.271, 0.854, 0.382])

Figure 11.12 – Deleted cells cause Names to return #REF!

7. Deleting the rows removed the cells, but not the Names. To resolve a #REF ! error related to a named cell, you can do the following:
• Use the Undo command to restore the deleted cells.
• Recreate the Names in a new location.
• Delete the Names by way of Name Manager:
• Click on a name and then click Delete.
• Click the Value heading to group the deleted Names, click on the first name that you wish to remove, hold down the Shift key while you click on the last name, and then click Delete.
• Hold down Ctrl (⌘), select two or more Names, and then click Delete

8. While we’re here, let’s finish up our tour of Name Manager: click the Name column if you have sorted on another column and wish to arrange the Names back into alphabetical order. You can sort Names based on any column heading.
9. Click the Filter button to filter Names based on their scope, error status, and type of name. Choose Clear from the top of this list at any point to redisplay all Names.
10. The Refers to field at the bottom of Name Manager enables you to edit the cell reference or data associated with a name without displaying the Edit Name dialog box. Click the checkmark button if you make any changes or click the X button to discard any edits that you have embarked on and then wish to discard.
11. Press Escape (or click Close) to dismiss the Name Manager dialog box.