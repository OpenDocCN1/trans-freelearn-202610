Excel will recalculate the formula and realize that it does not recognize the LAMBDA or LET functions. Regardless, choose File | Info | Check for Issues | Check Compatibility to display the dialog box shown Figure 9.2:

![](bbox=[0.2637, 0.1805, 0.7188, 0.4773])

Figure 9.2 – Compatibility Checker

4. The Compatibility Checker dialog box may report one or more scenarios, with some examples provided here:

+   - **Significant loss of functionality**: Click the Find link adjacent to each issue to navigate to the cell or cells that utilize worksheet functions or features that are not backwards compatible with one or more versions of Excel. Incompatible functions may initially return valid results when the spreadsheet is opened, but then such worksheet functions will return #NAME? when any change is made that triggers Excel to recalculate the worksheet.

> > Nuance
> > The Select versions to show drop-down list shown in Figure 9.2 allows you to determine which prior version of Excel you want to ensure compatibility with so that you can minimize false alarms. Keep in mind that this list only appears when one or more Significant loss of functionality issues appear. If you care about compatibility with, say, Excel 2019 or later, you can select that version from the list so that you won’t be alerted to incompatibilities in Excel 2016 and earlier. Enable Check compatibility when saving this workbook if you want Excel to automatically run Compatibility Checker each time you save your file, or you can manually run Compatibility Checker from time to time. This is not a global setting in Excel, meaning you’ll have to configure Compatibility Checker on a workbook-by-workbook basis.