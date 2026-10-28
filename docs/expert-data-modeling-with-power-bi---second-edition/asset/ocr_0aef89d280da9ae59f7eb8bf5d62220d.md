```
DimDate[DateKey]
    )
```

Follow these steps:

+   1. From the **Optimize** tab on the ribbon, click **Performance Analyzer**.
+   2. On the **Performance Analyzer** pane, click the **Start recording** button.
+   3. Put a **Card** visual on the report canvas and use the **Internet Sales** measure.
+   4. This adds a **Card** section in the **Performance Analyzer** pane. Expand the **Card** section.

The following image shows the preceding steps:

![](bbox=[0.177, 0.431, 0.822, 0.68])

Figure 10.20: Performance Analyzer’s results for the Internet Sales Card visual

As the preceding image shows, the results do not include a **Direct query** section, which indicates that the underlying query hit the aggregation table.

Let us add another **Card** visual, use the **Internet Sales Due** measure, and look at the results in Performance Analyzer again. The following image shows the results: