In this chapter, we will learn about Structured Streaming, which is the Apache Spark **application programming interface (API)** that allows us to make computations on streaming data in the same way you would express a batch computation on static data. Structured Streaming is a fault-tolerant engine that allows us to perform optimized operations for low-latency **Structured Query Language (SQL)**-like queries on the data thanks to the use of Spark SQL, while ensuring data consistency.

We can use Structured Streaming when creating ETL pipelines that will allow us to filter, transform, and clean data as it comes in a raw structure, formatting data types into more efficient storage formats, applying efficient partition strategies based on relevant columns, and so on. Specifically, in this chapter, we will look at the following topics:

+   - Using the Structured Streaming API
+   - Using different sources available in Azure Databricks when dealing with continuous streams of data
+   - Recovering from query failures
+   - Optimizing streaming queries
+   - Triggering streaming query executions
+   - Visualizing data on streaming dataframes
+   - Example on Structured Streaming

Without further ado, we will start our discussion by talking about Structured Streaming models and how can we leverage their architecture to improve performance when working with streams of data.

# Technical requirements

This chapter will require you to have an Azure Databricks subscription available to work on the examples, as well as a notebook attached to a running cluster.

Let's start by looking into Structured Streaming models in more detail to find out which alternatives are available to work with streams of data in Azure Databricks.