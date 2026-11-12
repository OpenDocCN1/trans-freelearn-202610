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

+   • Unions append data in creating additional rows.
+   • A union should contain datasets that have many common columns.
+   • Unions should only be used for the same-structured data stored across multiple data sources.
+   • Keep an eye on performance when using unions. With each additional dataset in the union, you will increase the complexity of Tableau's VizQL query language.

Since you just became a union expert, it's time to move on to the next feature Tableau offers: **blending**! Blending helps you to combine datasets that have a different level of granularity. Think of *one-to-many* relationships between datasets. One dataset has a unique key per row; the other has multiple rows per key. To avoid duplicating rows in the first-mentioned dataset, Tableau deployed blending long before relationships were part of the software package.

# Do we still need blends?

Relationships make data blending a little less needed, and it can be seen as legacy functionality. However, for the sake of completeness and for older Tableau versions (below 2020.2), let's consider a summary of data blending in the following sections. In a nutshell, data blending allows you to merge multiple disparate data sources into a single view. Understanding the following four points will give you a grasp of the main points regarding data blending:

+   • Data blending is typically used to merge data from multiple data sources. Although, as of Tableau 10, joins are possible between multiple data sources, there are still cases when data blending is the only