![](bbox=[0.226, 0.078, 0.88, 0.475])

| Year of Ord.. | Central | East | South | West |
| :--- | :--- | :--- | :--- | :--- |
| 2022 | 466 | 560 | 349 | 676 |
| 2023 | 476 | 664 | 340 | 650 |
| 2024 | 609 | 797 | 413 | 815 |
| 2025 | 784 | 965 | 518 | 1,112 |

Figure 1.41: Dimensions and Measures

As shown in Figure 1.41, step 3 brings meaning. Placing Region and Order Date on the Columns and Rows shelves provides context, which imparts meaning to the visualization.

# Row-level, aggregate-level, and table-level calculations

There are three levels of calculations in Tableau: row, aggregate, and table. To understand how these three levels function, it’s important to understand the Tableau processes. We’ll do so with an example that considers the Number of Records and Quantity fields from the Orders dataset.

In earlier versions of Tableau, Number of Records was a field created automatically. This is not the case anymore; therefore, please create a calculated field called Number of Records by selecting Analysis | Create Calculated Field and typing 1 inside it: