# Creating and Deploying a Function App in Azure

One of the critical decisions architects and developers make in the phase of architecting a new cloud software or application is how to connect to the backend services, run background processing, run backend tasks, and carry out more tasks, such as scheduling and sending emails, without affecting the main application processing. For this, the Azure Functions app can be useful.

We can use Azure Functions to execute code in a cloud environment in a serverless way. All we need to do is to write less code with a low cost for a specific problem without caring about the whole application, even the infrastructure where we will run it. We will focus, in this case, on logic and business scope. We can execute Azure Functions in response to events as well.

In this chapter, we will cover the basic concepts of Azure Functions and the hosting plan options. We will explore the development of Azure functions and develop durable functions, which are an extension of Azure Functions.

In this chapter, we're going to cover the following main topics:

+   - Exploring Azure Functions
+   - Developing Azure functions
+   - Developing durable functions

# Exploring Azure Functions

Azure Functions is similar to Azure WebJobs, with some differences related to scaling policies, language support, and trigger events. They are both built on Azure App Service. Because Azure Functions is built on the WebJobs SDK, it shares the same triggers and connections with or to Azure services.

Azure Functions acts as a modern serverless architecture providing event-driven, configured cloud computing for application development.