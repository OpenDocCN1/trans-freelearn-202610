```
'Populated Values' END
WHEN 7 THEN IF ISNULL ([Happiness Score]) THEN 'Null Values' ELSE
'Populated Values' END
WHEN 8 THEN IF ISNULL ([Health (Life Expectancy)]) THEN 'Null Values' ELSE
'Populated Values' END
WHEN 9 THEN IF ISNULL ([Standard Error]) THEN 'Null Values' ELSE
'Populated Values' END
END
```

+   9. Remove Region and Country from the Rows shelf.

+   10. Place Null & Populated on the Rows and Colour shelves and % Populate on the Columns and Label shelves:

![](bbox=[0.2, 0.31, 0.8, 0.46])

Figure 2.4: Populated values

+   11. Change the colors to red for Null Values and green for Populated Values if desired. You can do so by clicking on Colour in the Marks card and then Edit Colours.

+   12. Click on the arrow in the upper-right corner of the Select Field parameter on your sheet and select Single Value List.

+   13. Select various choices in the Select Field parameter and note that some fields have a high percentage of null values. For example, in the following diagram, 32.98% of records do not have a value for Region:

![](bbox=[0.2, 0.64, 0.8, 0.8])

Figure 2.5: Comparing null and populated values

Building on this exercise, let’s explore how we might clean and extrapolate data from existing data using the same dataset.