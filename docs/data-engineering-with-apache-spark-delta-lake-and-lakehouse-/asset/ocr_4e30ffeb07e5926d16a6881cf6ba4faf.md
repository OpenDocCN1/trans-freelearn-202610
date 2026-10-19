Then came the period when the cloud started to become a reality and organizations started to move their Hadoop/Spark/Kafka clusters to the cloud. In the very early stages of the cloud evolution, using a self-managed service was perhaps the only option available for data engineering.

# IaaS use case

Here are a few use cases where using an IaaS layer is warranted:

+   - An organization that has heavily **customized** Hadoop/Spark/Kafka clusters. This means they are tied to specific versions that are unavailable on the cloud PaaS equivalent service.
+   - The organization has developed its own libraries for data engineering and data science that can't easily be deployed on PaaS or SaaS offerings on the cloud.
+   - An organization where security regulations do not permit the use of multi-tenant cloud services. This forces them to use single-tenant dedicated cloud resources.
+   - An organization that cannot tolerate variability in the compute power of the use of shared services.

Let's take a look at how a typical Hadoop/Spark/Kafka deployment is performed using self-managed services that use the **Infrastructure as a Service (IaaS)** layer on Microsoft Azure.

**Azure Virtual Machines (Azure VMs)** allows you to create on-demand compute resources for data processing. The following list describes some of the features available using Azure VMs:

+   - Take the inventory of the on-premises Hadoop/Spark/Kafka nodes – the total number of nodes, aggregated CPU, and aggregated memory.
+   - Shortlist virtual machines on Microsoft Azure that offer similar aggregated CPU and memory as the on-premises inventory. This step requires careful planning and calculations. It is not necessary to procure the same number of nodes on Microsoft Azure, so long as the aggregated CPU and memory meet or exceed the on-premises deployment. In distributed computing, horizontal scalability of nodes is preferable. It is highly recommended to compare different classes of virtual machines on Microsoft Azure; in many cases, you may be able to procure smaller class virtual machines that are cheaper than higher class ones, both offering the same aggregated CPU and memory.
+   - Procure virtual machines on Microsoft Azure.