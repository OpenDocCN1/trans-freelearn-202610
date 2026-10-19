```
selectedSheet.getRange("B2").setValue("This is a test");
// Set fill color to FFFF00 for range B2 on selectedSheet
selectedSheet.getRange("B2").getFormat().getFill().
setColor("FFFF00");
}
```

The pane on the right-hand side of the screen is laid out similarly to Script Lab – with a code editor, an area with the log messages from running code, and several other useful command buttons.

The code editor has a Save button. Files are saved in your OneDrive folder under Documents | Office Scripts. It's not an automatic save (unlike Script Lab), so make sure you save before running.

The ... menu in the right-hand panel gives the Add button option. Selecting this (unsurprisingly) adds a button to the current workbook that will run the script, as in Figure 7.14.

![](bbox=[0.295, 0.363, 0.72, 0.67])

Figure 7.14 – Sharing our script via a button in our workbook

More importantly, creating this button automatically shares our script with people who can view the workbook. This is probably the easiest way to make sure our workbook and script get distributed together. But there are more detailed sharing options available in the Share menu option of the ... menu.

Since much of the layout of Office Scripts is similar to Script Lab, we've kept the descriptions brief. A good starting point to learn more is the Office Scripts documentation at https://learn.microsoft.com/en-us/office/dev/scripts/.

Let's prove that the two scripting approaches are similar by modifying our previous Script Lab example to work in Office Scripts.