+   23. Drag **Wage Eur** and **Overall** to Columns.

+   24. In the **Data** pane, click the drop-down arrow and select **Create Calculated Field**. Name the field % of Club Wage. Enter the following formula: SUM([wage_eur]) / TOTAL(SUM([wage_eur])).

+   25. Drag % of Club Wage to Columns.

+   26. Right-click the % of Club Wage pill and choose **Edit Table Calculation...**. Under **Compute Using**, select **Pane (Down)** so the percentages reset for each club. Format the field as a percentage via right-click | **Default Properties** | **Number Format...**

![](bbox=[0.19, 0.23, 0.64, 0.705])

Figure 5.51: Number Format...

This allows you to see how each player's wage compares with their teammates'.

+   27. Click the drop-down arrow in the **Data** pane and select **Create Calculated Field...**. Name the calculation **Overall vs Club Avg**. Enter AVG([overall]) - WINDOW_AVG(AVG([overall])).

+   28. Drag **Overall vs Club Avg** to Columns.