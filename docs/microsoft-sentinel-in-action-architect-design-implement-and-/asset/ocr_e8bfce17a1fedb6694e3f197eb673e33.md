That is all there is to creating a workspace using the Azure portal. While this is very easy to do, there may be times when you will want to perform these same actions using command-line actions, and that will be described next.

# Creating a workspace using PowerShell or the CLI

There are times when you need to be able to consistently recreate an Azure Sentinel environment. Perhaps you are just testing all the various configuration options, creating environments for many different subscriptions for an international company, or creating instances for customers. No matter the reason, if you need to create many Microsoft Sentinel environments that are all the same, using PowerShell or the **Command-Line Interface (CLI)** is a better option than doing it in the Azure portal.

# Creating an Azure Resource Management template

When creating a new Log Analytics workspace using PowerShell in this lab, you will use an **Azure Resource Management (ARM)** template to perform the actual configuration. While you can create the workspace directly using either of the technologies, using an ARM template provides additional benefits, including being able to easily recreate the workspace, use the ARM template in a DevOps workflow, or use it in Azure Blueprints.

> **Note**
> 
> A complete discussion of ARM templates is beyond the scope of this lab, but briefly, an ARM template is a JSON file that describes what needs to be created in Azure. It contains parameters, which are the values that a user will provide to determine items such as name, location, and pricing tier. It can also have variables, which are internal values that can be used to determine other values. It will also have a list of one or more resources, which are the Azure resources to create.

Go to `https://docs.microsoft.com/en-us/azure/azure-monitor/platform/template-workspace-configuration` and copy the JSON text. You will be pasting this into a file that you create later.

In this example, you will be prompted for the workspace name and the location, but the pricing tier will default to `pergb2018` due to the presence of a `defaultValue` entry. If you do not wish to have those defaults, you can either change the values shown or remove the entire `defaultValue` line, including the comma at the end, in which case you will be prompted for the values when executing the command.