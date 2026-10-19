> **Important note**
> Having different data types of columns that join with each other can lead to a serious performance drag during querying. The curation stage is an ideal phase to trace and fix these kinds of issues.

+   - After careful review of the data, it was discovered that the following two orders reference `product_id` rows that do not exist in the `products` table. In other words, they are *orphan* rows.

![](bbox=[0.26, 0.3, 0.7, 0.38])

Figure 7.5 – Orphan rows in store_orders

Before making it available to end users, we should either exclude or fix invalid data.

# Non-uniform data

Non-uniform data exists in varying forms across different datasets. We have found a couple of instances of non-uniform data in the bronze layer:

+   - In the Electroniz `stores` database, the `country` column exists in the `store_customers` table as follows:

![](bbox=[0.4, 0.68, 0.58, 0.78])

Figure 7.6 – Country data in store_customers

Whereas it exists like this in the e-commerce transactions:

![](bbox=[0.42, 0.88, 0.57, 0.97])

Figure 7.7 – Country data in e-commerce transactions