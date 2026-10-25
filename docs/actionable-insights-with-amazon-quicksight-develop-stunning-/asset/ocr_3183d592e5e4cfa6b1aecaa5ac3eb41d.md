You will then see the list of columns in your dataset. You will notice that by default, users and groups with access to the dataset have access to all columns. To restrict a column, you will need to select it and then add the users or groups you want to enable access for. This selection will override the "everyone" default setting and only the users or groups you add will have access. Once completed, you will notice a padlock icon next to your dataset to confirm that security restrictions are present.

# ## Adding row-level security

QuickSight supports row-level security for your datasets. Row filters are particularly useful when you want certain users or groups to have different levels of access to a subset of data depending on their username or group membership. To apply row-level security you will need to provide a permissions file or create a permissions table in your database and then add it to QuickSight. In our example, the `vendorid` column has two distinct values, `2` and `1`, each representing a different vendor. Let’s imagine a scenario where users from vendor `1` need to have access to the dataset, but they should only be able to access the vendor `1` data and not data from vendor `2`. Likewise, vendor `2` should only be allowed to view their data. For this example, we can apply row-based security controls.

# ## Applying a row-level filter

For our example, we can consider two different users, `user1` for vendor `1` and `user2` for vendor `2`. QuickSight will match the username accessing the dataset with the value of the `UserName` field and then apply the filter or filters using the remaining column. In our example, it will use the `vendorid` column from the `yellowtrips_3mo` dataset.

See our following mapping table for the scenario described previously:

| UserName | vendorid |
|---|---|
| user1 | 1 |
| user2 | 2 |

Figure 3.20 – Mapping table