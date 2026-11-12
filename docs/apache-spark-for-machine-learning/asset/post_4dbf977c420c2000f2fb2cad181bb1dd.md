

The following code joins two tables using the cust_id column:

```
df_left.join(df_right,on="cust_id",how="right").show()
df_left.join(df_right,on="cust_id",how="right_outer").show()
df_left.join(df_right,on="cust_id",how="rightouter").show()
```

Here is the output:

| cust_id | order_id | amount | name |
|---|---|---|---|
| 1 | 1001 | 100 | john |
| 1 | 1004 | 200 | john |
| 3 | 1003 | 300 | tony |
| 2 | 1002 | 200 | mike |
| 4 | NaN | NaN | kent |

## The full join

The full join fetches data if it is present in either of the two tables.

Figure 2.17 explains the FULL join operation. In the following figure, there are two tables, named Left_Table-1 and Right_table-1. The cust_id column exists in both the tables and, hence, can be used as a join column.

## Full Join

![](img/b40816015a9835648eac2b186dac82d1_70_0.png)

### Left Table - 1

| order_id | cust_id | amount |
|---|---|---|
| 1001 | 1 | 100 |
| 1002 | 2 | 200 |
| 1003 | 3 | 300 |
| 1004 | 1 | 200 |
| 1005 | 6 | 200 |

### Right table - 1

| cust_id | name |
|---|---|
| 1 | john |
| 2 | mike |
| 3 | tony |
| 4 | kent |

### joined - 1

| cust_id | order_id | amount | name |
|---|---|---|---|
| 6 | 1005 | 200 | None |
| 1 | 1001 | 100 | john |
| 1 | 1004 | 200 | john |
| 3 | 1003 | 300 | tony |
| 2 | 1002 | 200 | mike |
| 4 | NaN | NaN | kent |

Join column
Records that exist in both the right table and left table

Figure 2.17 – An example of a full Join

The following code joins two tables using the cust_id column:

```
df_left.join(df_right,on="cust_id",how="full").show()
df_left.join(df_right,on="cust_id",how="fullouter").show()
df_left.join(df_right,on="cust_id",how="full_outer").show()
```

The output matches what is shown in Figure 2.17.

## The cross join

A cross join returns the Cartesian product of two relations.

The cross join is an extremely expensive data join operation, often requiring large memory. Hence, it needs to be enabled before using it, as shown in the following code:

```
spark.conf.set("spark.sql.crossJoin.enabled", "true")
df_left.crossJoin(df_right).show()
```

This output is shown in Figure 2.18, which explains the cross join operation. In the following figure, there are two tables, named Left Table-1-1 and Right table-1-1. The cust_id column exists in both the tables and, hence, can be used as a join column.

![](img/b40816015a9835648eac2b186dac82d1_71_0.png)

Figure 2.18 – An example of a cross join

The following code demonstrates the joining of two tables using joins such as semi, leftsemi, and left_semi:

```
df_left.join(df_right,on="customer_id", how="semi").show()
```

| customer_id | order_id | amount |
|---|---|---|
| 1 | 1001 | 100 |
| 1 | 1004 | 200 |
| 3 | 1003 | 300 |
| 2 | 1002 | 200 |

The following code snippet joins two tables and displays the joined table:

```
df_left.join(df_right,on="customer_id",how="leftsemi").show()
```

Here is the output:

| customer_id | order_id | amount |
|---|---|---|
| 1 | 1001 | 100 |
| 1 | 1004 | 200 |
| 3 | 1003 | 300 |
| 2 | 1002 | 200 |

The following code snippet joins two tables and displays the joined table:

```
df_left.join(df_right,on="customer_id",how="left_semi").show()
```

Here is the output:

| customer_id | order_id | amount |
|---|---|---|
| 1 | 1001 | 100 |
| 1 | 1004 | 200 |
| 3 | 1003 | 300 |
| 2 | 1002 | 200 |

## Anti join

An anti join returns the rows from one DataFrame that do not have corresponding rows in another DataFrame. It's similar to LEFT OUTER JOIN, where the result is filtered to include only the rows from the left DataFrame for which there is no match in the right DataFrame.

Figure 2.19 explains the anti join operation. In the following figure, there are two tables, named Left Table-1-2 and Right Table-1-2. The cust_id column exists in both the tables and, hence, can be used as a join column.

