
In addition to the system-defined events, it is also possible to define custom events. A custom event is created by making a subclass of the Custom Event base class. As usual, the subclass can have custom properties defined on it. A custom event is fired by an application calling the Subscribable.raiseEvent(CustomEvent) method. Once triggered, event handlers are called as for a system event. At first glance, it may seem pointless to have applications firing custom events just to call event handlers with custom logic. The advantages are the same as mentioned at the beginning of this section: moving business logic out of the application and into the server tier. We'll show an example of using custom events in this way in the sample application in Chapter 12, The DUCK Sample Application.

## Subscriptions

Tying events to handlers is done via a subscription mechanism. On the one hand, a subscription points to a class or an instance from which events can be triggered. On the other hand, the subscription points to an Event Action which identifies the event handler to be executed and other details. Finally, the subscription includes a list of event types of interest.

A subscription may be applied to a specific object, in which case it's called an instance subscription. A subscription may be applied to an entire class of objects, in which case it's called a class subscription. A class subscription is used whenever any instance of that class (including subclass instances) has an event. For performance purposes, a filter expression— similar to a CE SQL WHERE clause— may be supplied. A subscription is only used if the filter expression evaluates to true for the triggering object.

There can be any number of separate class or instance subscriptions for a particular object. Within the synchronous and asynchronous types, there is no defined order in which the event handlers are called. In fact, the order may change from one event firing to another.

A subscription is classified as either synchronous or asynchronous, depending on when the associated event handler is executed. There is more difference between these than the timing, however, and the difference is commonly misunderstood.

> As a terminology convenience, events or event handlers are sometimes referred to as being synchronous or asynchronous. This is not technically correct because the designation is always made on the subscription object. An event can have either kind of subscription, and an event handler can be invoked both ways.

A synchronous subscription executes the event handler before any object update has actually been committed. Its purpose is to either allow or veto the update. The event handler may not modify the triggering object. It is executed within the same application server transaction as the triggering update. This has two consequences:

- The logic within the event handler is included within the transaction timeout of the overall update. This makes it fairly important that the event handler logic be executed efficiently and not include things like waiting for responses from distant servers.
- If the event handler code throws an exception, it causes the entire update transaction to roll back, and the calling application will receive that exception (though it will be wrapped in one or more other exceptions). This is the desired behavior and is exactly the mechanism for vetoing the update. If the event handler approves of the update, it merely exits normally without an exception.

> The CE does not throw an exception if the event handler for a synchronous subscription updates the triggering object. This has allowed many developers to ignore the rule, assuming it is merely a best practice. Nonetheless, it has always been the rule that synchronous subscription event handlers are not allowed to do that. Even if it works in a particular instance, it may fail at random times that escape detection in testing. Don't fall into this trap!

An asynchronous subscription executes the event handler after any object update has been committed. The time delay is typically small, but there is no timing guarantee. There is an ordering guarantee for multiple updates to the same triggering object (but not for multiple subscriptions on the same triggering object). Obviously, an asynchronous subscription event handler cannot veto a change that has already been committed. Its purpose is to react to a change. It might implement other CM changes or even updates to external systems. If an event handler for an asynchronous subscription throws an exception, the calling application will not be notified because the processing is being done in a background thread in the CE. In fact, the only notification is via the CE server logs. The event handler is called within its own transaction, so an exception will not affect any other event handlers or application threads.

Because event handlers are server-side code and are not controlled by the access rights of the user performing the action that triggered the event, security access checks are disabled for event handler code (though it is always possible for an event handler to find out who the triggering user was). Needless to say, only event handlers from trustworthy sources should be used.

## Audit logging

For compliance, governance, or other business reasons, it is sometimes necessary to record who performed various actions. The CE feature for this is called **audit logging**. Audit logging can record not only who performed an action but also who attempted to perform an action and failed for security access reasons. In addition to update events, audit log entries can be configured for object retrievals and document content retrievals (those activities are not available for normal subscriptions and event handlers).

Because the types of activities for which audit logging is desirable are very similar to the types of events for which event handlers might be used, there is quite a bit of synergy in the two subsystems. The counterpart to the event subscription object is the audit definition object. Audit definitions are configured as a list directly on a class definition and cannot be applied to an object instance.

Audit logging records are written to a database table within the Object Store, and those records are exposed as objects of the CE class `Event`. They can be directly queried like any other CE object via `CE SQL`. The audit definition controls some of the information that is recorded in the Event table for update events. You can choose to record a copy of the modified (post-change) object, the modified and original (pre-change) object, or neither.

> When the original or modified object is retrieved via an `Event` object, the objects come from the copies in the Event table. They often bear little resemblance to the object as it exists in the main part of the Object Store repository since it may have undergone many changes or even have been deleted since the audit record was written.

### Content Access Recording Level

Documents have a property called `DateContentLastAccessed`. As you can guess from the name, it records the timestamp of the last content retrieval for that document. Most sites do not really need this information, and there is a non-trivial performance cost for the CE to maintain it.

Therefore, frequency of update of that property can be controlled via the Object Store property `ContentAccessRecordingLevel`. The choices are not at all (the default), daily, hourly, or on every access. For example, if the configured value is daily, then the `DateContentLastAccessed` property on a given document will not be updated if the previous value is within 24 hours of a retrieval operation.

![](img/2295191c450a0c86b2d82df6cec04526_239_0.png)

## Event handlers

The event handler is the piece of site-supplied code that will be executed by the CE in response to a triggered event.

Event handlers are written in Java and implement the CE API EventActionHandler interface. The CE calls a method on that interface to inform the handler about the circumstances of the triggered event. The event handler is provided with the Id of the subscription object involved in the event firing and an event object which provides specific details. The class of event object and information available within it will be specific to the type of event fired, but it always includes a copy of the triggering object. There is always sufficient information for the handler to deduce the specifics of the changes that triggered the event.

The code for the event handler is stored in a CE object called a Code Module, and an event handler is linked indirectly to it. A code module is a Document subclass whose content is either a Java class or a Java Archive (JAR). We prefer to use a JAR file because it makes it simple to divide the implementation into multiple classes, associate resources with the event handler, and just generally package things neatly.

> Whether packaged as a Java class or as a JAR file, the code module is pointed to by the Action.CodeModule property. If you create a new version of the code module, that property will still point to the previous specific document version. This is generally not an issue in production, but it can cause some head-scratching during development.

It's also possible to put the code for an event handler onto the classpath of the application server. That can save some time during iterative development, but is not recommended for production deployment. Besides the added logistical inconvenience of deploying the event handler code to multiple application server instances, the presence of the event handler code on the most general classpath can have unexpected behavioral side-effects for other applications or for the application server itself.

## Workflow launch

One particular event handler is provided with the CM product. It is used to launch workflow processes in the configured Process Engine (PE) server. This event handler knows all the logic involved in contacting the PE server and actually launching the workflow. There is a pair of subscription subclasses called Instance Workflow Subscription and Class Workflow Subscription. Each has properties specific to workflow launch. For example, each has an IsolatedRegionNumber property which indirectly gives connectivity information for the PE server.
