
next section, we look at **time intelligence** in data modeling.

## Time intelligence and data modeling

**Time intelligence** is one of the most powerful and commonly used functionalities in Power BI. For those coming from a SQL development background, it is pretty clear how hard it is to build time intelligence analysis in a relational database system such as SQL Server. These complex calculations are easily accessible in Power BI using just a handful of time intelligence functions. This section briefly looks at the common challenges of working with time intelligence functions in Power BI.

There are currently 35 time intelligence functions available in Power BI.
You can find the complete list of functions on the Microsoft website:
https://docs.microsoft.com/en-us/dax/time-intelligence-functions-dax?WT.mc_id=DP-MVP-5003466.

## Detecting valid dates in the date dimension

When dealing with periodic calculations in time intelligence, it is often hard to just get valid dates to show in the visuals.

Let’s have a look at how to do this with a scenario.

A business needs to see the following calculations:
- Internet Sales Month to Date (MTD)
- Internet Sales Last Month to Date (LMTD)
- Internet Sales Year to Date (YTD)
- Internet Sales Last Year to Date (LYTD)

## Internet Sales Last Year Month to Date (LY MTD)

Writing the preceding measures is super easy using the existing DAX functions. The calculations are as follows.

To calculate MTD, use the following DAX expressions:

```
Internet Sales MTD =
TOTALMTD(
    [Internet Sales]
    , 'Date'[Full Date]
)
```

Use the following DAX expressions to calculate LMTD:

```
Internet Sales LMTD =
TOTALMTD(
    [Internet Sales]
    , DATEADD('Date'[Full Date], -1, MONTH)
)
```

Use the following DAX expressions to calculate YTD:

```
Internet Sales YTD =
TOTALYTD(
    [Internet Sales]
    , 'Date'[Full Date]
)
```

Use the following DAX expressions to calculate LYTD:

```
Internet Sales LYTD =
TOTALYTD (
    [Internet Sales]
    , DATEADD('Date'[Full Date], -1, YEAR)
)
```

Finally, use the following DAX expressions to calculate LY MTD:

```
Internet Sales LY MTD =
TOTALMTD(
    [Internet Sales]
    , SAMEPERIODLASTYEAR('Date'[Full Date])
)
```

Note

The SAMEPERIODLASTYEAR('Date'[Full Date]) and DATEADD('Date'[Full Date], -1, YEAR) act the same. We used different functions to demonstrate the possibilities.

We can then put a table visual on the report canvas to show the calculations side by side with **Full Date** from the **Date** table. Everything looks good unless we sort the **Full Date** column in descending order, which is when we realize that there is an issue. We get null values for the **Internet Sales**, **Internet Sales MTD**, and **Internet Sales LMTD** measures for many dates. We are also getting a lot of duplicate values for the **Internet Sales YTD** measure.

The following figure illustrates the results:

Figure 2.21: Future date-related issues in the periodic time intelligence calculations

This is indeed an expected behavior. We must cover all dates from 1st January to 31st December in the date dimension. So, we expect null values for future dates for the Internet Sales, Internet Sales MTD, and Internet Sales LMTD measures and duplicate values for the Internet Sales YTD measure. The following figure shows more results by scrolling down the table visual. We can see that the last date with a valid transaction is 28/01/2014, which means all the preceding measures must finish their calculations by that date:

![](img/5d2bce74fd6fceebed14fd1488bf7088_84_0.png)

Figure 2.22: The last valid date is 28/01/2014

One way of solving the issue is to return BLANK() for the invalid dates. The following calculation shows the Internet Sales MTD Blanking Invalid Dates measure, which returns null if there is no transaction for a particular date:

```
Internet Sales MTD Blanking Invalid Dates =
VAR lastorderDate = MAX('Internet Sales'[OrderDateKey])
RETURN
IF(
    MAX('Date'[DateKey]) <= lastorderDate
    , TOTALMTD(
        [Internet Sales]
        , 'Date'[Full Date]
    )
)
```

What we are doing in the preceding measure is simple. We get the maximum of **OrderDateKey** from the **Internet Sales** table, and then we add a condition that if an **OrderDateKey** value does not exist, we return a blank; otherwise, we return the `TOTALMTD()` calculation.

The following figure shows the results of the `Internet Sales MTD Blanking Invalid Dates` measure:

