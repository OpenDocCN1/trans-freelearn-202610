The following is a list of several important findings that were discovered during those sessions:

+   - There are five data sources that need to be ingested, as follows:
+     - **Sales database**: This database stores data for sales that happen in brick-and-mortar stores throughout the US. The sales database is a **Microsoft SQL server** hosted on **Azure SQL**.
+     - **E-commerce transactions**: **Electroniz** also does the online selling of its products. Every time a transaction occurs on the online store, an event will be streamed to a central resource. The sales data is in a structured format.
+     - **Currency conversion data**: **Electroniz** sells products in varying currencies on the online store. The currency conversion data is required to convert all sales into USD. The conversion data is available using **REST API** calls. The currency conversion data is in a semi-structured format.
+     - **E-commerce website tracking**: All visitors to the **Electroniz** online store are tracked on the web server. This information is instrumental in tracking users by country and helps the advertising team perform targeted advertising. The website log is available as a file that can be downloaded from **Azure Blob storage**. The website tracking data is in an unstructured format.
+     - **Geo-location data**: Geo-location data is necessary to map the IP addresses from the web server logs to the country of origin. This data can be downloaded from an **HTTP** source. The geo-location data is in a structured format.
+   - Transactions in the sales database and online store happen very frequently throughout the day. The lakehouse needs to be kept up to date with a maximum delay of 1 hour.
+   - The consumers of data are data analysts, business intelligence developers, and data scientists. The preferred analytical tool for data analysts and business intelligence groups is **SQL**, whereas data scientists prefer to use **Apache Spark**.
+   - All PII data, including credit cards, addresses, and phone numbers, needs to be masked.
+   - All data needs to be encrypted at rest and in motion.
+   - Using the new lakehouse, the **Electroniz** team is looking for aggregated metrics for sales and inventory. They would like to streamline their shipping operations and the intelligent distribution of advertising budgets based on available metrics.