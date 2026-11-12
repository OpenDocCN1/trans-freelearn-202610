# Introducing the Overall Solution and Artifacts

Similar to what you did in Chapter 13, Present and Defend: First Mock, start by introducing yourself; this should be no more than a single paragraph. It should be as simple as the following:

My name is [your name], and I am here today to present the proposed solution for [the company's name in the scenario].

Then, proceed with another short paragraph that briefly describes the company’s business. This will help you to get into presentation mode and break the ice. You can simply utilize a shortened version of the first paragraph in the scenario, such as the following:

The scenario describes a European electric utility company that serves several cities across Germany, Italy, France, Portugal, Belgium, and the UK. The company operates in 40 cities. It offers services to both B2C customers (residential) and small and medium B2B customers.

Remember that you should use no more than a minute to present yourself and set the scene. Now you can start explaining the key need of the company using a brief paragraph, such as the following:

The company has been struggling with its existing CRM solutions for many years, and, as a new strategic movement, decided to switch to Salesforce. It is looking to use its new CRM to launch a global, unified new set of sales and service processes.

It is now time to explain the current and to-be landscape architecture. Use and show your landscape architecture diagram to guide the audience through it; never just stand next to it and speak in a monotone. You should be able to look at your diagram and describe the as-is and to-be landscape. Your pitch could be as follows:

The company uses six different ERP systems. They are used for service fulfillment, customer onboarding, switching, and activation. The plan is to retain all of them and integrate the new solution with them using their APIs. It is worth mentioning that the Belgium ERP does not offer an API.

Data will be traveling from Salesforce to these systems and vice versa. To develop a scalable and flexible architecture, we should aim to have a unified communication interface between Salesforce and the ERP systems. Middleware such as MuleSoft would provide that capability. It will also offer the ability to orchestrate the data flow between these systems and handle any encountered exceptions.

MuleSoft will also be able to communicate directly with the flat-file database that the Belgium ERP is based on using its built-in connectors. MuleSoft will not be transferring a massive amount of data. This is what pointed me to the need for an ESB rather than an ETL. However, I am also proposing an ETL to handle data migration.