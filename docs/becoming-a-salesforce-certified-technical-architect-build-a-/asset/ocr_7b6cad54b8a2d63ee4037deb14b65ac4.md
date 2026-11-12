+   • **Relevant Salesforce features:** Callouts could be considered a relevant feature, such as callouts invoked from a Visualforce page or a Lightning component. This does not include an asynchronous callout invoked using future methods or similar capabilities. Salesforce External Services called from Flows or Apex is also relevant to this pattern.

> Note
> You can find more details at the following link: https://packt.link/B7jk7.

# Remote Process Invocation: Fire and Forget

Here is some high-level info about this pattern:

+   • **Description:** This pattern is used to invoke a remote web service and continue with other processes without waiting for a response. The system is not blocked from doing other operations. In a classic single-threaded desktop application, the application would invoke a remote web service and continue with other operations.

+   • **Timing:** This pattern is asynchronous. In some cases, it can be considered near real-time.

+   • **Operational layer:** This pattern operates on both the data layer and the business logic layer, similar to the RPI request and reply pattern.

+   • **Key use cases:** Invoking a remote web service to execute an operation without waiting to know the result. This could include passing a data structure that gets copied to the remote system. This pattern can be modified to include a callback. In that case, Salesforce would expect the remote system to call back once done processing and sending the result. This is a common pattern due to its near real-time nature, agility, scalability, and small overhead.

+   • **Relevant Salesforce features:** Several Salesforce features are relevant, such as outbound messages, platform events, **change data capture** (CDC), the Salesforce streaming API, callouts using future methods, callouts using queueable classes, email notifications, and push notifications.

> Note
> You can find more details at the following link: https://packt.link/dl4cl.

# Batch Data Synchronization

Here is some high-level info about this pattern:

+   • **Description:** As indicated by its name, this pattern is mainly used to copy a dataset from one system to another. The dataset usually contains a bulk of records to be more efficient.

+   • **Timing:** This pattern is asynchronous, usually scheduled to run at a certain time and interval. Execution time is normally subject to the size of the copied dataset.