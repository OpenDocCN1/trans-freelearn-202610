This will produce the following visualization:

# Ship Mode

| Category | Region | First Class | Second Class | Standard Class |
|---|---|---|---|---|
| Furniture | East | 1 | 2 | 3 |
| | South | 4 | 5 | 6 |
| | West | 7 | 8 | 9 |
| Office Supplies | East | 1 | 2 | 3 |
| | South | 4 | 5 | 6 |
| | West | 7 | 8 | 9 |
| Technology | East | 1 | 2 | 3 |
| | South | 4 | 5 | 6 |
| | West | 7 | 8 | 9 |

Figure 5.34: Example 4

Here, we count the different Category options per Region and Ship Mode combinations at the deepest level.

In the fifth example, configure the partitioning and addressing settings as follows:

+   * Partitioning: Category
+   * Addressing: Ship Mode, Region
+   * At the level: Deepest
+   * Restarting every: None