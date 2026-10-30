| ROLE ID (CHILD NODE) | ROLE NAME | PARENT ROLE ID | ROLES | ROLE PATH |
|---|---|---|---|---|
| 1 | Salesperson 1 | 10 | 10, 20, 30 | / 10/20/30 |
| 2 | Salesperson 2 | 10 | 10, 20, 30 | / 10/20/30 |
| 3 | Salesperson 3 | 11 | 11, 20, 30 | / 11/20/30 |
| 10 | Regional Manager 1 | 20 | 20, 30 | / 20/30 |
| 11 | Regional Manager 2 | 20 | 20, 30 | / 20/30 |
| 20 | Vice President 1 | 30 | 30 | / 30 |
| 21 | Vice President 2 | 30 | 30 | / 30 |
| 30 | CEO | Not applicable | Not applicable | Not applicable |

Figure 4.11 – An example of a flattened dataset showing how ancestors are stored

More details on how the flatten transformation is used will follow in Chapter 5, Advanced ETL Using TCRM Data Prep.

# Edit attributes

Use this transformation to change the column names and value formats. For example, you can edit the following:

+   - **Label**
+   - **API Name**
+   - **Length**
+   - **Precision**
+   - **Scale**

# Drop columns

Drop unwanted input columns from the recipe. This will leave you with a subset of columns to use in transformations and include in the final dataset. For example, you can drop input columns used for a calculated column.

# Detect sentiment

This transformation is used to determine the sentiment of text, whether positive, negative, or neutral, and adds those values to the specified column.

If Einstein cannot determine the sentiment of a field, the sentiment is set to null.