
Now create a directory called `META-INF` under the classes directory and copy the `module.xml` file into it. Then our `classes` directory should look like the following screenshot:

![](img/e2fb05de168053e6227dab034eff9353_146_0.png)

Now create a ZIP file from the classes directory and rename the ZIP file into the `counter-module.mar`.

Deploying the module is as simple as dropping the file into the `TOMCAT_HOME/webapps/axis2/WEB-INF/modules` or `repository/modules` directory. In this case, let's focus on deploying the module in Tomcat or your favorite web application server. As we know, Axis2 does not support module hot deployment, so just dropping the module will not make any changes to the runtime system. To deploy our module and apply the necessary changes to runtime, we need to restart Axis2; in other words, we need to restart the application server.

It should be noted that just deploying a module does not add its handlers into the handler chain; to add handlers into the system, it is necessary to engage the module. Now, let's see how we engage a module in Axis2. For this purpose, we can use the Axis2 web administration console. Moreover, to make the story simple, let's try to engage the module to all the services in the system using the web administration console (we can engage to a single service also using the administration console). We can engage the module to the system by carrying out the following steps:

- 1. Go to http://localhost:8080/axis2/.
- 2. Click on the Administration tab. This will open up a new page and ask for the username and password.

> There are other ways of engaging a module, for example, to engage a module globally, a user can edit axis2.xml and add the <module ref="modulename" /> element. Moreover, if a user wants to engage it to particular services, he/she can do that by editing the services.xml and adding the same tag mentioned earlier.

## Advanced module.xml

Here, we looked at a very simple module.xml, but when it comes to very complex applications, we need to have many more configurations in a module.xml file. We might need to have parameters, WS-Policy, and endpoints. Now let's look at them one at a time in brief.

### Parameters

Adding a parameter here is the same as adding a parameter in services.xml or axis2.xml. We just need to add the following tag into module.xml and Axis2 will do the right thing for us:

```
<parameter name="foo">bar</parameter>
```

We can have any number of parameters in a module.xml, and when we want to access the parameter, we can do that by carrying out the following steps:

- - First we need to get the AxisModule. We can do that either by using the init method (Axis2 passes the AxisModule) or we need to get the corresponding AxisModule from the ConfigurationContext (inside the Module implementation class) or from messageContext (inside a handler).
- - After this, we can ask for the parameter from the AxisModule.

### WS-Policy

Specifying a WS-Policy element in module.xml is one way of configuring a module. We can add WS-Policy element inside a module.xml. If we consider the Sandesha2 (reliable message implementation) module, we can find the following policy element:

```
<wsp:Policy xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy" xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd" xmlns:sandesha2="http://ws.apache.org/sandesha2/policy" wsu:Id="RMPolicy">
    <sandesha2:RMAssertion>
        [REST OF THE FILE]
    </sandesha2:RMAssertion>
</wsp:Policy>
```

### Endpoints

In Axis2, an endpoint is an operation of a web service. So, adding an endpoint is the same as adding an operation. The key question is—why do we need to add an endpoint to a module? To understand this, let's assume that we have a module and this module has a set of control operations. The most suitable example is reliable messaging, as it has a number of control messages (for example, create sequence, last message, terminate sequence). Say, we need to invoke a service in a reliable manner. It first needs to set up a sequence with the service. For this, it will send the control message called `createSequence` to the service we need to access. But that service does not have that method, so if we try to send the `createSequence` message without adding the method, Axis2 will throw an exception saying **Unable to dispatch**. Therefore, adding an endpoint will solve that issue. Hence, when we engage Sandesha, it adds a method called `createSequence` to the service at the time we engage the module (or all the services, if we engage that to the whole system). When a request comes, Axis2 will dispatch without having any problem. This operation or the endpoint has its own message receiver to do the right thing.

So, it is obvious that when a module needs control operations exchange, it is required to add endpoint to represent those operations. Adding an endpoint is very simple. What we need to do is add an operation element(s) along with a message receiver and a set of action mapping. To get an idea about that, let's take the Sandesha module as a reference. Its `module.xml` has the following operation tag to add the control operations. Remember, when we have the operation tag in `module.xml`, Axis2 will do all the processing, including creating `AxisOperation` and adding that. As a module author, all we need to do is specify them in the `module.xml` file:

```
<operation name="Sandesha2OperationInOut" mep="http://www.w3.org/2006/01/wsdl/in-out">
    <messageReceiver class="org.apache.sandesha2.msgreceivers.RMMessageReceiver"/>
    <!-- namespaces for the 2005-02 spec -->
    <actionMapping>http://schemas.xmlsoap.org/ws/2005/02/rm/CreateSequence</actionMapping>
    <actionMapping>http://schemas.xmlsoap.org/ws/2005/02/rm/AckRequested</actionMapping>
    ..................
</operation>
```

### Writing an Axis2 Module

If we look at the Sandesha module.xml, we will be able to learn and find out more about writing a module.xml file.

### Summary

In this chapter, you learned about the importance of the Axis2 module. We also learned that the Axis2 module provides a very flexible way to extend the Axis2 core functionality and provides quality service. Moreover, we discussed the module and related concepts by writing a sample module and demonstrating most of the commonly used configuration settings. In our sample application, we discussed how to write handlers, how to write module implementation classes, and finally, how to put everything together and deploy the module. At the end of the chapter, we learned how to engage a module to Axis2.

In this chapter, we discussed how to write and deploy services and how to write and deploy modules. In the next chapter, we are going to discuss how to invoke a remote service. There, we will discuss how to use Axis2 as a client and invoke services in a number of ways.

## The Client API

Web service is one of the commonly used approaches for applications integration (composition), when integrating applications; an application can act as either a consumer (client) or a producer (server). A web service framework should be able to deploy services as well as access the services. So far, we have discussed the deployment side of it. Thus, in this chapter, we will focus on the client side and how to use Axis2 to access remote services. In high level, Axis2 runtime does not differentiate between the client side and the server side. Moreover, it uses the same execution framework at the server side as well as the client side. As we already discussed, in the server side, we have services. Thus, to keep the symmetry in the client side as well, Axis2 creates a dummy service when we use the client API. In this chapter, we will look at the various aspects of the client API, with the example use case.

In this chapter, we will discuss most of the commonly used APIs of the Axis2 client, as well as explaining how to use them with sample code. First, we will discuss the various ways of creating the Axis2 client instance. Then, we will discuss the available APIs and finally, we will discuss the more advanced uses of Axis2 client APIs. Particularly, we will be covering the following items in detail:

- - The idea of the web service client
- - Synchronous and asynchronous service invocation
- - Axis2 client APIs, namely, `ServiceClient` and `OperationClient`
- - Demonstrate `ServiceClient` APIs with examples
- - Demonstrate `OperationClient` APIs with examples
