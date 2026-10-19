
## Source

Data sources are locations that store data required for data analytics. Some well-known sources of data are listed here:

- Databases and data warehouses
- Files
- **Application programming interfaces (APIs)**
- Sensor data from **internet of things (IoT)** devices
- Social networking and other **software-as-a-service (SaaS)** sites

## Ingestion

A typical pipeline starts by ingesting data from one or more data sources. Over the years, data engineers have employed both push and pull mechanisms for accomplishing this action. Depending on the source of data, data engineers may need to choose between batch ingestion or streaming ingestion.

The complexity of ingesting from a data source is directly dependent on whether the nature of data is static or dynamic. Ingesting from a static data source is simple—the real complexity in data engineering arises when dealing with dynamic data sources. Data engineers need to employ effective **change data capture (CDC)** techniques to ingest data in an incremental fashion.

> **Important note**
> **Azure Data Factory (ADF)** loads incremental (delta) data using the watermark technique. A watermark is the last updated timestamp of data that was fetched in the last iteration.

## Transformation

Before the ingested data is put to real use, it needs to pass through several maturity-gaining operations such as **curation**, **standardization**, **declassification**, **deduplication**, and—finally—**aggregation**. This process is collectively referred to as **data transformation**. In other words, the process of data transformation matures data to such a level that it can be readily and easily consumed for analytical operations.

Depending on the use case, data transformation can be a very complex operation. The complexity depends on several factors, including the following:

- Number and variety of data sources
- Complexity of business logic required for cleaning, declassification, and aggregation operations
- Frequency of data ingestion and subsequent data transformations
- Size of ingested data and how far back in time the data gets aggregated—days, weeks, months, or years

## Destination

A data destination is a location that stores the results of the transformations. A data destination can be one of the following two types:

- **Final**—This destination type stores the end results of the transformations. In traditional architecture, it is referred to as the transformation layer, and in lakehouse architecture, it is called the gold layer.
- **Intermediate**—This destination type stores the intermediate results of the transformations. In traditional architecture, it is referred to as the raw or curation layer and in lakehouse architecture, it is called the bronze or silver layer.

## Workflow

A workflow can be best described as a unit of work that has a sequence of actions. A workflow is designed in such a way that it can function in a repeatable fashion, usually triggered by a pre-defined schedule or event-based. A typical workflow is a collection of the following actions:

- **Input and output (I/O) actions**—Actions that perform data ingestion from data sources and storing data in destinations
- **Transformation actions**—Actions that perform data transformations
- **Control actions**—Actions that control the exception of the workflow including chaining, forking, joining, decision-making, and looping

## Monitoring

After a workflow has been triggered, each action in the workflow needs to be closely monitored. Monitoring should be set up and configured in such a way that it stores the state of each action for every pipeline run in the form of logs. The operational team should be alerted if any action failed so that corrective actions can be performed.

Monitoring data pipelines has several benefits, including the following:

- It helps to establish the overall health of the data pipeline running, and its success or failure.
- It helps establish the total runtime for the data pipeline. This information is critical for calculating the miss rate for **service-level agreements (SLAs)**.
- It helps identify performance bottlenecks. Having this information helps data engineers fine-tune code and manage resources.

> **Important note**
> In a data pipeline monitoring system, some key metrics to look out for are latency, throughput, and errors.

The understanding of what each component does in isolation, and how it contributes to the overall success of the data pipeline helps data engineers conceptualize, design, and develop components with a great degree of efficiency.

In the following sections, we will describe the various phases of the data pipeline creation process. In addition to the overall understanding of each phase, we will additionally learn about recommended tasks and actions that need to be accounted for. This list will hopefully give readers a solid baseline that can be further enhanced and built upon during real projects.

## Process of creating a data pipeline

In analytics-centric organizations, it is very common to have multiple data pipelines, each one addressing a different use case. To make matters worse, each use case may be owned by a different sub-group within the organization and require a different dataset. In such cases, it becomes extremely important to carefully plan and design the data pipeline operation so that efficiencies can be discovered and repetitive work can be avoided. The creation of data pipelines is done in phases. In the subsequent sections, we will learn about each phase separately.

Before we deep dive into the details, the following diagram is important to highlight how each phase stacks on top of the other. The most important thing to notice in this diagram is that if data engineers diligently follow the recommended actions for each phase, the workload for each phase significantly decreases, and success is virtually guaranteed:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_106_0.png)

Figure 4.2 – Various phases of data pipeline creation

Let's explore the steps that can help data engineers kick-start the pipeline design process.

## Discovery phase

The process of creating a data pipeline should start with a discovery phase. This phase not only gives data engineers an opportunity to know the team better, identify key stakeholders, and shortlist use cases but, more importantly, enables them to identify potential roadblocks and loopholes that could be detrimental to the overall success of a project.

### Identifying a product owner

A product owner is someone who has ultimate authority over the overall success of a project. The product owner should have direct access to senior management for matters related to goal setting and for keeping them up-to-date on overall progress. Additionally, the product owner should be able to able to answer or gather information around some key questions relating to the following areas:

- Current and future state connectivity of connectivity to cloud
- Security regulations that the organization needs to comply with
- Any SLAs that need to be met
- End-user locations—on-prem, remote, or a combination of these
- How much automation is desired for infrastructure and code deployment
- Whether the deployment needs to be cloud vendor-agnostic

### Shortlisting use cases

Create a list of individual use cases that need to be addressed by talking to each sub-group separately within the organization. Make sure that each use case has a designated owner who will act as the designated **knowledge champion** and has authority over matters relating to data access.

### Conducting discovery sessions

Data engineers should conduct discovery sessions with each sub-group to collect the following information:

- Use-case description and end goals.
- Identification of data sources—location, connectivity information, any challenges with establishing connectivity such as **multi-factor authentication (MFA)**, tokens, and firewall issues.
- Identification of datasets—list of tables, files, or APIs with corresponding data type and schema.
- What is the frequency of schema changes?
- The approximate data size of historical and incremental data for each data source.
- What is the approximate size of incremental data?
- What is the tolerable data lag between **online transaction processing (OLTP)** and **online analytical processing (OLAP)**? Is it a day, a few hours, or near real time?
- Details regarding data consumers—data analysts/data scientists.
- Is the intended use case brand new or did the **extract, transform, load (ETL)** code previously exist?
- If the use case did previously exist, ask for the existing ETL code. Also, ask for any supporting dashboards and reports.
- If the use case is brand new, ask for a written document explaining the intended joins, aggregations, and filters.
- Does **personally identifiable information (PII)** need to be masked?
