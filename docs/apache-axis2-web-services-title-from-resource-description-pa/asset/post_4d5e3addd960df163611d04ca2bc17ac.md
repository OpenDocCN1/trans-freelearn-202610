
In order to manage a SOAP session, a client has to send an additional reference parameter in the SOAP header, which is named as serviceGroupId (it will be sent to the client when he invokes a service that deploys in SOAP session the first time). In the meantime, a SOAP session has to provide a way to manage sessions across not only a single service invocation, but also for multiple services in a service group. As long as we are in the same SOAP session, we can manage service related data in ServiceContext and if we want to share data across other services in the group, then we can use ServiceGroupContext to store the session-related data.

When we deploy a service in a SOAP session and when a client tries to access the service the first time, Axis2 will generate serviceGroupId and send that to the client as a reference parameter in wsa:ReplyTo, as shown next. However, we should mention here that to have SOAP session support both the client and the server, it has to have WS-addressing support.

```xml
<wsa:ReplyTo>
    <wsa:Address>http://www.w3.org/2005/08/addressing/anonymous</wsa:Address>
    <wsa:ReferenceParameters>
<axis2:ServiceGroupId xmlns:axis2="http://ws.apache.org/namespaces/axis2">urn:uuid:65E9C56F702A398A8B11513011677354</axis2:ServiceGroupId>
    </wsa:ReferenceParameters>
</wsa:ReplyTo>
```

So if a client wants to live in the same session, then he/she has to copy that reference parameter and send it back to the server when he/she invokes the service the second time. As long as a client sends the valid ServiceGroupId, he/she can use the same session, and a service can maintain the session-related data. Unlike request session scope, SOAP session has a default timeout period, if the client does not touch the service for a period of 30 seconds, then the session will expire, and if the client sends the old serviceGroupId, he/she will get an AxisFault. We can change the default timeout period by changing the server's axis2.xml file as follows:

```xml
<parameter name="ConfigContextTimeoutInterval">30000</parameter>
```

By changing the parameter value, we can have a timeout interval that we want.

As mentioned earlier, deploying a service in a SOAP session requires that we change services.xml as follows:

```xml
<service name="MyService" scope="soapsession">
</service>
```

The createPayLoad method is shown here:

```java
public static OMElement createPayLoad(int intValue) {
    OMFactory fac = OMAbstractFactory.getOMFactory();
    OMNamespace omNs = fac.createOMNamespace(
        "http://ws.apache.org/axis2"
```

Once we run the code, we will see following output on the client side:

```text
10
10
```

This means that though we invoke the service twice, we have gotten the same output. This simply means no session has taken place, so now let's change our client code a bit and see what we are getting. Just add the following line of code and run the client again:

```java
opts.setManageSession(true);
sc.setOptions(opts);
```

Now you should see the following output on the client side:

```text
10
20
```

So this simply tells us that we have invoked the service in a session-aware manner.

### Option 2: Using the service client

When we use the service client, once we set the session management flag to true, it will send back the transport cookie as well. So if we run the following code, we will see the following outcome:

```java
ServiceClient sc = new ServiceClient();
Options opts = new Options();
opts.setTo(new EndpointReference(
"http://127.0.0.1:8000/axis2/services/MyService"));
opts.setAction("urn:add");
opts.setManageSession(true);
sc.setOptions(opts);
OMElement ele = sc.sendReceive(createPayLoad(10));
System.out.println(ele.getFirstElement().getText());
ele = sc.sendReceive(createPayLoad(10));
System.out.println(ele.getFirstElement().getText());
```

![](img/e2fb05de168053e6227dab034eff9353_179_0.png)

It simply tells us that we have to invoke the service deployed in the transport session (invoked in a session-aware manner). In this case, we do not need to have the addressing module.

### Application scope

Application scope has the longest lifetime compared to all the others and the lifetime of the application session is equal to the lifetime of the system. If we deploy a service in the application scope, there will be only one instance of that service and obviously there will be only one ServiceContext for that service too. In the world of Axis2, if we consider the memory footprint and if we don't want to manage a session, then a good idea is to deploy the service in the application scope.

When we deploy a service in the application scope, a client does not need to send any additional data to use the same session.

To deploy a service in the application scope, we need to change axis2.xml, as shown here:

```xml
<service name="foo" scope="application">
</service>
```

> Service deployed in the application session scope is not guaranteed to be threading-safe.

### Managing sessions using ServiceClient

As we know by now, managing a session in the client side involves a bit of work. As mentioned earlier, both in the SOAP session and transport session, a client has to send the session-related data, if he/she wants to live in the same session. Maybe he can do that for a SOAP session by copying the required reference parameters. However, with transport session, how can a user get access to transport, to copy, and send cookies?

To make life easier, Axis2 has the inbuilt capability of managing sessions in the client session by just setting a flag, and we have used that already. Then, depending on the service-side session, it will send the corresponding data as long as we use the same service client. So, the main requirement is to use the same service client to invoke the service if you want to live in the same session.

If we want to live in the same session, we can create a service client, shown as follows, and re-use the created service client object to invoke the service:

```java
Options options = new Options();
options.setManageSession(true);
ServiceClient sender = new ServiceClient();
sender.setOptions(options);
```

Once we create the ServiceClient, as shown here, if the service deploys in a SOAP session, it will copy the serviceGroupId and send that from the second invocation onwards. If the server sends the session ID, such as HTTP cookies, it will copy that to ServiceContext (in the client side) and send it back to the server when the client invokes the service for the second time.

### Summary

Stateless nature is one of the main characteristics of web services. However, this is a limitation for the advanced web services developers. Developing an enterprise level application using web services is not easy, unless we have a session management layer. Axis2 has the level four of sessions to address enterprises level web service development issues. In this chapter, we discussed different types of sessions available in Axis2. After that, we used a sample service and a sample client to demonstrate the different sessions and how they work.

In the next chapter, we will focus on another very good web service specification, called JAXWS. There, we covered the JAXWS annotations, deployment, and some examples to illustrate how to use it.

## 11 Developing JAX-WS Web Services

Java API for XML-based web services is a popular standard, targeting Java developers who write web services and web service clients. The conventional **contract first** approach, which consists of defining the web service contract in a WSDL and then generating the appropriate code from those WSDLs, is not the most natural way of programming for Java developers. The JAX-WS specifications allow you to write a regular Java code and annotate the code appropriately, in order to make them into web services. Therefore, JAX-WS is a **code first** approach for developing web services and clients in Java. The ease of development is achieved through writing everything as Java classes and not having to write any specific deployment descriptor file such as `services.xml` files utilized by Axis2 AAR services. Older Java Web Service specifications such as JAX-RPC required the web service classes to implement a specific interface. With JAX-WS, there is no such restriction and regular POJOs can be made into web services with minimal changes. JAX-WS also allows contract first development. You will see how JAX-WS-annotated web services can be generated starting from WSDL using the wsimport tool later in this chapter. At the time of writing, Axis2 only supports the JAX-WS 2.0 specification.

In this chapter, we will be looking at:

- JAX-WS Annotations
- Server-side JAX-WS
- Client-side JAX-WS

By the end of this chapter, you will have a good understanding about JAX-WS annotations and how to develop JAX-WS services and clients in Apache Axis2.
