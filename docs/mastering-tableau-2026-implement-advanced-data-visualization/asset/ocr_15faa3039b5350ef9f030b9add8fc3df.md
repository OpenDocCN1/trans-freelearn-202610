**Note**
Remember, addressing means selecting the dimensions in the table calculation settings, while partitioning is the de-selection of dimensions, as described in Figure 5.17.

In the first example, configure the partitioning and addressing settings as follows:

+   • Partitioning: Category, Region
+   • Addressing: Ship Mode
+   • At the level: -
+   • Restarting every: -

This will produce the following visualization:

| Category | Region | First Class | Second Class | Standard Class |
| :--- | :--- | :--- | :--- | :--- |
| Furniture | East | 1 | 2 | 3 |
| Furniture | South | 1 | 2 | 3 |
| Furniture | West | 1 | 2 | 3 |
| Office Supplies | East | 1 | 2 | 3 |
| Office Supplies | South | 1 | 2 | 3 |
| Office Supplies | West | 1 | 2 | 3 |
| Technology | East | 1 | 2 | 3 |
| Technology | South | 1 | 2 | 3 |
| Technology | West | 1 | 2 | 3 |

Figure 5.31: Example 1

Here, we count the different ship modes per Category and Region combinations.

In the second example, configure the partitioning and addressing settings as follows:

+   • Partitioning: Category, Ship Mode
+   • Addressing: Region
+   • At the level: -
+   • Restarting every: -