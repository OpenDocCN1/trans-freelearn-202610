The following section will discuss implementing an auto-scaling mechanism within your application.

# Using Auto Scaling to Deliver Highly Available Applications

By incorporating Amazon EC2 Auto Scaling into your application design, you can maximize the benefits of the AWS cloud. For example, auto-scaling provides a higher tolerance for failure by automatically shutting down unhealthy instances and replacing them with new ones. Amazon EC2 Auto Scaling can also use multiple Availability Zones to deploy instances in different zones.

Auto-scaling also improves the availability of goods and services by automatically adjusting resources to meet user demand. This ensures that your application has the necessary resources to operate efficiently.

Furthermore, Amazon EC2 Auto Scaling enables more intelligent budgeting by allowing the elastic scaling of resources on demand. Launching EC2 instances only when needed and terminating them when not in use helps save costs since you only pay for the instances you utilize.

When incorporating auto-scaling for EC2 to achieve elasticity and load balancing for high availability, you end up with a robust solution that offers reliability, scalability, and cost-effectiveness.

Now that you have understood the architectural pattern for building scalable, highly available, and cost-effective solutions, it’s time to examine the security of your data and application. In the next section, you move on to encryption technologies on AWS and examine AWS Key Management Service (AWS KMS).

# Introduction to Encryption on AWS and KMS

Security is paramount when you plan to host and access your application in the cloud. In Chapter 2, Securing Access with AWS Identity and Access Management, you discovered the importance of authentication and authorization. Later, in Chapter 4, Building Private Networks in the Cloud with Amazon VPC, you learned about network security and measures to ensure that only specific traffic related to our application was permitted. In this section, you will learn about data encryption and services on AWS that can help ensure your data isn’t readable to anyone who does not have authorization to view the data. This is a critical component of your overall security design.

The following are two types of encryption methodologies that you need to be aware of when working with data:

+   - **Encryption in transit** – This refers to encrypted data en route between network endpoints. It may be stored unencrypted on a disk.
+   - **Encryption at rest** – This refers to data that is encrypted when stored on a disk. As the data is created/stored on a storage system, it is encrypted. Accessing the data from a disk would require decrypting it first to make it readable.