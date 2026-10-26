# Available options to create a ServiceClient

There are multiple ways to create a ServiceClient instance, and of course, we can choose the most suitable constructors for us. No matter how we create a ServiceClient, it is required to have an an Axis2 runtime (ConfigurationContext) to invoke the service. Either user explicitly provides an instance of configuration context or Axis2 creates it internally.

Earlier, we discussed ConfigurationContext and how to create it. So we can use those techniques to create ConfigurationContext when creating a ServiceClient, or as we will see, we can create ServiceClient with ConfigurationContext as null and let Axis2 create ConfigurationContext for us.

# Type 1: Creating a ServiceClient using its default constructor

The easiest way to create a ServiceClient is to use its default constructor, as shown below. In this case, it creates a ConfigurationContext using the Axis2 default configuration file, which is available in the Axis2 JAR file. In the meantime, it creates an anonymous (dummy service) service with three operations (three operations to support WSDL 2.0 MEPs). Even though we create a service client this manner, we can use the created client to asses any web service.

```
ServiceClient serviceClient = new ServiceClient();
```

When we are trying to create serviceClient inside an Axis2 system (as an example, a handler tries to create a ServiceClient to invoke a service, or if service tries to invoke some other service), then it is created using server's ConfigurationContext. In this case, all the properties, transports, and modules in the server are accessible to the ServiceClient, and that scenario is called as a client running inside a server.

# Type 2: Creating a ServiceClient with your own ConfigurationContext

When we want to create a ServiceClient with our own configuration data, we can use the following constructor. As we know by now, there are a number of ways to create ConfigurationContext. At the same time, there are many instances where we want to create ServiceClient with our own AxisService, which might have configured with custom QoS (Quality of Service), parameters, WS-Policy, and so on.

```
ServiceClient serviceClient =
    new ServiceClient (configContext, axisService);
```