
Machine learning and AI processes can use the full history or the refined data as their input data and produce more refined data, which in this way can be added naturally to the desired data marts.

## Feeding back to the source systems

The last potential step in the data flow is about the data generated in the data platform and the desire to use it in other systems.

One common use case is taking some data calculated in the data platform back to some operational system. This is very common in marketing and CRM, where the data platform can be used to calculate customer segments and all sorts of measures that then need to be fed back into the marketing tools and CRM platform.

Another increasingly common use case is when some machine learning process is building valuable data for operational systems, such as sales forecasts or predictive maintenance figures.

In all these cases, we can similarly approach the topic as we have seen with moving the data from the source system to the data platform, with the big difference that many systems come with some way to get data out but offer little to no way to load data in.

Even when the target application does have an API to load back data, it is way less common for data loaders to have the same wide range of abilities to load data back.

Despair not, as there are tools that focus on the “reverse ETL” process, that is, moving the data back from the data warehouse to the business tools/operational systems, such as Census (available at `https://www.getcensus.com/`).

Another case of data generated in the data platform is when the people using the BI tools can edit data in the reports on the BI tool and have it somehow sent back to the data platform itself. This is often called the write-back feature in BI tools.

Some sample use cases are when people can use their BI tool to enter missing information or to provide updated values in specific reports, such as managing inventory levels or updating forecasts.

In this case, it is much better to treat the new data, written back from the BI tool, as new input for the data platform and integrate it as part of the transformation chain, instead of directly changing the data mart that is powering the report. This is again in the spirit of making the changes to data visible and being able to reproduce the final result from scratch without having to deal with the previous state.

Now that we have explored the data life cycle and its evolution, we will move on to learning about the modern data stack, which has made data engineering much more seamless.

## Understanding the modern data stack

When we talk about data engineering, we encompass all the skillsets, tooling, and practices that cover the data life cycle from end to end, as presented in the previous section, from data extraction to user data consumption and eventually including the writing back of data.

This is a huge set of competencies and tools, ranging from security to scripting and programming, from infrastructure operation to data visualization.

Beyond very simple cases, it is quite uncommon that a single person can cover all that with a thorough understanding and good skills in all the areas involved, let alone have the time to develop and manage it.

### The traditional data stack

The traditional data stack used to be built by data engineers developing ad hoc ETL processes to extract data from the source systems and transform it locally before loading it in a refined form into a traditional database used to power reporting. This is called an ETL pipeline.

The process included creating some ad hoc integration to extract the data from the source system, find what was to be extracted, and design an extraction logic. Often, it also included the required data transformations, including summarization, before finally loading the “digested” data into an otherwise-normal database to be used for reporting.

Using a transactional database such as MySQL or SQL Server for reporting poses huge issues and calls for all sorts of tricks to make reporting possible. The main issue is that those databases are optimized for many small operations that interact with the whole line of data in a table. They are row oriented.

The fact is that analytics is the opposite: it requires a few queries on a big amount of data that often just use a few columns instead of all the possible ones. To solve the needs of analytical workloads, columnar databases have been created, but in the first decade of the 2000s, they were not widely known, and they were still quite expensive if confronted with “normal” row-oriented OLTP DBs.

One of the most problematic compromises to be done using traditional OLTP DBs was the application of business rules and summarization of data before loading it into the reporting DB. This created a situation where any change in the source data or business rules would affect the whole history of the data. This in turn called for a fix in the transformation logic and in the existing data to conform to the new logic. As you can see, this is double work, and scary too, as you had to modify the only copy of your historical data.

Another issue was that size and power of the tools were limited as well as the time to write integrations and then keep them running, so only the really needed data was loaded, often retaining only limited periods of time.

The data analysts would take the data loaded in the database and build reports and dashboards for the end users, plus doing the occasional deep dive into some aspect of the data.

Even in these deep dives, they just had what they had: if something was summarized too much, they could not do anything but ask for a new load, and if some useful data was not available, they could again just ask the data engineers to add it, by creating another ETL pipeline.

No wonder the final users were often left to wait for new data to be added or received half-answers with the available data.

The division of work between the data engineers and the analysts also made it more difficult for the data analysts to develop a good understanding of the data and its limitations, and for the data engineers to understand the use and quality of the data that they delivered.

Usually, little to no tests or data quality checks were in place, leaving the data team exposed to continuous firefighting at both ends of the data life cycle: changing/missing data from the source systems and stale/bad data in the reports.

Another point is that given the technical difficulties of keeping the ETL pipelines working and the distance of the data engineers from the business questions, often, more effort went into making the data flow, a traditional and objective metric no one can dispute, than understanding the data and making sure of its quality.

### The modern data stack

Fast forward to the mid-2010s and we find ourselves in a much better situation:

- Analytical data warehouses in the cloud, such as BigQuery, Redshift, and Snowflake, are more and more common and becoming an affordable option for most data teams
- Data movement/integration tools, such as Stitch and Fivetran, make it very easy to move the data from traditional databases and common business platforms to the cloud data warehouse
- Modern BI tools start to provide improved abilities for self-service reporting that power users can exploit

The advent of cloud-based data warehouses is the turning point as it made available a huge amount of storage and processing power without the need to buy capacity upfront.

On the consumer side, this enabled companies to load in these cloud **Data Warehouses (DWs)** vast amounts of data and to process it much faster and cheaper, spending a fraction of the cost of on-premises systems.

On the commercial side, this enabled the creation and rise of data movement tools that made it quick and painless to load data to one of these standard cloud DWs from the most used systems, such as traditional DBMS and operational and SaaS software.

Adopting this modern stack made it simple to easily load raw data from the source systems into your cloud DW, reducing the need for the most complex and time-consuming jobs in the data engineering world.
