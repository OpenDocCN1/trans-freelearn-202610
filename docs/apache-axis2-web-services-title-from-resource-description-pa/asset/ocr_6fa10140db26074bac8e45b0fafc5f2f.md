In this case, either (or both) of the arguments can be null. If both are null, that is obviously equivalent to the default constructor case of ServiceClient. If ConfigurationContext is null, depending on the location you are trying to create the ServiceClient, either a new one will be created using the Axis2 default configuration or a server's ConfigurationContext will be used. Similarly, if AxisService is null, an anonymous service is created.

# Type 3: Creating a dynamic client (client on the fly)

The idea behind a dynamic client is to create a ServiceClient on the fly, or simply create a client for a given WSDL at runtime and use the created ServiceClient to invoke the corresponding service. When we create the ServiceClient in this manner, the corresponding AxisService object is configured according to the WSDL document. We will see the advantages of the dynamic client in the middle of this chapter. The constructor for creating a dynamic client is given as follows:

```
ServiceClient dynamicClient =new ServiceClient(
configContext,wsdlURL, wsdlServiceName, portName);
```

+   - configContext: configurationContext can be null; if it is null, the logic mentioned in Option 2 will be applied.
+   - wsdlURL: This argument should not be null as it specifies the URL for the WSDL file.
+   - wsdlServiceName: As you know, the WSDL document might have multiple service elements. So if you want to pick a specific service element, you can pass the QName of that service element. The value of this argument can be null. If it is null, the first one from the service element list will be considered as the service element.
+   - portName : As you know, a service element in a WSDL file might have multiple ports as well. So if you want to select a specific port, you can pass the name of the port as the value of this argument. Then, if the value is null again, the first one from the port list will be selected as the port.

A sample WSDL to understand what the mentioned service elements can be is shown here (the ports are shown here as well):

```
<wsdl:service name="MyService">
    <wsdl:port name="ServicePort1" binding="axis2:ServiceBinding1">
    <soap:address location="http://127.0.0.1:8080/axis2/services/
                MyService"/>
    </wsdl:port>
    <wsdl:port name="ServicePort2" binding="axis2: ServiceBinding2 ">
```