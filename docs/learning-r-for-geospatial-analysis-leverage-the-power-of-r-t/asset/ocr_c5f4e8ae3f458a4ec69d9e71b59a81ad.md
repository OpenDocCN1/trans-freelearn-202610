+   - Second, we need to split the variable column into two: the variable itself (`mmnt` or `mmxt`) and the year (2006, 2007, 2008, 2009, or 2010). Substring extraction with `substr` (see *Chapter 2, Working with Vectors and Time Series*, for more information) comes in handy for this purpose since we can see that characters 1-4 in each element of `spainT$variable` consistently correspond to the variable, and characters 6-9 correspond to the year (to be sure, there are other ways to extract substrings in less convenient scenarios; for instance, the `strsplit` function can be used in the present context). Using the following two expressions, we first create a new (numeric) column holding the year values, and then modify the `variable` column to retain just the variable names:

```
spainT$year = as.numeric(substr(spainT$variable, 6, 9))
spainT$variable = substr(spainT$variable, 1, 4)
head(spainT)
```

|   | x        | y        | variable | value     | year |
|---|----------|----------|----------|-----------|------|
| 1 | 103242.08 | 4857128  | mmnt     | 0.7383187 | 2006 |
| 2 | 106842.08 | 4857128  | mmnt     | 0.7392765 | 2006 |
| 3 | 121242.08 | 4857128  | mmnt     | 0.7458803 | 2006 |
| 4 | 124842.08 | 4857128  | mmnt     | 0.7456079 | 2006 |
| 5 | 128442.08 | 4857128  | mmnt     | 0.7454912 | 2006 |
| 6 | 92442.08  | 4853528  | mmnt     | 0.7061661 | 2006 |

The final `data.frame` object is now complete.

# Summary

To summarize, in this chapter you learned how to interpolate point data in space to produce continuous rasters using four different methods. The first of these, nearest-neighbor interpolation, we coded ourselves, while the other three (IDW, OK, and UK) were applied using functions in the gstat and automap packages. You also learned how to perform LOOCV to calculate RMSE and assess prediction accuracy. Finally, we saw two examples of how to automate a complex procedure to perform it repeatedly over an array of parameter sets.

In the next chapter, you are going to learn how to produce more elaborate plots involving spatial data in R with ggplot2.