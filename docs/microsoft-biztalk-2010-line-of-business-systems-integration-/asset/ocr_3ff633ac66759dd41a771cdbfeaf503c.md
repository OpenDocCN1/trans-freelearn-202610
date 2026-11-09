![](bbox=[0.25, 0.14, 0.75, 0.4])

There is a starter solution in the code that accompanies this book. In the Chapter 6 folder, you can find a Visual Studio 2010 solution named Chapter6.BuildingAccessManagement.sln. Open this solution prior to building the components below.

# Building the BizTalk solution components

We need to create a single message schema to start with, and then configure our BizTalk and service endpoints.

# Defining message schemas

Our first step is to build the schema definition describing the request for access and then create a web service that accepts a message of that type.

+   1. Open the Chapter6.BuildingAccessManagement.sln solution and create a new BizTalk project named Chapter6.BuildingAccessManagement.BizTalk.
+   2. Configure the project with a strong name key and set it to deploy to a BizTalk application named Chapter 6.
+   3. Add a new schema named AccessRequestEvent_XML.xsd to the BizTalk project.