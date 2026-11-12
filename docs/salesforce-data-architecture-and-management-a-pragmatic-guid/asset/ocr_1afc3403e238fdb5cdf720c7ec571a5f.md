# # Salesforce Page Optimizer

This is a tool specifically designed to monitor the performance of Salesforce Communities. It is provided as a plugin for Google Chrome and provides actionable insights into how the page is performing. We will discuss this in more detail later.

# # Salesforce Lightning Inspector

This is another Google Chrome extension that helps to diagnose performance bottlenecks. It can be used to inspect different aspects of an application, such as events that fired and client-side storage details. The **Performance** tab shows details of how long components are taking to get executed and where the potential bottlenecks are.

# # Salesforce reports

Salesforce has out-of-the-box tools and reports that can be used to monitor the limits on API calls. We established earlier that API limits can create serious issues for business-critical applications that rely on external systems via integrations or AppExchange packages.

Out of the box, you can use the System Overview page, which shows the API calls for the last 24 hours as seen in the next screenshot. You should also consider setting up API usage notifications that send an email every _x_ number of hours when the API usage reaches a certain threshold. The system keeps on sending emails until the API usage goes below the specified threshold:

![](bbox=[0.166, 0.55, 0.838, 0.879])

Figure 6.10 – The total number of API calls made in the last 24 hours; in this case, 20