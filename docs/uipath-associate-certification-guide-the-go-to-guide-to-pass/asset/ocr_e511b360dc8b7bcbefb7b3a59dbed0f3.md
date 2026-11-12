The chapter consists of the following sections:

+   - Exploring data tables
+   - Creating and customizing data table variables
+   - Interacting with Excel files
+   - Practicing Excel automation

At the end of this chapter, you will understand the most common methods used to perform data manipulation in Excel using different Excel interaction methods.

# Technical requirements

Before starting with this chapter, please have UiPath Studio and the Assistant installed. This chapter requires a general idea of how to use Studio to create variables and simple workflows. Furthermore, you must have the Microsoft Office package installed on your machine.

# Exploring data tables

We have all used Microsoft Excel at some point in our life. In every Excel file, we see the following concepts:

+   - *Workbook*: This refers to the Excel file itself. From now on in our course, we will be using this word to refer to Excel files.
+   - *Worksheet*: Every workbook can have multiple worksheets that contain data.
+   - *Row*: Every worksheet consists of rows of data.
+   - *Column*: Every data row consists of multiple columns that contain different data values referring to the type of data each column holds.
+   - *Cell*: A cell in an Excel sheet contains a single value or a formula. Every row consists of multiple cells.

Similarly, a data table is a variable type that can store data in the same structural format as an Excelsheet.

You might wonder about the differences between a data table and a worksheet. A data table is a memory representation of a single database table that can hold multiple rows consisting of multiple columns of different data types. Alternatively, a worksheet has the same structure, but it can visually represent its data using tables and graphics.