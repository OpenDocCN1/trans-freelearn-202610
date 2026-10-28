> On top of the preceding benefits, Hybrid Tables provide real-time data on large tables, which is exceptional

Incremental refresh is inevitable when dealing with massive datasets with billions of rows that do not fit into our data model in Power BI Desktop. Now that we understand incremental refresh and hybrid tables, let’s implement them in Power BI.

# Configuring incremental refresh policy and hybrid table in Power BI Desktop

We use Power BI Desktop to configure incremental refresh. Once we publish the model into the Power BI service, the first data refresh transfers all data from the data source(s) to the Power BI service. Therefore, the first refresh takes longer. Any future data refreshes are incremental. Configuring incremental refresh in Power BI Desktop is simple. The following steps explain the process:

+   1. First, we must define two DateTime parameters in Power Query Editor, RangeStart and RangeEnd, which are reserved for defining incremental refresh policies.

> Note
> 
> Power Query is case sensitive, so the names of the parameters must be RangeStart and RangeEnd.

+   2. The next step is to filter the data on either an Int64 (integer) or DateTime column using the RangeStart and RangeEnd parameters when the value of the DateTime column is between RangeStart and RangeEnd. Therefore, for scenarios where our table has a Smart Date Key column instead of DateTime, we have to convert the RangeStart and RangeEnd parameters to Int64. The Int64 or DateTime values must have an equal to (=) sign either on RangeStart or RangeEnd, not both.