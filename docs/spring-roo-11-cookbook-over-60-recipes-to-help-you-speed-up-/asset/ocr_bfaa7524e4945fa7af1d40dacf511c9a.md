# # Spring Web Flow configuration

The webflow-config.xml defines a HandlerMapping that returns a FlowHandler for initiating execution of a flow, as shown here:

```
```xml
<bean class=
"org.springframework.webflow.mvc.servlet.FlowHandlerMapping">
  <property name="order" value="0" />
  <property name="flowRegistry" ref="flowRegistry" />
</bean>
```
```

The order property specifies the priority assigned to the FlowHandlerMapping with respect to other HandlerMapping implementations configured in the web application context. The value can be anything from Integer.MIN_VALUE to Integer.MAX_VALUE. The lower the value of the order property, the higher the priority of the HandlerMapping implementation. So, if a HandlerMapping implementation configured in the web application context specifies a value of order property more than 0, then it has a lower priority than FlowHandlerMapping. The priority of a HandlerMapping implementation comes into play when DispatcherServlet attempts to find a handler for processing the request.

> > The HandlerMapping configuration with highest priority (that is, lowest order value) is first consulted by DispatcherServlet to find a matching handler and HandlerMapping configuration that doesn't specify an order property is given the lowest priority.

The flowRegistry property of FlowHandlerMapping specifies a FlowDefinitionRegistry that contains a registry of all the flow definitions in the application. The following <flow-registry> element of Spring's webflow schema defines a flow registry:

```
```xml
<webflow:flow-registry id="flowRegistry"
  flow-builder-services="flowBuilderServices"
  base-path="/WEB-INF/views">
  <webflow:flow-location-pattern value="/**/flow.xml" />
</webflow:flow-registry>
```
```

The base-path attribute specifies the location relative to which flow definition XML files are located. The value /WEB-INF/views of the base-path attribute means that the flow definition XML files are located relative to the /WEB-INF/views folder. The <flow-location-pattern> specifies the location pattern for finding the flow definition XML files, relative to the base-path. The value /**/flow.xml means that the flow.xml files inside the /WEB-INF/views folder or its subfolder represent the flow definition XML files.