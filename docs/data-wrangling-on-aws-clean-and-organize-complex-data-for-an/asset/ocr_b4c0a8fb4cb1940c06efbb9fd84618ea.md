+   - Creating business rules that can be executed in SQL format and storing them in a metadata store (such as DynamoDB). For each run, execute the stored data quality rules and identify the records that do not conform. The load should only process new data using partitions or a custom bookmark functionality.

+   - GreatExpectations is a Python-based open-source library for data validation, profiling, and documentation of the data. With this library, we can create assertions on what is expected out of the dataset. The library also supports several built-in expectations. Once the expectations are executed, you will get results in a formatted HTML document called Data Docs.

Now, we will explore data visualization, which is the last stage in the data-wrangling pipeline, in the next section.

# Data visualisation

Data visualization is the phase where you will create visuals and charts to better communicate the findings of your analysis to business users. A picture is worth a thousand words and an idea/message can be better communicated with charts/dashboards than tables/text data.

# Visualization with Python libraries

In this section, we will explore creating dashboards using Python libraries such as matplotlib (https://matplotlib.org/) and Seaborn (https://seaborn.pydata.org/index.html). There are more Python libraries that can help in visualizing data, but we will not compare all those libraries here.

We will use the same sports dataset for this section as well. There are other datasets such as NY taxi trips datasets that we can use to cover different visualization aspects, but we will use the sports data for continuity purposes in this chapter. Let us consider a use case, where we want to visualize the following requirements:

+   - The number of objects sold on a daily basis and their price range
+   - The price range for tickets sold per seat category to better understand how the ticket pricing band is structured

In order to get the number of tickets sold, we will use the data from the ticket_purchase_hist table. The data from the table will have the information as shown in the following screen shot: