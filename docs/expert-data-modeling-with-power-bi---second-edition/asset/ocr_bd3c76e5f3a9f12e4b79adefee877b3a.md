calculated columns or calculated tables in a Datamart. We also cannot define aggregations or define Object-level security.

+   A replacement for or an alternative to data warehouses. As discussed earlier, a datamart is a business-centric subset of a data warehouse that does not contain the full history of data. Indeed, data warehouses are the best candidates to be used as a source for a Power BI Datamart; therefore, they are not meant to replace data warehouses.

Lastly, the Datamart Editor, which we will cover next, is not a replacement for Power BI Desktop. While we can implement data preparation and data model in a Datamart, this capability is not mature enough to replace the need to develop solutions in Power BI Desktop. At the time of writing this book, there are so many restrictions, especially for data modeling, making the Datamart capability far behind Power BI Desktop. Besides, this capability is a Premium feature, while Power BI Desktop is free. Moreover, Power BI Desktop supports much more data sources than Datamarts. Therefore, they are not replacing Power BI Desktop.

Now that we know what Power BI Datamarts are and what they are not, let us get familiar with the Datamart Editor, the development tool in the Power BI Service.

# The Datamart Editor

The **Datamart Editor** is a visual designer tool to implement Power BI Datamarts in the Power BI Service. The Datamart Editor provides data preparation with Power Query as well as data modeling capabilities. However, it does not include reporting capabilities. So report writers must create reports outside of the Datamart Editor. The Datamart Editor also provides the ad-hoc querying capability to analyze the data in two flavors: **Visual query** and **SQL query**. While the Datamart Editor is similar to Power BI Desktop, it has a long way to go to get to the point that Power BI Desktop is at today. The following image shows the current version of the Datamart Editor: