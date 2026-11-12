Before you start, make yourself familiar with the six integration patterns mentioned earlier in the Design Your Integration Interface Using the Right Integration Pattern section on using the right integration pattern. In this mini scenario, you will go through practical examples of some of these integration patterns. This chapter will focus on the logical thinking required to design an integration interface.

You are also advised to go through the scenario once to build an initial picture of the required solution. Then, go through the requirements and try to solve them yourself. Once you are done with your attempt, compare that with the suggested solution. Try to practice explaining the end-to-end solution to someone else, who would play the role of a panel judge, describing a particular integration interface.

# Scenario

**Packt medical equipment (PME)** has been selling medical equipment to health providers worldwide for the past 75 years. PME currently sells more than 20,000 different device models from various brands. PME has experienced massive growth in recent years. They sold nearly 5 million devices last year, and they expect their sales to grow by 10% per year.

# Current Situation

PME operates in multiple geographies. Each has a variety of CRM solutions tailored to working with distributors. PME would like to replace all the existing CRM solutions with a Salesforce-based solution. However, they would like to retain two existing systems as they believe they offer a valuable set of tailored services:

+   - A centralized, browser-based **Enterprise Resource Platform (ERP)** system, used by the PME account managers to view the financial details of distributors they have a relationship with. The system is accessible only from within the company's intranet.

+   - A centralized inventory management system that holds data about the devices available at each location. It includes updated information about the arrival date and time of devices in transit to distributors. Moreover, the system allows for getting information about the devices at distributor locations.

Data is currently entered manually into the inventory management system. The system is not integrated with other systems and does not have any exposed APIs. The system utilizes an MS SQL database to store the data. The ERP system is also not integrated with other systems. However, it has a rich set of SOAP and REST APIs.

Users are required to authenticate to each of these systems before they can use them. Authentication is currently done separately as each of these systems stores its own user credentials.

The ordering process consists of three stages, namely negotiation, confirmation, and shipping. Orders are placed quarterly.