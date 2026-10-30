
## What is TCRM and what is it used for?

By the end of this section, you should have a good grasp of what we are referring to when we speak of TCRM, and how it is used in the real world.

The questions *what is TCRM?* and *what is it used for?* will be answered in the following sub-sections.

### What is TCRM?

TCRM is not a Salesforce app – TCRM is a data analysis and business insights platform with integrated machine learning insights from Einstein Discovery.

To expand upon the high-level summary shown in Figure 1.1, TCRM is a platform that combines the following features:

- Native, two-way integration with the Salesforce CRM platform.
- On-platform data extraction, combination, and transformation in the data manager.
- External connectivity to a variety of other platforms and cloud storage providers.
- Data visualization, analysis, and exploration.
- A data action framework to enable decision-making based on insights.
- Embedded intelligence from the Salesforce Einstein platform, such as Einstein Sentiment analysis.
- Insights can be shared in various ways, such as Salesforce Chatter, URL generation, and downloads.
- On-dashboard notifications to alert users when certain criteria are met.
- A mobile TCRM Analytics App.
- TCRM/Einstein Analytics APIs and SDKs.
- Various data security tools to mask incoming data and restrict/filter outgoing insights as required.

At a technical level, as far as architecture goes, here are some details:

- In TCRM, data is searched for using an inverted index that enables fast query results – datasets containing up to a billion rows can be queried in seconds.
- TCRM data is loaded into a non-relational store via a dynamic, horizontally scalable key-value pair approach.
- The TCRM workflow engine is a software application that is designed to help users enforce a series of recurring tasks that make up a "business process" or "workflow." It applies limited transformations upon data ingestion but largely stores information in its original form.
- Data is consumed and stored in a non-relational inverted index as key-value pairs. These key-value pairs only store non-null data values, which improves efficiency and speed.
- TCRM's query engine employs techniques such as vector encoding, differential encoding, and incremental encoding to compress data and perform efficient and faster queries on compressed data.
- Quantitative data is queried in a columnar store in RAM across Salesforce's cloud.
- TCRM provides Einstein Analytics Actions, which allow you to seamlessly go from question to answer to action without logging into a separate solution – such as creating a task, updating a record, creating a case, posting to Chatter, and more!
- Heavy compression, optimization algorithms, and parallel processing enable fast and efficient queries on very large datasets.
- Mobile-first design – TCRM enables data creation, analysis, and action directly from mobile devices.
- Einstein Analytics is an open, scalable, and extensible platform. Being built on the secure infrastructure of Salesforce, the world's top CRM, TCRM provides easy-to-use APIs that enable deep relationships with other tools and platforms.
- Salesforce has worked with Informatica, MuleSoft, Snowflake, and other vendors to build interfaces with BI and data solutions out of the box.
- TCRM delivers fast-rendering graphics that connect the relationships between views of data by drawing SVG graphics within the browser and using an animation engine for faceting and filtering. This demands fewer resources from the user's device or the server.
- The TCRM platform is built upon Salesforce's trusted, multilayered approach to data availability, privacy, and security.

Here is a visual representation of Salesforce and the TCRM architecture:

![](img/06379e43783b9ddfee45e9da5192efe1_25_0.png)

Figure 1.2 – Salesforce and the TCRM architecture

The following is a simple graphic that explains how TCRM and Einstein Discovery (referred to as **Einstein Analytics Plus**) fit within a typical data architecture that includes Salesforce:

Einstein Analytics Plus

![](img/06379e43783b9ddfee45e9da5192efe1_25_1.png)

Figure 1.3 – The TCRM and Salesforce technology stack

As you can see, the business outcome of TCRM is intelligent experiences built upon the stable, secure platform of the world's #1 CRM, Salesforce.

### What is TCRM used for?

TCRM is used to extract, aggregate, transform, and visualize data. That is, it is used to take raw data, process it with the aid of business acumen, and tell a story.

For example, I created the following TCRM dashboard for a business in the property development space with dummy data:

