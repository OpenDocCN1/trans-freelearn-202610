
The amount of time it takes for the auto-discovery job to complete depends on the number of tables, the volume of data, and whether data sampling was used:

![](img/50e1dac0750b69f2b8e8efff000eb13b_92_0.png)

Figure 4.14 – Data quality dashboard

Attaining the desired data quality is an evolving process. Here, we learn over time and establish a collection of data quality rules to handle data conditions, monitor data quality metrics, and fix our applications and/or source systems until we achieve the desired data quality results:

![](img/50e1dac0750b69f2b8e8efff000eb13b_93_0.png)

Figure 4.15 – Quality rule to identify any order quantity <100 or >1,000

4. Finally, we must get to a business-ready state where data is ready to be consumed by the end users for analytics:

![](img/50e1dac0750b69f2b8e8efff000eb13b_95_0.png)

Figure 4.17 – Example DataOps workflow by personas

We now understand the overall process the data goes through and the interactions that occur across the different personas. Now, let's explore how we can bring some automation to data quality assessment.

## Automation rules around data quality

Data quality assessment evaluates data against a set of criteria. This set of criteria depends on the business context of the data, including what information the data represents, what the criticality of the information is, and the purpose of the information. Data quality dimensions and custom data quality rules should be used together to perform data quality assessments. Data profiling provides an initial view of the characteristics and distribution of data. Automation rules leverage this information, from data profiling to assessing quality, such as checking for Null or valid date formats, and so on.

Cloud Pak for Data allows us to construct and deploy automation rules. These automation rules help automate tasks to ensure we have the highest quality data and include the following:

- Applying rule definitions based on data quality dimensions
- Assigning terms or stewards to assets

## Data integration at scale

Based on *IDC reports*, it is estimated that data scientists spend 80% of their time preparing and managing data for building AI models. This, coupled with an IBM survey where 91% of organizations are not using their data effectively, means that businesses are struggling to deliver value from data silos. Data integration is a combination of technical and business processes that are used to combine data from disparate sources into meaningful and valuable information. This is a critical aspect of an organization's overall information architecture strategy:

![](img/50e1dac0750b69f2b8e8efff000eb13b_99_0.png)

Figure 4.21 – Data integration, an enterprise challenge

IBM's DataStage service on IBM Cloud Pak for Data delivers a modernized data integration solution for cleansing and delivering trusted data anywhere, at any scale and complexity, on and across multi-cloud and hybrid cloud environments. There are many reasons for this major shift in how data integration tools are deployed and used with the rise of AI. The increase in volume and variety of data is one, but organizations are seeing a need to adopt a process-oriented approach to manage the data life cycle with DataOps, improve business performance, and increase competitiveness.

## Considerations for selecting a data integration tool

The following are some key considerations we should look at when selecting a data integration tool/service:

- The ability to cater to a multi-cloud environment
- Performance at scale
- Integration with governance catalogs
- A wide array of connectors that support traditional RDBMS sources and other non-traditional sources of data
- The ability to track lineage
- Integrated data quality

## Advantages of leveraging a cloud-native platform for ETL

From an architectural standpoint, IBM Cloud Pak for Data brings data integration to cloud-native platforms by breaking down the monolithic stack into microservices. Some of the advantages of taking this modernized approach include the following:

- Being able to deploy in minutes, enabling standard deployment and management while retaining the flexibility to modify environment/job parameters as required.
- Increased reliability due to inherent high availability in Kubernetes/OpenShift.
- Reduced management burdens due to automated updates.
- You can automate management by "application group," so administrators can use namespaces to manage access control and provisioning options.
- You can monitor and manage at the application level with platform- and service-level features.
- You can scale microservices independently to respond to changing needs:

![](img/50e1dac0750b69f2b8e8efff000eb13b_101_0.png)

Figure 4.23 – Automatic workload balancing

Containerizing your data integration technology enables you to operate in a hybrid cloud environment across on-premises and cloud platforms.

## Master data management

Organizations have a lot of data surrounding products, services, customers, and transactions, and it is important to derive insights from all of this data to maintain a competitive advantage. Master data management is a comprehensive process that defines and manages an organization's critical customer and product data. It provides a single, trusted view into data across the enterprise, with agile self-service access, analytical graph-based exploration, governance, and a user-friendly dashboard to accelerate product information management and master data programs.

## Extending MDM toward a Digital Twin

A Digital Twin is a generic concept that was originally derived from the Internet of Things (IoT) space, wherein we have a digital representation of a physical thing. In the context of MDM, this translates to the ability to capture user interactions and derived attributes that can help service the customers more effectively, using the information that we know about them. This takes us beyond the standard attributes such as name, address, contact details, product details, and others we would source from MDM:

![](img/50e1dac0750b69f2b8e8efff000eb13b_102_0.png)

Figure 4.24 – Extending MDM toward Digital Twin

Once the model has been defined, mapping the source attributes to the attributes in the model can be done by manually mapping them or using the ML-powered process of automatically classifying the attributes. As part of this step, it is possible to also eliminate redundant attributes and/or attributes that will not be mapped back to the MDM attributes:

![](img/50e1dac0750b69f2b8e8efff000eb13b_104_0.png)

Figure 4.26 – Attribute, manual, and automated matching

Once the matching process is complete, the end users can search for Person and/or other custom record types and find results either by entities or individual record types. Let’s use a simple example to understand the value of entity information. Every organization has customer profile data, and they also track the prospects of future business opportunities. Business users can benefit from entity data by getting answers to the following questions:

- Does the prospect list have duplicates?
- How many existing customers are on the prospect list?
- Are there any non-obvious relationships between the customers and prospects?
- Can I get better data points from new sources of information?

## 5
Analyzing: Building, Deploying, and Scaling Models with Trust and Transparency

There is a lot of data collected across an enterprise, and this presents a unique opportunity to harvest insights that can be used to improve efficiency and serve our customers better. **IBM Cloud Pak for Data** has an extensive collection of services from **International Business Machines Corporation (IBM)**, as well as third-party services that help with the various aspects of analyzing data.

The platform caters to different personas including business analysts, data engineers, data scientists, data stewards, and so on, enabling cross-persona collaboration. All of these personas are interested in different aspects of data and they have varied skillsets when it comes to analyzing the data, starting from something as simple as the need to execute canned reports on a regular basis to ad hoc reporting and dashboarding and more advanced analytics such as statistical analysis, **machine learning (ML)**, or **artificial intelligence (AI)**.
