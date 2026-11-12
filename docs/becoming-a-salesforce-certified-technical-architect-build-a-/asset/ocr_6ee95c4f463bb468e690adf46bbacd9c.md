PLU shared the following requirements for the scheduled automatic meter reading process.

# Scheduled Automatic Meter Reading

The automatic meter reading process is most commonly used across different sites/properties. PLU shared the following requirements:

+   - Smart meters are widely used across the countries covered. However, PLU has worked with four different vendors during the past three years. Each provided and installed their own smart meter devices. Each vendor has a different cloud-based platform used to control and communicate with the meters. The last three months’ meter readings are also stored on these platforms. PLU currently has subscriptions to all four cloud-based platforms.

+   - Smart readers must be read monthly. Two of the device models can push data periodically to a server. All models support pull operations. PLU is looking to unify the way it retrieves the smart meter data from all its vendors. All four platforms offer a rich set of SOAP and REST APIs. They offer web services that can be used to pull the meter readings from a smart meter. In addition, two of them also offer a pub/sub interface that allows a real-time recipient of meter readings.

PLU shared the following requirements for the customer registration process.

# Customer Registration

The customer portal represents a significant part of PLU’s strategy to modernize customer service. They shared the following requirements for customer registration:

+   - Customers cannot self-register in the community except via invitation. The B2C customers’ access to the portal should be generated after signing up for a PLU service.

+   - PLU would like to expose its products to unauthenticated users via a public website. The users can subscribe online to PLU’s service by providing information such as the number of households, expected power consumption, and address details. The system should automatically determine and display the right tariff for the customer. PLU is expecting this to happen via integration with Power Sales. The UI should be responsive to both browsers and mobile phones.

+   - The customer should be able to confirm the tariff. This should generate the necessary objects in Salesforce to store the customer and contract details. Furthermore, user access to the customer portal should be created.

+   - Upon the creation of user access, the customer should receive an email notification to set a password. The password must meet strict complexity requirements.

+   - Once logged in, the customer should be able to view their contact and contract details. Customers should be able to log in to the portal using a PLU-branded mobile application as well.

+   - Customers should be able to invite one more contact to the portal to co-manage a particular property and contract.