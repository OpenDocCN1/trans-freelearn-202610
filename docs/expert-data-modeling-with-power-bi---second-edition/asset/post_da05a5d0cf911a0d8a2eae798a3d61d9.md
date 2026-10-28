
![](img/5d2bce74fd6fceebed14fd1488bf7088_782_0.png)

Figure 15.3.1: Implementing RLS in Datamarts

So far, we have created a new RLS role and added a new rule to it. The next step is to assign members to the role. Follow these steps to do so:

- 8. From **Manage security roles**, click the **Assign** tab.
- 9. Select the **North Americans** role.
- 10. Type in the account names or security groups that you want to apply the role to and select them. I have a security group named **North American Users** that I select.
- 11. Click the **Add** button.
- 12. Click the **Save** button to apply the changes.
- 13. Click **Close**.

The following image shows the preceding steps:

## Figure 15.32: Assigning members to RLS roles in Datamarts

Now that we have assigned members to the RLS role, it is time to test the role to see if it works as expected. Follow these steps:

- 1. Click the View as button.
- 2. Select the North Americans role.
- 3. Click OK.

The following image shows the preceding steps and the results:

## Figure 15.33: Testing RLS roles in Datamarts

As the preceding image shows, the **DimSalesTerritory** table only shows the rows where the value of the **SalesTerritoryGroup** column is `North America`. This filter propagates to the **FactInternetSalesLive** table via the relationship between the two, which is exactly what we want.

As mentioned earlier, defining RLS in Datamarts applies it to the underlying Azure SQL database. We use SSMS to connect to the underlying Azure SQL DB. The underlying base SQL tables are not exposed to us; instead, there are SQL views created for each table. We can use SSMS to script the views to observe the construction of the views. The following image shows the scripts of the **model.DimSalesTerritory** view. The highlighted part of the code is where RLS is applied:

![](img/5d2bce74fd6fceebed14fd1488bf7088_784_0.png)

Figure 15.34: Scripting SQL views to see how RLS is applied at the database level in Datamarts

In this section, we learned what Datamarts are, and how to implement and work with them. While writing this chapter, the Datamarts feature is still in public preview and has not generally been available. Therefore, it may differ from what is available when reading this book. In the next section, we discuss some new DAX functions.

# New DAX functions

As we all know, Microsoft constantly adds new features, options, and functions to Power BI. While writing this book, Microsoft introduced 11 new DAX functions. This section briefly covers the new functions that I personally find more interesting and have many use cases. This section does not explain the syntax of the new functions; instead, we will refer to Microsoft’s official documentation website and explain the trickier points.

This section uses the `AdventureWorksDW2019` SQL Server database as our sample data source, which can be downloaded from the following URL:

https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&WT.mc_id=DP-MVP-5003466.

## NETWORKDAYS()

This function is very useful in calculating the number of working days by considering weekends and public holidays. We could calculate the number of working days before this function became available, but this function offers another level of convenience to Power BI developers. The syntax for this function, as per Microsoft’s official documentation website, is as follows:

```
NETWORKDAYS(<start_date>, <end_date>[, <weekend>, <holidays>])
```

See Microsoft’s official documentation here: https://learn.microsoft.com/en-us/dax/networkdays-dax?WT.mc_id=DP-MVP-5003466.

As you see, the function accepts a start date and an end date as required parameters, but the `weekend` and `holidays` are optional. The **holidays** parameter must be a list of public holidays in date format. For instance, the business wants to calculate adding working days from `AdventureWorksDW2019`, considering the 1st to the 5th of January each year is a public holiday. To achieve the requirement, import the **DimDate** table

into Power BI Desktop and use the following DAX expression to add a new calculated column to the table:

```
Is Public Holiday =
    AND(
        DimDate[EnglishMonthName] = "January"
        , AND(
            DimDate[DayNumberOfMonth] >= 1
            , DimDate[DayNumberOfMonth] <= 5
        )
    )
```

The following image shows the results after filtering the values of the **Is Public Holiday** calculated column to **True**:

![](img/5d2bce74fd6fceebed14fd1488bf7088_786_0.png)

Figure 15.35: Considering the 1st to the 5th of January each year as a public holiday

