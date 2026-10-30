
- In Volume Gateway mode, the storage gateway presents block storage. There are two ways of using this, one is **Gateway Cached**, and the other is **Gateway Stored**:
- **Gateway Stored** is a volume storage gateway running locally on-premises and it has local storage and an upload buffer. A total of 32 volumes can be created and each volume can be up to 16 TB in size for a total capacity of 512 TB. Primary data is stored on-premises and backup data is asynchronously replicated to AWS in the background. Volumes are made available via **Internet Small Computer Systems Interface (iSCSI)** for network-based servers to access. It connects to **Storage Gateway Endpoint** via an HTTPS public endpoint and creates EBS snapshots from backup data. These snapshots can be used to create standard EBS volumes. This option is ideal for migration to AWS or disaster recovery or business continuity. The local system will still use the local volume, but the EBS snapshots are in AWS, which can be used instead of backups. It's not the best option for data center extension because you require a huge amount of local storage.
- **Gateway Cached** is a volume storage gateway running locally on-premises and it has cache storage and an upload buffer. The difference is that the data that is added to the storage gateway is not local but uploaded to AWS. Primary data is stored in AWS. Frequently accessed data is cached locally. This is an ideal option for extending the on-premises data center to AWS. It connects to **Storage Gateway Endpoint** via an HTTPS public endpoint and creates **S3 Backed Volume (AWS-managed bucket)** snapshots that are stored as standard EBS snapshots.

## Snowball, Snowball Edge, and Snowmobile

They belong to the same product clan for the physical transfer of data between business operating locations and AWS. For moving a large amount of data into and out of AWS, you can use any of the three:

- **Snowball**: This can be ordered from AWS by logging a job. AWS delivers the device to load your data and send it back. Data in Snowball is encrypted using KMS. It comes with two capacity ranges, one 50 TB and the other 80 TB. It is economical to order one or more Snowball devices for data between 10 TB and 10 PB. The device can be sent to different premises. It does not have any compute capability; it only comes with storage capability.
- **Snowball Edge**: This is like Snowball, but it comes with both storage and computes capability. It has a larger capacity than Snowball. It offers fastened networking, such as 10 Gbps over RJ45, 10/25 Gb over SFP28, and 40/100 Gb+ over QSFP+ copper. This is ideal for the secure and quick transfer of terabytes to petabytes of data into AWS.
- **Snowmobile:** This is a portable data center within a shipping container on a truck. This allows you to move exabytes of data from on-premises to AWS. If your data size exceeds 10 PB, then Snowmobile is preferred. Essentially, upon requesting Snowmobile, a truck is driven to your location, you plug your data center into the truck, and transfer the data. If you have multiple sites, choosing Snowmobile for data transfer is not an ideal option.

## AWS DataSync

AWS DataSync is designed to move data from on-premises storage to AWS, or vice versa:

- It is an ideal product from AWS for data processing transfers, archival or cost-effective storage, disaster recovery, business continuity, and data migrations.
- It has a special data validation feature that verifies the original data with the data in AWS, as soon as the data arrives in AWS. In other words, it checks the integrity of the data.
- Let’s understand this product in depth by considering an example of an on-premises data center that has SAN/NAS storage. When we run the AWS DataSync agent on a VMWare platform, this agent is capable of communicating with the NAS/SAN storage via an NFS/SMB protocol. Once it is on, it communicates with the AWS DataSync endpoint and, from there, it can connect with several different types of location, including various S3 storage classes or VPC-based resources, such as the **Elastic File System (EFS)** and FSx for Windows Server.
- It allows you to schedule data transfers during specific periods. By configuring the built-in bandwidth throttle, you can limit the amount of network bandwidth that DataSync uses.

## Processing stored data on AWS

There are several services for processing the data stored in AWS. We will go through AWS Batch and AWS EMR (Elastic MapReduce) in this section. EMR is a product from AWS that primarily runs MapReduce jobs and Spark applications in a managed way. AWS Batch is used for long-running, compute-heavy workloads.

## AWS EMR

EMR is a managed implementation of Apache Hadoop provided as a service by AWS. It includes other components of the Hadoop ecosystem, such as Spark, HBase, Flink, Presto, Hive, Pig, and many more. We will not cover these in detail for the certification exam:

- EMR clusters can be launched from the AWS console or via the AWS CLI with a specific number of nodes. The cluster can be a long-term cluster or an ad hoc cluster. If you have a long-running traditional cluster, then you have to configure the machines and manage them yourself. If you have jobs to be executed faster, then you need to manually add a cluster. In the case of EMR, these admin overheads are gone. You can request any number of nodes to EMR and it manages and launches the nodes for you. If you have autoscaling enabled on the cluster, then with the increased requirement, EMR launches new nodes in the cluster and decommissions the nodes once the load is reduced.
- EMR uses EC2 instances in the background and runs in one availability zone in a VPC. This enables faster network speeds between the nodes. AWS Glue uses EMR clusters in the background, where users do not need to worry about the operational understanding of AWS EMR.
- From a use case standpoint, EMR can be used for processing or transforming the data stored in S3 and outputs data to be stored in S3. EMR uses nodes (EC2 instances) as the computing units for data processing. EMR nodes come in different variants, including Master node, Core node, and Task node.
- The EMR master node acts as a Hadoop namenode and manages the cluster and its health. It is responsible for distributing the job workload among the other core nodes and task nodes. If you have SSH enabled, then you can connect to the master node instance and access the cluster.
- The EMR cluster can have one or more core nodes. If you relate to the Hadoop ecosystem, then core nodes are similar to Hadoop data nodes for HDFS and they are responsible for running tasks therein.
- Task nodes are optional and they don't have HDFS storage. They are responsible for running tasks. If a task node fails for some reason, then this does not impact HDFS storage, but a core node failure causes HDFS storage interruptions.
- EMR has its filesystem called EMRFS. It is backed by S3 and this feature makes it regionally resilient. If a core node fails, the data is still safe in S3. HDFS is efficient in terms of I/O and faster than EMRFS.

In the following section, we will learn about AWS Batch, which is a managed batch processing compute service that can be used for long-running services.

## AWS Batch

This is a managed batch processing product. If you are using AWS Batch, then jobs can be run without end user interaction or can be scheduled to run:

- Imagine an event-driven application that launches a Lambda function to process the data stored in S3. If the processing time goes beyond 15 minutes, then Lambda has the execution time limit. For such scenarios, AWS Batch is a better solution, where computation-heavy workloads can be scheduled or driven through API events.
- AWS Batch is a good fit for use cases where a longer processing time is required or more computation resources are needed.
- AWS Batch runs a job that can be a script or an executable. One job can depend on another job. A job needs a definition, such as who can run a job (with IAM permissions), where the job can be run (resources to be used), mount points, and other metadata.
- Jobs are submitted to queues where they wait for compute environment capacity. These queues are associated with one or more compute environments.
- Compute environments do the actual work of executing the jobs. These can be ECS or EC2 instances, or any computing resources. You can define their sizes and capacities too.
- Environments receive jobs from the queues based on their priority and execute the jobs. They can be managed or unmanaged compute environments.
- AWS Batch can store the metadata in DynamoDB for further use and can also store the output to the S3 bucket.

> **Note**
> If you get a question in exams on an event-style workload that requires flexible compute, a higher disk space, no time limit (more than 15 minutes), or an effective resource limit, then choose AWS Batch.
