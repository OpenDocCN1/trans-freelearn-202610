# Cluster analysis

In this section, we will illustrate how to implement a cluster analysis directly in Spark using k-means. K-means is a form of unsupervised learning and is an excellent place to begin to explore big data, especially if your data is large and you have no clear definition of target variables.

# Preparing the data for analysis

We will first perform some data preparation in order to normalize the data for k-means. Normalization is pretty much a requirement for k-means, since it forces each variable to be scale-independent so that measuring distances between the k-means clusters is also scale-independent.

Recall that one way to normalize a variable is to first obtain the mean of the variable and then divide by its standard deviation.

# Reading the data from the global views

We saved the diabetes dataset as a table and a global view in the previous section, so we can now read it back using SQL. We will first read the training table (`temp.df_view`) and then follow by reading the test table (`test_view`). The resulting objects are both Spark dataframes.

After reading the table in, print the count of the rows. If you want to see the counts as one result, you can use a function such as cat() to print the results on one line:

```
df = sql("select age,
				pregnant,
				glucose,
				pressure,
				insulin,
				pedigree,
				triceps,
				mass
from global_temp.df_view")
```