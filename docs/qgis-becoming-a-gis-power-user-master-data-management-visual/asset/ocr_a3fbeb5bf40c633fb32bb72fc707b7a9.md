```
classes[class_value] += 1
else:
    classes[class_value] = 1
print classes
```

The following screenshot shows what the screen looks like:

![](bbox=[0.177, 0.235, 0.822, 0.455])

# How it works...

In the first line, we use `import processing` because it offers a very handy and convenient function, `processing.features()`, to access layer features, which we use in line 7. It is worth noting that, if there is a selection, `processing.features()` will only return an iterator of the selected features.

In line 3, we get the currently active layer object. Line 5 creates the empty dictionary object, which we will fill with the unique values and corresponding counts.

The `for` loop, starting on line 5, loops through all features of the layer. For each feature, we get the value in the classification field (line 7). You can change the column name to analyze other columns. Then, we only need to check whether this value is already present in the `classes` dictionary (line 8) or whether we have to add it (line 11).

# There's more...

If you are using an SQL database, such as Spatialite or PostGIS, you can achieve similar results with a query using the COUNT and GROUP BY functions to count the number of features or records per class:

```
SELECT class, COUNT(*)
FROM poi_name_wake
GROUP BY class
```