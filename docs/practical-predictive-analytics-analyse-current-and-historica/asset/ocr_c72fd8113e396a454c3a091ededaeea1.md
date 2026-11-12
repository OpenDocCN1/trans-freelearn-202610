| | | | |
|---|---|---|---|
| 1 | 1 | 1 | 17 |
| 2 | 1 | 2 | 15 |
| 3 | 1 | 3 | 11 |
| 4 | 2 | 1 | 6 |
| 5 | 2 | 2 | 20 |
| 6 | 2 | 3 | 11 |
| 7 | 3 | 1 | 9 |
| 8 | 3 | 2 | 33 |
| 9 | 3 | 3 | 43 |
| 10 | 4 | 1 | 11 |
| 11 | 4 | 3 | 13 |
| 12 | 4 | 4 | 12 |

For switching the rows with the columns, we can use the spread function from the tidyr package:

```
install.packages("tidyr")
library(tidyr)
sales_horizontal <- spread(sales_vertical, Month, sales)
sales_horizontal
```

The last line prints the results of the transpose to the console. Take a look at the output of the console to verify that what used to be rows (memberids) are now columns. Each of the sales figures also appears as columns, with the column name designating the specific month. Additionally, for each month of data in which a sales figure did not appear for a particular month, NA will appear. For example, in the original data, member 12 was the only member who had sales figures for month 4. That will appear as 12, in the month 4 column, but since the other members had no sales for that month, they will appear as NA:

```
>sales_horizontal <- spread(sales_vertical, Month, sales)
>sales_horizontal
  memberid 1 2 3 4
1        1 17 15 11 NA
2        2 6 20 11 NA
3        3 9 33 43 NA
4        4 11 NA 13 12
```

# Dummy variable coding

A dummy variable is a binary flag (0 or 1) that designates the presence or