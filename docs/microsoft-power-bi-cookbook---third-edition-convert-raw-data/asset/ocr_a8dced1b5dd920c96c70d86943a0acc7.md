# Getting ready

To prepare for this recipe, follow this step:

+   - Open the `Controlling Visual Interactions.pbix` file and save it as `Utilizing Graphical Visualizations.pbix`.

# How to utilize graphical visualizations

To implement this recipe, use the following steps:

+   1. Select the Europe page.
+   2. From the **Visualizations** pane, select the **Scatter chart** visualization and add the `Sales Territory Country` column from the `Sales Territories` table as the **Legend**.
+   3. From the `Internet Sales` table, add the `Internet Net Sales` measure as the **X-Axis**, the `Internet Customer Count` measure as the **Y-Axis**, and the `Internet Order Quantity Total` measure as **Size**.
+   4. From the `Dates` table, add the `Date` column (*not* a **Date** hierarchy) to **Play Axis**.
+   5. In the **General** sub-pane of the **Format** pane under **Properties**, set **Height** to 381 and **Width** to 594. Then, under **Position**, set **Horizontal** to 160 and **Vertical** to 295.
+   6. Expand the **Title** format card and change the text to `Internet Sales and Customer Count Trend by Country`, and set **Horizontal alignment** to **Center**.
+   7. Switch to the **Visual** sub-pane and set **Legend** to **Off**. Then, expand the **Y-axis** section and under **Title**, change **Title text** to `Distinct Customers`.

![](bbox=[0.195, 0.532, 0.901, 0.863])

*Figure 4.26: Scatter chart visualization*