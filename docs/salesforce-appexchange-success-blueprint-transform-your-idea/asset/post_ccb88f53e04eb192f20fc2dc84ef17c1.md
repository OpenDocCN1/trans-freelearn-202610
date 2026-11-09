
You can only turn on package usage logs and subscriber snapshots. Package usage summaries are automatically provided.

Once App Analytics is enabled, you’ll have a wealth of actionable data at your fingertips. Now, that’s a step in the right direction for your app’s success on the AppExchange marketplace!

## Data types in AppExchange App Analytics

In the domain of AppExchange, data serves as a compass, guiding you through the maze of decision-making processes. AppExchange App Analytics presents three major categories of data types that shed light on your application’s performance and user interactions, enabling you to enhance your strategies. These data types comprise the following:

- Package usage logs
- Package usage summaries
- Subscriber snapshots

Let’s dive deeper into each of these categories.

### Package usage logs

Package usage logs empower you with comprehensive data on user behavior and interactions within your application. These logs encapsulate information about the interactions of users with each packaged component in your application, including every UI, API-based, Lightning-based, and Apex operation. Additionally, they log each **Create, Read, Update, Delete** (**CRUD**) operation on components and custom objects in your packages. This excludes events from scratch orgs. With package usage logs, you get to analyze user interactions and adoption rates, and consequently make informed decisions concerning feature development.

This data is available for a short window of 45 days from the date the log event occurred. Quick action is required if organizations want to archive or analyze this data.

However, reading package usage log data requires a specific approach. Each line in the log describes an interaction of a user with one of your packaged components. Hence, to comprehend that interaction, one needs to scrutinize each log line, identifying the accessed packaged component, the person interacting with it, and the nature of that interaction.

### Package usage summaries

Package usage summaries are your go-to for high-level metrics organized by calendar month. They provide you with an understanding of user access to your package and the operations performed. These summaries capture UI, API-based, Lightning-based, and Apex operations, logging each CRUD operation on components and custom objects in packages. However, they exclude events from sandbox, scratch, and trial orgs.

Package usage summaries are available for download for a significant 10 years from the summary file log date. This extended period ensures that organizations can conduct longitudinal studies and track package usage trends.

These summaries are typically accessible to both partners and subscribers at the start of the following month. For instance, you can obtain a usage summary for May at the beginning of June. As an AppExchange partner, you can request monthly usage summaries using the **AppAnalyticsQueryRequest** object in the SOAP API from the LMO that owns the package.

### Subscriber snapshots

Subscriber snapshots provide you with a momentary overview of subscriber activity. They offer a daily snapshot of org, package, and custom entity data, recorded at 00:00 UTC and immediately available for download. You can request a snapshot for a specific date and time or a range of dates and times, receiving one snapshot per valid date and time requested.

By examining these snapshots, you can identify usage trends by org and package. This data allows you to understand the tendencies of your subscribers and make informed decisions to enhance your package. Similarly to package usage logs, snapshot data can be downloaded for 45 days from the snapshot date. Given its nature, it’s essential to capture and analyze this data promptly to understand subscriber behavior and attributes at specific moments.

There’s one more thing. Not all App Analytics data is gathered in every type of Salesforce environment. The specifics of the data collection can vary significantly based on the org type and status, as shown in the following table:

| Data type | Production | Sandbox | Scratch org | Trial |
|-----------|------------|---------|-------------|-------|
| Package usage log | Yes | Yes | No | Yes |
| Package usage summary | Yes | No | No | No |
| Subscriber snapshot | Yes | No | No | Yes |

Table 10.1: App Analytics data collection across different org types

## How to request data

Efficiency is key when handling data in AppExchange App Analytics. While manual data requests might work for ad hoc needs, they could prove to be tedious in the long run. Package Visualizer is a handy tool for such spontaneous data requests, as detailed in Chapter 2 of this guide. However, to streamline your operations and ensure continuous insights, you should consider scheduling automated queries.

### Scheduling and automating your queries

The crux of automation is writing code that will create query request records on your schedule, monitor them, retrieve the data, and store your AppExchange App Analytics data for easy access. For instance, you can store the data in a custom object in your PBO.

There are several automation options at your disposal, including the following:

- Custom API integrations using REST or SOAP API calls
- Salesforce DX automation using the CLI
- Salesforce flows
- Apex

For example, you can automate the retrieval of package usage summaries using Apex scheduled jobs. As you get started, storing logs in your PBO org using Apex would be a sufficient approach. However, when you decide to automate the retrieval of package usage log data, which can significantly increase in volume over time, you should consider a more scalable storage solution, such as Heroku or **Amazon Web Services (AWS)**.

### Automating log downloads using Apex

To streamline the process of downloading logs, you can use Apex to create an automatic process. This will require you to develop a simple solution that should be capable of the following tasks:

- **Regularly make AppAnalyticsQueryRequest requests:** This can be achieved through the implementation of a schedulable Apex class. This class allows you to specify the timing of when these requests should be made, helping to automate the process.
- **Download logs**: This involves performing an **HttpRequest**. This will retrieve the logs from the server and download them to your specified location.
- **Parse retrieved CSV files**: Once the CSV files containing the logs have been downloaded, the next step is to parse them. This involves interpreting the data and transforming it into a more manageable format.
- **Create PackageUsageSummary__c records**: After parsing the CSV files, you’ll use this information to create **PackageUsageSummary__c** records. These records can then be stored and used for further analysis.
- **Write Apex unit tests**: Writing unit tests in Apex is critical to ensure the robustness and reliability of your solution. This helps you to detect and fix problems early, thereby improving the quality of your software.

To help you kickstart this development, examples of the aforementioned steps are available at the following GitHub repository:
https://github.com/developerforce/partner-intelligence-basics. Here, you’ll find samples and guides that you can use to set up your Apex implementation for automatic log downloads.

### Managing data storage and using external solutions

As the volume of your data grows, it might become challenging to store all of it in your PBO. In your base Salesforce setup, without buying any extra licenses or add-ons, you’re given 10 GB of data storage. This sounds like a lot, right? Well, to put it into perspective, this allows for roughly 5 million records. Now, is that a ton? Let’s think about it for a moment. Just considering 1 year of package usage summary logs from a single big client
