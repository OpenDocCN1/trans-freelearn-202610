As mentioned earlier, the key job of the SOAP processing framework is to process the SOAP message. However, just processing the message is not enough. It is necessary to provide additional functionalities, such as quality of service. Thus, it is required to have extensibility built onto the system. Extensibility of the SOAP processing model is provided through handlers. When a SOAP message is being processed, the handlers that are registered will be executed. The handlers can be registered in global, service, or in operation scopes, and the final handler chain is calculated by combining the handlers from all the scopes.

The handlers act as interceptors and they process the parts of the SOAP message and provide the *quality of service* features (a good example of quality of service could be security or reliability). Usually handlers work on the SOAP headers, yet they may access or change the SOAP body as well.

The concept of a flow is very simple and it consists a series of phases wherein a phase is referred as a collection of handlers. Depending on the MEP for a given method invocation, the number of flows associated with it may vary.

**In-Only MEP:** In the case of an In-Only MEP, the corresponding method invocation has only one pipe, that is, the message will only go through in-pipe (inflow).

**In-Out MEP:** On the other hand, in the case of an In-Out MEP, the message will go through two pipes, that is, in-pipe (inflow) and out-pipe (outflow). When a SOAP message is being sent, an `OutFlow` would begin.

**Fault-Flow:** The `OutFlow` invokes the handlers and ends with a **transport sender** that sends the SOAP message to the target endpoint. The SOAP message is received by a **transport receiver** at the target endpoint, which reads the SOAP message and starts the `InFlow`. The `InFlow` consists of handlers and ends with the **message receiver,** which handles the actual business logic invocation.

A phase is a logical collection of one or more handlers, and sometimes a phase itself acts as a handler. Axis2 introduced the phase concept as an easy way of extending core functionalities. In Axis 1.x, we needed to change the global configuration files if we want to add a handler into a handler chain. But Axis2 makes it easier by using the concept of phases and phase rules. Phase rules specify how a given set of handlers inside a particular phase are ordered. The following figure illustrates a flow and its phases:

![](bbox=[0.27, 0.73, 0.7, 0.79])