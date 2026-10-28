XlsxWriter is an actively maintained package by the developer community that provides support for interacting with XLSX files. The package provides a lot of useful functionalities and supports the creation and management of workbooks as well as worksheets in individual workbooks. You can install it by running the following command in a Terminal or Command Prompt:

```
pip install XlsxWriter
```

Once installed, you can import the xlsxwriter module as follows:

```
import xlsxwriter
```

So, let's look at how we can start creating XLSX files with the support of the XlsxWriter package.

# Creating a workbook

To start working on XLSX files, we first need to create them. An XLSX file is also known as a workbook and can be created by calling the Workbook class from the xlsxwriter package, as follows:

```
workbook = xlsxwriter.Workbook(filename)
```

The call to the Workbook class opens a binary file, specified with the filename argument, and returns an instance of workbook that can be used to further create worksheets and write data.

# Creating a worksheet

Before we can start writing data to an XLSX file, we first need to create a worksheet. This can be done easily by calling the add_worksheet() method of the workbook object we obtained in the previous section:

```
worksheet = workbook.add_worksheet()
```

The add_worksheet() method creates a new worksheet, adds it to the workbook, and returns an object mapping the worksheet to a Python object, through which we can write data to the worksheet.

# Writing data to the worksheet

Once a reference to the worksheet is available, we can start writing data to it by calling the write method of the worksheet object, as shown here:

```
worksheet.write(row_num, col_num, col_value)
```

As you can see, the write() method takes three parameters: a row number (row_num), a column number (col_num), and the data that belongs to the [row_num, col_num] pair, as represented by col_value. This call can be repeated to insert multiple data items into the worksheet.