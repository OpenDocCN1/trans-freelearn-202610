In the next section, we will learn how we can use Azure API Management to proxy a public API.

# Exercise 2 – using Azure API Management to proxy a public API

When we design a solution to consume one or more APIs, communication between the different services, systems, and scripts will have to go through an API. Before describing these communications and designing our APIs, we must take into consideration several elements, such as the following:

+   - The route structure
+   - Authentication and authorization
+   - Rate limiting

With Azure API Management, before data is received or sent, you can easily proxy an existing API and modify the input and output.

In most cases, we want to modify the structure of an existing public API, add authentication, limit the number of incoming requests, or even cache the results obtained. This is what we will cover in this section, and we will discover the ease of managing an API and consuming it quickly.

We can use an Azure Logic Apps proxy, Azure App Service, or Azure Functions apps, but in our example, we will use a public API.

In this exercise, we will use the previously imported API.

# Importing an OpenAPI schema for proxying

There are different formats that the Azure API Management service will import.

In the list of available methods, the /BillingList method provides a list of billing details in JSON format. Using a standard PowerShell query, as seen in the following code snippet, will allow us to retrieve the results through the API proxy:

```
Result = Invoke-RestMethod -URI '< https://
healthcarebillingapi.azurewebsites.net/BillingList>'
$Result.data.summary
```

Developers tend to make changes to output keys returned to them. We are going to select the method we need to modify; thereafter, we will select the same methods in **Outbound Processing** and select **Add Policy**. After that, select **Other policies**, and in **Transformation policies**, look for the **Find and replace string in body** option.