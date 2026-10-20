![](bbox=[0.27, 0.08, 0.69, 0.33])

Figure 2.16 – Adding a user

Users can be added to our workspace to work collaboratively with our notebooks and data.

# Working with VNets in Azure Databricks

Azure Databricks can be deployed within a custom virtual network. This is called VNet injection and is very important from a security perspective. When we deploy with default settings, inbound traffic is closed, but outbound traffic is open without restrictions. When we use VNet injection and we deploy directly to a custom virtual network, we can apply the same security policies around all our Azure Services, to meet compliance and security requirements.

In case you are working in data science or exploratory environments, it's good to leave the outbound traffic open to be able to download packages and libraries for Python, R, and Maven, and Ubuntu packages also.

As we have mentioned before, Azure Databricks works on two planes of service. The first is the control page, which we use through the Databricks API to work with workspace assets. The second is the data plane where the clusters are deployed. It is this second plane that is deployed to your customized virtual network. The benefits of this are as follows:

+   - A secure connection to other Azure services, such as Azure Storage, through service endpoints
+   - A connection to on-premises data sources to be used in Azure Databricks
+   - Connecting Azure Databricks to a network virtual appliance to monitor and control outbound traffic