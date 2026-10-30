# Custom formulas

Custom formulas enable you to create a derived value or column in a Data Prep recipe that displays values based on a calculation that can include input from fields in the same row or across rows—for example, you can create a **Profit** column based on input from **Revenue** and **Cost** columns. Formulas must be entered in **Enterprise Architect (EA)**-SQL format, a collection of standard and custom functions for numeric, string, and date data.

When you add a function in the formula window, the necessary parameters will be added for you. You can see an example of this here:

```
months_between(timestamp2, timestamp1, roundOff(optional))
```

# Date functions

As you may have guessed, you use date functions to calculate values from dates in your recipe. The date functions available are listed here:

+   - `add_months`—Returns the date with a specified number of months after the start date
+   - `current_date`—Returns the current date
+   - `current_timestamp`—Returns the current date and time
+   - `datediff`—Returns the number of days between the start date and the end date
+   - `date_add`—Returns the date with a specified number of days after the start date
+   - `date_format`—Converts the timestamp to a specified date format
+   - `date_sub`—Removes a specified number of days from the start date
+   - `day`—Returns the day component of the date/timestamp
+   - `dayofmonth`—Returns the day-of-the-month component of the date/timestamp
+   - `dayofweek`—Returns the day-of-the-week component of the date/timestamp, where 1 = Sunday, 2 = Monday, and so on
+   - `dayofyear`—Returns the day-of-the-year component of the date/timestamp
+   - `months_between`—Returns the number of months between two timestamps
+   - `now`—Returns the current date and time in a specified format
+   - `to_date`—Converts the date string to a specified date format