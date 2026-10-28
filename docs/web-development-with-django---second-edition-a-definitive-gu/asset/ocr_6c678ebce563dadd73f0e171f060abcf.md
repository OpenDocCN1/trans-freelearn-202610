# Technical requirements

The complete code files for this chapter can be found at https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter13

# Working with CSV files inside Python

There are several reasons we may need to export the data in our application. One of the reasons may involve analyzing that data – for example, we may need to understand the demographics of users registered on the application or extract patterns of application usage. We may also need to find out how our application is working for users to design future improvements. Such use cases require data to be in a format that can be easily consumed and analyzed. Here, the CSV file format comes to the rescue.

CSV is a handy file format that can be used to quickly export data from an application in a row-and-column format. CSV files usually have data separated by simple delimiters, which are used to differentiate one column from another, and newlines, which are used to indicate the start of a new record (or row) inside the table.

Python has great support for working with CSV files in its standard library, thanks to the csv module. This support enables the reading, parsing, and writing of CSV files. Let’s take a look at how we can leverage the csv module provided by Python to work on CSV files and read and write data from them.

# Working with Python’s csv module

The csv module from Python allows us to interact with files that are in CSV format, which is nothing but a text file format – that is, the data stored inside the CSV files is human-readable.

The csv module requires that the file is opened before the methods supplied by the csv module can be applied. Let’s take a look at how we can start with the very basic operation of reading data from CSV files.

# Reading data from a CSV file

Reading data from CSV files is quite easy and consists of the following steps:

First, we must open the file by running the following script:

```
csv_file = open('path to csv file')
```

Here, we are reading the file using the Python open() method, which requires the fully qualified name of the file. This should be opened so that it can be passed as the parameter to the method.

Then, we must read the data from the file object using the csv module’s reader method:

```
import csv
csv_data = csv.reader(csv_file)
```