
- `ServiceContext`: This will represent the runtime of one service and the context lifetime will be the lifetime of the session. There can be one or many service contexts in the system, depending on the session scope of the corresponding service.
- `OperationContext`: This context is to represent the life time of a MEP (Message Exchange Patterns). The lifetime of an operation context is mostly less than the lifetime of the `ServiceContext`.
- `MessageContext`: The lifetime of an incoming message is represented by the message context. If two handlers in a given execution chain want to share data, then the best way is to store them in the message context. One `OperationContext` may have one or more `MessageContexts`.

## Session initializing and session invalidating

When talking about the session management, one thing that comes to mind is the lifetime of the session. Most of the Web applications, like e-commerce, banking, hotel reservation, and so on, have a predefine lifetime. If you are not active for that time, it will automatically terminate your session and prompt you to log in again. Hence, there is a particular time when a session gets started and another when the session finishes. So whoever writes a session aware service might need to know when a session starts and when it finishes. To facilitate this, Axis2 uses Java reflection and optional interfaces to inform the service implementation class. Actually, there are two ways that a service author is notified when a session starts and finishes, irrespective of the session scope.

## Java reflection

In this case, the service author has to implement the following two methods in his/her service implementation class, if he/she wants to be notified as to when the session starts and finishes. At runtime, when a session starts, Axis2 looks to see whether the following methods are there in the service implementation class; if so, it calls the right method:

```
//This method will be called when a session start
public void init(ServiceContext serviceContext) {
    // Our code goes here
}
//This method will be called when a session finishes.
public void destroy(ServiceContext serviceContext) {
    // Our code goes here
}
```

## Using the optional interface

When we use Java reflection, there is a very slight probability of making mistakes in the method's name and parameters, so it is good to user interface. That would lead you to use the method in the correct manner and avoid mistakes. The corresponding interface that Axis2 provides is called org.apache.axis2.service.Lifecycle, which has the same two methods we discussed earlier. If you want to get a notification as to when a session starts, you should write your service implementation class to implement that particular interface; then Axis2 will automatically call the right method. So we can write our service implementation class as shown below:

```
public class MyService implements Lifecycle {
    public void init(ServiceContext context) throws AxisFault {
    }
    public void destroy(ServiceContext context) {
    }
}
```

## Accessing MessageContext

In the first part of the chapter, we discussed that keeping class variables anywhere in Axis2 is not a good practice (for example, service implementation class, handler class). There, we also mentioned the fact that keeping local variables also leads to issues in the concurrent environments. Hence, it is not a good idea to keep variables inside the service implementation class. When managing sessions, the right way is to store the data in one of the context and access them whenever needed. Though Axis2 passes ServiceContext when the session starts, storing it inside the service implementation is not the right approach (storing as a class variable). Therefore, we need to have a mechanism to access those contexts. As we learned before, if you have access to MessageContext, then you can access the entire context hierarchy using that. Thus, if we can get access to the current MessageContext, we can consider accessing the entire context hierarchy. So the next question is how do we get the current MessageContext inside to the service class? That is very straightforward; Axis2 sets MessageContext into ThreadLocal, so from that, we can access the MessageContext. Let's say that we want to access the MessageContext inside the method called foo. We can do that as follows:

```
public void foo(){
    MessageContext messageContext = MessageContext.getCurrentMessageContext();
}
```

This can be used to access the current MessageContext, irrespective of the session scope we use.

Having discussed all the necessary pieces of session management, now it is time to discuss about the different types of available sessions in Axis2. As mentioned above, there are four different types of sessions. We will discuss all of them in detail here.

## Request session scope

Request session scope is the default session scope in Axis2. When we deploy a service without knowing anything about session management, our service will be deployed in the request session scope. The lifetime of this session is limited to the method invocation lifetime or the request processing time. When we deploy a service in the request scope, it simply means that we are not going to worry about the session management at all. So it is like having no session management.

Once we deploy a service in the request session scope, for each and every invocation, a new service implementation class will be created. Say we have to deploy a service called Foo in the request scope; then if a client invokes the service ten times, 10 instances of the service class will be created.

If we want to specify the scope explicitly, we can still do that by adding a scope attribute to the service element in services.xml, as follows. However, as mentioned earlier, deploying a service in the request scope does not require any modifications (the following is optional):

```
<service name="Foo" scope=" request">
</service>
```

To get an understanding about request scope, create a service using the following service class, deploy it, and invoke it:

```
public class MyService implements Lifecycle {
    public void init(ServiceContext context) throws AxisFault {
        System.out.println("I'm inside init method ");
    }
    public void destroy(ServiceContext context) {
        System.out.println("I'm inside destroy method");
    }
    public String foo(String foo) {
        return foo;
    }
}
```

To invoke the service, we just type the following in the browser:

```
http://localhost:8080/axis2/services/MyService/foo?foo=foo
```

We will see the following in the server console:

![](img/e2fb05de168053e6227dab034eff9353_174_0.png)

If we continue to do so, we will see it getting printed every time we invoke the service.

It should be noted here that even if we deploy a service in a request scope, there are many ways of keeping our service as a state full service. One way is to store the state in Axis2 global runtime (ConfigurationContext) and retrieve whenever necessary. Notably, this will add more overhead to the system, as there is only one ConfigurationContext, and if everything tries to access it, it will be a bottleneck.

## SOAP session scope

The idea of SOAP session is to have a transport-independent way of managing a session between two SOAP nodes; obviously, between the client and the server. Here, Axis2 uses SOAP headers in order to manage the session. In the SOAP session scope, it has a slightly longer lifetime than the request session scope, and deploying a service in a SOAP session is required to change services.xml as well. Managing a SOAP session requires both the client and the service to be aware of the sessions, that is, the client has to send the session-related data if he/she wants to access the same session and the service has to validate the user using the session-related data.

> SOAP-based session management is not interoperable; part of the reason is that there's no agreement upon a standard specification to manage the session. There is a standard specification called WS-Context, but none of the commonly used web service frameworks have implemented it.