![](img/06379e43783b9ddfee45e9da5192efe1_26_0.png)

Figure 1.4 – A TCRM dashboard that visualizes property development information

What is the purpose of this dashboard? How did I use TCRM in this use case?

1. I loaded the raw data tables into TCRM.
2. I combined these tables into one dataset using TCRM.
3. I transformed the data to prepare it for visualization in TCRM.
4. I used TCRM to design, build, and share the analytics dashboard.
5. I was able to "slice and dice" the data, diving into the information to make sense of it and tell a story, with TCRM.

Now, TCRM is just one of several data analytics tools in the Salesforce ecosystem. Some others are as follows:

- Salesforce Reports and Dashboards
- Einstein Analytics
- Tableau
- Datorama
- Pardot analytics
- Marketing analytics

The main three that we are concerned about in the context of this book are Salesforce Reports and Dashboards, TCRM, and Tableau. How do these analytics work together? Here is a simple overview:

| Salesforce Reports and Dashboards | Tableau CRM | Tableau |
|---|---|---|
| Salesforce Reports and Dashboards are your default tool for a quick win with real time data. | Most of your users are already in Salesforce. | Your insights will be shared among Salesforce and non-Salesforce users, or business users that are not using Salesforce |
| They are your "go-to" option for a quick win based upon real-time data. | You want to quickly embed and action analytics in Salesforce. | You want the option of an on-premises architecture. |
| Quick to configure. | You are looking for easily embedded AIs and supervised machine learning. | You require remote, federated queries. |
|  | You want a native two-way integration with Salesforce. | You need more flexibility around how the BI platform fits in with the business tech stack. |

Figure 1.5 – How do the three main Salesforce analytics tools work together?

## Which analytics tool should you use for which business use case?

Here are some questions to consider (see https://marktossell.com/2020/06/20/tableau-or-einstein-analytics-which-is-best-for-you/ and https://marktossell.com/2020/10/31/are-you-confused-about-einstein-tableau-and-tableaucrm-read-this/ for more detailed information):

- Who is going to use the business insights?
- Where are they going to view and action the insights?
- How will they share those insights with others?
- How much of the data for the dashboards is stored in Salesforce?
- Where is the external data stored?
- How will external data be connected to the BI tool?
- What in-house resources will work on the analytics tool?
- What platforms are users accustomed to using?
- How many of the data consumers are Salesforce CRM users?
- What BI platform license structure suits the business best?

The following diagram shows a very simple way of looking at the comparison between TCRM and Tableau:

![](img/06379e43783b9ddfee45e9da5192efe1_28_0.png)

Figure 1.6 – A simple comparison of Tableau and TCRM

Now, you might be wondering, how are TCRM and Tableau different? The following tables will help you make sense of the differences between the two platforms:

### Differences and Synergies in the Architecture

| Architecture | Einstein Analytics | Tableau |
|---|---|---|
| Cloud | ✓ | ✓ |
| On-premises | ✗ | ✓ |
| Provides a persistent, dedicated data store | ✓ | ✓ |
| Connectors | ✓ | ✓ |
| Remote, federated queries | ✗ | ✓ |
| Action framework | ✓ | ✗ |
| Templated apps/templates | ✓ | ✗ |
| Data discovery and predictions | ✓ | ✗ |
| Marketing intelligence | ✗ | ✗ |

Figure 1.7 – Differences and synergies in the architecture for Tableau and TCRM/Einstein Analytics

When it comes to Salesforce Reports and Dashboards, how do you know when you need to upgrade to TCRM? Here are some points to guide you:

- You need to incorporate external data.
- You want contextual record actions to action insights in CRM.
- You want to use embedded analytics.
- You require greater customizability.
- You wish to use the Analytics mobile app.
- You need predictive insights and augmented analytics.
- You want to take advantage of TCRM pages.
- You need advanced trending and waterfall charts.
- You require flexible and powerful data modeling in the data flow editor and data recipes.
- You want superior performance for up to 10 billion rows.
