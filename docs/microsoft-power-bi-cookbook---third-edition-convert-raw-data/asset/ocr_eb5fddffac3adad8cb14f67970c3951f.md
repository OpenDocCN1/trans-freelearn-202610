+   - Indicator columns, such as Weekday Indicator, Holiday Indicator, and Working Day Indicator. The values for these columns should be report-friendly, such as Holiday and Non-Holiday.

+   - Starting and ending date columns for the different grains supported, such as Week Ending Date and Period Ending Date.

+   - For each natural hierarchy column created, add a corresponding integer column for sorting purposes. Examples include a natural hierarchy string (2009-Jan), and an integer column, such as Calendar Year Month Number (200901).

Another aspect of design includes considering the use of offset and/or sequential number columns. Offset columns denote the number of date granularity increments from the current period. For example, for year-month granularity, if the current period (today) is 2024-01, then 2023-12 would be -1, 2023-11 would be -2, etc. Offset columns can greatly simplify date intelligence calculations. For example, the previous 6 months would be offsets -1 to -6 inclusive.

Sequential numbering columns can be used as an alternative or as a supplement for offset columns. Sequential numbering columns provide an integer value that starts at either 0 or 1 for the earliest date granularity and increments by one. Thus, if the earliest year-month in the date table is 2020-01 then that would be given a value of 1, and then 2020-02 would be 2, 2020-03 would be 3, etc.

Additional design advice includes the following:

+   - Look to integrate other calendars, such as a periods or fiscal calendar, into the same date table in the source database. Distinct views can be created to support role-playing dimensions. See the There’s more... section.

+   - Identify common date manipulations taking place in the existing reports, or by business users in Excel or Power BI, and consider adding a date dimension column to eliminate or simplify this work.

+   - Ensure that date columns (for example, Prior Year Date) and number columns (such as Calendar Year) are stored as date and integer data types, respectively, as this allows arithmetic and DAX functions, such as MAX and MIN, to operate without any type conversion.

+   - If the date dimension table is updated daily as part of a data warehouse Extract-Transform-Load (ETL) process, columns identifying the current and prior periods such as IsCurrentFiscalPeriod and IsPrior60Days are also common.

# How to prepare the date dimension

To implement this recipe, perform the following steps:

+   1. Create a new Power BI Desktop file.

+   2. Open the Power Query Editor by clicking Transform data on the ribbon of the Home tab.

+   3. Create a query that imports the Dates sheet from DateTable.xlsx, and call this query BaseDateTable.

+   4. Create a new blank M query and use the following code that references the BaseDateTable query, retrieves the last three years of dates, and computes 11 additional columns via M functions: