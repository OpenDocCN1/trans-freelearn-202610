# ServiceClient with working samples

Now we know the available ways for creating a service client, but there is nothing like code to explain those. So the best way to understand ServiceClient API is to write a few real samples. But first, we need to start the Axis2 server and deploy the following sample service in the server. Only after that can we write a useful client to invoke the service. We can create and deploy a service by using the following steps:

+   1. Open your favorite IDE and write the following Java class or the service implementation class.

```
public class MyService {
    //method which has a return value
    public String echo(String value) {
        return value;
    }
    // does not have a return value
    public void update(int value) {
        System.out.println("value is :" + value);
    }
}
```

Sample service implementation class has two methods—one has a return value and the other does not.

+   2. Writing the services.xml file for the service, your services.xml will look like the following. See the references section to learn about Axis2 deployment.

```
<serviceGroup>
    <service name="MyService">
        <messageReceivers>
            <messageReceiver mep="http://www.w3.org/2004/08/wsdl/in-only" class="org.apache.axis2.rpc.receivers.RPCInOnlyMessageReceiver"/>
            <messageReceiver mep="http://www.w3.org/2004/08/wsdl/in-out" class="org.apache.axis2.rpc.receivers.RPCMessageReceiver"/>
        </messageReceivers>
        <parameter name="ServiceClass" locked="false">
            MyService
        </parameter>
    </service>
</serviceGroup>
```

```
<soap12:address location="http://127.0.0.1:8080/axis2/services/MyService"/>
</wsdl:port>
</wsdl:service>
```