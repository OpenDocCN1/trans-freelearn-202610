
## Understanding unions

Sometimes you might want to analyze data with the same metadata structure that is stored in different files—for example, sales data from multiple years, different months, or countries. Instead of copying and pasting the data, you can perform a union function on them. We already touched upon this topic in Chapter 3, Using Tableau Prep Builder, but a union is basically where Tableau will append new rows of data to existing columns with the same header. For the following exercise, we will use FIFA data (from the PlayStation game, not the World Cup). The data is from Kaggle (https://www.kaggle.com/datasets/stefanoleone992/fifa-22-complete-player-dataset?resource=download) and ships in multiple CSVs; each CSV contains data for 1 year, and male/female are split too.

For our analysis, we want to combine all the files into one. Hence, we need to perform a union by taking the following steps:

- 1. Download the CSV files from GitHub (https://github.com/PacktPublishing/Mastering-Tableau-2026/tree/main/Chapter04) or Kaggle (https://www.kaggle.com/datasets/stefanoleone992/fifa-22-complete-player-dataset?resource=download).
- 2. In the dashboard belonging to this chapter, open the Connect to data pane, and click on Text file:

![](img/66cf6b89349fda3581a4f196cb3c98a4_150_0.png)

Figure 4.10: Text file

- 3. Locate the downloaded FIFA data on your machine and connect to any one of the files.
- 4. Create a union by dragging and dropping a second file from the left pane on top of the initial file until the word **Union** appears–a sign that when dropping the data now, the data will be used for a union:

![](img/66cf6b89349fda3581a4f196cb3c98a4_151_0.png)

Figure 4.11: Union

- 5. Alternatively, click on the arrow of the primary dataset and select **Convert to Union**...

![](img/66cf6b89349fda3581a4f196cb3c98a4_151_1.png)

Figure 4.12: Convert to Union...

- 6. In the following popup (Figure 4.13), the user has the option to select all data tables that should be part of the union individually by dragging and dropping or by using a Wildcard union, which includes all tables in a certain directory based on the naming convention—a * sign represents any character. A players* instance will include any file that starts with players. Respectively, female* will include all files starting with female, and * will include all files:

![](img/66cf6b89349fda3581a4f196cb3c98a4_152_0.png)

Figure 4.13: Select all data

Unions are used to combine data that is similar and can be appended underneath each other. They are most often used for multiple data files where each has a different date stamp, a different region, a different manager, and so forth. By using a union, you want to make the dataset more complete. We saw that Tableau allows unions by dragging and dropping, as well as right-clicking on the initial data source. We also talked about unions that combine one or more other datasets, using drag and drop or wildcards. Figure 4.14 presents a typical union example:

| Dataset A | | | Dataset B | | | Union | | |
|---|---|---|---|---|---|---|---|---|
| Sales 2021 | | | Sales 2022 | | | Sales 2021&2022 | | |
| Year | Month | Sales | Year | Month | Sales | Year | Month | Sales |
| 2021 | 12 | 639 | 2022 | 12 | 4323 | 2021 | 12 | 639 |
| 2021 | 11 | 3758 | 2022 | 11 | 5757 | 2021 | 11 | 3758 |
| 2021 | 10 | 6836 | 2022 | 10 | 4444 | 2021 | 10 | 6836 |
| 2021 | 9 | 1721 | 2022 | 9 | 625 | 2021 | 9 | 1721 |
| 2021 | 8 | 5942 | 2022 | 8 | 191 | 2021 | 8 | 5942 |
| 2021 | 7 | 9853 | 2022 | 7 | 6480 | 2021 | 7 | 9853 |
| 2021 | 6 | 7338 | 2022 | 6 | 4231 | 2021 | 6 | 7338 |
| 2021 | 5 | 6144 | 2022 | 5 | 2696 | 2021 | 5 | 6144 |
| 2021 | 4 | 7285 | 2022 | 4 | 4600 | 2021 | 4 | 7285 |
| 2021 | 3 | 7560 | 2022 | 3 | 6999 | 2021 | 3 | 7560 |
| 2021 | 2 | 561 | 2022 | 2 | 8123 | 2021 | 2 | 561 |
| 2021 | 1 | 7844 | 2022 | 1 | 9602 | 2021 | 1 | 7844 |
| | | | | | | 2022 | 12 | 4323 |
| | | | | | | 2022 | 11 | 5757 |
| | | | | | | 2022 | 10 | 4444 |
| | | | | | | 2022 | 9 | 625 |
| | | | | | | 2022 | 8 | 191 |
| | | | | | | 2022 | 7 | 6480 |
| | | | | | | 2022 | 6 | 4231 |
| | | | | | | 2022 | 5 | 2696 |
| | | | | | | 2022 | 4 | 4600 |
| | | | | | | 2022 | 3 | 6999 |
| | | | | | | 2022 | 2 | 8123 |
| | | | | | | 2022 | 1 | 9602 |

Figure 4.14: Union

Let us look at aspects we need to remember:

- • Unions append data in creating additional rows.
- • A union should contain datasets that have many common columns.
- • Unions should only be used for the same-structured data stored across multiple data sources.
- • Keep an eye on performance when using unions. With each additional dataset in the union, you will increase the complexity of Tableau's VizQL query language.

Since you just became a union expert, it's time to move on to the next feature Tableau offers: **blending**! Blending helps you to combine datasets that have a different level of granularity. Think of *one-to-many* relationships between datasets. One dataset has a unique key per row; the other has multiple rows per key. To avoid duplicating rows in the first-mentioned dataset, Tableau deployed blending long before relationships were part of the software package.

## Do we still need blends?

Relationships make data blending a little less needed, and it can be seen as legacy functionality. However, for the sake of completeness and for older Tableau versions (below 2020.2), let's consider a summary of data blending in the following sections. In a nutshell, data blending allows you to merge multiple disparate data sources into a single view. Understanding the following four points will give you a grasp of the main points regarding data blending:

- • Data blending is typically used to merge data from multiple data sources. Although, as of Tableau 10, joins are possible between multiple data sources, there are still cases when data blending is the only feasible option to merge data from two or more sources. In the following sections, we will see a practical example that demonstrates such a case.

- - Data blending requires a shared dimension. A date dimension is often a viable candidate for blending multiple data sources.
- - Data blending aggregates and then matches. On the other hand, joining matches and then aggregates.
- - Data blending does not enable dimensions from a secondary data source. Attempting to use dimensions from a secondary data source will result in a * instance or null in the view. There is an exception to this rule, which we will discuss later, in the Adding secondary dimensions section.

Now that we have introduced relationships, joins, and unions, I would like to shift your focus a bit to data structures within your workbook. You might have set up the perfect join or union, started dragging and dropping fields onto your workbook canvas, used a filter, used a calculated field, and then received some unexpected results. Tableau is behaving, just not the way you like it. Why might that be?! The order of operation here is key. It is essential to know when which filter will be applied and how this affects your data. Therefore, next in line is the order of operations.

## Exploring the order of operations

Isn't a data blend the same thing as a left join? This is a question that new Tableau authors often ask. The answer, of course, is no, but let us explore the differences. The following example is simple, even lighthearted, but it does demonstrate profound consequences that can arise from incorrect aggregation resulting from an erroneous join.

We will explore in which order aggregation happens in Tableau. This will help you understand how to use blends and joins more effectively.

For this example, we will use the following data:

| Pants | | Shirts | |
|---|---|---|---|
| Salesperson | Pants Amount | Sales Associate | Shirts Amount |
| Tanya | 100 | Tanya | 50 |
| Tanya | 100 | Tanya | 50 |
| Zhang | 100 | Zhang | 50 |
| Zhang | 100 | Zhang | 50 |

Figure 4.15: Pants and shirts sales
