
In order to meet the requirement to store the generated PDF, I am proposing updating the tablet app so that we can use SharePoint APIs to upload the PDF directly into SharePoint. I have assumed that the tablet app can be updated as it is custom developed. I have also assumed that it is a native app to allow the desired UI capabilities. Once the PDFs have been uploaded into SharePoint, they can be exposed to Salesforce using Files Connect. The actual files will continue to be stored in SharePoint, but only while their links are available in Salesforce.

I am also proposing updating the tablet app so that it uses the Salesforce SDK to retrieve information such as customer name and ID, which will be required to upload the PDF to the right location. The tablet app will authenticate to SharePoint and Salesforce using the OpenID Connect user-agent flow. Each PUB designer will have a user created in both SharePoint and Salesforce. I am assuming I can use the employee ID as a federation identifier to map the users across systems.

Please note that at this stage, you have no details about the IDP being used by PUB. However, a few paragraphs ahead, you will notice the following statement:

All existing systems are connected with the corporate Azure AD for internal users’ SSO.

You can now update your landscape architecture diagram and integration interfaces accordingly. Keep doing that on an incremental basis. You might need to change and redraw things at this stage. Therefore, it is recommended to start on scratch paper and then copy the artifacts into their final form so that you can make them look as professional as possible.

Now, move on to the next paragraph, which starts with the following line:

PUB has a set of CRMs (not Salesforce, except in Italy) that are used to track the project development and sales activities, which is different by nature in each region.

This seems aligned with the plan to utilize a multi-org strategy. PUB wants to integrate each of the regional ERPs with the new system, which will be based on Salesforce. Considering that you are proposing a multi-org strategy, it is fair to assume that each of these ERPs will be integrated with its counterpart regional CRM (that is, the regional Salesforce instance).

The next thing that should come to your mind is how to integrate Salesforce with PUB's ERPs. Going back to the topics you covered in Chapter 3, Core Architectural Concepts: Integration and Cryptography, you know that these systems can be integrated in multiple ways. Considering the pros and cons you covered in that chapter about using middleware and considering the different tools, you should consider the following options:

- Utilize an ESB: Considering that ESBs are capable of handling complex orchestration requirements
- Utilize an ETL tool: Considering that ETL tools are designed for heavy-duty data replication

Note

You are expected to come up with the most suitable solution from a technical perspective. Lean architecture and the ability to scale for an enterprise should always be part of your target; both cannot be achieved with point-to-point integrations.

The scenario did not specify the number of records that are expected to be transferred between the systems. It also did not specify any orchestration requirements. You must make some assumptions here, clearly communicate them, and use them as the base for your proposed solution.

In the review board, you are expected to come up with a clear recommended solution based on the communicated requirements, assumptions, and logic. You can mention the other options, but you must clearly select and communicate the one you recommend.

You can explain your proposed solution for this paragraph’s requirements:

*In order to meet the requirement of integrating the regional ERPs with the new system, which is based on Salesforce, I propose using an ESB as a middleware. There has been no communicated requirement describing the amount of daily data being exchanged between these systems.*

*ETLs are better at handling bulk data replications. Still, I assumed that the number of records is relatively low and therefore it is preferred to use ESB, considering its superior capabilities in handling data orchestration requirements, which is likely to be required by the enterprise. Considering that the new solution will be based on a multi-org strategy, each of the ERPs will be integrated with its regional Salesforce instance.*

*I have also assumed that the ESB will be cloud-based and regionally hosted to ensure that data never leaves the region. I am proposing MuleSoft as an ESB because it is one of the leading ESBs on the market with built-in Salesforce connectors.*

Whenever you are proposing to use a third party, you must name it. You have come across two examples so far: you named the proposed PaaS platform, and now you have named the integration middleware. You do not have to pick Salesforce-owned products. You should pick a product that you are familiar with and that you know for sure will fulfill the requirement. Again, remember that you should cover this requirement without unnecessarily losing time. The preceding paragraph can be presented in 60-90 seconds.

Now, moving on to the next paragraph, which starts with the following line:

*PUB has a central system that is used for scheduled maintenance operations.*

This is a straightforward requirement. PUB wants to replace its legacy scheduled maintenance system with a new one. You do not need to be an expert in field service solutions, but you should be familiar with some of them and understand the capabilities they offer. In this case, your proposed solution could be as follows:

*In order to fulfill this requirement, I propose using Salesforce Field Service. It can be configured to generate scheduled maintenance work orders, which would increase the efficiency of the field service and maintenance team.*

**Note**
Remember to add Salesforce Field Service to your diagram. It is a package that gets installed in your org, so make sure your diagram reflects that. Also, do not forget to include the retiring legacy system. Once this diagram is ready, it will help you present your solution engagingly and attractively.

Now, move on to the next paragraph, which starts with the following line:

*All existing systems are connected with the corporate Azure AD for internal users' SSO.*

The scenario is unlikely to clearly mention the system that acts as an identity provider. You should have enough market knowledge to recognize that Azure AD is one of the leading products in this space. Based on the scenario, it must be acting as the identity provider in this landscape. Make sure you include SSO interfaces as part of your landscape diagram and integration interfaces list.

Moving on to the next paragraph, it starts with the following line:

*Germany has rolled out a mobile application that allows employees to access the German CRM from anywhere in the world when they are connected to the internet.*

Clearly, this is a mobile application requirement. For these requirements, you should be able to select the right mobile strategy. You provided an example of this earlier with the tablet app. This paragraph does not contain enough information to help you select the right mobile strategy, but the next paragraph does. It starts with the following statement:

*Customers should be able to download a PUB-branded mobile application to view their online account, raise tickets against their properties, and update their profile.*

You can select a mobile strategy based on the Salesforce mobile app, a native app, a hybrid app, or an HTML5-based app. Considering that the application should be PUB-branded, you might think that the Salesforce mobile app is not suitable.

However, Salesforce has recently launched a new service called **Mobile Publisher**, which allows you to deliver a branded version of its mobile app to end customers. All possible mobile strategies can provide branding, access to an online profile, and integration with Salesforce.
