
- For example, the first plot displays a boxplot for insulin levels for diabetes=1
- Plot 2 shows a histogram of insulin levels with 20 bins defined
- Plot 3 shows a scatter plot between age and insulin levels
- Plot 4 shows the correlation matrix of age, insulin, and the number of months pregnant:

![](img/672c0cf8ba3c000a6529b117ed96945e_541_0.png)

![](img/672c0cf8ba3c000a6529b117ed96945e_541_1.png)

![](img/672c0cf8ba3c000a6529b117ed96945e_541_2.png)

![](img/672c0cf8ba3c000a6529b117ed96945e_541_3.png)

## Running pairwise correlations directly on a Spark dataframe

Correlation and covariance functions are available which operate directly on Spark dataframes. The following example shows that, for patients with diabetes, there is an 11% correlation between age and glucose level:

```
corr <- corr(out_sd1, "glucose", "age", method = "pearson")
corr
```

- (1) Spark Jobs

[1] 0.1130607

## Cleaning up and caching the table in memory

Since Spark excels at processing in-memory data, we will first remove our intermediary data and then cache our out_sd dataframe, so that subsequent queries run much faster. Caching data in memory works best when similar types of queries are repeated. In that way, Spark is able to know how to juggle memory so that most of what you need resides in memory.

However, this is not foolproof. Good Spark query and table design will help with optimization, but out-of-the-box caching usually gives some benefit. Often, the first queries will not benefit from memory caching, but subsequent queries will run much faster.

Since we will no longer use the intermediary dataframes we created, we will remove them with the rm function, and then use the cache() function on the full dataframe:

```
#cleanup and cache df
rm(out_sd1)
rm(out_sd2)
cache(out_sd)
```

## Some useful Spark functions to explore your data

### Count and groupby

We can also use the `Count` and `groupby` functions to aggregate individual variables.

Here is an example of using this to tally the number of observations by outcome. Since the result is another dataframe, we can use the `head` function to write the results to the console.

### Note

You might have to alter the number of rows returned by head if you change the query. It is always a good idea to filter results using a function such as `head`, to make sure that you are not printing hundreds of rows (or more).

However, you also need to ensure that you do not cut off all of your output. If you are unsure as to the number of rows, first assign the result to a dataframe and then check the number of rows (with `nrow`) first:

This code line count the number of rows by outcome. I know that there should be only 2 outcomes, but I place the count function within a head statement just to program defensively.

```
head(SparkR::count(groupBy(out_sd, "outcome")))
```

### (5) Spark Jobs

| | outcome | count |
|---|---|---|
| 1 | 0 | 500000 |
| 2 | 1 | 268000 |

Printing values from a Spark dataframe is very quick. Here are a few values from each outcome group. Note that the sample_bin is negative for outcome == 0, and positive for outcome == 1:

First print some of the negatives. We can see that some of the pregnant numbers are negative, showing that the simulation wasn't perfect.

```
head(SparkR::filter(out_sd,out_sd$outcome == 0),5)
```

### (2) Spark Jobs

| sample_bin | outcome | pregnant | glucose | pressure | triceps | insulin | mass | pedigree |
|---|---|---|---|---|---|---|---|---|
| -144 | 0 | -1.481 | 73.4 | 66.6 | 19.8 | 104 | 23.9 | 0.112 |
| -395 | 0 | 1.105 | 105.6 | 71.0 | 22.3 | 115 | 30.5 | 0.948 |
| -205 | 0 | -0.376 | 132.1 | 65.9 | 17.5 | 117 | 25.8 | 0.283 |
| -442 | 0 | 0.853 | 149.1 | 68.0 | 18.4 | 164 | 25.7 | -0.196 |
| -471 | 0 | 3.480 | 115.2 | 58.5 | 32.2 | 239 | 31.6 | 0.754 |

| | age |
|---|---|
| 1 | 33.2 |
| 2 | 26.0 |
| 3 | 25.3 |
| 4 | 34.3 |
| 5 | 32.6 |

Now print some of the positives.

```
head(SparkR::filter(out_sd,out_sd$outcome == 1),5)
```

| | sample_bin | outcome | pregnant | glucose | pressure | triceps | insulin | mass | pedigree |
|---|---|---|---|---|---|---|---|---|---|
| 1 | 78 | 1 | 1.943 | 157 | 61.5 | 15.7 | 129.6 | 29.4 | 1.223 |
| 2 | 212 | 1 | 4.364 | 149 | 71.7 | 41.6 | 240.0 | 25.0 | 0.314 |
| 3 | 110 | 1 | 0.859 | 173 | 74.4 | 30.0 | 379.5 | 32.8 | 0.445 |
| 4 | 237 | 1 | 1.855 | 107 | 74.4 | 34.9 | 86.2 | 42.3 | 0.530 |
| 5 | 253 | 1 | 9.171 | 105 | 78.1 | 33.0 | -67.8 | 35.6 | 0.386 |

