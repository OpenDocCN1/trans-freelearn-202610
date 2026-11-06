To set up the integration, follow these steps:

+   1. Ensure that you have *Project Owner* access in Atlas.
+   2. In Atlas, navigate to your project, then click on **Integrations**.
+   3. Click the **Datadog** tile.
+   4. Enter your Datadog API key. You can find or create this in your Datadog account under **Integrations > APIs**.
+   5. Configure which clusters and specific metrics you want Datadog to monitor.
+   6. Optionally, to track high-cardinality metrics, enable `sendDatabaseMetrics` and `sendCollectionLatencyMetrics`. This can be done via the Atlas UI or the Atlas Administration API.
+   7. Once configured, you can access your MongoDB Atlas dashboard and metrics within Datadog.

For more detailed steps and information, refer to the official MongoDB Atlas documentation on Datadog integration at https://www.mongodb.com/docs/atlas/tutorial/datadog-integration/#procedure.

![](bbox=[0.14, 0.53, 0.2, 0.62])

> **Atlas project metrics automatically sent to Datadog**
> 
> If you configure your Atlas project to send alerts and events to Datadog, you do not need to follow this procedure separately. Atlas sends project metrics to Datadog through the same integration used for alerts and events.

Once the integration is complete, you can start exploring the prebuilt dashboards in Datadog to get an overview of your Atlas cluster’s performance. Consider setting up alerts for key metrics such as CPU utilization, disk space, and replication lag to be notified of potential issues. You can also begin to correlate MongoDB metrics with your application and infrastructure data to get a holistic view of your system. We will delve deeper into performance monitoring strategies and alerting best practices in subsequent sections.

# Application performance monitoring platforms

Platforms such as **New Relic**, **AppDynamics**, and **Dynatrace** specialize in **application performance monitoring (APM)**. Their primary strength lies in tracing application requests end-to-end, providing deep visibility into how application code interacts with the database.