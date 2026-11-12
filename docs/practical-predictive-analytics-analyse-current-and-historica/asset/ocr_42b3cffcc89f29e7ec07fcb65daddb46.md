# Building our first Spark dataframe

One of the challenges of working with Spark is finding analytic solutions when working with very large datasets. As a preparation step, in this chapter, we will build our very own large Spark dataframe.

Also note that the concept of large dataframe is obviously relative. Since the free databricks will limit the size of any dataframe created, we will end up building a 1-million-row dataframe of about 11 variables.

I will also show you how to build a similar dataset in base R, so that you can perform your own testing and be able to judge the performance benefits received from performing the analytics on Spark.

# Simulation

We will end up building this Spark dataframe via simulation. This will take up a good chunk of this chapter. I feel this is a better way to go rather than importing an existing public dataset in which you cannot control the makeup of the data. With a simulated dataset, you are free to size it however you like (subject to account restrictions).

However, you are always free to import whatever dataset you would like and the analytic concepts that follow will be the same.

+   1. Preliminaries first, you will need to register and log on to your databricks account.
+   2. Next, create a cluster. Give it a name, such as MyCluster.
+   3. To conform with the examples in this chapter, make sure you choose Spark 2.1. This is very important. Since Spark is an Apache open source product, capabilities can evolve, so we want to make sure you can reproduce your code.
+   4. Select `Create Cluster`.