# # Assigning a correlation token to each service request

A microservices architecture is distributed in nature and many services interact with each other to complete a business use case. A correlation token is a unique string (preferably, a **globally unique ID, or GUID**) that is assigned to each request for troubleshooting purposes.

For example, if there is a long-chain operation where many services are involved, passing the correlation token to the service helps to investigate the issue easily if any of the services fail during that transaction. Usually, each service has its own database and keeps the correlation token within the database record as well.

These two things are worth mentioning:

+   - Have a correlation ID that correlates the end-user request to requests made to various microservices so that we can quickly locate logs in each of the services that were involved in an end-user request.
+   - The cost to store and make logs searchable can be large and, for that, some companies end up sampling or storing the logs for a shorter period—both approaches have their pros and cons. If sampling is used, care should be taken to ensure that all microservices involved in an end-user request keep logs of the same end-user request or we won’t be able to track a failed request.

The following points should be considered when discussing correlation tokens:

+   - When is the correlation token assigned?
+   - Who is responsible for assigning the correlation token?
+   - Do you save correlation tokens in the service's database?
+   - What is the format of the correlation token?

# ## Defining a microservices chassis framework

A microservices chassis framework is a foundational framework for fundamental cross-cutting issues such as logging, security, exception handling, tracing and distribution, and so on. The following aspects are important to consider when evaluating this factor:

+   - Do you have a microservices chassis framework in place?
+   - Are your services secured and do you handle security constraints inside each service?