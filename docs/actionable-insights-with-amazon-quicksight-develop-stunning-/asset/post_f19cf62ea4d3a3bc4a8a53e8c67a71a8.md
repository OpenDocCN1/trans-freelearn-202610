
8. Then, let's copy 3 months' worth of data into the data warehouse. Let's use the COPY command, as follows:

```
copy yellowtrips_3mo from
's3://data-lake-afo59dkg84/yellowtrips/yellow_
tripdata_2020-10.csv'
iam_role 'arn:aws:iam::xxxxxxxxxxxx:role/
RedshiftSpectrumRole' FORMAT AS CSV dateformat 'auto'
ignoreheader 1;
copy yellowtrips_3mo from
's3://data-lake-afo59dkg84/yellowtrips/yellow_
tripdata_2020-11.csv'
iam_role 'arn:aws:iam::xxxxxxxxxxxx:role/
RedshiftSpectrumRole' FORMAT AS CSV dateformat 'auto'
ignoreheader 1;
copy yellowtrips_3mo from
's3://data-lake-afo59dkg84/yellowtrips/yellow_
tripdata_2020-12.csv'
iam_role 'arn:aws:iam::xxxxxxxxxxxx:role/
RedshiftSpectrumRole' FORMAT AS CSV dateformat 'auto'
ignoreheader 1;
```

9. At this stage, we have a data lake with 12 months' worth of data and a data warehouse that contains only the most recent data (3 months). One of the characteristics of the modern data architecture is that it allows its users to query the data lake from the data warehouse. Feel free to query the data and start getting an understanding of the dataset. Let's create the external schema so that we can enable the Spectrum feature. Use the following command in your Redshift editor. Replace the role ARN with the values from your environment:

```
create external schema spectrum_schema from data catalog
database 'my-data-lake-db'
iam_role 'arn:aws:iam::xxxxxxxxxxxx:role/
RedshiftSpectrumRole'
create external database if not exists;
```

10. Let's just compare the size of the two tables using a simple count(*) query:

```
select count(*) from public.yellowtrips_3mo;
select count(*) from spectrum_schema.yellowtrips;
```

The first query will run against the recent data in the data warehouse. The second will run against the first query using the Spectrum nodes using the data in the data lake. As expected, the number of records in the data lake should be much higher than the number of records in the data warehouse. Specifically, the query result was 24,648,499 for the year 2020 and 4,652,013 records for the last 3 months of 2020.

> Note
> The Spectrum queries use the Spectrum nodes and are charged separately from the Redshift cluster. Every query incurs an added cost based on the data it needs to scan. Refer to the AWS pricing for details.

Feel free to experiment with the data and trigger a few queries to understand the dataset. When you finish with the Redshift cluster, you can **pause** the cluster so that you stop the on-demand billing. Once the cluster is paused you will only pay for the cluster storage.

## Summary

Congratulations, you have reached the end of the first chapter! By now, you should have a good understanding of the AWS analytics ecosystem and its data lake and data warehousing options. In this chapter, we discussed in detail the key differences between data warehouses and data lakes. We also discussed the modern data architecture on AWS, and we looked at its main components in more detail. Finally, during the step-by-step section in this chapter, you had a chance to create a data lake and a data warehouse from scratch, and you loaded an open dataset for further analysis later on. We also defined Spectrum tables and queried the data lake directly for the data warehouse.

In the next chapter, we will discuss the basic concepts of Amazon QuickSight, understand its main benefits, and learn how to set up a QuickSight account.

## Questions

- 1. What is the difference between data lakes and data warehouses?
- 2. What is schema on read and what is schema on write?
- 3. How can we identify the schema of new data in a data lake on AWS?
- 4. Which AWS storage service is ideal for a data lake on AWS?
- 5. What data is better served from the data lake in the modern data architecture?
- 6. How do Redshift Spectrum tables differ from Redshift tables?

## Further reading

