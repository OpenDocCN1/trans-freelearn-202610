# User transactions

All CE server updates are done transactionally. Every update submitted is committed or rolled back atomically. The CE takes care of starting a transaction, performing the operations, and concluding the transaction.

The CE APIs do not have any features to allow your application to start the transaction. You might want to do that if you had multiple operations you wanted to do atomically (and those operations would not be suitable for a single CE API batch update). You might also want to do it if you wanted to include updates to something else (something other than CE objects) in the same transaction.

J2EE APIs include facilities for creating and managing a transaction within an application. The principal interface is UserTransaction. It is because of these J2EE APIs that there are no transactional classes or methods in the CE Java API – they are not needed because they are available from the J2EE container. When you have started a UserTransaction and are using EJB transport, the necessary context for that transaction can be automatically carried to the CE server. In that case, any CE updates will be part of your UserTransaction and will be committed or rolled back based on J2EE transaction calls your application makes.

There is no need to inform the CE Java API about individual transactions because the propagation is automatically taken care of by the EJB programming model. However, to provide symmetric behavior with CEWS transport, the CE server will ignore any incoming client-side UserTransaction context unless the client connection is flagged to participate in the transaction. This is easily accomplished in application code by using ConfigurationParameter.CONNECTION_PARTICIPATES_IN_TRANSACTION.

> The use of a UserTransaction is a powerful feature, but it shouldn't be used casually. It tends to lead to longer lock hold times at the database level, and that in turn can lead to a variety of well-known negative database consequences. Most of these are not specific to the CE.
> 
> For multiple CE updates, it is rare that they can't be accomplished via a single UpdatingBatch in the CE API. For enlisting additional resources, there is not much choice but to use a UserTransaction. In that case, structure your application so that the elapsed time while the transaction is open is as short as possible.