![](bbox=[0.073, 0.073, 0.865, 0.52])

Figure 7.49: Delay vs. Benchmark

Next, we will create sheet 2: KPI Tiles (LOD Metrics Summary):

+   1. Open a new worksheet.
+   2. Drag **Origin** to **Rows**.
+   3. Drag **YEAR(Flight Date)** to **Columns**. Also drag **AVG(Arr Delay)** and **Arr Delay without Origin = { EXCLUDE [Origin]: AVG([Arr Delay]) }** to **Columns**.
+   4. Make the two measures dual axis and synchronize them also.
+   5. Select **Bar** in the marks card for the **Arr Delay** and **Line** for the other measure.
+   6. Place **YEAR(Flight Date)** to the Filter shelf and keep 2018, 2019, and 2020. Now you've got a summary panel powered by LODs.