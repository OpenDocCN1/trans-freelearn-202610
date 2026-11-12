# Another OneR example

This example uses the much larger diabetes dataset. Since most of the variables in this dataset are numeric, OneR can bin all of them:

+   1. First, read the Spark diabetes table using SQL, which has already been registered in a previous chapter.
+   2. Collect a 15% random sample of the data and assign it to an R (not Spark!) dataframe named "local".
+   3. Bin all of the available variables based upon their ability to predict the outcome and assign it to an R dataframe named "data":

```
library(OneR)
df = sql("SELECT outcome, age, mass, triceps, pregnant,
glucose, pressure, insulin, pedigree
FROM global_temp.df_view")

local = collect(sample(df, F, .15))

data <- optbin(local,outcome~.)
summary(data)
```

| age | mass | triceps | pregnant | glucose | pressure | insulin | pedigree | outcome |
|---|---|---|---|---|---|---|---|---|
| (-1.54,37.1]:118790 | (12.4,35]:121754 | (0.703,32.8]:128435 | (-5.29,4.91]:117506 | (38.9,139]:139019 | (35.3,75.6]:114164 | (-149,205]:131628 | (-0.548,0.529]:111914 | 0:121724 |
| (37.1,74] : 67698 | (35,54.2]: 64734 | (32.8,58.3] : 58053 | (4.91,13.8 ] : 68982 | (139,220] : 47469 | (75.6,112] : 72324 | (205,447] : 54860 | (0.529,1.69] : 74574 | 1: 64764 |

4. Run the OneR model using all of the variables to predict the outcome. Recall that the outcome is an indication of whether or not diabetes is present: