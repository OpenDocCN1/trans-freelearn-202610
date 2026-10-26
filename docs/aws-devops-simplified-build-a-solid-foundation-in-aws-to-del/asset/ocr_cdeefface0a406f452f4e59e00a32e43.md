Have a look at the following diagram:

![](bbox=[0.139, 0.128, 0.861, 0.553])

Figure 12.3: Simulating failures with AWS Fault Injection Simulator

For a typical web application fronted by a load balancer and application code deployed in EC2 instances that persist data in a Multi-AZ RDS database, one could follow these steps for failure injection, using the FIS service:

+   1. Random EC2 instances in the architecture are shut down, and the application, as a whole, is still expected to work fine as incoming requests should be distributed to the remaining EC2 instances by the load balancer. During these resiliency tests, one starts with a *hypothesis* on what they expect to happen when a failure occurs and confirms that hypothesis by actually carrying out the corresponding action. To continuously check the response of an application API during this time, one could leverage the **AWS CloudWatch Synthetics** service, which repeatedly tests an endpoint, generating traffic just like a regular end user.
+   
+   2. After the compute instances’ failure simulations are validated, one can move ahead with database-level resiliency tests. The FIS service supports restarting the active database instance from an RDS cluster, in a Multi-AZ setup. This should mimic the scenario where a particular AZ is affected, resulting in a trigger of the database failover process.