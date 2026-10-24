+   2. Choose Home | Highlight Cells Rules | Duplicate Values.
+   3. Accept the default choice of Duplicate from the Format cells that contain: dropdown or choose Unique.
+   4. Optionally assign formatting of your choice and then click OK.

> Tip
> I will discuss ways to create new lists that are free of duplicates in Chapter 10, Lookup and Dynamic Array Functions.

+   5. Optional: Right-click any of the duplicate cells and then choose Filter | Filter by Selected Cells Value.
+   6. Optional: Change the value of cell E5 to 13566a to see the color-coding will vanish from cells E5 and E9 because the cells will no longer be duplicates.
+   7. Optional: Data | Reapply or Home | Sort & Filter | Reapply to apply the filter again, which will hide rows 5 and 9.
+   8. Choose Data | Filter or Home | Sort & Filter | Reapply to turn the filter feature off and display all the rows again.

> Nuance
> The word Blue appears three times in cells H3:H17 of the Duplicate Values worksheet, but the Duplicate Values Conditional Formatting rule I applied only formatted the first two instances. This is because the third instance has the word Blue with a trailing space. Users often inadvertently bump the space bar when entering data, which can create inconsistencies. Excel’s TRIM function offers an easy way to eliminate extraneous spaces.

Now that we have reviewed all the Highlight Cells rules, let’s take a brief look at the top and bottom rules.

# # Top and bottom rules

The top and bottom rules allow you to identify values based on their relative positions within a list. Four out of the six reference 10 Items or 10%, but you can enter any number or percentage that you wish. To apply any of these rules, do the following:

+   1. Select a range of cells that you wish to format, such as cells H3:H22, of the Top and Bottom Rules worksheet in this chapter’s example workbook.
+   2. Choose Home | Conditional Formatting | Top/Bottom Rules and then select a rule:
+      • Top 10 Items: Formats the 10 largest values in the list unless you specify a different value