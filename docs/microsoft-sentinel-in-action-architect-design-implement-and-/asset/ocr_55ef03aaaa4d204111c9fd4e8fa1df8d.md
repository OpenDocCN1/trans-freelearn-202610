In the following screenshot, a simple query was entered and run. The query was entered in the query window, and you can see that the results are shown below it:

![](bbox=[0.13, 0.15, 0.87, 0.46])

Figure 5.3 – Executed query

There is a lot more you can do with the ADX, so feel free to play around with it. Now that you understand how we are going to run the queries and view the results, let's look at some of the various commands KQL has to offer.

# Introduction to KQL commands

Unlike SQL, the query starts with the data source, which can be either a table or an operator that produces a table, followed by commands that transform the data into what is needed. Each command's output can be passed into the next command by using the pipe ( | ) delimiter.

What does this mean? If you are familiar with SQL, you would write a statement such as Select * from table to get the values from the table. The same query in KQL would just be table, where table refers to the name of the log. It is implied that you want all the columns and rows. Later, we will discuss how to minimize what information is returned.

We will only be scratching the surface of what KQL can do here, but it will be enough to get you started writing your own queries so that you can develop queries for Microsoft Sentinel.