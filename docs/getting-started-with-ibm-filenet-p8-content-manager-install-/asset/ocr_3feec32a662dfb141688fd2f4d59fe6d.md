# Create a new database

Select the **Databases** node under instance db2inst1, right-click, and select **Create Database**. We suggest you create your database with **automatic maintenance**. You (or the enterprise database administrator) might make a different choice for a large production site, but automatic maintenance is simplest for this all-in-one system. This will be the database for the CE’s Global Configuration Database (GCD). You can give it any name you want, but GCD is logical and memorable. You can accept the defaults for any questions as you work through the wizard. A new entry GCD will appear under the **Databases** node.

![](bbox=[0.188, 0.313, 0.781, 0.609])

# Drop default user tablespace

If there is a default user tablespace, you can drop it since its configuration will not be suitable for our use here. Expand the GCD node and select the **Table Spaces** folder. In the main panel, select **USERSPACE1**, right-click, and select **Drop**.

# Create a buffer pool definition

All tablespaces must use a 32 KB page size (this is a CE requirement). The defaults in DB2 9.5 and 9.7 are 4 KB. That means you will have to create a new buffer pool definition. Select the **Buffer Pools** node under GCD, right-click, and select **Create**. Make sure the new buffer pool has a **Page size** of 32 and give it a meaningful name. We call ours **WJC32K**.