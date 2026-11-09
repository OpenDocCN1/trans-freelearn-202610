# Building BizTalk application

In this section, we will discuss the BizTalk components required in order to build out a solution that will consume a Custom SharePoint list and generate a flat file.

# Generating schemas for SharePoint Web Service

In order for BizTalk and SharePoint to effectively communicate with each other, we need a contract between the two systems. We will establish this contract by generating schemas for the SharePoint Web Service by using the Consume Adapter Service Wizard. This will ensure that when BizTalk sends a request to SharePoint, it is able to understand the request and provide a response that BizTalk is expecting. Before generating our SharePoint schemas, we need to do one thing and that is add the schema that allows us to communicate with the database table that we previously created.

+   1. Now that we have the Database structure in place, we can use the Consume Adapter Service Wizard to generate our `GetListGUIDTypedPolling.xsd`. Detailed instructions on how to generate this schema are not covered in this chapter as Typed Polling is covered in *Chapter 2, WCF SQL Server Adapter*.
+   
+   2. The Web Service exposes a Web Service Description Language (WSDL) much like most other Web Services. We want to consume this WSDL using the Consume WCF Service wizard. We can launch this wizard by right mouse clicking on the **Chapter8-ManipulatingLists** project and selecting **Add**.

![](bbox=[0.38, 0.47, 0.62, 0.58])