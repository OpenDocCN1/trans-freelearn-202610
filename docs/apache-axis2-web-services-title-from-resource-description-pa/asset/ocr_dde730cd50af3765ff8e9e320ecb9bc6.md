# ServiceGroupContext

When a message is received by Axis2, a ServiceGroupContext is created to store and shared data. To create a ServiceGroupContext, it is required to have an AxisServiceGroup object available. One AxisServiceGroup may have one or many ServiceGroupContext. Nevertheless, there is only one AxisServiceGroup associated with the context. The lifetime of the ServiceGroupContext depends on the service scope (for example, application, transport, SOAP session, and request). If the service scope is application, then the lifetime will be the same as system lifetime. However, if the service scope is "request", then there will be a ServiceGroupContext created for each and every invocation. If we want to share data across multiple services in one service group at runtime, then the ServiceContext provides the means to do so.

# ServiceContext

A ServiceContext represents the runtime data for a given service. To create a ServiceContext, we need to have a ServiceGroupContext object and an AxisService object available. The lifetime of the ServiceContext depends on the scope of the service. If we want to share data across multiple invocations of the same service, then ServiceContext can be used as the placeholder to store that data.

For example, let's say we have a service with three operations—login, doSomething, and logout. Also, assume that we need to share data across these three operations. Here, ServiceContext can be used to achieve these objectives. The number of ServiceContexts in one ServiceGroupContext is dependent on the number of AxisServices in the corresponding AxisServiceGroup.

# OperationContext

An OperationContext represents the lifetime of an MEP. More often than not, the lifetime of an operation context is less than the lifetime of the service context. We can use OperationContext to share data among messages in an MEP. For example, if we want to share data between a request and the response, then OperationContext can be used to achieve that. The number of OperationContext contexts in a ServiceContext is not related to the number of AxisOperation operations in an AxisService; it only depends on the number of invocations, for example, one OperationContext for each invocation.