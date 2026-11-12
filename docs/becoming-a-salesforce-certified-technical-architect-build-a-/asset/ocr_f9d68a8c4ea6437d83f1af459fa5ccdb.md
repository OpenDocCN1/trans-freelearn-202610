The company uses a heavily customized CPQ solution that they are planning to retain. We will integrate with that solution using fine-grained APIs exposed by MuleSoft as the solution itself offers a poor API. Power Sales will be invoked at the right stage to calculate the price for a custom quote.

The company also relies on electricity and gas smart meters and regular radar-based meters that need to be supported. MuleSoft will again play a key role here as a unified integration middleware.

The proposed solution is replacing a set of legacy CRM solutions across the target countries. We will explain the data migration strategy later, but we propose an ETL tool such as Informatica to facilitate that.

The proposed solution is based on a single Salesforce org. It is understood that the company has recently acquired a new company that is also using Salesforce as its core CRM solution. But I believe that the two businesses are different and would likely need different sets of business processes. Furthermore, I assumed that the acquired company would continue to operate independently for a few years.

Based on that, I believe that we can start with a single org to accommodate the company’s requirements. This org will host all the functionalities described in this scenario. The acquired Salesforce org will continue to operate independently, and MuleSoft will be used to link that org with other systems.

The proposed solution will also utilize Marketing Cloud for customer segmentation, email, and mobile communications. We will also use some third-party applications from AppExchange, such as DocuSign.

I have created a full list of required integration interfaces and the authentication mechanism for each. I have also created a data model diagram, which I will explain further during this presentation.

You have set the scene and briefly explained your solution. Remember to call out your org strategy (and the rationale behind it) during your presentation. You can now proceed by explaining the licenses they need to invest in. Switch to the actors and licenses diagram and continue with your pitch. Remember to use your body language to assert your authority on the stage by moving across it (if you are doing the presentation in person). Your presentation could be as follows:

I have identified the following actors. They will need different types of licenses based on their expected roles. For example, the field sales agents [point to them on the diagram] will need a Sales Cloud license because they need to manage the new and renewal opportunities for customers.