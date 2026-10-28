Next, we will continue configuring our project. We need to select the latest framework supported by the worker function. Azure Functions provides many templates to use depending on your scenario, as shown here:

![](bbox=[0.19, 0.17, 0.79, 0.42])

Figure 4.5 – Function templates

By default, we have Http trigger, but we have more, including the following:

+   - **Http trigger**: This is used when the code is triggered by using an HTTP request.
+   - **Timer trigger**: This is used to execute any batch tasks by defining a planned schedule and also for cleanup tasks.
+   - **Cosmos DB Trigger**: This is used to process when Azure Cosmos DB documents are added or updated in collections in the NoSQL database.
+   - **Blob trigger**: This is used for image resizing or to process Azure Storage blobs when they are added to containers.
+   - **Queue trigger**: This is used for responding to messages when they are pushed in an Azure Storage queue.
+   - **Event Grid trigger**: This is used to build event-based architectures if we want to respond to some events delivered to an Azure event grid.
+   - **Event Hub trigger**: This is used in some scenarios, such as user experience, application instrumentation or workflow processing, and **Internet of Things (IoT)**, and to respond to events delivered to an Azure event hub.
+   - **Service bus queue trigger**: This is used to connect your code to other Azure services or on-premises services by listening to message queues.
+   - **Service bus topic trigger**: This is used to connect your code to other Azure services or on-premises services. To do that, we will subscribe to topics.