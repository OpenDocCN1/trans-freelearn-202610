In order to manage a SOAP session, a client has to send an additional reference parameter in the SOAP header, which is named as serviceGroupId (it will be sent to the client when he invokes a service that deploys in SOAP session the first time). In the meantime, a SOAP session has to provide a way to manage sessions across not only a single service invocation, but also for multiple services in a service group. As long as we are in the same SOAP session, we can manage service related data in ServiceContext and if we want to share data across other services in the group, then we can use ServiceGroupContext to store the session-related data.

When we deploy a service in a SOAP session and when a client tries to access the service the first time, Axis2 will generate serviceGroupId and send that to the client as a reference parameter in wsa:ReplyTo, as shown next. However, we should mention here that to have SOAP session support both the client and the server, it has to have WS-addressing support.

```
<wsa:ReplyTo>
    <wsa:Address>http://www.w3.org/2005/08/addressing/anonymous</wsa:Address>
    <wsa:ReferenceParameters>
<axis2:ServiceGroupId xmlns:axis2="http://ws.apache.org/namespaces/axis2">urn:uuid:65E9C56F702A398A8B11513011677354</axis2:ServiceGroupId>
    </wsa:ReferenceParameters>
</wsa:ReplyTo>
```

So if a client wants to live in the same session, then he/she has to copy that reference parameter and send it back to the server when he/she invokes the service the second time. As long as a client sends the valid ServiceGroupId, he/she can use the same session, and a service can maintain the session-related data. Unlike request session scope, SOAP session has a default timeout period, if the client does not touch the service for a period of 30 seconds, then the session will expire, and if the client sends the old serviceGroupId, he/she will get an AxisFault. We can change the default timeout period by changing the server's axis2.xml file as follows:

```
<parameter name="ConfigContextTimeoutInterval">30000</parameter>
```

By changing the parameter value, we can have a timeout interval that we want.

As mentioned earlier, deploying a service in a SOAP session requires that we change services.xml as follows:

```
<service name="MyService"  scope=" soapsession">
</service>
```