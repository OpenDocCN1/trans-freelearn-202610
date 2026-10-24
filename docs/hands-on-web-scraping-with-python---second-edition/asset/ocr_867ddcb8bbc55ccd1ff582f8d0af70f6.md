As shown in the following code, the with statement is provided with w+, encoding, and newline arguments:

```
import csv
def writeto_csv(data, filename, columns):
    with open(filename, 'w+', newline='', encoding="UTF-8")
    as file:
        writer = csv.DictWriter(file, fieldnames=columns)
        # Column header

        writer.writeheader()                # writes header
        writer = csv.writer(file)
        for element in data:
            writer.writerows([element])
            # writes each list element as row
```

There is a class called DictWriter in the csv library that has a few dedicated methods for writing CSV headers (writeheader()) and rows (writerows()). Throughout this book, we are using the writeto_csv() function, which receives the three arguments listed here:

+   - data: A list of elements that will be added as CSV rows
+   - filename: The filename that will be created
+   - columns: A list of the column names or headers for the CSV file

With some sample data provided in the following holidays list, a call to the defined writeto_csv() function has been made, which will create a file called holidays.csv with Date and Name columns, and rows from the holidays list:

```
holidays = [['2023-01-02', "New Year's Day"],
['2023-01-16', 'Martin Luther King, Jr. Day'], .......,
['2023-11-10', 'Veterans Day'], ['2023-11-23',
'Thanksgiving Day'], ['2023-12-25', 'Christmas Day']]

writeto_csv(holidays, 'holidays.csv', ['Date', 'Name'])
# function call
```

The raw holidays.csv file looks as shown in Figure 10.2: