```
In [10]: type(mpg)
Out[10]: pandas.core.series.Series

In [11]: df_wtVsMpg = pandas.DataFrame()

In [12]: df_wtVsMpg['wt'] = wt

In [13]: df_wtVsMpg['mpg'] = mpg

In [14]: p = ggplot2.ggplot(df_wtVsMpg)

In [15]: type(p)
Out[15]: rpy2.robjects.lib.ggplot2.GGPlot

In [16]: p.plot()
```

code6.py

+   - **Lines 2-3:** These lines import and activate the new method of moving data to/from IPython/R. This is the preferred method since v0.16.0, at which point the `pandas.rpy` interface was deprecated and marked for removal in some future version.
+   - **Line 7:** This line actually imports R's `mtcars` data frame into a panda DataFrame.
+   - **Lines 8-10:** These lines illustrate the DataFrame's ability to be addressed as a Python dict and the type of the result.
+   - **Lines 11-13:** Here, we start out with an empty DataFrame and add columns dynamically.