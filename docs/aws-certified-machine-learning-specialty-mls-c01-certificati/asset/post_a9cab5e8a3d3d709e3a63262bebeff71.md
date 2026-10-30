
If you want to show compositions while considering a time dimension, then your most common options would be a **stacked area chart**, a **stacked 100% area chart**, a **stacked column chart**, or a **stacked 100% column chart**. For reference, take a look at the following chart, which shows the sales per region from 2016 until 2020:

![](img/209e007237d34506e7ba16180f2e905c_150_0.png)

Figure 4.12 – Plotting compositions with a stacked 100% column chart

As we can see, stacked 100% column charts help us understand compositions across different periods.

## Building key performance indicators

Before we wrap up these data visualization sections, I want to introduce **key performance indicators**, or **KPIs** for short.

A KPI is usually a single value that describes the results of a business indicator, such as the churn rate, **net promoter score (NPS)**, **return on investment (ROI)**, and so on. Although there are some commonly used indicators across different industries, you are free to come up with a number, based on your company’s needs.

To be honest, the most complex challenge associated with indicators is not in their visualization aspect itself, but in the way they have been built (the rules used) and the way they will be communicated and used across different levels of the company.

From a visualization perspective, just like any other single value, you can use all those charts that we have learned about to analyze your indicator, depending on your need. However, if you just want to show your KPI, with no time dimension, you can use a **widget.**

Alright, that's all I have to talk about regarding data visualization techniques for the AWS machine learning specialty exam. Now, let's take a look at Quick Sight, an AWS service where you can implement all these visualization techniques you have just learned.

## Introducing Quick Sight

Amazon Quick Sight is a cloud-based analytics service that allows you to build data visualizations and ad hoc analysis. Quick Sight supports a variety of data sources, such as Redshift, Aurora, Athena, RDS, and your on-premises database solution.

Other sources of data include S3, where you can retrieve data from Excel, CSV, or log files, and **Software as a Service (SaaS)** solutions, where you can retrieve data from Salesforce entities.

Amazon Quick Sight has two versions:

- Standard Edition
- Enterprise Edition

The most important difference between these two versions is the possibility of integration with Microsoft **Active Directory (AD)** and encryption at rest. Both features are only provided in the enterprise edition.

> **Important note**
> Keep in mind that AWS services are constantly evolving, so more differences between the standard and enterprise versions may crop up in the future.
> You should always consult the latest documentation of AWS services to check what's new.

In terms of access management, Quick Sight offers a very simple interface you can use to control user access. In the standard edition, you have pretty much two options for inviting a user to your Quick Sight account:

- You can invite an IAM user.
- You can send an invitation to an email address.

If you invite an IAM user, then they can automatically log into your account and see or edit your visualization, depending on the type of permission you have provided during Quick Sight user creation. If you have invited an email address, then the email owner has to access his/her mailbox to complete this operation.

Removing a user is also pretty straightforward. The only extra information you have to provide while removing a user is whether you want to transfer the orphaned resources to another user into your account, or whether you want to delete all the user's resources.

If you are playing with the enterprise edition, this process of authorizing users can be a little different since you have AD working for you. In this case, you can grant access to AD groups, and all the users from that group would be granted access to your account on Quick Sight.

Also, remember that in both versions, all data transfer is encrypted; however, you will only find encryption at rest in the enterprise edition.

When you are bringing data into Quick Sight, you are technically creating what we call **datasets**. Datasets, in turn, are imported in an optimized structure into Quick Sight, known as the **Super-fast, Parallel, In-memory, Calculation Engine (SPICE)**. That's why Quick Sight can perform data visualization on big data.

We know that Quick Sight offers high availability and durability since the data that's imported into it is replicated.

Something else you should be aware of is that Quick Sight does not only allow you to plot your data but also perform some small data preparation tasks, such as renaming fields, computing new fields, changing data types, preparing queries to retrieve data from the source, and joining tables from the same source.

Let's summarize the main steps for working with Quick Sight:

- 1. User creation and authorization.
- 2. Connecting to a **data source**.
- 3. Bringing data into a **dataset**.
- 4. Your dataset will be imported into **SPICE**.
- 5. From a dataset, you can create an **analysis**.
- 6. Finally, inside your analysis, you can add **visuals**.
- 7. In case you want to go further, you can create a snapshot of your analysis and place it in a **dashboard**. Alternatively, you can group your analysis into **stories**.

That brings us to the end of this chapter about data visualizations! Now, let's take a look at what we have learned.

## Summary

We have reached the end of this chapter about data visualization. Let's take this opportunity to provide a quick recap of what we have learned. We started this chapter by showing you how to visualize relationships in your data. Scatter plots and bubble charts are the most important charts in this category, either to show relationships between two or three variables, respectively.

Then, we moved on to another category of data visualization, which aimed to make comparisons in your data. The most common charts that you can use to show comparisons are bar charts, column charts, and line charts. Tables are also useful to show comparisons.

The next use case that we covered was visualizing data distributions. The most common types of charts that are used to show distributions are histograms and box plots.

Then, we moved on to compositions. We use this set of charts when we want to show the different elements that make up the data. While showing compositions, you must be aware of whether you want to present static data or data that changes over time. For static data, you should use a pie chart, a stacked 100% bar chart, or a treemap. For data that changes over time, you should use a stacked area chart, a stacked 100% area chart, a stacked column chart, or a stacked 100% column chart.

The last section of this chapter was reserved for Quick Sight, which is an AWS service that you can use to visualize your data. You learned about the different versions and features of the service, and you were then introduced to SPICE.

Well done! In the next chapter, you will learn about AWS's data storing services, such as S3, EBS, EFS, and many others. That is going to be a very important chapter for your certification journey, so make sure you're prepared! However, before we jump into that new chapter, let's practice a little more for the exam!

## Questions

1. You are working as a data scientist for a fintech company. At the moment, you are working on a regression model that predicts how much money customers will spend on their credit card transactions in the next month. You believe you have created a good model; however, you want to complete your residual analysis to confirm that the model errors are randomly distributed around zero. What is the best chart for performing this residual analysis?

- a) Line chart
- b) Bubble chart
- c) Scatter plot
- d) Stacked bar chart

Answer
C, In this case, you want to show the distribution of the model errors. A scatter plot would be a nice approach to present such an analysis. Having model errors randomly distributed across zero is just more evidence that the model is not suffering from overfitting. Histograms are also nice for performing error analysis.

2. Although you believe that two particular variables are highly correlated, you think this is not a linear correlation. Knowing the type of correlation between these two variables is crucial for determining the type of algorithm that you are going to use in later phases of this project. Which type of chart could you use to show the correlations between these two variables?

- a) Line chart
- b) Bubble chart
- c) Histogram
- d) Scatter plot

Answer
D, Correlation is a type of relationship. In this case, we just have two variables, so a scatter plot would be a good approach to this problem.
