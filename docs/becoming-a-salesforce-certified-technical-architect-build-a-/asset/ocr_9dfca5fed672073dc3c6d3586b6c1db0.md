PME is looking to modernize its landscape and offer more standardized processes worldwide. Moreover, they are looking to increase the productivity of their users by avoiding double data entry as much as possible.

# Requirements

PME shared the following requirements:

+   - The PME security team has mandated the use of Single Sign-On (SSO) across all the systems used as part of the new solution, including the ERP and the inventory management system.
+   - Device orders should originate from Salesforce.
+   - Before the negotiations stage, PME should be able to set a maximum and minimum allocation for each distributor's device type.
+   - In the negotiations stage, the distributor should be able to place an order, which contains a list of the device models and quantities.
+   - If each model's requested quantity falls within the defined allocation per model, the order should be automatically confirmed.
+   - If each model's requested quantity falls outside the defined allocation per model, the order should be sent to the account manager for approval. The system should automatically retrieve the four values indicating the financial status of the distributor from the ERP.
+   - The account manager should utilize the financial health indicator values, in addition to the historical orders for this distributor, to determine the right action.
+   - When the order is about to be shipped, the inventory management system may create multiple shipments. For each shipment, a list of the devices and their unique manufacturer IDs is created. The shipments and their line items should be visible to the distributor's fleet managers in Salesforce. The status of each line item should be in transit.
+   - When the shipment is delivered, the distributor's fleet managers are given three days to confirm the shipment's receipt. They should confirm the status of each and every device identified by its unique manufacturer ID. The status of each line item should be available in stock.
+   - The account manager should be notified if more than three days have passed without receiving a confirmation.
+   - On a regular basis, the distributor's fleet managers update each device's inventory status identified by its manufacturer ID. They should indicate whether the device has been sold, returned, damaged, or is still available in stock. This information should also be updated in the inventory system.

PME is looking for your help to design a scalable integrated solution that meets their requirements and ambitious roadmap.