When decoupling services, keep the dependencies as minimal as possible. For example, if the decomposition is done for a particular business domain, take the tables and recreate them in a separate database. You can not only alter the database schema but also use any other persistence technology to keep its domain-specific information. If some transactions span multiple services, start decomposing the service that has minimal dependency on other services.

Since the monolith to microservices transition is not a one-step process, to keep the actual application up and running, you need to replicate the data to let the system work. This is needed until all the services are decomposed as separate services.

The following diagram shows the purchase workflow of a standard e-commerce system. Once the user hits the checkout service, the user is taken to a payment gateway to pay, the order is generated, and an email notification is sent:

![](bbox=[0.15, 0.38, 0.85, 0.55])

Figure 4.5 – Series of operations that occur when the checkout operation is used in an e-commerce system

Considering the previous example, the first service to transition is a notification service that has no dependency on other services. However, order generation is dependent on this service because when the order is generated, the notification needs to be sent out.