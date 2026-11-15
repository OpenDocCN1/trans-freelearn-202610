
## Saving Output to External Database

3. A third option to save Prep output has been added to Tableau 2020.3: writing the resulting data to an external database. If you want to do so, select the `Database` table option from the dropdown. At the time of writing, seven databases are compatible with the write-back functionality, but more will be added based on user demand:

![](Images/B18435_03_55.png)

Figure 3.55: Save to external database

Since the Tableau release 2022.3, you can write back to CRM Analytics too. To do this, you need a Salesforce account and the according license. Therefore, we will not discuss this option further, but if you are a Salesforce Analytics user, check this out!

This section has shown us how to save our work and, even more so, the output of our Prep flows. To me, the ability to save output to an external database is the best new feature, because it is the easiest way to share my output in a structured and organized way with others. So not only I but also other users can work with the prepped data, saving additional work. The flow can then be reused not only by me but by anyone.

## Summary

We started this chapter with an introduction to Prep. We looked at the GUI and how we can connect data to it. After that, we did some exercises regarding data preparation. This can be divided into five parts: data cleaning, unions and joins, aggregating, pivoting, and scripting. We also considered several additional options to improve the usefulness of your data, including inserting steps from other flows, and incremental refreshes. To round out this chapter on Prep, we looked at exporting data. Here, we saw that the new dataset can be saved as a file or data extract, written back to an external database, or pushed directly to Tableau Server.

Just like Tableau Desktop, Prep is very much self-explanatory and highly visual. Colors, symbols, and highlights make it easy to get used to this extract, transform, and load tool, which is invaluable for preparing your data before manipulating it on the main Tableau interface!

In the next chapter, we’ll continue our exploration of data in Tableau. We’ll explore how to prepare data for Tableau by looking at joins, blends, and data structures.

## Learn More on Discord

To join the Discord community for this book – where you can share feedback, ask questions to the author, and learn about new releases – follow the QR code below:

[https://packt.link/tableau](https://packt.link/tableau)

![](Images/QR_Code2044012316175764640.png)

# Chapter 4: Learning about Joins, Blends, and Data Structures

## Introduction

Connecting Tableau to data often means more than connecting to a single table in a single data `source`. You may need to use Tableau to join multiple tables from a single data source. For this purpose, we can use joins, which combine a dataset row with another dataset’s row if a given key value matches. You can also join tables from disparate data sources or union data with a similar metadata structure.

Sometimes, you may need to merge data that does not share a common row-level key, meaning if you were to match two datasets on a row level like in a join, you would duplicate data because the row data in one dataset is of much greater detail (for example, cities) than the other dataset (which might contain countries). In such cases, you will need to blend the data. This functionality allows you to, for example, show the count of cities per country without changing the city dataset to a country level.

Also, you may find instances when it is necessary to create multiple connections to a single data source in order to pivot the data in different ways. This is possible by manipulating the data structure, which can help you achieve data analysis from different angles using the same dataset. It may be required to discover answers to questions that are difficult or simply not possible with a single data structure.

In this chapter, we will discuss the following topics:

*   Relationships
*   Joins
*   Unions
*   Blends
*   Understanding data structures

In version 2020.2, Tableau added functionality that will allow you to join or blend without specifying one of the two methods. It is called `relationships`. We will start this chapter off by explaining this new feature before we investigate the details of joins, blends, and more.

## Relationships

Although this chapter will primarily focus on joins, blends, and the manipulation of data structures, let’s begin with an introduction to `relationships`: a new functionality available since Tableau 2020.2, and one that the Tableau community has been waiting a long time for. It is the new default option in the data canvas; therefore, we will first investigate `relationships`, which belong to the `logical layer of the data model`, before diving deeper into the join and union functionalities that operate on the `physical layer`.

To read all about the physical and logical layers of Tableau’s data model, visit the Tableau help pages: [https://help.tableau.com/current/online/en-us/datasource_datamodel.htm](https://help.tableau.com/current/online/en-us/datasource_datamodel.htm).

For now, you can think of the logical layer as being able to count smarter. Imagine a customer table with unique customer names and an order table with hundreds of recorded orders. In a logical layer, a customer name with multiple orders (thus multiple rows of order and customer IDs in the order table) will be counted as one customer because the logical layer is smart and it recognizes the customer name, while the physical layer would count one customer for each order and hence it would count the same customer as multiple customers if they ordered multiple times. You could only solve this by counting distinct values (which is not required when using the logical layer).

In the following screenshot, you can see the data source canvas, with two datasets combined in a relationship on the left-hand side and the same datasets combined using a `join` on the right-hand side. Please note that relationships only show a line between the tables (`Orders`, `People`, and `Returns`) whereas joins indicate the type of join by two circles:

![](Images/B18435_04_01.png)

Figure 4.1: Relationships and joins on the data source canvas

A key difference is that the preview of the data, at the bottom, will show only data from the selected table in relationships, compared to all data when using joins. This makes sense because the granularity of data can change in relationships, depending on the fields you are using in your dashboard. Joins, however, have a fixed level of granularity, which is defined by the type of join and join clauses you choose.

Relationships are the default for Tableau Desktop 2020.2 and higher. However, if you still want to make use of the join functionality, you can drag a dataset into the data source canvas, click on the drop-down arrow, and select `Open…`:

![](Images/B18435_04_02.png)

Figure 4.2: From relationship to join canvas

This will open the dataset as you used to see it in the data pane in Tableau in versions before 2020.2 and you will be able to use joins the old way, as we will describe in the `Joins` section.

The line between two datasets in a relationship (based on the logical layer) is called a `noodle` due to its al dente spaghetti looks – yum! Tableau detects a relationship as soon as you drag in the second data source to the data source canvas, but you can add more key columns or remove and adjust them if needed by clicking on the noodle itself:

![](Images/B18435_04_03.png)

Figure 4.3: Relationship keys

If you open older dashboards in Tableau 2020.2 or later versions, you will see that the joined data will be shown as `Migrated`. This is intentional. Just click on the migrated data source and Tableau will switch from the logical to the physical layer, meaning you will see the join-based data source canvas instead of the relationship canvas.
