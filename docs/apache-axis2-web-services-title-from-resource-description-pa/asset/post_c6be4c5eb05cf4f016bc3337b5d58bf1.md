
## Specifying message receivers at the service level for the whole service

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

According to the `services.xml`, `org.apache.axis2.rpc.RPCMessageReceiver` is the message receiver for all of the in-out operations (any operation that belongs to the in-out MEP will assign this message receiver as its message receiver) in the service. Whereas, the service level message receiver for an in-only MEP is `RPCInOnlyMessageReceiver`. If you redeploy the `HelloWorld` service with the new `services.xml`, you will definitely get the same result when you invoke the service again.

## Specifying the service level message receiver and overriding them through operations

There may be an instance where a service author wants to use different message receivers for some of the operations while he/she has already defined service level message receivers. The overriding of a service level message receiver by an operation can be easily achieved by just adding a message receiver element to the operation that you want to assign a different message receiver to. A sample `services.xml` that follows this technique is shown next:

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
    <operation name="sayHello">
        <messageReceiver class="org.apache.axis2.receivers.RawXMLINOutMessageReceiver"/>
    </operation>
</service>
```

Operation `sayHello` uses a different message receiver than its service level message receivers.

All the public methods in the service implementation class are exposed whether you specify them in the `services.xml` or not. Axis2 calculates the MEP of an operation by checking its corresponding Java method. If the method's return type is void, then the MEP will be in-only, or else it will be in-out. Depending on the MEP, the correct message receiver will be set.

## Adding third-party resources

There are many instances where you will want to use third-party libraries in your web service. Also, by now, you should know that each service and module in Axis2 is isolated. So each service gets its own class loader. When you have third-party libraries that you want to include with your web service, Axis2 has a mechanism to do just that. This is done by simply creating a folder named lib inside the service archive file and dropping the libraries or resources inside it.

Imagine you have a service that needs to use libraries named foo.jar and bar.jar and a resource named xyz.properties in your service. Your service archive file should then look like the following screenshot:

![](img/e2fb05de168053e6227dab034eff9353_130_0.png)

## Service WSDL and schemas

When you start writing relatively more complex applications, you will feel the need for having a WSDL document and schema files inside the service archive file. When considering enterprise level applications, you cannot let clients rely on an auto-generated WSDL document. In order to have a solid and well defined WSDL document, it is necessary to deploy the service along with the WSDL document file and the corresponding XML Schema files. Therefore, when you want to add a WSDL document file, the place to put it would be inside the META-INF directory of a service archive. The only thing you have to remember when doing so is that the service name given in the services.xml and the service name defined in the WSDL document should be the same. A service archive file with the included WSDL file will have the following structure:

## Filling in the service skeleton

Axis2 generates a service skeleton class with methods that can throw UnsupportedOperation. All that we have to do is implement the service skeleton class according to the business logic you want to provide.

## Running the ant build file

After completing the service skeleton, the next step is to create a service archive file using the modified generated code. To make this job simple, Axis2 generates an Ant build file to create the service archive file for you. What you have to do is open a command line console and go to the folder where you output the generated code and then type ant build in the console to run the Ant build file. The service archive will now be created for you.

In this chapter, we only skimmed through the contract first approach. You will learn more about code generation and databinding in detail later in this book. We will also be covering a number of samples as well as the available tools.

## Summary

Turning a Java class into a web service is very straightforward in Axis2. Once you know how to write a `services.xml` file correctly, you can create more complex applications rather than just POJOs. Deploying a service is just a matter of creating a service archive file and dropping it into the services directory in the Axis2 repository. WSDL's first approach is the easiest way of creating a service since Axis2 has inbuilt support for code generation and it has a set of tools to make the job even easier.

In this chapter, we discussed how to convert a simple class file into a web service and how to access that in a REST manner. We discussed what happens when the service becomes a little complex by adding a package name. We also discussed how to use the archive-based deployment mechanism. Finally, we briefly discussed the contract first approach.

In the next chapter, we will discuss how to extend Axis2's core functionality and provide additional service quality using the Axis2 module.

## 8

## Writing an Axis2 Module

Web services are gaining a lot of popularity in the industry and have become one of the major enabler for application integration. In addition, due to the flexibility and advantages of using web services, everyone is trying to enable web service support for their applications. As a result, web service frameworks need to support new and more custom requirements. As we have already discussed in the previous chapters, one of the major goals of a web service framework is to deliver incoming messages into the target service. However, just delivering the message to the service is not enough; today's applications are required to have reliability, security, transaction, and other quality services.

Due to the popularity of web services, standard bodies are producing new web service standards, and it is hard to support those new standards if the web service framework is not flexible enough. From the very beginnings of Axis2, flexibility and extensibility were the two main design considerations. The idea of Axis2 modules is to extend the core functionality of the system without performing any changes to the core system. For example, Axis2 supports reliability and security as two separate modules, and the core engine is fully independent of those two qualities of service modules.

In this chapter, we will discuss the power of Axis2 modules and how to use them to extend Axis2 to support for your own requirements. In particular, we will discuss the following items:

- Brief history of the Axis2 module
- Introducing module concept
- Structure of the module
- Module configuration file (`module.xml`)
- Optional module implementation class
- Steps to writing a `module.xml` file
- Deploying and engaging a module
- Brief overview of the WS-Policy and its usage in modules
