Data standardization if often referred to as the zscore and is widely used to identify outliers on your variable, which we will see later in this chapter. For the sake of demonstration, the following table simulates the data standardization of a small dataset. The input value is present in the "Age" column, while the scaled value is present in the "Zscore" column:

| Age | Mean | Standard Deviation | Zscore |
| --- | --- | --- | --- |
| 5 | 31,83 | 25,47 | -1,05 |
| 20 | 31,83 | 25,47 | -0,46 |
| 24 | 31,83 | 25,47 | -0,31 |
| 32 | 31,83 | 25,47 | 0,01 |
| 30 | 31,83 | 25,47 | -0,07 |
| 80 | 31,83 | 25,47 | 1,89 |

Figure 3.11 – Data standardization in action

Make sure you are confident when applying normalization and standardization by hand in the AWS Machine Learning Specialty exam. They might provide a list of values, as well as mean and standard deviation, and ask you the scaled value of each element of the list.

# Applying binning and discretization

**Binning** is a technique where you can group a set of values into a bucket or bin; for example, grouping people between 0 and 14 years old into a bucket named "children," another group of people between 15 and 18 years old into a bucket named "teenager," and so on.

**Discretization** is the process of transforming a continuous variable into discrete or nominal attributes. These continuous values can be discretized by multiple strategies, such as **equal-width** and **equal-frequency**.

An equal-width strategy will split your data across multiple bins of the same width. Equal-frequency will split your data across multiple bins with the same number of frequencies.