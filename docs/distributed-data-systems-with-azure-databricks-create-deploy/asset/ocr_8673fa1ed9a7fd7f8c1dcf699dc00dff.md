Azure Databricks has a functionality for formatting SQL code in notebook cells, so as to reduce the amount of time dedicated to formatting code, and also to help in applying the same coding standards in all notebooks. To apply automatic SQL formatting to a cell, you can select it from the cell context menu. This is only applicable to SQL code cells:

![](bbox=[0.12, 0.18, 0.85, 0.41])

Figure 1.24 – Automatic formatting of SQL code

Applying the autoformatting of SQL code is a feature that can improve the readability of our code, and reduce possible mistakes due to bad formatting.

# Exploring data management

In this section, we will dive into how to manage data in Azure Databricks in order to perform analytics, create ETL pipelines, train ML algorithms, and more. First, we will briefly describe types of data in Azure Databricks.

# Databases and tables

In Azure Databricks, a database is composed of tables; table collections of structured data. Users can work with these tables, using all of the operations supported by Apache Spark DataFrames, and query tables using Spark API and Spark SQL.

These tables can be either global or local, accessible to all clusters. Global tables are stored in the Hive metastore, while local tables are not.

Tables can be populated using files in the DBFS or with data from all of the supported data sources.