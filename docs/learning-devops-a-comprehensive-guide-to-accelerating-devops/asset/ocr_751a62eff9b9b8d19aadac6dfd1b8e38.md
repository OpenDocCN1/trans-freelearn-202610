# # Additionally, we can use the az cli script with the login command:

```
az login
```

Once authenticated, we can run the Terraform workflow.

In our scenario, we begin with an empty Azure subscription without any Azure resource groups; however, in the real world, our subscription might already contain a resource group.

Before running Terraform, in the Azure portal, check that you do not have a resource group in your subscription, as follows:

![](bbox=[0.1318, 0.3083, 0.8658, 0.6694])

Figure 2.9 – No resource group on Azure

To run Terraform, we need to open a command-line Terminal such as CMD, PowerShell, or Bash and navigate to the directory where the Terraform configuration files we wrote earlier are located.

The Terraform configuration is executed in several steps, including initialization, the preview of changes, and the application of those changes.

Next, let’s take a look, in detail, at the execution of these steps, starting with the initialization step.