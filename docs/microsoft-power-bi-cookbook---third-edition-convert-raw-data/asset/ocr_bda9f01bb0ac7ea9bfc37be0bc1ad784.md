2. Use Search in the upper-right corner to find the certified (the blue star/checkmark icon) Bullet Chart visualization, click the Bullet Chart visual, and then click the Add button.

![](bbox=[0.19, 0.1, 0.9, 0.32])

Figure 9.6: Adding a custom visual from AppSource

3. Click the OK button.

4. Create the following measures:

```
Sales 2011 = SUMX(FILTER('Internet Sales',YEAR([OrderDate])=2011),[SalesAmount])
Sales 2012 = SUMX(FILTER('Internet Sales',YEAR([OrderDate])=2012),[SalesAmount])
Sales 2012 Improve = [Sales 2011] * .7
Sales 2012 Sat = [Sales 2011] * .8
Sales 2012 Good = [Sales 2011] * .9
Sales 2012 VG = [Sales 2011]
```

5. Select the Bullet Chart icon in the Visualizations pane to add it to the report canvas.

6. Configure the visual as follows:

+   - Category: SalesTerritoryCountry column
+   - Value: Sales 2012 measure
+   - Needs Improvement: Sales 2012 Improve measure
+   - Satisfactory: Sales 2012 Sat measure
+   - Good: Sales 2012 Good measure
+   - Very Good: Sales 2012 VG measure