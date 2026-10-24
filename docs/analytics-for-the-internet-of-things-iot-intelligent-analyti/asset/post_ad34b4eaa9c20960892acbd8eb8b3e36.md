
For IoT analytics, it is important to consider the use cases and what datasets you will need to support before deciding real-time streaming analytics is what you actually need. When you are looking for insights and new business opportunities, you will almost always be using datasets with long term history. Data from the latest hour or minute adds little incremental value for those use cases.

Even though the lure of real-time analytics is tempting, you have to consider if the additional cost and maintenance of streaming technology is worth it. In most cases for IoT analytics, it is not and could be better served by increasing the batch processing frequency. You also have limitations in what you can do with streaming analytics versus batch analytics when incorporating additional datasets. Unless those datasets are also real-time, they are only usable for less frequent batch processing.

While this is the case for *exploratory analytics* to find insights, it may not be the case for *productionalizing* the insights from those analytics. You may want near real-time analytics processing in some cases, such as when applying a predictive model to streaming sensor data to predict an equipment failure. You may want real-time streaming analytics for reporting dashboards that are monitored by fast reaction customer support teams.

These are separate decisions, however. The recommendation is to avoid the overhead of adding streaming analytics unless there is a strong business case to do so. *Real-time* sounds good, but you need to make sure the incremental gain is worth the additional expense in both time and resources.

## Lambda architectures

Lambda architectures consist of an arrangement of data and processing technologies intended to serve both regular batch processing activities and near real-time streaming activities, in a way that allows results from both to be combined into a single view. The following descriptions and diagram highlight the conceptual components that make up the three layers to the architecture:

- **Batch:** This layer keeps the master dataset and regular batch processing of the data to create views to support analytics, dashboards, and reporting linked to the Serving layer. The master dataset is immutable, meaning the data does not change once incorporated, and it is appended with new data as time goes on. A long-running Hadoop cluster would be an example.
- **Speed:** This layer is focused on recent data only (the streaming data). It handles requests that have low-latency requirements in terms of recency of data. Algorithms need to be fast and incremental to operate in the speed layer. Amazon Kinesis would be an example technology that operates in the speed layer.
- **Serving:** This layer indexes batch views so they can be served up with low latency. This includes batch views from both the Batch and Streaming layers.

![](img/9f2a55db1236f995dcd9392acbc82910_166_0.png)

Lambda architecture. Source: MapR technologies

### Handling change

Change is constant, as contradictory as that sounds. The architecture, data model, and technology will constantly evolve over time. You will need to decide how to handle change in order to keep flexibility in your data storage and processing architecture. You will want to use tools that are decoupled from each other to allow future analytics to be easily integrated into the data processing stream.

Fortunately, components in the Hadoop ecosystem were intentionally designed to be decoupled from each other. They allow the mixing and matching of components and were built to be extensible for new frameworks not even invented yet. Cloud infrastructures allow for easy testing and incorporation of new technologies and software.

You will need to design a process that takes your analytics and data processing code from experimentation, to development, to production. This holds true for your overall infrastructure as well. A common practice is to keep three separate environments: one for development, one for testing, and one for production.

In order to save costs, you may decide to reduce this to two: a hybrid development and test environment, and a production environment. Either way, you will need a structured process to move your insights from initial ideas to fully-fledged parts of your standard operation.

## Summary

In this chapter, we discussed several cloud infrastructure technologies specific to IoT and processing incoming data from devices in the field. We also reviewed some strategies to collect and store IoT data in order to enable analytics. The Hadoop ecosystem and architecture was introduced with some detail on the key components for IoT analytics.

You also learned when and why to use Spark for data processing. We discussed tradeoffs between streaming and batch processing. The Lambda architecture was introduced. Deciding how to handle change was discussed to build in flexibility to allow future analytics to be integrated with data processing.

## 6 Getting to Know Your Data - Exploring IoT Data

"Yes, I know the three-month average is 45.2, but what does it mean? We have over 2 terabytes of data now. But what does it tell us?"

Your boss is in one of his moods again. He is peppering you with questions, and you are not sure how to answer. Your first instinct was to spit back numbers from the weekly reports, but it obviously did not work.

"Is the data even any good?" he continues, "Are we building up value or just cluttering up the basement with junk?"

You start to shrug but wisely stop yourself. You think the data is very valuable, but you actually do not know much about the dataset beyond the numbers that are included in the reports you have been asked to develop. You realize you are not even sure what the individual data records look like. You know that averages can hide a lot of things, but no one has ever asked you to look any deeper.

You straighten your shoulders and say confidently, "There is value in the data. We are just not looking at all aspects of it. I will show you there is more to it."

You do not feel confident at all. You are not sure how you are going to show where business value is buried just waiting to be discovered. But you hide it well.

Your boss lets out a harrumph-like noise, clearly not really buying what you are selling. He turns around and walks off down the hallway muttering to himself.

> Data is the sword of the twenty-first century, those who wield it well, the samurai.
> 
> -Eric Schmidt and Jonathan Rosenberg in How Google Works

This chapter is focused on exploratory data analysis for IoT data. You will learn how to ask and answer questions of the data. The first part of the chapter will be on understanding the data quality. Then, we will move on to getting to know the data better and what it represents. We will use Tableau and R for examples. You will learn strategies for quickly assessing data and starting the hunt to find value.

This chapter covers the following topics:

- Exploring and visualizing data
- A quick Tableau overview:
  - Techniques to understand data quality
  - Basic time series analysis
  - Getting to know categories in the data
  - Analyzing with geography
- Looking for attributes that have predictive value
- Using R to augment visualization tools
- Industry-specific examples:
  - Manufacturing
  - Health care
  - Retail

### Exploring and visualizing data

The first step in any analytics, *especially* IoT analytics, is getting to know your data. Like a future spouse, you need to know everything you can about it. Know its flaws and its strong points. Know which attributes can be annoying and make sure you are able to live with them. Get to know its future earning potential. And find all this out before it is too late and you are bound to it.

In this chapter, we will use a couple of tools to quickly learn quite a bit about a sample dataset. Examples will follow along with methods to dive into the data in order to find its strengths and weaknesses. For exploration and visualization, we will use Tableau. For more statistical evaluations, we will use the statistical programming language R.

#### The Tableau overview

**Tableau** is a business intelligence and analytics software tool that allows you to connect to dozens of different database and file types, drag and drop to build visualizations, and easily share them with others. There are two main versions, desktop and server. The desktop is a high-powered tool you install on your laptop to connect and manipulate datasets. A server is a web-based component where you publish your desktop visualizations for others to easily interact with them.

We will only focus on the Tableau desktop in this chapter as our mission is to explore data versus communicating analytic findings. It is free to try for a limited period. You can download it from the company's website (https://www.tableau.com/products/desktop/download).

The first step in working with your data using Tableau is to connect to your dataset. Tableau has many different connectors able to link to a wide variety of databases and file formats. Think of Tableau as a window into your data, a way to link to it and explore it. It is separate from your data though, keep them distinct in your mind.
