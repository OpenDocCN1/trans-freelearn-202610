| ROLE ID (SELF ID) | ROLE NAME | PARENT ROLE ID | ROLES | ROLEPATH |
|---|---|---|---|---|
| 1 | Salesperson 1 | 10 | 10, 20, 30 | \\10\20\30 |
| 2 | Salesperson 2 | 10 | 10, 20, 30 | \\10\20\30 |
| 3 | Salesperson 3 | 11 | 11, 20, 30 | \\11\20\30 |
| 10 | Regional Manager 1 | 20 | 20, 30 | \\20\30 |
| 11 | Regional Manager 2 | 20 | 20, 30 | \\20\30 |
| 20 | Vice President 1 | 30 | 30 | \\30 |
| 21 | Vice President 2 | 30 | 30 | \\30 |
| 30 | CEO | Not applicable | Not applicable | Not applicable |

Figure 5.24 – Flatten transformation table

How is this facilitated by the **Flatten Transformation** node? The `UserRole` object in Salesforce holds all the information on role hierarchy, and this object is used in **Flatten Transformation** as follows:

+   1. Select **Add Input Data** in the **Recipe**.
+   2. Select the `UserRole` object.
+   3. Add a **Transformation** node.
+   4. Select the **Role ID** column.
+   5. Select **Flatten**.

The **Transformation** node looks like this:

![](bbox=[0.116, 0.584, 0.848, 0.751])

Figure 5.25 – The Transformation node in TCRM, prior to configuration