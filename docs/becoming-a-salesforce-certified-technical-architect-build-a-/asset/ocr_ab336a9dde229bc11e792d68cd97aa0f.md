To synchronize these values back to the inventory management system, I will utilize another MuleSoft scheduled Batch job. I am assuming that it will run every hour and that this delay is acceptable to the business. The interface will copy the changed shipment line item statuses back to the inventory management system. This will be another interface utilizing the Batch data synchronization pattern.

I thought of utilizing platform events or change data capture (CDC) to send the changed data from Salesforce to MuleSoft. But I preferred using scheduled batch to ensure the consistency of delivering all changes to the inventory management system, especially since there was no shared requirement indicating a need for near real-time data synchronization. This interface will also be easier to build, considering that we already have a similar interface operating in the opposite direction.

The same authorization and security mechanisms used by the shipment synchronization interface will be used by this one too.

That was the last shared PME requirement. You can update all the diagrams and see how they look.

Here is the landscape architecture diagram:

![](bbox=[0.158, 0.443, 0.857, 0.742])

# Figure 9.7 – Landscape architecture (final)