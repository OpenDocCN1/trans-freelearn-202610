The good news is that the platform rewards you for running your code in async by giving you increased governors in areas such as Heap, SOQL queries, and CPU governor. You can find the full details in their documentation (attempting to document them in a book is a nonstarter as they are constantly being improved and removed in some cases).

> Understanding Execution Governors and Limits: http://www.salesforce.com/us/developer/docs/apexcode/Content/apex_gov_limits.htm.

# General async design considerations

The platform provides some straightforward ways in which you can execute code in async. What is often overlooked though is error handling and general user experience considerations. This section presents some points to consider functionally when moving a task or process into async or the background to use a more end user-facing term:

+   • Type of background work: I categorize work performed in the background in two ways, application and end user:

+   o Application is something that the application requires to support future end user tasks, such as periodic recalculation, major data manipulation, or the on boarding of data from an external service, for example.

+   o End user is something that is related (typically initiated) to an end user activity that just happens to require more platform resources than are available in the interactive (or foreground) context. The latter can be much harder to accept for business analysts and end users, who are constantly demanding real-time response times. If you do find that this is your only route, focus hard on the user interaction and messaging aspects as follows.