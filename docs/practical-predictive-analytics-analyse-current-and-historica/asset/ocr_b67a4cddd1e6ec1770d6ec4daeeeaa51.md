# Reading the Stop and Frisk tableÂ

The first code chunk will read the StopFrisk dataframe, similar to how it was performed earlier in this section using spark.sql. Observe that the syntax for sql using Python is very similar to what we have been using with R.

Within the SQL call, the outcome variable frisked is mapped to a binary variable using a CASE statement. The reason for doing this is that the MLlib algorithm handles integer data much better than character data. If using character data, it often needs to be mapped to an integer or a labeled point.

The resulting dataframe (df2) is then displayed using the show(5) function, which is the Python equivalent to the R head(df2, 5) function:

```
%python
from pyspark.mllib.tree import DecisionTree, DecisionTreeModel
from pyspark.mllib.util import MLUtils
import pyspark.mllib
import pyspark.mllib.regression
from pyspark.mllib.regression import LabeledPoint
from pyspark.sql.functions import *
from pyspark.ml.feature import StringIndexer
df2 = spark.sql("SELECT case when frisked='Y' THEN 1 else 0 END a
df2.show(5)
```

|frisked|sex|race|age|weight|
|---|---|---|---|---|
| 1| 0| W| 33| 190|
| 1| 0| B| 14| 140|
| 0| 0| B| 14| 140|
| 0| 0| B| 14| 180|
| 0| 0| B| 13| 160|