
Other benefits of AWS Quick Start include a free trial of Cloud Pak for Data for up to 60 days and a detailed deployment guide. However, it requires an AWS account – the customer is responsible for infrastructure costs. For storage, the customer has two options: OpenShift Container Storage (OCS), which was recently renamed OpenShift Data Foundations (ODF), or Portworx.

![](img/50e1dac0750b69f2b8e8efff000eb13b_144_0.png)

Figure 6.3 – Terraform automation

Also available on AWS, Azure, and IBM Cloud is Terraform automation, whose benefits include the following:

- Easy full-stack deployment to allow consistency and repeatability
- Increased productivity with Infrastructure as Code to write and execute code to define, deploy, and update your infrastructure in minutes
- Idempotency, with code that works correctly no matter how many times you run it
- Self-service through automation for the entire deployment process to schedule and kick off deployments as needed

## Azure Marketplace and QuickStart templates

Azure Marketplace is an online applications and services marketplace ideal for IT professionals and cloud developers interested in Cloud Pak for Data software and its services. It enables customers to discover, try, buy, and deploy a solution in just a few clicks. Like AWS, Microsoft requires an Azure account and one of the two supported storage options, namely OCS or Portworx. Also, customers can have a trial at no cost for up to 60 days:

![](img/50e1dac0750b69f2b8e8efff000eb13b_145_0.png)

Figure 6.4 – Cloud Pak for Data on Azure Marketplace

Let's move on to cover Cloud Pak for Data as a service.

## Cloud Pak for Data as a Service

Most of IBM's data and AI products are currently available as a service and packaged under a single Cloud Pak for Data subscription that is consumption-based, allowing customers to only pay for what they use. Furthermore, to accelerate the modernization of existing workloads and help customers with moving to the cloud/managed services, IBM has launched an initiative called Hybrid Subscription Advantage (HSA) that offers existing software customers discounts to use SaaS instead of software. In this section, we will cover a detailed overview of Cloud Pak for Data as a Service, including the capabilities available, how it's priced and packaged, and how IBM is enabling its customers to modernize through HSA.

As mentioned before, Cloud Pak for Data as a Service allows customers to modernize how they collect, organize, analyze, and infuse AI with no installation, management, or updating required. In other words, it helps deliver all the benefits of an integrated data and AI platform without the overhead of managing the infrastructure or software. The different services constituting Cloud Pak for Data as a Service are the same as for Cloud Pak for Data software, with the existing gaps addressed as part of the roadmap. Here is a short list of available services along with their descriptions and value propositions:

- 1. **IBM Watson Studio**: Watson Studio democratizes machine learning and deep learning to accelerate the infusion of AI in your business to drive innovation. Watson Studio provides a suite of tools and a collaborative environment for data scientists, developers, and domain experts. It includes capabilities to deploy models with IBM Watson Machine Learning and manage them at scale, ensuring accuracy and governance with Watson OpenScale. Last but not least, it comes with Auto AI, which enables normal users to build complex models with just a few clicks.
  - a) **IBM Watson Machine Learning**: IBM Watson Machine Learning is a full-service IBM Cloud offering that makes it easy for developers and data scientists to work together to integrate predictive capabilities with their applications. The Machine Learning service is a set of REST APIs that you can call from any programming language to develop applications that make smarter decisions, solve tough problems, and improve user outcomes.
  - b) **IBM Watson OpenScale:** IBM Watson OpenScale tracks and measures outcomes from AI throughout its life cycle and adapts and governs AI in changing business situations. It also helps with drift explainability and bias detection.

![](img/50e1dac0750b69f2b8e8efff000eb13b_147_0.png)

Figure 6.5 – Watson OpenScale

- 2. **IBM Watson Knowledge Catalog:** Simplify and organize your enterprise data with IBM Watson Knowledge Catalog, making it easy to find, share, and govern data at scale. Customers can create a 360-degree view of their data, no matter where (or in what format) it is stored, auto-discover the data from a myriad of supported data sources, collaborate with fellow users, and control data access by defining policies and monitoring enforcement.

- 3. **Data Virtualization** enables you to query data across many systems without having to copy and replicate it, saving time and reducing costs. Data Virtualization queries data from its source, simplifying your analytics by providing the latest and most accurate data. The Data Virtualization service automatically organizes your data nodes into a collaborative network for computational efficiency. You can define constellations with large or small data sources. Data is never cached in the cloud or on other devices. Also, credentials for your private databases are encrypted and stored on your local device.

- 4. **IBM Db2**: A fully managed, highly performant relational data store running the enterprise-class Db2 database engine, IBM Db2 is built to take on the toughest mission-critical workloads on the planet, with advanced features such as adaptive workload management, time travel query, query federation, in-database AI, row/column access control, auditing, and support for JSON, XML, and geospatial datasets. Customers can independently scale and manage the compute and storage requirements for their deployments. Self-service managed backups to object storage with point-in-time recovery allows data to be restored to any specified time while all customer data is encrypted in motion and at rest.
  - a) **IBM Db2 Warehouse**: A fully managed elastic cloud data warehouse that delivers independent scaling of storage and compute. It delivers a highly optimized columnar data store, actionable compression, and in-memory processing to supercharge your analytics and machine learning workloads.

- 5. **IBM DataStage** offers industry-leading batch and real-time data integration to build trusted data pipelines across on-premises and hybrid cloud environments allowing any integration style (ETL or ELT) to prepare data for AI. Please note that ETL stands for **Extract, Transform, Load**, while ELT is extract and load followed by processing. Work with your peers on DataStage Flows and control admin, editor, or viewer access to your projects. Easily perform data integration work in a no-code/low-code environment with a friendly user interface. Scale horizontally or vertically as needed in a secure cloud environment. Take advantage of shared platform connections and integrations with other products in Cloud Pak for Data.

- 7. **IBM Analytics Engine** helps develop and deploy analytics applications using the open source Apache Spark and Apache Hadoop. Customize the cluster using your own analytics libraries and open source packages. Integrate with IBM Watson Studio or third-party applications to submit jobs to the cluster. An HIPAA readiness option is available in the Dallas region for standard hourly and standard monthly plans. Define clusters based on your application’s requirements by choosing the appropriate software package, version, and size of the cluster. Use the cluster if required and delete it again when all the jobs have run. Customize clusters with third-party analytics libraries and packages, and deploy workloads from IBM Watson such as IBM Watson Studio and Machine Learning. Build on the ODPi-compliant Apache Spark and Apache Hadoop stack to expand on open source investments. Integrate analytics tools using standard, open source APIs, and libraries.

![](img/50e1dac0750b69f2b8e8efff000eb13b_150_0.png)

Figure 6.8 – IBM Analytics Engine

- 8. **IBM Match 360 with Watson** helps quickly build data pipelines for analytics and other data science use cases using master data. Start with your IBM MDM Advanced or Standard Edition entities, or with any data assets containing party information from the knowledge catalog and quickly map and model new attributes to your data model for a more complete view of your customers. The AI-powered matching engine speeds up configuration using statistical methods clients have relied on to produce accurate results. Results can be accessed via RESTful APIs, exported to flat files, or viewed online via the entity explorer. Extend existing master data entities from MDM Advanced or Standard Edition with governed data assets.