| Full Date | Internet Sales | Internet Sales MTD Blanking Invalid Dates |
|---|---|---|
| 28/01/2014 | 2,643.61 | 45,694.72 |
| 27/01/2014 | 1,477.61 | 43,051.11 |
| 26/01/2014 | 1,847.46 | 41,573.50 |
| 25/01/2014 | 1,747.67 | 39,726.04 |
| 24/01/2014 | 1,502.85 | 37,978.37 |
| 23/01/2014 | 1,817.99 | 36,475.52 |
| 22/01/2014 | 1,351.26 | 34,657.53 |
| 21/01/2014 | 1,937.95 | 33,306.27 |
| 20/01/2014 | 1,505.83 | 31,368.32 |
| 19/01/2014 | 1,823.92 | 29,862.49 |
| 18/01/2014 | 1,153.38 | 28,038.57 |
| Total | 29,358,677.22 | |

Figure 2.23: Internet Sales MTD Blanking Invalid Dates

While this calculation may work for some scenarios, it is not a correct calculation in some other scenarios, such as ours. Looking more precisely at the results reveals an issue. The following figure reveals the problem:

| Full Date | Internet Sales | Internet Sales MTD Blanking Invalid Dates |
|---|---|---|
| 7/02/2011 | 21,088.06 | 119,199.78 |
| 8/02/2011 | 17,891.35 | 137,091.13 |
| 9/02/2011 | 21,469.62 | 158,560.75 |
| 10/02/2011 | 11,929.73 | 170,490.48 |
| 11/02/2011 | 14,313.08 | 184,803.56 |
| 12/02/2011 | 21,113.06 | 205,916.62 |
| 14/02/2011 | 3,578.27 | 209,494.89 |
| 15/02/2011 | 21,266.34 | 230,761.23 |
| 16/02/2011 | 17,109.47 | 247,870.70 |
| 17/02/2011 | 21,266.34 | 269,137.04 |
| 18/02/2011 | 28,016.32 | 297,153.36 |
| Total | 29,358,677.22 | |

*Annotation: 13/02/2011 is missing*

```
CALCULATETABLE(
    VALUES('Date'[Full Date])
    , 'Date'[IsValidDate] = TRUE()
)
, -1
, MONTH
)
```

Use the following DAX expressions to calculate Internet Sales YTD with Valid Dates:

```
Internet Sales YTD with Valid Dates =
CALCULATE(
    TOTALYTD(
        [Internet Sales]
    , CALCULATETABLE(
            VALUES('Date'[Full Date])
        , 'Date'[IsValidDate] = TRUE()
        )
    )
)
```

Use the following DAX expressions to calculate Internet Sales LYTD with Valid Dates:

```
Internet Sales LYTD with Valid Dates =
TOTALYTD (
    [Internet Sales]
    , DATEADD(
        CALCULATETABLE(
            VALUES('Date'[Full Date])
        , 'Date'[IsValidDate] = true()
        )
    , -1
    , YEAR
    )
)
```

And finally, use the following DAX expressions to calculate Internet Sales LY MTD with Valid Dates:

```
Internet Sales LY MTD with Valid Dates =
TOTALMTD(
    [Internet Sales]
    , SAMEPERIODLASTYEAR(
        CALCULATETABLE(
            VALUES('Date'[Full Date])
            , 'Date'[IsValidDate] = TRUE()
        )
    )
)
```

The following figure shows the results of putting the preceding measures in a table visual without filtering the visual with the IsValidDate column:

![](img/5d2bce74fd6fceebed14fd1488bf7088_89_0.png)

Figure 2.26: Periodic calculations with improved measures

This method also fixes the issue of showing invalid dates when we use the Full Date column in a slicer visual. It is much nicer if we only show the valid dates in the slicer. The following figure illustrates a slicer visual showing Full Range Dates ranges from the Date table, side by side with another slicer visual showing Valid Dates Only:

![](img/5d2bce74fd6fceebed14fd1488bf7088_90_0.png)

Figure 2.27: The date slicer filtered by IsValidDate

In the preceding image, we used the IsValidDate flag column to filter the values of the left slicer.

## Period-over-period calculations

In many cases, businesses need period-over-period calculations, such as year-over-year and month-over-month calculations. Period-over-period calculations can vary from business to business, but the principles remain the same. This section has a broader focus. The scenario we use in this section is based on a real-world and highly demanded business requirement: dynamic measure selection. In this section, we learn how to use the out-of-the-box features available in Power BI Desktop to solve some data visualization challenges. In May 2022, Microsoft announced a new feature called Fields Parameters.

Let’s move forward with the dynamic measure selection scenario. The business requires you to analyze the Internet Sales over different periods as follows:
- Month-over-month internet sales variance
- Year-over-year internet sales variance
