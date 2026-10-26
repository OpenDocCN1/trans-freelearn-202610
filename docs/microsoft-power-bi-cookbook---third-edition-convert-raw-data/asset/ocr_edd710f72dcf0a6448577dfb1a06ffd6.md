# Power Query

Get data

# Connect to data source

# SQL Server database

Database

Learn more

# Connection settings

Server *

localhost

Database

AdventureWorksDW2022

> Advanced options

# Connection credentials

Connection

Create new connection

Connection name

localhost\AdventureWorksDW2022

Data gateway

[On-premises] FabricGateway

Authentication kind

Windows

Username

[redacted]

Password

••••••••••••

Figure 2.25: Data source connection information

+   5. On the Choose data page, mark the checkboxes next to FactInternetSales, DimCustomer, DimDate, and DimProduct, and then click the Transform data button.

+   6. A web-based version of Power Query Editor is presented. Here, you can transform your data exactly like using Power Query Editor in Power BI Desktop. In our case, we can just click the Save & close button, as no data transformations are necessary.

+   7. In the Save your dataflow dialog, enter a name for your dataflow, such as AdventureWorksDW2022 Internet Sales, and click the Save button.

+   8. In the upper-right corner, a notification appears. Click the Refresh now button to retrieve the data from SQL Server.

Once refreshed, the dataflow can be used within Power BI Desktop. To do so, open Power BI Desktop, click Get data in the ribbon on the Home tab, and choose Dataflows. The created dataflow and associated tables appear in the workspace within the Navigator dialog. You can select the desired tables and click the Load button.