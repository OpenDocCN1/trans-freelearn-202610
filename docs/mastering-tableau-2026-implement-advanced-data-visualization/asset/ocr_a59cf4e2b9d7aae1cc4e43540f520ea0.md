5. Drag in:

+   - Top left and middle: Sheet 1: Delay Over Time vs Benchmark
+   - Bottom left and middle: Sheet 3: Arrival Delay by Airline
+   - Right top to bottom: Sheet 2: Delay by Origin
+   - Arrange them using vertical and horizontal containers if you like a clean layout

6. For the final polish, add the following titles:

+   - Dashboard: Flight Delays (LOD Dashboard)
+   - Edit colors and fonts according to your liking;

![](bbox=[0.09, 0.27, 0.86, 0.605])

Figure 7.52: Final Dashboard

And now you've got your LOD-driven Tableau dashboard

# Summary

We began this chapter by exploring why LODs are so impactful and why their inclusion in Tableau was so lauded. Next, we built two playgrounds to explore how the three LODs—FIXED, EXCLUDE and INCLUDE—work. Tableau's default is to base calculations on the dimensions visible in the view. For example, if you have states in your view, the sales amount will be presented by state. If you are adding cities, the sales amount will be adjusted by state, by city. But, if you want to manipulate this default logic, you can use LODs. They allow you to calculate measures based on any dimension, no matter whether that dimension is represented in the view or not. We also saw that FIXED LODs are higher in the order of operations in Tableau than EXCLUDE and INCLUDE LODs. This is important to remember so that you use the correct LOD and/or filter in your dashboard.

In the next chapter, we'll turn our attention to the visual side of Tableau and explore different chart types and less common but very useful visualizations.