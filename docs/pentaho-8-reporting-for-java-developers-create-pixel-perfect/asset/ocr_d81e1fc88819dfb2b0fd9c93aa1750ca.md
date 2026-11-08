# Business intelligence reporting

When you've reached the limits of operational reporting, the next logical step is to move your data into a data warehouse. This move is often referred to as business intelligence reporting. Reporting alone does not provide the necessary tools to make this transition. You will need an **Extract, Transform, Load (ETL)** tool, such as Pentaho Data Integration, along with a sensible warehouse design, such as a snowflake schema.

![](bbox=[0.14, 0.38, 0.21, 0.43])

> *A **snowflake schema** is a logical arrangement of tables in a multidimensional database, such that the entity relationship diagram resembles a snowflake shape. A multidimensional database is defined as "a variation of the relational model that uses multidimensional structures to organize data and express the relationships between data".*

This type of use allows business users to monitor changes over time. It also helps gain performance benefits by pre-calculating aggregations and defining schemas that are built in mind for summarized reporting. Until recently, data warehousing and business intelligence had been limited to large enterprises due to the cost of software and limited expertise. With open source tools becoming more widely available, a large number of small and medium size businesses are deploying data warehouses in order to get solutions for the critical questions in their business domain. Common examples of data warehouse reporting include combining sales and inventory data into a single location for reporting, or combining internal proprietary sales data with publicly available market trends and analysis.

Pentaho Reporting's flexible data source support makes it easy to incorporate reports in your business intelligence solutions. Also, with Pentaho Reporting's speed and scalability, you can deploy Pentaho Reporting with the confidence that reports will be executed efficiently.

As with all approaches, there are limitations to this approach. In traditional warehousing, data is usually batched nightly, weekly, or monthly. Therefore,