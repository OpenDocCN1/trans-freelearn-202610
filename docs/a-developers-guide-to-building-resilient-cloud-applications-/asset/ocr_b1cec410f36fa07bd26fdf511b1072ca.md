# Create a storage account

Basics   Advanced   Networking   Data protection   Encryption   Tags   Review

# Project details

Select the subscription in which to create the new storage account. Choose a new or existing resource group to organize and manage your storage account together with other resources.

Subscription *
MVP

Resource group *
(New) PacktRG
Create new

Figure 8.2 – Create a storage account – Project details in the Basics tab

In the following screenshot, you can see how we configure the instance details:

# Instance details

If you need to create a legacy storage account type, please click here.

Storage account name *

Region *
(US) East US

Performance *
Standard: Recommended for most scenarios (general-purpose v2 account)
Premium: Recommended for scenarios that require low latency.

Figure 8.3 – Create a storage account – Instance details in the Basics tab

The data in the Azure storage account is always replicated to ensure durability and high availability. Select a replication strategy that matches your durability requirements. Select from the list of four elements:

+   - Locally-redundant storage (LRS): This is the lowest-cost option. It includes basic features and protection against server rack and drive failures. You can select this option if you have a simple scenario, which is not critical or for testing.
+   - Geo-redundant storage (GRS): This is used in backup scenarios because it is an intermediate option with a failover capability in another secondary region.
+   - Zone-redundant storage (ZRS): This is also an intermediate option, but it provides protection against data center-level failures. It can be used for high-availability scenarios.