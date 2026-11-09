# Splitting strings

A lightweight version of the text to columns feature, available in Google Spreadsheets but not in Excel, is the split() function. This function just takes a string value and splits it into its component pieces. Be aware that you need to provide enough new columns for the newly split data to fit into. In the following example, we have used the same data as the previous example but created three new columns to hold the split values from D.

| fx | =split(D1,"-") |
| --- | --- | --- | --- | --- |
|  | A | B | C | D |
| 1 | 2014 | 9 | 19 | 2014-09-19 |
| 2 | 2014 | 9 | 19 | 2014-09-19 |
| 3 | 2014 | 9 | 19 | 2014-09-19 |

# Concatenating strings

The concatenate() function takes a number of strings, either as cell references or as quoted strings, and attaches them together inside a new cell. In the following example, we use the concatenate() function to join the date and time strings into one. This function is available in both Excel and Google Spreadsheets, as shown in the following figure:

| fx | =CONCATENATE(B1," ",C1) |
| --- | --- | --- |
|  | A | B | C |
| 1 | 2014-09-19 14:10:47 | 2014-09-19 | 14:10:47 |
| 2 | 2014-09-19 14:10:47 | 2014-09-19 | 14:10:47 |
| 3 | 2014-09-19 14:10:47 | 2014-09-19 | 14:10:47 |

# Conditional formatting to find unusual values

Both Excel and Google spreadsheets have conditional formatting features. Conditional formatting uses a set of rules to change the appearance (format) of a cell or cells depending on whether some criteria are met (condition). We can use this to find data that is too high, too low, missing, or otherwise strange. Once we have identified it, we can clean it.