### Anti Join

![](img/b40816015a9835648eac2b186dac82d1_73_0.png)

| order_id | cust_id | amount |
|---|---|---|
| 1001 | 1 | 100 |
| 1002 | 2 | 200 |
| 1003 | 3 | 300 |
| 1004 | 1 | 200 |
| 1005 | 6 | 200 |

| cust_id | name |
|---|---|
| 1 | john |
| 2 | mike |
| 3 | tony |
| 4 | kent |

| cust_id | order_id | amount |
|---|---|---|
| 1005 | 6 | 200 |

Join column
Records that exist in both the right table and left table

Figure 2.19 – An example of an anti join

The following code demonstrates the joining of two tables using joins such as anti, leftanti, and left_anti:

```
df_left.join(df_right,on="customer_id",how="anti").show()
df_left.join(df_right,on="customer_id",how="leftanti").show()
df_left.join(df_right,on="customer_id",how="left_anti").show()
```

Here's the output showing the result for all three lines:

```
+-------+--------+------+
|customer_id|order_id|amount|
+-------+--------+------+
|      6|    1005|   200|
+-------+--------+------+
```

This concludes our walk-through of several code examples to understand joins.

## Summary

In this chapter, we learned about data preprocessing and how Spark supports various data operations, such as ingesting data, cleaning and transforming it, aggregating it, data windowing, and data joining. We learned about the different types of data operations through several coding examples.

In the next chapter, we will learn about feature reengineering.

## Feature Extraction and Transformation

Feature extractors and transformers are key components in the field of machine learning and data preprocessing. They help to convert raw data into a suitable and more effective format to train machine learning models. Feature extractors simplify data inputs by transforming them into a lower-dimensional space. Conversely, feature transformers involve modifying or transforming the features themselves into a format that’s more suitable for machine learning algorithms.

Understanding these concepts is crucial for building efficient and accurate predictive models.

In this chapter, we're going to cover the following main topics:

- Learning about feature extractors
- Working with feature transformers
- Exploring feature selectors

By the end of this chapter, you will know several different techniques to extract features through data transformation.

### Technical requirements

You can find the code files for this chapter on GitHub at https://github.com/PacktPublishing/Apache-Spark-for-Machine-Learning/tree/main/Chapter03.

### Learning about feature extractors

In this section, we will learn what a feature extractor is, why it is very important in machine learning, and its various techniques. A feature extractor in machine learning and data processing is a method or algorithm that automatically identifies and extracts relevant features from raw data, for use in building machine learning models. Feature extraction improves the efficiency and accuracy of subsequent analyses and predictions.

### The key aspects of feature extractors

Feature extraction is a critical process in machine learning and data analysis, where the goal is to reduce the number of input variables to those that are most meaningful to the task at hand. It involves transforming raw data into a set of features that are efficient and effective for modeling.

Here are the key aspects of feature extraction:

- Capturing relevant information: Effective feature extraction aims to capture the most relevant information from raw data. This involves identifying which features are most informative and representative of the underlying patterns or structures in the data.

- Data simplification: By extracting key features, the complexity of the data is reduced, making it easier to work with. This simplification can lead to more efficient data processing and analysis, especially with large datasets.

- Improving model accuracy and performance: By focusing on the most important features, feature extraction can lead to improved model accuracy and performance. It helps a model to concentrate on the aspects of data that are most predictive, rather than being distracted by noise or irrelevant information.

- Handling diverse data types: Feature extraction techniques vary based on the type of data, such as numerical, categorical, text, or image. Each data type requires different methods to effectively extract features.

- Contextual and domain knowledge: Incorporating domain knowledge can significantly enhance the feature extraction process. Understanding the context and specific characteristics of data can guide the selection of appropriate features and extraction techniques.

- Balancing information loss and efficiency: There is often a trade-off between information retention and efficiency. While reducing the feature space can make models more efficient, it can also lead to the loss of potentially important information. The challenge is to strike a balance that maximizes efficiency without significantly compromising the quality of information.

- Facilitating better data visualization and interpretation: Extracted features can often make data visualization and interpretation more manageable and insightful, as they distill data into its most informative components.

### Algorithms for feature extraction

Let's review some of the algorithms used in feature extractors:

- Text data:
  - Bag of words: Transforms text into a fixed length set of features, representing the frequency of certain words.