Now we create a measure using the following DAX expression to calculate the number of working days when the weekends in our sample are **Saturday** and **Sunday**; hence, the weekend parameter is 1:

```
Working Days =
NETWORKDAYS(
    FIRSTDATE(DimDate[FullDateAlternateKey])
    , LASTDATE(DimDate[FullDateAlternateKey])
    , 1
    , CALCULATETABLE (
        VALUES (DimDate[FullDateAlternateKey])
        , DimDate[Is Public Holiday] = TRUE
```

The following image shows the results in a Card visual:

![](img/5d2bce74fd6fceebed14fd1488bf7088_787_0.png)

Figure 15.36: Calculating working days using the NETWORKDAYS() function

We can have a separate table containing the public holidays to use in the NETWORKDAYS() function.

You can download the Chapter 15, NETWORKDAYS.pbix sample from here: https://github.com/PacktPublishing/Expert-Data-Modeling-with-Power-BI-Second-Edition/blob/28e2af1762336a3b3961c41e9020de8200/Samples/Chapter%2015/Chapter%2015,%20NETWORKDAYS.pbix.

## EVALUATEANDLOG()

The EVALUATEANDLOG() function was revealed by DAX maestro Jeffrey Wang for the first time through his blogpost in August 2022: https://pbidax.wordpress.com/2022/08/16/introduce-the-dax-evaluateandlog-function/. The EVALUATEANDLOG() function is useful for debugging DAX code, especially when evaluating virtual tables.

As we all know, previously, there weren’t any options in Power BI Desktop to see the results of the virtual tables if we used some third-party tools such as DAX Studio. With the EVALUATEANDLOG() function, we still require third-party tools, but we are not required to deconstruct the DAX code to get the subsequent results of each part of the code. Make no mistake—the EVALUATEANDLOG() function is useful for debugging all kinds of DAX code, not just virtual tables. Nevertheless, I mostly find it helpful with virtual tables. We can use this function by adding it to our existing DAX code.

The syntax of the function is as follows:

```
EVALUATEANDLOG(<Value>, [Label], [MaxRows])
```

The Value parameter accepts any scalar or table DAX expression. We can add a label by specifying the Label optional parameter and setting the maximum number of retrieving rows, by specifying the MaxRows optional parameter.

You can read more on Microsoft’s official documentation web page here: https://learn.microsoft.com/en-us/dax/evaluateandlog-function-dax?WT.mc_id=DP-MVP-5003466.

Whenever the xVelocity engine sees the EVALUATEANDLOG() function in code, it runs the DAX expressions of the Value parameter to be shown in the Power BI Desktop visuals. It also sends the results in JSON format to the DAXEvaluationLog event of the event log. We can then capture the results using either SQL Server Profiler or a new community tool built by Jeffrey Wang: DAX Debug Output. We use the latter in this section. The DAX Debug Output tool is available for download here: https://github.com/pbidax/DAXDebugOutput.

Now, let us evaluate the Working Days measure created in the previous section’s sample file, Chapter 15, NETWORKDAYS.pbix. We only need to change the DAX expression we previously wrote to the following:

```
NETWORKDAYS(
EVALUATEANDLOG(FIRSTDATE(DimDate[FullDateAlternateKey]),
```

```
```dax
"StartDate")
    ,
EVALUATEANDLOG(LASTDATE(DimDate[FullDateAlternateKey]),
"EndDate")
    , 1
    , EVALUATEANDLOG(SUMMARIZECOLUMNS(
        DimDate[FullDateAlternateKey]
        , TREATAS({TRUE}, DimDate[Is Public Holiday])
    ), "VirtualTable", 10)
)
```
```

As you see, I added the `EVALUATEANDLOG()` to three parts of the code to capture the results. The third one captures the top 10 rows of the virtual table. Now I run the **DAX Debug Output** tool and connect the currently running Power BI report file to capture the `DAXEvaluationLog` event.

The following image shows the results of the first `EVALUATEANDLOG()` in the DAX Debug Output tool:

![](img/5d2bce74fd6fceebed14fd1488bf7088_789_0.png)

Figure 15.37: The results of the first EVALUATEANDLOG() in the DAX Debug Output tool

The following image shows the results of the second `EVALUATEANDLOG()` in the DAX Debug Output tool:
