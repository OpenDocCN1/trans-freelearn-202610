In this exercise, we will draw a square around Null Island:

+   1. Recreate the following dataset in Excel:

| | A | B | C |
|---|---|---|---|
| 1 | Point | Latitude | Longitude |
| 2 | 0 | -1 | -1 |
| 3 | 1 | -1 | 1 |
| 4 | 2 | 1 | 1 |
| 5 | 3 | 1 | -1 |
| 6 | 4 | -1 | -1 |

Table 8.1: Sample dataset

+   2. Copy and paste the dataset into Tableau. By doing so, a new data source called Clipboard_... will appear

![](bbox=[0.088, 0.442, 0.865, 0.662])

Figure 8.1: Null Island coordinates

+   3. Remove all fields from the worksheet

+   4. Convert **Point** to a dimension. This can be accomplished by either right-clicking on **Point** and selecting **Convert to Dimension**, or by dragging it to the dimensions portion of the **Data** pane.

+   5. Double-click on **Latitude** and **Longitude**. It doesn't matter in which order; Tableau will automatically place **Longitude** on the **Columns** shelf and **Latitude** on the **Rows** shelf.