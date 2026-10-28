Figure 8.24: Count of the ProductKey column by setting its Summarize by property

> **Is nullable:** We can set this property for the columns that are not supposed to have any null values, such as our primary keys. However, bear in mind that if we toggle this property to **No**, then if, in the future, there is a null value in the column, we get an error message while refreshing the data.

# Hierarchies

Hierarchies are abstract objects in our model that show how different columns relate to each other from a hierarchical viewpoint. We can create hierarchies in the data model from the **Report** view, the **Data** view, or the **Model** view. To create a **Calendar hierarchy** in the **Date** table from the **Model** view, follow these steps:

+   1. Right-click the **Year** column.
+   2. Select the **Create hierarchy** option from the context menu.
+   3. From the **Properties** pane, change the **Name** value of the hierarchy to **Calendar Hierarchy**.
+   4. Select the **Month** and **Date** columns from the dropdown list from the **Hierarchy** section.
+   5. Click the **Apply Level Changes** button.

The following figure shows the preceding steps: