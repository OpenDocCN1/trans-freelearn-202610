
The next step is to format all **DateTime** columns as **Time**. The following figure shows the results of running the preceding DAX expressions:

![](img/5d2bce74fd6fceebed14fd1488bf7088_114_0.png)

## Figure 2.40: Generating a Time table with DAX

## Note

> The preceding figure shows the results sorted by the **Time** column in ascending order.

The next step is to create a relationship between the **Order Time** column from the **Internet Sales** table and from the **Time** table.

The following figure shows how we can add some area charts to the report canvas and visualize **Internet Sales** by different time buckets:

![](img/5d2bce74fd6fceebed14fd1488bf7088_115_0.png)

Figure 2.41: Visualizing Internet Sales by different time buckets

The best practice is to move all data transformations, such as generating Date or Time tables, to the source system as much as possible. If that is not an option for any reason, then it is best to take care of the transformations in Power Query Editor.

## Summary

In this chapter, we discussed different aspects of DAX and how it can affect our data modeling. We looked at some real-world scenarios and challenges and how to solve them with DAX.

In the next chapter, we look at the Power Query (M) expression language in more detail. We also go through some hands-on scenarios and prepare a star schema in Power Query step by step.

## Join us on Discord!

Join The Big Data and Analytics Community on the Packt Discord Server!

Hang out with 558 other members and enjoy free voice and text chat.

https://packt.link/ips2H

![](img/5d2bce74fd6fceebed14fd1488bf7088_116_0.png)

## Introducing the Power Query M formula language in Power BI

Power Query is a data preparation technology offering from Microsoft to connect to many different data sources from various technologies, enabling businesses to integrate data, transform it, make it available for analysis, and get meaningful insights from it. Power Query can currently connect to many data sources.

It also provides a **custom connectors software development kit (SDK)** that third parties can use to create their data connectors. Power Query was initially introduced as an Excel add-in that quickly became a vital part of the Microsoft data platform for data preparation and transformation.

Power Query is currently integrated with a few Microsoft products such as **Dataverse** (also known as **Common Data Service (CDS)**), **SQL Server Analysis Services Tabular models (SSAS Tabular)**, **SQL Server Integration Services (SSIS)**, **Azure Data Factory (ADF)**, and **Azure Analysis Services (AAS)**, as well as Power BI and Excel. Therefore, learning about Power Query helps data professionals to support data preparation in multiple technologies. You can find the full list of Microsoft products and services using Power Query from the Microsoft official website via this link:

https://docs.microsoft.com/en-us/power-query/power-query-what-is-power-query?WT.mc_id=DP-MVP-5003466#where-can-you-use-power-query

Power Query M is a formula language capable of connecting to various data sources to mix and match the data between those data sources, which then loads into a single dataset. In this section, we introduce Power Query M.

## Power Query is CaSe-SeNsItIvE

While Power Query is a *case-sensitive* language, **Data Analysis Expressions (DAX)** is not, which may confuse some developers. Power

Query and DAX are different worlds that came together in Power BI to take care of different aspects of working with data. Not only is Power Query case-sensitive in terms of syntax, but it is also case-sensitive when interacting with data. For instance, we get an error message if we run the following function:

```
datetime.localnow()
```

This is because the following is the correct syntax:

```
DateTime.LocalNow()
```

Ignoring Power Query’s case sensitivity in data interactions can become an issue that is hard and time-consuming to identify. A real-world example is when we get **globally unique identifier (GUID)** values from a data source containing lowercase characters. Then, we get some other GUID values from another data source with uppercase characters. When we match the values in Power Query to merge two queries, we do not get any matching values. But if we turn the lowercase GUID into uppercase, the values match.

Comparing two string values with different character cases in Power Query does not raise any errors. The data mashup engine simply compares the two values and returns *false* because the two strings are not the same. So there are no errors to raise; however, the result is incorrect.

For example, the following two GUID values are not equal in Power Query, while they are equal in DAX:

```
D5E99E0E-0737-45B2-B62A-4170B3FEFC0E
d5e99e0e-0737-45b2-b62a-4170b3fefc0e
```

## Queries

In Power Query, a query contains **expressions**, **variables**, and **values** encapsulated by `let` and `in` statements. A `let` and `in` statement block is

structured as follows:

```
let
    Variablename = expression1,
    #"Variable name" = expression2
in
    #"Variable name"
```

As the preceding structure shows, we can have spaces in the variable names. However, we need to encapsulate the variable name using a number sign (#) followed by quotation marks—for example, #"Variable Name". By defining a variable in a query, we create a query formula step in Power Query. Query formula steps can reference any previous steps. Lastly, the query output is the variable that comes straight after the in statement. Each step must end with a comma, except the last step before the in statement.

## Expressions

In Power Query, an expression is a formula that results in values. For instance, the following image shows some expressions and their resulting values:

![](img/5d2bce74fd6fceebed14fd1488bf7088_121_0.png)

Figure 3.1: Expressions and their values

**Record value**: A record is a set of fields that make up a row of data. To create a record, we use brackets []. Inside the brackets, we mention the *field name* and an *equal sign* followed by the field’s *value*. We separate different fields and their values using a comma, as follows:

```
[
    First Name = "Soheil"
  , Last Name = "Bakhshi"
  , Occupation = "Consultant"
]
```

The following image shows the expression and the values:

![](img/5d2bce74fd6fceebed14fd1488bf7088_124_0.png)

Figure 3.3: Defining a record in Power Query

Note
When defining a record, we do not need to put the field names in quotation marks. As illustrated in the previous image, records are shown vertically.

We can expand a structured column to get its primitive values, as illustrated in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_127_0.png)

Figure 3.6: Table with a structured column The following image shows the result after expanding the structured column to new rows:

![](img/5d2bce74fd6fceebed14fd1488bf7088_127_1.png)

Figure 3.7: Table with an expanded structured column

Function value: A function is a value that accepts input parameters and produces a result. To create a function, we put the list of parameters (if any) in parentheses, followed by the output data type. We use the goes-to symbol (=>), followed by the function's definition. For instance, the following function calculates the end-of-month date for the current date:

```
```powerquery
() as date =>
Date.EndOfMonth(Date.From(DateTime.LocalNow()))
```
```

The preceding function does not have any input parameters but produces an output. The following image shows a function invocation without parameters that returns the end-of-month date for the current date (today’s date):

![](img/5d2bce74fd6fceebed14fd1488bf7088_128_0.png)

Figure 3.8: Invoking a custom function

## Types

In Power Query, values have types. A **type value** classifies other values. There are two general categories for types: **primitive types** and **custom types**.

## Primitive types

A value can have a primitive type, as follows:

- `binary`
