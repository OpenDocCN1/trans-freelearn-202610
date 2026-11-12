# Missing Completely at Random (MCAR)

Some percentage of missing values will always occur naturally. When this happens, the missing data is known as Missing Completely at Random (MCAR). An example of this is if 2% of a survey was not recorded due to a glitch in a survey response system. If this were the case, one could assume that it was not related to any other variable in the data. MCAR variables are also not correlated with its own data values, that is it would also not be related to any other of the non-missing values that appear in the data.

# Testing for MCAR

When you suspect that your data is MCAR, there are various statistical tests can help you determine if MCAR may be occurring. One important test is the Little test, which we will now run on the test missing value dataset.

First, install the BaylorEdPsych package, which contains the LittleMCAR test:

```
try (require(BaylorEdPsych) || install.packages("BaylorEdPsych", de
library(BaylorEdPsych)
```

Now, run the LittleMCAR test:

```
test_mcar<-LittleMCAR(all.df)
```

Print the missing values found by the test. 4% of the data was found to be missing. This makes perfect sense, since 5% of the 1,000 males and 3% of the 1,000 females were generated with NAs.

```
print(test_mcar$amount.missing)
print(test_mcar$p.value)
```

```
> print(test_mcar$amount.missing)
     Age Gender Education
Number Missing   80.00   0        0
Percent Missing  0.04    0        0
```

Print the value of the test statistic. High values of p imply the data is MCAR, while low values imply there is some pattern. At the .05 significance level, it