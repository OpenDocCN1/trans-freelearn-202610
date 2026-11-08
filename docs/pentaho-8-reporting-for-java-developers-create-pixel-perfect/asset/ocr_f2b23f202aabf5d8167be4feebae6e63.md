Included in both the subgroups, you can find a version of each OLAP data source named as denormalized (the default data sources assume they are normalized). Normalized and denormalized are two possible classifications of MDX queries, depending on the structure of the resulting dataset. It's not within the scope of this book to elaborate over the details of data normalization/denormalization, but the following example can shed some light over the differences between the two.

Here is an example of a normalized form of an MDX query:

| [Time].[(All)] | [Time].[Years] | [Measures].[Revenue] | [Measures].[Quantity] |
|---|---|---|---|
| All years | 2015 | 150,000 | 1,000 |
| All years | 2016 | 170,000 | 1,100 |
| All years | 2017 | 190,000 | 1,200 |

Here is an example of a denormalized form of the same MDX query:

| [Time].[(All)] | [Time].[Years] | [Measures].[MeasuresLevel] | Measure |
|---|---|---|---|
| All years | 2015 | Revenue | 150,000 |
| All years | 2016 | Revenue | 170,000 |
| All years | 2017 | Revenue | 190,000 |