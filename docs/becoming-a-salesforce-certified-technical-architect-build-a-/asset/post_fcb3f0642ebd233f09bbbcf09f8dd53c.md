
Your proposed approach must deliver value to your customer that other approaches might not. You truly must *believe* in this, unlike a salesperson who preaches just to hit a sales target. You are the client’s trusted advisor; when you propose a solution, you must believe in it and clearly articulate its value. Otherwise, your uncertainty will reflect on you during the presentation and Q&A, and you will not radiate the confidence expected from a CTA.

*This information should be sent to the ERP, which generates a customer invoice after 72 hours.*

PPA has three ERPs hosted on-premises, probably in entirely different physical locations. How can you transfer the data from Salesforce to the right ERP system to generate the invoice? How can you connect a cloud-based application such as Salesforce to an on-premises application?

If your answer is anything but *middleware*, then you are likely stepping into a minefield. Proposing a point-to-point approach here would likely result in a failure in the integration domain. The first challenge would simply be: how would Salesforce Connect to a system protected behind a firewall?

Coming up with unreasonable assumptions such as moving the ERP to a DMZ (which means it is pretty much exposed to the outer world) will open a can of worms that you would not want to deal with. Making such unreasonable assumptions could draw more challenging questions from the judges that you might not be able to answer correctly.

As you already have MuleSoft in your landscape, use it. If you do not have it yet, then this is a solid reason to propose it. You can suggest other middleware as well. Considering that this requirement would need a system capable of orchestrating the call with multiple systems and considering that this interface is not meant to deal with a massive amount of data, an ESB is a reasonable option.

You can propose any ESB tool you feel comfortable with and not stick with MuleSoft just because it is a Salesforce product. Remember, you have to believe in the value of your solution. In this scenario, you are proposing MuleSoft because it is a leading middleware, according to many credible sources. Moreover, it is assumed that you are more familiar with it than other similar middleware. But feel free to propose whatever tool you believe can deliver a similar functionality and that you feel comfortable with.

You still need to specify the integration pattern, authentication mechanism, and how you will protect your data in transit. You might need to explain how MuleSoft will communicate with an on-premises solution during the presentation. You came across a similar situation in *Chapter 11, Communicating and Socializing Your Solution*.

When you select the pattern, you need to consider the object(s) you plan to copy over to the ERP.

Based on your proposed data model, you would likely need to sync the *Order*, *OrderItem*, and *Case* objects, assuming that the *Account* and *Contact* tables are already synced along with lookup tables such as *Product*.

Which pattern do you think would serve you best here? Which Salesforce functionality can be used to sync all three objects with as little technical burden as possible? You can think of the following functionalities and tools:

- **Outbound messages**: Outbound messages can only contain data from one object. However, you can send the lead object (such as `Order` in this case) to MuleSoft, request MuleSoft to do a callback to retrieve the rest of the objects, and then transfer them to the ERPs. This is a good option that you should shortlist, although it requires a bit of custom development in MuleSoft.

- **Platform events**: They can be designed to contain data from multiple objects. However, you are dealing with an object that has one-to-many relations here. You cannot model a single platform event that combines data from the parent and the child records except if you go with non-standard solutions such as defining a long text field with JSON content. You are discouraged from proposing such an approach due to the associated technical challenges (creating the JSON structure, populating it, and parsing it at the other end) unless it is unavoidable.

- **Future methods**: You can develop an Apex callout to invoke a remote service exposed by MuleSoft and pass the data. Considering that this needs to happen in an asynchronous fashion, you would need to use future methods (or queueable interfaces). You are discouraged from proposing such an approach due to the associated technical challenges (additional custom development, retry mechanism, logging mechanism, and so on) unless it is unavoidable.

- **Salesforce Flows**: Using Flows, you can invoke the remote web service using a configurable point-and-click functionality. This sounds good, but you still need to consider the required supporting functionalities, such as retry and logging mechanisms.

- **Batch data sync**: This approach might sound old-fashioned, but it is effortless to put in place and, on many occasions, good enough for the business. You can configure a Batch job in MuleSoft to run every hour, pick the relevant records from all target objects, then send them over to the ERP in the right order. This is likely to be the most cost-effective option as well.

The evaluation of the preceding functionalities depends on the use case you are trying to solve. For example, in other use cases, you might find that platform events are optimal.

Once the data is synced to the ERP, you can assume that the ERP has all that is needed to generate the invoice in the given timeframe. Update your landscape architecture diagram and your list of integration interfaces, then move on to the next requirement.

Once an invoice is generated, a notification would be sent to the client with the invoice amount and instructions to settle any remaining balance.

You can solve this requirement in multiple ways. You can assume that the whole process is going to take place off-platform. The ERP generates the invoice and the email, then sends them across to the client.

Alternatively, you can go with a totally on-platform solution, where the ERP generates the invoice and its line items and MuleSoft replicates that to Salesforce, then fires an email notification from Salesforce containing the invoice and its detailed line item. However, in this case, you would need to deal with at least 10 million invoices every year and at least an equal number of invoice line items with the potential to go up by 50% (assuming half the invoices contain two line items). This means you will need to consider the new invoice and invoice line item objects in your LDV strategy.

However, if you read the scenario’s requirement carefully, you will notice that the requirement is to share the invoice amount only, probably with an invoice number, issue date, due date, and a few other attributes, which are usually included in the invoice header.

You can either store this information in a custom object related to Order or as attributes on Order itself. Both options are fair as long as you can clearly communicate why you chose one over the other.

A related custom object will allow you to store multiple invoices if needed, such as a provisional and a final invoice. It adds one LDV to your data model, but that is fine as long as you are articulating the value of this approach accurately and including this object in your LDV mitigation strategy.

Remember, there is no one correct answer. There are many ways to solve a problem, but you just have to rationally justify your approach and back it up with a solid technical understanding of the associated pros and cons.

Introduce a new custom object to the data model to hold the invoice header details. You will also require a new integration interface between the ERPs and Salesforce to copy the data. As usual, you need to provide all the required information to describe the interface.

Update your data model, list of integration interfaces, and landscape architecture diagram, then move on to the next requirement.

The sales agent updates the reservation status to closed once the inspection is done.

This event chronologically precedes the ones mentioned in the last two requirements. However, that is not an issue here because this requirement can easily be fulfilled. You just need to ensure you draw the right chronological order in your diagrams. The agent is expected to update Sub_Status__c on Order to closed. Make sure not to miss such easy points.
