# Custom deployment

Deploy from a custom template

Select a template   Basics   Review + create

Automate deploying resources with Azure Resource Manager templates in a single, coordinated operation. Create or select a template below to get started. Learn more about template deployment

Build your own template in the editor

# Common templates

+   - Create a Linux virtual machine
+   - Create a Windows virtual machine
+   - Create a web app
+   - Create a SQL database

# Load a GitHub quickstart template

Quickstart template (disclaimer)  101-databricks-workspace

Deploy an Azure Databricks workspace.

Author: jeffpang
Last updated: 2020-09-21
Learn more

Select template   Edit template

Figure 2.18 – Custom deployment

2. After selecting the template, you need to provide the required values and then select Review + create, and finally Create:

Home >

# Deploy an Azure Databricks Workspace

Azure quickstart template

Select a template   Basics   Review + create

Template
101-databricks-workspace
1 resource
Edit template   Edit parameters

# Deployment scope

Select the subscription to manage deployed resources and costs. Use resource groups like folders to organize and manage all your resources.

Subscription *   Pay-As-You-Go
Resource group *   Create new

# Parameters

Region *   East US
Workspace Name *
Pricing Tier   premium
Location   [resourceGroup().location]

Review + create   < Previous   Next : Review + create >

Figure 2.19 – Creating Azure Databricks from an ARM template