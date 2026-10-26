# Handlers and phase rules

As we discussed, a module is a collection of handlers, so a module could have one or more handlers. Irrespective of the number of handlers in a module, module.xml provides a convenient way to specify handlers. Most importantly, module.xml can be used to provide enough configuration options to add a handler into the system and specify the exact location where the module author would like to see the handler running. In Chapter 4, Execution Chain, we learnt more about phase rules as a mechanism to tell Axis2 to put handlers into a particular location in the execution chain, so now it is time to look at them with an example.

Before learning how to write phase rules and specifying handlers in a module.xml, let's look at how to write a handler. There are two ways to write a handler in Axis2:

+   - Implement the org.apache.axis2.engine.Handler interface
+   - Extend the org.apache.axis2.handlers.AbstractHandler abstract class

In this chapter, we are going to write a simple application to provide a better understanding of the module. Furthermore, to make the sample application easier, we are going to ignore some of the difficulties of the Handler API. In our approach, we will extend the AbstractHandler. When we extend the abstract class, we only need to implement one method called invoke. So the following sample code will illustrate how to implement the invoke method:

```
public class IncomingCounterHandler extends AbstractHandler implements CounterConstants {
public InvocationResponse invoke(MessageContext messageContext) throws AxisFault {
    //get the counter property from the configuration context
    ConfigurationContext configurationContext = messageContext.getConfigurationContext();
    Integer count = (Integer) configurationContext.getProperty(INCOMING_MESSAGE_COUNT_KEY);
    //increment the counter
    count = Integer.valueOf(count.intValue() + 1 + «»);
    //set the new count back to the configuration context
    configurationContext.setProperty(INCOMING_MESSAGE_COUNT_KEY, count);
    //print it out
    System.out.println(«The incoming message count is now « + count);
    return InvocationResponse.CONTINUE;
}
}
```