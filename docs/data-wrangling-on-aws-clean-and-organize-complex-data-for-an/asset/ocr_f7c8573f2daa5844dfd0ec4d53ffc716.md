The first step of the data wrangling process is data discovery. This is one of the most important steps of data wrangling. In data discovery, we familiarize ourselves with the kind of data we have as raw data, what use case we are looking to solve with that data, what kind of relationships exist between the raw data, what the data format will look like, such as CSV or Parquet, what kind of tools are available for storing, transforming, and querying this data, and how we wish to organize this data, such as by folder structure, file size, partitions, and so on to make it easy to access.

Let's understand this by looking at an example.

In this example, we will try to understand how data discovery varies based on the persona. Let's assume we have two colleagues, James and Jean. James is a data engineer while Jean is a data analyst, and they both work for a car-selling company.

Jean is new to the organization and she is required to analyze car sales numbers for Southern California. She has reached out to James and asked him for data from the sales table from the production system.

Here is the data discovery process for Jane (a data analyst):

+   1. Jane has to identify the data she needs to generate the sales report (for example, sales transaction data, vehicle details data, customer data, and so on).
+   2. Jane has to find where the sales data resides (a database, file share, CRM, and so on).
+   3. Jane has to identify how much data she needs (from the last 12 months, the last month, and so on).
+   4. Jane has to identify what kind of tool she is going to use (Amazon QuickSight, Power BI, and so on).
+   5. Jane has to identify the format she needs the data to be in so that it works with the tools she has.
+   6. Jane has to identify where she is looking to store this data – in a data lake (Amazon S3), on her desktop, a file share, and sandbox environment, and so on.

Here is the data discovery process for James (a data engineer):

+   1. Which system has requested data? For example, Amazon RDS, Salesforce CRM, Production SFTP location, and so on.
+   2. How will the data be extracted? For example, using services such as Amazon DMS or AWS Glue or writing a script.
+   3. What will the schedule look like? Daily, weekly, or monthly?
+   4. What will the file format look like? For example, CSV, Parquet, orc, and so on.
+   5. How will the data be stored in the provided store?