| | age |
|---|---|
| 1 | 25.8 |
| 2 | 19.4 |
| 3 | 46.3 |
| 4 | 16.7 |
| 5 | 26.6 |

### Covariance and correlation functions

Correlations and covariances can also be computed directly from a Spark dataframe. For our example, we can see that there is a larger correlation between age and glucose for non-diabetic patients:

First the diabetic outcomes. Correlation is .113 :

```
> corr(SparkR::filter(out_sd,out_sd$outcome==1), "glucose", "age", method = "pearson")
```

[1] 0.113

Now, the non-diabetic outcomes. Correlation is .22:

```
> corr(SparkR::filter(out_sd,out_sd$outcome==0), "glucose", "age", method = "pearson")
```

[1] 0.22

For the entire population the correlation is 0.26:

```
> corr(out_sd, "glucose", "age", method = "pearson")
```

- (1) Spark Jobs
- Job 295 View (Stages: 1/1)

[1] 0.269

## Creating new columns

Usually it's necessary to create some new transformation based on existing variables which will improve a prediction. We have already seen that binning a variable is often done to create a nominal variable from a quantitative one.

Let's create a new column, called agecat, which divides age into two segments. To keep things simple, we will start off by rounding the age to the nearest integer.

```
filtered <- SparkR::filter(out_sd, "age > 0 AND insulin > 0")
filtered$age <- round(filtered$age,0)
filtered$agecat <- ifelse(filtered$age <= 35,"<= 35","35 Or Older")
SparkR::head(SparkR::select(filtered, "age","agecat"))
```

(1) Spark Jobs

| | age | agecat |
|---|---|---|
| 1 | 26 | <= 35 |
| 2 | 19 | <= 35 |
| 3 | 46 | 35 Or Older |
| 4 | 17 | <= 35 |
| 5 | 26 | <= 35 |
| 6 | 37 | 35 Or Older |

In the code which you just ran, you may notice that some commands are prefaced by SparkR::

This is done to let the program know which version of the function we wish to apply, and it is always good practice to preface commands in this way, in order to avoid syntax errors and misapplying identically named functions which occur between SparkR and normal R.

### Constructing a cross-tab

Now that we have categorized age, we can run a cross-tab which counts outcomes by the age category.

Since there are only two outcomes and two age categories, this results in a four-cell crosstab:

- 1. First, display the results using the special Databricks `display` command.
- 2. After the results appear as shown in the table below, you can click the plot button (2nd icon on the bottom left) and the Customized Plot dialogue will appear, which will allow the results to be plotted as a bar chart. The plot show that diabetes occurs more frequently in the higher age group than in the lower age group, while the reverse is true for the non-diabetes group:

```
table <- crosstab(filtered, "outcome", "agecat")
display(as.data.frame(table))
```

| outcome_agecat | 35 Or Older | <= 35 |
|---|---|---|
| 1.0 | 156000 | 109000 |
| 0.0 | 179000 | 297000 |

### Customize Plot

- All fields:
  - outcome_agecat
  - 35 Or Older
  - <= 35
  - <id>

Keys:
outcome_agecat

Series groupings:

Values:
35 Or Older   <= 35

![](img/672c0cf8ba3c000a6529b117ed96945e_550_0.png)

Options:
( ) Grouped (selected)
( ) Stacked
( ) 100% Stacked

Aggregation: SUM
Display type: Bar chart

[Cancel] [Apply]

## Contrasting histograms

Histograms are also a quick way to visually inspect and compare outcome variables.

Here is another example of using the Spark histogram function to contrast the mean values of body mass index for diabetic versus non-diabetic patients in the study. For the first bar chart, we can see a peak bar of about 38.9 BMI, versus a peak bar of 29.8 for non-diabetic patients. This suggests that BMI will be an important variable in any model we develop:

This code uses the SparkR histogram function to compute a histogram with 10 bins. The centroids gives the center value for each of the 10 bins. The most frequently occurring bar is the bar with a center value of 38.9 with a count of about 50,000. This type of histogram is useful for quickly getting a sense of the distribution of variables, but is somewhat lacking in labeling, and controlling various elements since as scales and ranges. If you wish to fine tune some of the elements you may want to start by using a collect() function to download a sample to your local R machine. We will discuss this capability later on.
