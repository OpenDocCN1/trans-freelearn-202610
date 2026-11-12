
# Partitioned Multi-Cloud Pattern

Applications are deployed in cloud environments on a best-fit basis while maintaining portability.

## Key Components

Some key components in a partitioned multi-cloud pattern include the following:

* **Compute partition**: In this pattern, cloud environments are purely leveraged as an infrastructure deployment platform to support workload portability through adherence to open standards. For instance, services such as AWS EC2, Google Compute Engine, and Azure VMs can be selected based on specific computing needs for a service or application. The same can be extrapolated to hypervisor-level portability for VM-based live migrations that leverage Hyper-V, KVM, or VMware.
* **Container partition**: In this model, a common container orchestration platform is chosen across multiple clouds that can help deploy, port, or migrate applications seamlessly. For instance, services such as **Google Kubernetes Engine** (**GKE**), AWS EKS, and **Azure Kubernetes Service** (**AKS**) can simplify the deployment of a Kubernetes cluster across multiple clouds, including on-premises as a way to develop and deploy modern cloud-native microservices-based applications.
* **Data storage partition**: Leveraging a common data storage architecture can help enterprises seamlessly extend a unified storage platform to enhance data durability, and redundancy, and optimize costs. For instance, AWS S3, Google Cloud Storage, and Oracle Object Storage can be leveraged using a common S3-based storage API.
* **Network partition**: A network partition is critical to a multi-cloud deployment architecture pattern. By leveraging software-defined networking, organizations can define virtual private clouds to fulfill distinct networking requirements while also enabling seamless inter-cloud communication through cross-connects. Several public cloud providers either natively support cross-connects or can be enabled through a cloud exchange service provider such as Equinix, Digital Realty, or CenturyLink.

## Benefits

A partitioned multi-cloud offers some unique benefits to enterprises, such as the following:

* **Best-of-breed approach**: Organizations have the ability to tap into specialized services from different cloud providers, maximizing capability and efficiency by having best-of-breed solutions.
* **Vendor independence**: Avoidance of vendor lock-in is a primary advantage for enterprises, as the partitioned approach mitigates the risk of vendor lock-in, making it easier to switch providers for individualized services as needed.
* **Resource optimization**: The optimal use of resources and specialized services offered by different cloud providers can often lead to better resource efficiency and financial outcomes compared to a one-size-fits-all approach.
* **Standardization**: A partitioned multi-cloud can enable the standardization of enterprise IT that can lead to the seamless adoption of modern technologies and open standards.

## Challenges

While this pattern offers several advantages, including flexibility and cost-effectiveness, it also comes with challenges that should not be overlooked.

* **Complexity**: Managing multiple cloud providers adds a layer of complexity in terms of administration, billing, and governance.
* **Data transfer and integration**: Transferring data between different cloud providers can incur costs and latency issues. Meticulous planning is essential to mitigate cross-cloud latency and egress costs.
* **Security strategy**: Defining and managing a unified security posture and compliance that can be uniformly applied across partitions can become challenging when dealing with multiple cloud providers.
* **Skillset and effort**: A diverse skillset is required to build, manage, and optimize services from different cloud platforms. The standardization of tech stacks and automation is essential to address this challenge.

# Edge Computing Deployment Pattern

In a hybrid edge computing deployment pattern, the primary objective is to share the computational load between a centralized cloud infrastructure and edge computing nodes located closer to the data source. This helps businesses create a flexible deployment pattern, where data can be processed locally at the edge or sent to a central server based on computational complexity, latency sensitivity, consumer experience, or other specific requirements. The typical applications of a hybrid edge compute deployment pattern include the following:

* **Internet of Things** (**IoT**) deployments include devices, sensors, and gateways that are deployed at the edge of the network in various settings such as retail stores, hospitals, manufacturing facilities and logistics. IoT deployments at the edge generate substantial volumes of data that are processed locally using the available computing power. Subsequently, the correlated and consolidated data are transmitted to a central cloud computing infrastructure for further processing and analysis.
* Kiosks, **point of sale** (**POS**) systems, and **next unit of computing** (**NUC**) can process data at the edge to enable extremely fast response times to improve consumer experience or be micro-batched before being sent to central cloud services. The typical applications of this use case can be found in restaurants and retail stores.
* The remote connectivity or areas lacking network availability guarantees can take advantage of this pattern to deliver business-critical services, for example, sea-faring vessels, agricultural farms, remotely deployed surveillance bots/devices that can synchronize data with a central cloud server on a periodic basis.

## Benefits

Some key benefits of this pattern are the following:

* **Low-latency applications**: Hybrid edge computing patterns reduce latency by enabling data processing closer to the source. This is particularly important in applications that require real-time analytics or control, such as autonomous vehicles or industrial automation systems.
* **Efficient bandwidth utilization**: By preprocessing data at the edge, only pertinent information needs to be sent to the centralized cloud, thereby saving bandwidth and reducing transmission costs.
* **Scalability**: This approach allows organizations to start small at the edge and scale upwards, incorporating more centralized cloud resources as needed.
* **Resource optimization**: The hybrid edge model allows tasks to be assigned to the most suitable processing unit, be it an edge node or a central cloud server, based on the resource requirements and constraints.

## Challenges

The challenges of this pattern include the following:

* **Technical complexity**: Managing a hybrid system is more complex than a purely centralized or edge-based model due to the need for dynamic task allocation, data synchronization, and network management.
* **Upfront costs**: The initial setup costs for edge infrastructure can be high. However, this may be offset by the long-term operational cost savings and the value delivered by the overall solution.
* **Security and compliance**: Hybrid models, due to their distributed nature, can present more security challenges that organizations need to actively manage. For instance, securing edge devices is extremely critical, as they can be easy targets for bad actors to gain access to corporate data and **Man-in-the-middle** (**MITM**) attacks.

# Redundant Hybrid Deployment Pattern

Redundant hybrid deployment refers to an architectural pattern where computational resources are strategically replicated across both cloud and data centers. In this pattern, redundancy is introduced at each level to ensure high availability and fault tolerance, enabling tasks and data to be processed in more than one location with synchronization mechanisms in place to maintain data consistency and integrity. Enterprises can leverage this pattern to enable three hybrid use cases:
