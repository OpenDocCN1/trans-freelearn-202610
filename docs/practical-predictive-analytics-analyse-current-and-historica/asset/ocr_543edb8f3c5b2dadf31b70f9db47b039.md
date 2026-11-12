# Performing some exploratory analysis on positives

Before we move on to exploring the entire Spark dataframe, we can look at some of the data already generated for positive cases. As you may recall from the prior chapter, this is stored in the Spark dataframe out_sdl.

We have generated some random sample bins specifically so that we can do some exploratory analysis.

We can use the filter command to extract random sample 1, and take the first 1,000 records:

+   - The `filter` is a SparkR command that allows you to subset a Spark dataframe
+   - The `display` command is a databricks command that is equivalent to the `View` command we have previously used and you can also use the `head` function as well to limit the number of rows that are displayed:

This code chunk extracts 1000 records from the positives and displays them:

```
small_pos <- head(SparkR::filter (out_sdl, out_sdl$sample_b
nrow (small_pos)
display (small_pos)
```

![](bbox=[0.125, 0.705, 0.88, 0.85])

The data appears in tabular form, and you can scroll up/down, or left/right to