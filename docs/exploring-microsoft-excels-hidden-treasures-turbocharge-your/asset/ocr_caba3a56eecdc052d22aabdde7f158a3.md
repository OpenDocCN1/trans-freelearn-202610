![](bbox=[0.129, 0.107, 0.854, 0.412])

# Figure 9.3 – Compatibility Mode

Compatibility Mode protectively disables any modern features that you cannot save to the .XLS file format. If you’re not aware of the nuance, then you may be baffled as to why certain features are disabled. To be clear, not every disabled feature in Excel’s Ribbon is attributable to Compatibility Mode. Although not shown in Figure 9.3, the Redo command is often disabled if the Undo stack has been cleared or you haven’t yet started editing a workbook. Conversely, Compatibility Mode purposefully disables the Analyze Data feature on the Home tab because the PivotTables and PivotCharts that you create with this feature cannot be saved to Excel 97-2003 workbooks.

> > Nuance
> > Compatibility Checker runs automatically when you save .XLS workbooks unless you clear the Check compatibility when saving this workbook checkbox in a given workbook.

The best way to eliminate Compatibility Mode confusion is to convert .XLS workbooks to the modern file format, but if you’re not careful, you might solve one problem and create a new one.

# Save As versus Convert command

Not that you would, but you could resave .XLS workbooks in the modern .XLSX format in this fashion:

+   1. Open the Chapter 9 – Compatibility Mode – Save As.xls workbook.
+   2. Choose File | Save As | Browse.