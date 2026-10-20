![](bbox=[0.115, 0.13, 0.85, 0.39])

Figure 1.1 – Databricks architecture

The immediate benefit this architecture gives to users is that there is a seamless connection with Azure, allowing them to easily connect Azure Databricks to any resource within the same Azure account and have a centrally managed Databricks from the Azure control center with no additional setup.

As mentioned previously, Azure Databricks is a managed application on the Azure cloud that is composed by a control plane and a data plane. The control plane is on the Azure cloud and hosts services such as cluster management and jobs services. The data plane is a component that includes the aforementioned VNet, NSG, and the storage account that is known as DBFS.

You could also deploy the data plane in a customer-managed VNet to allow data engineering teams to build and secure the network architecture according to their organization policies. This is called VNet injection.

Now that we have seen how everything is laid out under the hood, let’s discuss some of the core concepts behind Databricks.