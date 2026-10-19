Let's focus on validating the business requirements that were put forth by **Electroniz** at the start of the project:

+   1. First, Electroniz aimed for a near-real-time sales analytics platform that can serve their customer needs faster and better.

You may recall that **Electroniz** sells products both in-store and online. While building the bronze layer, we ingested sales data from both sources. To provide near-real-time analytics that includes both **in-store** and **online** sales data, we aggregated the sales data into one common view named **orders**. This view was created as part of the **create_silver_views** stored procedure, which is invoked using the **Create Silver Layer Views** activity in **electroniz_aggregation_pipeline**:

```
EXEC sp_executesql N'DROP VIEW IF EXISTS store_orders'
SET @sqlcmd = N'CREATE OR ALTER VIEW store_orders AS
SELECT * FROM openrowset(
BULK '" + @silverns + '/sales/store_orders/"', data_source = '"trainingds"', FORMAT = '"delta"')
AS rows'
EXEC sp_executesql @sqlcmd

EXEC sp_executesql N'DROP VIEW IF EXISTS ecomm_orders'
SET @sqlcmd = N'CREATE OR ALTER VIEW ecomm_orders AS
SELECT order_number, ...
FROM openrowset(
BULK '" + @silverns + '/sales/store_orders/"', data_source = '"trainingds"', FORMAT = '"delta"')
WITH ( order_number INT, product_name NVARCHAR(MAX), order_date DATE, order_mode NVARCHAR(MAX), email NVARCHAR(MAX), sale_price FLOAT, sale_price_usd FLOAT, updated_at DATETIME )
) AS rows'
EXEC sp_executesql @sqlcmd

EXEC sp_executesql N'DROP VIEW IF EXISTS orders'
SET @sqlcmd = N'CREATE OR ALTER VIEW orders AS
SELECT order_number, product_name, order_date, units, sale_price, order_mode, sale_price_usd, dbo.store_orders.updated_at
FROM dbo.store_orders
JOIN dbo.customers ON dbo.customers.customer_id = dbo.store_orders.customer_id
JOIN dbo.products ON dbo.products.product_id = dbo.store_orders.product_id
UNION ALL
SELECT order_number, email, product_name, order_date, 1 AS units, sale_price, order_mode, sale_price_usd, updated_at
FROM dbo.ecomm_orders'
EXEC sp_executesql @sqlcmd
```

Figure 8.39 – SQL for creating the orders view

The same view is also used to aggregate sales data by quarters and published as **ext_aggregated_sales**. This table is created as part of the **create_gold_tables** stored procedure, which is invoked using the **Create Gold Layer Tables** activity in **electroniz_aggregation_pipeline**:

```
SET @sqlcmd = N'CREATE EXTERNAL TABLE ext_aggregated_sales
WITH (
LOCATION = '" + @goldns + '/external/ext_aggregated_sales/"',
DATA_SOURCE = '" + @extds + '"',
FILE_FORMAT = '" + @format + '"'
)
AS SELECT YEAR(order_date) AS year, DATEPART(QUARTER, order_date) as quarter,
round(sum(sale_price_usd),2) as aggregated_sales_price
FROM orders
GROUP BY YEAR(order_date), DATEPART(QUARTER, order_date) '
EXEC sp_executesql @sqlcmd
```

Figure 8.40 – SQL for the ext_aggregated_sales external table