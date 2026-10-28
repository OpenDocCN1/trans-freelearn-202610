In our sample, we will use Http trigger. We can enable Docker.

For Authorization level, we can select Function, Anonymous, or Admin. In our case, we will select Anonymous, which enables anyone to call your function endpoint. In the default level—that is, Function—we have to present the function key in requests to access our function endpoint:

![](bbox=[0.18, 0.2, 0.84, 0.37])

Figure 4.6 – Enabling Docker and configuring authorization level

A function app requires a storage account that can be assigned or created when we publish the project to Azure Functions. If we select Http trigger, we don’t need an Azure Storage account connection string. This is why we need to select Use Azurite for runtime storage account (AzureWebJobsStorage).

We select the Create button to create the function project and Http trigger function:

![](bbox=[0.2, 0.52, 0.81, 0.84])

Figure 4.7 – Configuring a functions app project