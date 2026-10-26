# Specifying message receivers at the service level for the whole service

Think of a scenario where you have a large number of operations to be published in the services.xml file. In this case, adding message receivers for each and every operation seems like a headache. However, if you can specify a message receiver for the whole service, it would make the service author's job easier and would simplify the services.xml as well.

Axis2 has inbuilt support for all of the eight Message Exchange Patterns (MEPs) defined in W.S.D.L. 2.0. In a services.xml, you can specify an MEP and the corresponding message receiver, and then, depending on the MEP the operation belongs to, Axis2 will automatically pick up the message receiver and set it to the operation.

Inside the operation tag, you can add an attribute to specify the MEP of the operation as follows:

```
<operation name="sayHello" mep="http://www.w3.org/2004/08/wsdl/in-out" />
```

An example of defining service level message receivers for a given service is shown here:

```
<service>
    <Description>
        This is my first service, which will say Hello.
    </Description>
    <messageReceivers>
        <messageReceiver mep="http://www.w3.org/2006/01/wsdl/in-only" class="org.apache.axis2.rpc.receivers.RPCInOnlyMessageReceiver"/>
        <messageReceiver mep="http://www.w3.org/2006/01/wsdl/in-out" class="org.apache.axis2.rpc.receivers.RPCMessageReceiver"/>
    </messageReceivers>
    <parameter name="ServiceClass" locked="false">HelloWorld</parameter>
    <operation name="sayHello" mep="http://www.w3.org/2004/08/wsdl/in-out" />
</service>
```