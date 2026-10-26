# How it works

Dataflows ingest data from source systems and store that data as entities in Microsoft Common Data Model-compliant folders. This entity data is stored within Azure Data Lake Storage Gen2. You can think of entities as tables.

A refresh schedule can be created for a dataflow in the same way that Power BI Desktop queries can be refreshed within the Power BI service. Refreshes update the data within the Azure Data Lake Storage Gen2 infrastructure. Power BI Desktop reports connecting to the dataflow pull data from the data entities stored within Azure Data Lake Storage Gen2 folders.

Using dataflows, performance can be improved for both Power BI reports as well as the source data systems. Since only the dataflow interacts with the source system versus potentially many different Power BI Desktop queries, this potentially reduces the load on the source systems. In addition, particularly if the source systems are on-premises, refreshes for Power BI reports can be improved, since network latency for retrieving data via a data gateway is removed and Power BI semantic models are refreshed from data already present in the Microsoft cloud.

# There’s more…

The decision to use dataflows versus queries in Power BI Desktop depends on a number of factors, including your data preparation needs, collaboration requirements, and the scalability of your solution. Here are some considerations when choosing between a dataflow and a query in Power BI:

Consider Power BI dataflows in the following scenarios:

+   - **Centralized data preparation:** If there are multiple reports or dashboards that require the same or similar data preparation steps, using a dataflow can centralize these transformations, ensuring consistency across various reports.
+   - **Reuse of data transformations:** Dataflows allow the reuse of data transformation logic across different semantic models. If you find yourself applying similar transformations to the same tables in multiple semantic models, dataflows can help in creating a standardized set of transformations.
+   - **Collaboration:** When multiple users or teams need to collaborate on data preparation, dataflows provide a shared environment. This allows for collaboration on the data preparation process, and changes made by one user are reflected for others.
+   - **Scalability:** Dataflows are suitable for handling large volumes of data and complex data preparation scenarios. Dataflows can be optimized for performance and can be scheduled to refresh data at regular intervals.
+   - **Integration with Power Platform:** When working with other Power Platform services like Power Apps and Power Automate, using dataflows can facilitate data integration across these services through the Common Data Model.