- Big Data and Analytics Options on AWS – 2018 – AWS whitepaper: https://d0.awsstatic.com/whitepapers/Big_Data_Analytics_Options_on_AWS.pdf
- Cloud Native Data Virtualization on AWS – 2020 – AWS whitepaper: https://d1.awsstatic.com/whitepapers/cloud-native-data-virtualization-on-aws.pdf?did=wp_card&trk=wp_card

# 2 Introduction to Amazon QuickSight

In this chapter, we will introduce **Amazon QuickSight** and we will define its main components. We will discuss fundamental architectural topics such as networking and storage, and look closely at the different editions of Amazon QuickSight and understand the different options available during account creation. Finally, we will walk through a step-by-step guide to set up a QuickSight account, and configure the QuickSight permissions to access data in a data lake.

To summarize, in this chapter, we will cover the following:

- Introducing Amazon QuickSight
- Introducing QuickSight editions and user authorization options
- Setting up Amazon QuickSight

## Technical requirements

The only thing you will need for this chapter is an AWS account.

# Introducing Amazon QuickSight

Amazon QuickSight is a **business intelligence (BI)** cloud-native service. BI developers use QuickSight to develop data visualizations and dashboards and report the state of the business. Being cloud-native **BI** software, QuickSight can scale to thousands of users without the need to provision additional compute capacity. In fact, the concept of compute capacity doesn't exist in QuickSight: AWS is responsible for scaling QuickSight up or down when demand grows or shrinks, while BI developers focus on developing data visualizations.

Next, we will define the following core concepts of QuickSight:

- Datasets
- Analysis
- Visuals and insights
- Dashboards

## Datasets

**QuickSight datasets** are the data imported into QuickSight for visualization; a dataset can be built from one or more data sources. Amazon QuickSight supports a number of data sources. Datasets can either be cached in QuickSight (SPICE, which we will cover later in this chapter) or query the data source directly. We will discuss data sources and data preparation in more detail in _Chapter 3, Preparing Data with Amazon QuickSight_.

> **Note**
> Datasets and data sources are different concepts in QuickSight. A dataset can consist of one or more data sources. Typically, a degree of data preparation is applied to a data source before it becomes a dataset.

## Analysis

An analysis in QuickSight refers to the editing part of the application, typically used by a BI developer to build their visualizations. We will see how to create an analysis in _Chapter 4, Developing Visuals and Dashboards_.

> **Note**
> A QuickSight analysis can be developed against one or more datasets.

# Visuals and insights

A QuickSight analysis consists of onscreen components/widgets that contain data visualizations. A BI developer can add two different types of these components:

- Visuals
- Insights

**Visuals** are graphical representations of a dataset commonly using plot types and charts. Other visual types include tables (simple or pivot), geospatial visualizations such as maps, and others. We will cover the various visual types in detail in _Chapter 4, Developing Visuals and Dashboards_.

> **Tip**
> QuickSight has a special visual type called **AutoGraph**. It appears with the symbol. When selected, QuickSight automatically identifies and applies the most appropriate visual type for the data types and fields selected.

**Insights** are special components that use common analytical or ML functions to provide insights into data. The result of an insight appears in natural language. Amazon QuickSight provides both pre-built insights and customizable insights. We will talk more about QuickSight insights in _Chapter 6, Working with ML Capabilities and Insights_.

# Dashboards

Once an analysis is developed and ready to be published, a BI developer can export it into a QuickSight dashboard. A dashboard is a read-only version of an analysis. A dashboard is typically accessed by business users and shared by BI developers for reporting reasons. A BI developer can also add custom controls and add interactivity to their dashboards. We will cover QuickSight dashboards in detail in _Chapter 4, Developing Visuals and Dashboards_.

Next, we will define the different user roles found in Amazon QuickSight.

# Introducing Amazon QuickSight user types

QuickSight supports three different user types:

- Admin
- Author
- Reader
