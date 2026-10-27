While the cardinality of a relationship is the main driver in a data model and its implementation, the time perspective, which we will discuss in the next section, is one of the main drivers behind the semantics of the entities that are part of a context and therefore are used in a model.

Now that we have seen the basics of E-R modeling, it is time to look at a dimension that heavily influences the outcome of the model that we will draw: the time perspective.

# Time perspective

Another important aspect that we have not yet discussed is if and how the time perspective will influence an E-R model. The short answer is yes, it will, and often, this remains an implicit assumption, remaining mostly underexplained.

An E-R models a part of reality and expresses the constraints, or business rules, that a system trying to capture that reality should abide by. The role of the system, and specifically its time perspective, deeply influences the semantics of the entities, what rules are in force, and therefore the model that describes the entities and relations in that context.

Consider an application that manages a fleet of taxis. For that application, if you were to describe the direct relation between Driver and Taxi, you would say it is one-to-one, as a taxi needs a driver and a driver can drive one taxi at a time. This is pretty much the viewpoint of every application that uses an E-R model to describe how live objects collaborate and what they can do at a certain point in time.

![](bbox=[0.26, 0.52, 0.73, 0.58])

Figure 3.10: Relation between Driver and Taxi in a live application

In this case, it is clear that a driver drives one taxi and one taxi is driven by a driver at a time, so the upper bound is one, but what about the lower bound? Well, both Driver and Taxi make sense without the other, as a driver can have a day off or be available as a backup, and a taxi still exists even when it is not being driven.

The same kind of reasoning can be true for every operational application that has been designed to satisfy the operational needs only, without consideration for reporting.

In some cases, even in an operational application, the time perspective becomes embedded into intermediate entities that the organization already uses, such as shift, contract, performance, and so on. These are entities that have been created to keep track of the individual instances of a relationship over time.