We will fill in all the information required to create a functions app in the following table (see Figure 4.18):

| Setting | Description |
|---|---|
| Subscription | The subscription to use in order to create a new function app. |
| Resource group | You can create a new resource group or select an existing one. |
| Function app name | The name used to identify the function app. Characters that can be used are a-z (case insensitive), 0-9, and -. |
| Publish | Select code files or a Docker container. |
| Runtime stack | Select the runtime that will support the function—for example, the C# class library, the C# script, JavaScript, PowerShell, Java, and TypeScript. |
| Version | Installed runtime version. |
| Region | Select a region near you or near other services that the function will access. |
| Plan type | We have three plans: Consumption plan, Premium plan, and Dedicated plan. If you need more details related to these plans, you can check this link: `https://learn.microsoft.com/en-us/azure/azure-functions/functions-scale`. We will use the Consumption plan and pay only for functions app executions. |

Table 4.1 – Settings and description to create an Azure Functions app

When the functions app is created, we will check the resource group. These resources are added to the resource group:

+   - Application Insights
+   - Storage account
+   - Function app
+   - App Service plan