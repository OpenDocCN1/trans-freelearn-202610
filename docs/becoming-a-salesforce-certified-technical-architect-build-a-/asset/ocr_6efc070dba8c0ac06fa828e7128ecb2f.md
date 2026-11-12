I assumed that the number of shipments is relatively low. I assumed earlier that there are 100,000 orders created every year. Therefore, I am assuming two shipments per order. That is 200,000 shipments per year every year. According to the scenario, PME sold 5 million devices last year, with 10% year-on-year growth. This means that the shipment line item object could become an LDV after a few years.

Therefore, I propose using a MuleSoft scheduled Batch job to delete shipments sold by the distributors for more than a year from Salesforce. This assumes that these values will already exist in the inventory management system. Therefore, they would not be completely lost after they get deleted from Salesforce. I have considered this as another integration interface in my diagram.

You might think that coming up with all these words during the presentation is too difficult. That is true if you are not utilizing your diagrams. Note that the diagrams will contain all the info about authentication and security. You are simply using your diagrams in your presentation to tell the end-to-end story.

Update your data model, landscape architecture, and list of integration interfaces. Then, move on to the next requirement:

The account manager should be notified if more than three days have passed without receiving a confirmation.

This is another requirement that can be fulfilled in multiple ways. Your proposed solution could be the following:

I will utilize standard reports and dashboards for this. I will create a report showing overdue tasks of a specific type. I am assuming that a report will be created for each account manager and will be scheduled to be sent daily. This will ensure the account manager is notified. I could have used time-based workflows as well, but I believe reports will be good enough.

On a regular basis, the distributor's fleet managers update each device's inventory status, identified by its manufacturer ID.

Most of this has been covered already in previous solutions. However, make sure you read the requirement fully. Do not risk skipping a requirement and therefore missing a valuable point. Your proposed solution could be the following:

The first part of the requirement is already fulfilled using the new custom objects that we introduced. The distributor's fleet managers will be granted read/write access to these objects' status fields using field-level security. Moreover, I already mentioned that the shipment object would be linked to the order using a master/detail relationship. This means the records will be visible to all partner users who have access to the order record.