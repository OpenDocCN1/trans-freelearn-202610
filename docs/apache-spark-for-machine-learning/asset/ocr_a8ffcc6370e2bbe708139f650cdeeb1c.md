Let’s see how grouped aggregation works:

+   1. **Grouping data**: First, the data is divided into groups based on one or more keys. These keys can be columns in a dataset, such as a category, date, or region.

+   2. **Applying aggregation functions**: Once the data is grouped, various aggregation functions can be applied to each group independently. Common aggregation functions include sum, average (mean), count, maximum, minimum, and standard deviation.

+   3. **Result**: The result is a new dataset where each group is represented by a single row, and the columns contain the aggregated values.

The following diagram illustrates how you can apply the GroupBy function on various columns to group together records, based on a column value. Grouped records are shown in the center section.

Aggregate functions can then be applied to each of these grouped records to get group-specific aggregates, as shown in the right-hand section.

![](bbox=[0.11, 0.38, 0.85, 0.84])

Figure 2.9 – An example of applying aggregate functions to grouped data