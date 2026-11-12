> Note that, in this mode, other governors are still active, such as heap and CPU timeout. You might also want to consider combining this mode with the SOQL FOR LOOP approach described earlier. Also, the SOQL Aggregate queries will benefit from this (as these also count against the SOQL query rows governor).

# Processing unlimited result sets using Salesforce APIs

Salesforce does not limit the maximum number of rows that can be queried when using the Salesforce SOAP and REST APIs to retrieve record data, as in these contexts, the resources consuming and processing the resulting record data are off-platform.

One consideration to keep in mind, however, is the daily API limits—although you can read unlimited data, it is still chunked from the Salesforce servers and retrieving each chunk will consume an API call. Although less friendly, the Salesforce Bulk API is more efficient at retrieving more records while using less API calls to the Salesforce servers.

# Asynchronous execution contexts

Salesforce is committed to ensuring that the interactive user experience (browser or mobile response times) of your applications and that of its own is as optimal as possible. In a multitenant environment, it uses the governors as one way to manage this. The other approach is to provide a means for you to move the processing of code from the foreground (interactive) into the background (async mode). This section of the chapter discusses the design considerations, implementation options, and benefits available in this context.

As the code running in the background is, by definition, not holding up the response times of the pages the users are using, Salesforce can and does throttle when and how often async processes execute, depending on the load on the servers at the time. For this reason, it currently does not provide an SLA on exactly when an async piece of code will run or guarantee exactly when the Apex Schedule code will run at the allotted time. This aspect of the platform can at times be a source of frustration for end users, waiting for critical business processes to complete. However, there are some design, implementation, user interface, and messaging considerations that can make a big difference in terms of managing their expectations.