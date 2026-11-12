This will produce the following visualization:

| Category | Ship Mode | |
|---|---|---|
| Furniture | First Class | 1 |
| | Same Day | 1 |
| | Second Class | 1 |
| | Standard Class | 1 |
| Office Supplies | First Class | 2 |
| | Same Day | 2 |
| | Second Class | 2 |
| | Standard Class | 2 |
| Technology | First Class | 3 |
| | Same Day | 3 |
| | Second Class | 3 |
| | Standard Class | 3 |

Figure 5.27: Example 7

We count the **Ship Mode** and **Category** combinations at the deepest level, but we restart at every ship mode; therefore, we count **First Class** and **Furniture**, **First Class** and **Office Supplies**, and then **First Class** and **Technology**. Then, we move on to **Same Day** and its combinations, restarting at 1 again.

In the eighth example, configure the partitioning and addressing settings as follows:

+   - **Partitioning:** -
+   - **Addressing:** Ship Mode, Category
+   - **At the level:** Ship Mode
+   - **Restarting every:** None