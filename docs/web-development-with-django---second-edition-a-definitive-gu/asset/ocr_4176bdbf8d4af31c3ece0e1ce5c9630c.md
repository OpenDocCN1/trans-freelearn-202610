In the preceding code snippets, we created a dataset that we want to write to the XLSX file in the form of a list of lists. Once that was done, we obtained a new workbook object, which will be used to create an XLSX file. Inside this workbook object, we created a worksheet to organize our data in a row-and-column format, wrote the data to the worksheet, and closed the workbook to persist the data to the disk.

5. Now, let’s see whether the code you wrote works the way it is expected to work. For this, run the following command:

```
python3 xlsx_demo.py
```

Once the command has finished executing, you will see that a new file called sample_workbook.xlsx has been in the directory where the command was executed. To verify whether it contains the correct results, open this file with either Microsoft Excel or Google Sheets and view its contents. It should resemble what you can see here:

![](bbox=[0.13, 0.38, 0.88, 0.6])

Figure 13.6: Excel sheet generated using xlsxwriter

With the help of the xlsxwriter module, you can also apply formulas to your columns. For example, if you wanted to add another row that shows the average age of the people in the spreadsheet, you can do that simply by modifying the write_data() method, as shown here:

```
def write_data(worksheet, data):
    """Write data to the worksheet."""
    for row in range(len(data)):
        for col in range(len(data[row])):
            worksheet.write(row, col, data[row][col])
    worksheet.write(len(data), 0, "Avg. Age").
    # len(data) will give the next index to write to
    avg_formula = "=AVERAGE(B{}:B{})".format(
        1, len(data))
    worksheet.write(len(data), 1, avg_formula)
```