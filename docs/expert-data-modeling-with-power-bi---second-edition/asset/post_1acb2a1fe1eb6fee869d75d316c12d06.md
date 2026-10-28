
## Star Schema Preparation in Power Query Editor

In the previous chapter, we learned about some common data preparation steps, including data type conversions, split columns, merge columns, adding a custom column, and filtering rows. We also learned how to create summary tables using the Group By feature, appending data, merging queries, and pivoting tables.

This chapter uses many topics discussed in the past few chapters and helps you learn how to prepare a Star Schema in Power Query Editor. Data modeling in Power BI starts with preparing a Star Schema. In this chapter, we use the `Chapter 6, Sales Data.xlsx` file, which contains flattened data. You can download the file from here:

[https://github.com/PacktPublishing/Expert-Data-Modeling-with-Power-BI-Second-Edition/blob/28e2af1762336ab5236a3b3961c41e9020de8200/Samples/Chapter%2006/Chapter%206,%20Sales%20Data.xlsx](https://github.com/PacktPublishing/Expert-Data-Modeling-with-Power-BI-Second-Edition/blob/28e2af1762336ab5236a3b3961c41e9020de8200/Samples/Chapter%2006/Chapter%206,%20Sales%20Data.xlsx)

A common daily scenario is that we get a set of files containing data exported from a source system, and we need to build a report to answer business questions. Therefore, having the required skills to build a Star Schema on top of a flat design becomes handy.

In this chapter, we cover the following topics:

- Identifying dimensions and facts
- Creating dimension tables
- Creating fact tables

Figure 6.1: Power Query Editor connected to the Chapter 6, Sales Data.xlsx sample data

We must study the data by looking at the sample data itself as well as using Power Query Editor to find out the following:

- The number of tables in the data source
- The linkages between existing tables
- The lowest required grain of Date and Time

The preceding points are the most straightforward initial points we must raise with the business within the initial discovery workshops. These simple points help us understand the scale and complexity of the work. In the following few sections, we look at the preceding points in more detail.

### Number of tables in the data source

Our sample file contains two sheets that translate into two **base tables**: **Sales** and **Customer**. We extract the dimensions and facts from the base tables; therefore, they will not be loaded into the model in their original shape. From a modeling perspective, there must be a linkage between the two tables, so we need to raise this with the business, study the data, and see if we can find the linkage(s). The names of the tables in the source system (an Excel file in our case) are highlighted in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_314_0.png)

- The linkage between product data such as **Product Category**, **Product Subcategory**, and **Product**
- The linkage between sales order data such as **Sales Order** and **Sales Order Line**
- The linkage between `Date` and `Time`, such as **Order Date**, **Due Date**, and **Ship Date**

### Finding the lowest required grain of Date and Time

In most real-world cases, if not all, the business must analyze data over multiple dimensions such as **Product**, **Customer**, **Date** or **Time**, or both **Date** and **Time**. We must find the lowest grain of detail required to support the business requirements. You will often be faced with requirements such as:

> *As a Sales manager, I want to analyze sales data by branch, location, date, and time.*

In the above user story, a fact is an event that the business wants to analyze. So, the sales are a fact. On the other hand, the dimensions are everything that the fact is analyzed by. So, in the preceding user story, **branch**, **location**, **date**, and **time** are the dimensions. We also have a **Person** dimension to cover the *sales manager*. While we identify the facts and dimensions, it is a crucial task to identify the granularity of the data, too. The granularity of the data, also called the grain, is not so evident at first look. For instance, in the preceding user story, at what level of detail must the sales data be analyzed for the following dimensions?

- **Location:** `Continent, Country, City, County, Area, Postcode, and Street number`
- **Date:** `Year, Quarter, Month, Week, and Day`
- **Time:** `Hour, Minute, and Second`

In real-world scenarios, especially in large projects, we have to ask relevant questions of the business to identify the grain. In small projects, we can identify the grain by carefully studying the data. Let us look at our scenario.

In our example, the business needs to analyze the data over both Date and Time. But we have to be more descriptive about the level of Date and Time that the business requires to analyze the data. By studying the data, we find out that both the Sales and Customer tables have columns with the Date or DateTime data types. The Sales table has three columns containing the DateTime data type. Power Query Editor automatically detects the columns’ data types. Studying the data more precisely shows that the time element of the values in both DateTime columns is always 12:00:00 AM. So, most probably, the data type of the DueDate and ShipDate columns is Date, not DateTime. This means that the only column with the DateTime data type is the OrderDate column. The following image shows that the data in the OrderDate column is stored in Seconds:

![](img/5d2bce74fd6fceebed14fd1488bf7088_317_0.png)

Figure 6.3: Columns with the Date and DateTime data types in the Sales table

So, the grain of the OrderDate column can be in Seconds. We save this question and confirm it with the business later.

Let’s also look at the Customer table. The Customer table also has two columns with the Date data type, BirthDate and DateFirstPurchase, as shown in the following image:

![](img/5d2bce74fd6fceebed14fd1488bf7088_318_0.png)

Figure 6.4: The Date column in the Customer table

As the next step in our discovery workshops, we must clarify the following points with the business:

- Do they need to analyze the data for customers’ birth date?
- Do they need to have the **BirthDate** data at different date levels (`Year`, `Quarter`, `Month`, and so on)?
- Do they need to analyze the data for **DateFirstPurchase**?
- Do they need to show the **DateFirstPurchase** data at various date levels?
- What are the required levels of `Date` and `Time` for the **OrderDate** from the **Sales** table?
- What is the `Date` level for the **DueDate** and **ShipDate** columns from the **Sales** table?

In our imaginary discovery workshop, the business clarifies that they do not want to analyze the customers’ birth dates or first purchase dates. We also discovered that they need to analyze **OrderDate** from the **Sales** table over `Date` at the `Day` level and over `Time` at the `Minutes` level. They also confirm that the **DueDate** and **ShipDate** from the **Sales** table are at the `Day` level.

With the preceding findings, it is clear that we need to create a `Date` dimension and a `Time` dimension.

The next step is identifying the dimensions, their granularity, and their facts.

### Defining dimensions and facts

To identify the dimensions and the facts, we have to conduct requirement-gathering workshops with the business. We need to understand the business processes by asking WWWWWH questions; that is, *What*, *When*, *Where*, *Who*, *Why*, and *How*. This is a popular technique, also known as 5W-1H. The answers to these questions help us understand the business processes, which helps to identify the dimensions and facts. Let us have a look at some examples:

- The answer to the *What* question can be a product, item, service, and so on.
- The answer to the *When* question can be a date, time, or both and at what level, month, day, hour, minute, and so on.
- The answer to the *Where* question can be a physical location such as a store or warehouse, geographical location, and so on.

In the requirement-gathering workshops, we try to determine what describes the business and how the business measures itself. In our scenario, we imagine we have conducted several discovery workshops, and we found out the business requires the following sales data:

- Sales amount
- Quantity
- Costs (tax, freight, and so on)

The sales data must be analyzed by the following:
