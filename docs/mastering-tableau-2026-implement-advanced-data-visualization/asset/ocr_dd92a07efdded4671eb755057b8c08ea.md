```
CASE [Choose Excluded Dims 2]
WHEN 1 THEN SUM({EXCLUDE [1st Dim] : SUM([Sales])})
WHEN 2 THEN SUM({EXCLUDE [1st Dim],[2nd Dim] : SUM([Sales])})
WHEN 3 THEN SUM({EXCLUDE [1st Dim],[2nd Dim],[3rd Dim] : SUM( : SUM(
WHEN 4 THEN SUM({EXCLUDE [1st Dim],[2nd Dim],[3rd Dim], [4th
WHEN 5 THEN SUM({EXCLUDE : SUM([Sales])})
WHEN 6 THEN 0
END
```

# Figure 7.7: Case Exclude 2

The Case Exclude 2 calculation shows that if the user selects 1 in the Choose Excluded Dims 2 parameter (which will be displayed as 1st), 1st Dim will be excluded from the calculation in the view. That means that SUM(Sales) won't be partitioned over 1st Dim, even if it is part of the view.

+   8. Now, right-click and open the context filter on Product Name for inspection. In the resulting dialog box, click the Wildcard tab and note the settings, as shown in the following screenshot. The filter has been set to show only product names that start with S (Match value is not case-sensitive.):

![](bbox=[0.256, 0.505, 0.744, 0.785])

# Figure 7.8: Wildcard filter

+   9. After clicking OK, right-click on the filter and click Add to Context. Note that context filters are denoted in gray: