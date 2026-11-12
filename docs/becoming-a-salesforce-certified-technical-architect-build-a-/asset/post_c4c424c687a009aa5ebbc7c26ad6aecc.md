
PLU shared the following requirements for the customer service process.

## Customer Service

PLU is looking to expand the number of support channels. Furthermore, they are looking to introduce a more governed and standardized way of handling their customers. The new system must meet the following requirements:

- All customers should be able to create inquiries or complaints using a self-serve customer portal or by calling the call center. These requests should be assigned to the right agent based on multiple factors, including language, incident type, and customer type.
- If a complaint is not resolved within seven days, the SVP of the service should be notified.
- The system should automatically generate a forecasted meter reading for the next month based on the previous month's reading.
- The forecasted and actual electricity and gas consumption for every site should be displayed in the customer portal. The data should cover the last 24 months. Customers should also be able to view their past invoices and payments and should be able to view a PDF version of their invoices.
- If an energy failure is reported, a critical incident should be created and assigned to the right maintenance partner based on the property's address. All customers in the impacted region should receive an SMS and email message upon incident creation, status update, and incident resolution.

PLU shared the following requirements for the scheduled manual meter reading process.

## Scheduled Manual Meter Reading

The manual meter reading process is still required for older meter models. PLU shared the following requirements:

- Manual readings are expected to be taken every quarter. In some countries, this must happen every month due to regulations. The field service technicians drive or walk by the residential house or the business establishment and use the radar reader devices to identify nearby compatible readers served by PLU.
- The radar reader device has a screen that displays the meter ID of nearby PLU-served meters. The device can communicate with the meters to get their readings. The radar reader is not connected to the internet. However, it can be paired with a nearby Bluetooth device.
- PLU would like the radar reader devices to be paired with the field service technician's mobile phone, then use the phone to send the reading data to Salesforce.
- PLU is looking to optimize the visiting time and travel costs for their field service technicians.
- At any given time, PLU would like to track the location of its field service technicians.

PLU shared the following requirements for the scheduled automatic meter reading process.

## Scheduled Automatic Meter Reading

The automatic meter reading process is most commonly used across different sites/properties. PLU shared the following requirements:

- Smart meters are widely used across the countries covered. However, PLU has worked with four different vendors during the past three years. Each provided and installed their own smart meter devices. Each vendor has a different cloud-based platform used to control and communicate with the meters. The last three months’ meter readings are also stored on these platforms. PLU currently has subscriptions to all four cloud-based platforms.

- Smart readers must be read monthly. Two of the device models can push data periodically to a server. All models support pull operations. PLU is looking to unify the way it retrieves the smart meter data from all its vendors. All four platforms offer a rich set of SOAP and REST APIs. They offer web services that can be used to pull the meter readings from a smart meter. In addition, two of them also offer a pub/sub interface that allows a real-time recipient of meter readings.

PLU shared the following requirements for the customer registration process.

## Customer Registration

The customer portal represents a significant part of PLU’s strategy to modernize customer service. They shared the following requirements for customer registration:

- Customers cannot self-register in the community except via invitation. The B2C customers’ access to the portal should be generated after signing up for a PLU service.

- PLU would like to expose its products to unauthenticated users via a public website. The users can subscribe online to PLU’s service by providing information such as the number of households, expected power consumption, and address details. The system should automatically determine and display the right tariff for the customer. PLU is expecting this to happen via integration with Power Sales. The UI should be responsive to both browsers and mobile phones.

- The customer should be able to confirm the tariff. This should generate the necessary objects in Salesforce to store the customer and contract details. Furthermore, user access to the customer portal should be created.

- Upon the creation of user access, the customer should receive an email notification to set a password. The password must meet strict complexity requirements.

- Once logged in, the customer should be able to view their contact and contract details. Customers should be able to log in to the portal using a PLU-branded mobile application as well.

- Customers should be able to invite one more contact to the portal to co-manage a particular property and contract.

PLU shared the following requirements for the field sales process.

## Field Sales

The field sales process is essential to develop the B2B business. PLU shared the following requirements:

- The field sales agent visits a potential B2B lead and walks them through the different offers available using a handheld tablet. PLU would like to track all activities done with the client, even if the client decided not to use PLU’s services.

- Three days after completing the visit, an email survey should be sent to the B2B customer’s primary contact. Two different survey templates should be used, one for successfully signed deals and another for lost deals. If the field sales agent’s score is below 3 out of 10, a case should be automatically created and assigned to the field sales agent’s manager.

PLU shared the following data migration requirements.

## Data Migration Requirements

Considering the previously shared information about the current landscape, PLU shared the following data migration requirements:

- PLU has over 80 million customers in its legacy CRMs. The data is expected to contain a significant number of duplicates. The number of unique active customers is likely to be in the range of six million. PLU would like to migrate active customers only to Salesforce. PLU would like to understand how they can deduplicate the migrated records and link them with their corresponding ERP records, knowing that the same duplication also exists in the ERPs. There is no plan to do any significant data cleanup in the ERP.

- The legacy CRMs have details for over 200 million meters. The vast number of records is due to significant record redundancy. The actual number of meters to migrate is expected to be less than 10 million. PLU would like to clean up the data and maintain a single record for each meter to develop a 360-degree view of assets.

PLU shared the following accessibility requirements.

## Accessibility and Security Requirements

PLU is looking for guidance to design a secure solution. They shared the following requirements:

- Key customers and their meter readings are only visible to the key customer manager, who manages that customer and their managers, except for support agents, who can view all accounts in the org.

- The key customer manager should be able to delegate the visibility of a customer account to another manager for a specific period. Once that period is up, the record should not be visible to the delegated manager anymore.
- A complaint is only visible to the agent who is managing it and their manager. PLU would also like to define a set of super users who can view all complaints in the system.
- Inquiries should be visible to all support agents.
- The maintenance partner records should be visible to the support agents only. However, they should be editable only to the support agent who manages the direct relationship with that partner.
- B2B customers should be able to manage all properties and meters related to their account.
- B2C customers should be able to manage all their related properties. It is common to have a B2C customer associated with more than one property.

PLU shared the following reporting requirements.

## Reporting Requirements

PLU requested a rich set of reporting capabilities, which include the following:

- The global SVP of service would like a report showing service requests handled by the maintenance partners for a given year compared to data from the past four years.
- The global SVP of service would like a dashboard showing the number of inquiries and complaints received and resolved broken down by country and region. The dashboard should indicate the number of incidents resolved within the target timeframe versus those that ran over.
- Key customer managers would like a set of business intelligence reports showing business improvements gained by switching key customers from the previous tariffs to new tariffs.
- PLU would like to offer their customers a dashboard showing the change in their consumption across the past two years.

PLU shared the following project development requirements.
