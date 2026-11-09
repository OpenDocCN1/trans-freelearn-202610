This pattern involves updating the user interface of an application in response to changes in the underlying data hosted off-platform. This allows the application to provide real-time feedback to the user.

For instance, an ISV could use this pattern to update a cockpit in their AppExchange application whenever the underlying data hosted in their AWS instance changes. When a change is detected, the application updates the Salesforce cockpit to reflect the new data.

# Data virtualization

Data virtualization is an approach to data management that allows an application to retrieve and manipulate data without requiring technical details about the data, such as how it is formatted at source, or where it is physically located. This reduces the risk of data errors and the workload of moving data around that may never be used, and it does not attempt to impose a single data model on the data.

For example, an ISV might use data virtualization to provide a unified view of data from multiple sources in its AppExchange application. The application can access and manipulate the data as if it were all in a single location, even though it is actually spread across multiple databases.

# Choosing the right integration pattern

Selecting the appropriate integration pattern for your application is a key decision that can significantly impact its performance, user experience, and overall success. Before diving into the intricacies of coding, take a moment to engage in thoughtful contemplation. By asking yourself the following questions, you can streamline your development process and potentially save