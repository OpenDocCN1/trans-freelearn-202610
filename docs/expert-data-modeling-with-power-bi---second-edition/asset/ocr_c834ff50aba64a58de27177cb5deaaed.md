![](bbox=[0.177, 0.091, 0.822, 0.226])

Figure 8.3: The table description shows up in the Report view when hovering over a table

We can also use the description for documentation purposes.

+   - **Synonyms:** Adding synonyms helps explicitly with Power BI’s **Q&A** feature. The users usually use various terms to refer to the same thing. For instance, we have a table in the model named **Sales Demographic**. However, users may refer to it as **geography** or **geo**. So, we can add those phrases to the Synonyms box to help Q&A identify the Sales Demographic table if the user uses any of those phrases.
+   - **Row label:** This setting affects the data model in two ways, as follows:
+     1. **Q&A:** This helps Q&A create more helpful visuals. A **Row label** defines which column best describes a single row of data. For instance, in the **Product** table, the row label is usually the **Product** (or **Product Name**) column. Therefore, Q&A treats the **Product** as a column instead of a table when the user asks for **Sales by product**. The following image shows the results of asking the **Sale by product as clustered column chart** question from Q&A: