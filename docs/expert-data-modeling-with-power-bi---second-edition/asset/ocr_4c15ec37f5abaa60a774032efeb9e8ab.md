modelers can understand data quality, statistics, and data distribution within a column (not the overall dataset). For instance, a data modeler can quickly see a column’s cardinality, how many empty values a column has, and so forth.

As previously mentioned, the information provided by the **Column quality**, **Column distribution**, and **Column profile** features is calculated based on the top **1000** rows of data (by default), which sometimes leads to false information. It is good practice to set **Column profile** to get calculated based on the entire dataset for smaller amounts of data. However, this approach may take a while to load the column profiling information for larger amounts of data, so be careful while changing this setting if you are dealing with large tables.

To change the preceding setting from the status bar, proceed as follows:

+   1. Click the **Column profiling based on top 1000 rows** dropdown.
+   2. Select **Column profiling based on entire data set.**

The following image illustrates how to do this: