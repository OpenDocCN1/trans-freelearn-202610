![](bbox=[0.3945, 0.0977, 0.5684, 0.1709])

Figure 9.34 – Business rule task

A business rule task allows us to simplify our diagrams significantly by eliminating many unnecessary decision gateways and sequence flows, but most importantly, it makes our automated processes more flexible.

Imagine a company that sells its products in different markets to different customer segments. Some order more, some order less. Some are regular, long-time customers. Some are new customers we don't know yet. We can offer goods to the public sector through tenders or to private entities. Add to that different countries, legal systems, and currencies and, as you can see, the situation gets quite complicated. Will we offer the same price to everyone? Or will we vary the levels of discounts?

If we were to model the sales process, we would have to decide how we would approach the discount calculation. Shall we try to describe everything in the form of a single diagram? Such a diagram would be large and complicated to read. Moreover, based on such a model, it is extremely difficult to determine whether our rules are consistent, free of internal contradiction, or whether we have forgotten some important combination of conditions relevant to a certain customer segment. Modifying rules described in such a way is difficult and risky.

Such problems occur not only in processes related to discounts but also in determining insurance premiums, pricing complex services, sophisticated purchasing processes, or those related to accepting invoices or other documents, where there are multiple levels of acceptance, depending on the type and amount.

As you can see, using business process diagrams to show such aspects of **decision management** may not always be an acceptable choice. Luckily, there is a better way, popularized by **The Business Rules Manifesto**, which suggests that business process models should not contain detailed written rules, but that rules should be described and managed differently.

These rules are not only used for documentation purposes. A special type of software, known as a business rules engine, also allows those rules to be automated, and a BPMN business rule task assumes that a process can use this approach. By using a business rule task, we can greatly simplify our diagram by presenting a piece of the process where we refer to business rules, and then showing how this can affect our process.

As you will learn in the last part of this chapter, we can extend this even further thanks to the additional specification provided by OMG: **Decision Model and Notation (DMN)**.

If you would like to learn more about the concept of business rules, I would suggest reading The Business Rule Manifesto at `https://www.businessrulesgroup.org/brmanifesto.htm`.