
- **Level of control that's desired over the infrastructure:** The level of control that's desired over the infrastructure is an important factor that impacts the choice of service. In some cases, the cloud-provided service may lack some desired functionality and does not permit changing the underlying infrastructure. In this case, the data engineers may want to choose a service that offers complete control of the infrastructure.
- **Complete data isolation:** Some stringent compliance frameworks do not permit the use of multi-tenant services. In such cases, complete data isolation is mandatory, thus impacting the choices that may be suitable.

> **Important Note**
> There is an ongoing debate around whether cloud services are truly compliant with well-known frameworks such as HIPAA. The multi-tenancy nature of cloud storage is a huge roadblock in making that claim. It is fair to say that cloud storages support compliance frameworks but are not truly compliant.

- **Prior familiarity with certain frameworks:** This is an extremely important factor for organizations migrating their analytics from on-premises to the cloud. Since they are already familiar with a certain tool or framework, they tend to choose the closest option available in the cloud.
- **Cost:** Finally, the cost of the service is a major factor that impacts what choice should be made. Typically, the easier the usage and management of a particular service, the higher its cost. Therefore, before choosing a service, careful cost analysis is deemed necessary.

> **Important Note**
> As a data engineer, it is understandable to have a bias toward specific tools, languages, and frameworks. But once you leave, it becomes the customer's responsibility to manage this. Try to keep your bias aside and provide the customer with a solution that uses technologies that the customer feels comfortable with.

# Performing data engineering in Microsoft Azure

Data engineering in Microsoft Azure can be performed using the following three options:

- Self-managed data engineering services (IaaS)
- Azure-managed data engineering services (PaaS)
- Data engineering as a service (SaaS):

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_76_0.png)

Figure 3.1 – Data engineering option in Microsoft Azure

# Self-managed data engineering services (IaaS)

In the early phases of data engineering, using well-known distributed frameworks such as Hadoop, Spark, and Kafka rose sharply. As a result, many organizations were deploying Hadoop/Spark/Kafka using on-premises infrastructures. Since Hadoop/Spark/Kafka are multi-node frameworks, this meant the installations were performed using physical and virtual machines hosted on either the organization's owned or co-located data centers.

Then came the period when the cloud started to become a reality and organizations started to move their Hadoop/Spark/Kafka clusters to the cloud. In the very early stages of the cloud evolution, using a self-managed service was perhaps the only option available for data engineering.

# IaaS use case

Here are a few use cases where using an IaaS layer is warranted:

- An organization that has heavily **customized** Hadoop/Spark/Kafka clusters. This means they are tied to specific versions that are unavailable on the cloud PaaS equivalent service.
- The organization has developed its own libraries for data engineering and data science that can't easily be deployed on PaaS or SaaS offerings on the cloud.
- An organization where security regulations do not permit the use of multi-tenant cloud services. This forces them to use single-tenant dedicated cloud resources.
- An organization that cannot tolerate variability in the compute power of the use of shared services.

Let's take a look at how a typical Hadoop/Spark/Kafka deployment is performed using self-managed services that use the **Infrastructure as a Service (IaaS)** layer on Microsoft Azure.

**Azure Virtual Machines (Azure VMs)** allows you to create on-demand compute resources for data processing. The following list describes some of the features available using Azure VMs:

- Take the inventory of the on-premises Hadoop/Spark/Kafka nodes – the total number of nodes, aggregated CPU, and aggregated memory.
- Shortlist virtual machines on Microsoft Azure that offer similar aggregated CPU and memory as the on-premises inventory. This step requires careful planning and calculations. It is not necessary to procure the same number of nodes on Microsoft Azure, so long as the aggregated CPU and memory meet or exceed the on-premises deployment. In distributed computing, horizontal scalability of nodes is preferable. It is highly recommended to compare different classes of virtual machines on Microsoft Azure; in many cases, you may be able to procure smaller class virtual machines that are cheaper than higher class ones, both offering the same aggregated CPU and memory.
- Procure virtual machines on Microsoft Azure.
- Configure networking among nodes, open required firewall ports, and install operating system prerequisites.
- Install and configure Hadoop/Spark/Kafka and any other ecosystem tools, such as Sqoop, Oozie, Hive, and so on. These steps need to be performed on multiple nodes.
- Install and configure administration and monitoring software.

Here are a few pros and cons of self-managed services:

- Performing deployments using virtual machines provides the data engineer with complete freedom to choose any framework and any version, and have complete control over its dependencies.
- It should be evident from the deployment process outlined previously that choosing self-managed services requires careful planning, design, and execution.
- Due to the vast variety of touchpoints, the data engineering team in charge of the deployment needs to be skilled in several areas, including cloud deployments, networking, administration, and monitoring. Note that all these skills are above and beyond the usual expectations of a data engineering team, which would normally involve things such as pipeline development and data orchestration.
- Scaling a cluster using a self-managed service can become a challenge.

As we mentioned previously, deploying a Hadoop/Spark/Kafka cluster using self-managed services is not optional. However, there are ways you can minimize the effects of the limitations we have just discussed.

# Hadoop/Spark/Kafka distributions

The deployment process of Hadoop/Spark/Kafka clusters using VMs involves multiple nodes. A simpler way to deal with installing, upgrading, administrating, and monitoring these clusters is by using distributions such as **Cloudera**, **Hortonworks**, and **MapR**. Each of these distributions include software that enables cluster provisioning using a minimal setup. **Ambari** enables users to easily create, administer, and monitor a Hadoop cluster, whereas **Cloudera Manager** enables users to provision **Hadoop** and **Spark** clusters.

# Infrastructure as Code

**Infrastructure as Code (IaC)** is a practice of deploying cloud resources such as VMs using configuration files. Using this practice, you may either create the entire infrastructure or make modifications to it. Using IaC significantly decreases manual effort for reoccurring tasks while maintaining a high level of consistency and accuracy. We will discuss this practice in greater detail in *Chapter 11, Infrastructure Provisioning.*

Azure Resource Manager (ARM) centralizes IaC deployments using resource templates.

# Containerization

**Containerization** is the practice of abstracting application code from the underlying infrastructure. Using containerization provides several benefits, including segregating the host environment, choosing a software version, and dependency management. This means the cluster nodes can now run in their own isolated environment called containers. These containers can be orchestrated using the well-known orchestration engine known as **Kubernetes**. **Azure Kubernetes Service (AKS)** allows you to easily deploy and manage containerized applications.

> **Important Note**
> In the cloud era, a lot of organizations are adopting technologies that keep them cloud-agnostic. Who knows which vendor will drop their prices and when? Containerizing their applications keeps them open to moving between cloud vendors as desired.

# Azure-managed data engineering services (PaaS)

Right after the big data wave started to gain momentum, cloud vendors such as Microsoft Azure started to create managed services to provide customers with fast deployments and easy management. Before we go into the specifics of the services, let’s focus on what an Azure-managed service that uses the **platform as a service (PaaS)** layer on Azure means:

- The resource is deployed with minimal configuration and user input.
- Azure takes care of monitoring the resource for downtime and recovers it automatically in case a failure is detected.
- Globally available.
- Well integrated with other Azure services.
- Automatically provides data redundancy.
- Contains built-in security mechanisms for data protection.
