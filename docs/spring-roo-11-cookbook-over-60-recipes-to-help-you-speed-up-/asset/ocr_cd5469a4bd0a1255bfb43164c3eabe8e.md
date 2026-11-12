> For information on other elements defined in webmvc-config.xml file, please refer to Chapter 4, Web Application Development with Spring Web MVC.

The flex-config.xml makes use of the <message-broker> element of Spring's flex schema to configure and initialize a BlazeDS MessageBroker, as shown here:

```
flex-config.xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:flex="http://www.springframework.org/schema/flex"
    ...>

<flex:message-broker mapping-order="1">
    <flex:mapping pattern="/messagebroker/*"/>
    <flex:message-service default-channels="longpolling-amf" />
</flex:message-broker>

</beans>
```

By default, the <message-broker> element considers the /WEB-INF/flex/services-config.xml file as the configuration file for BlazeDS MessageBroker. If you change the name or location of the services-config.xml file in flightapp_flex, then use the services-config-path attribute of the <message-broker> element to specify your BlazeDS configuration XML file.

The <mapping> element of the flex schema maps incoming requests from the DispatcherServlet to /messagebroker/* path—the path to which BlazeDS MessageBroker channels are mapped in services-config.xml, as we'll see shortly.

The <message-service> element configures a BlazeDS flex.messaging.services.MessageService object that provides a publish-subscribe messaging between producers and consumers of messages in the application. The default-channels attribute specifies the message channel(s) used by Flex clients to access MessageService or to receive messages from it. The longpolling-amf value refers to the AMFChannel configured in services-config.xml, with polling enabled.

> It is important to note that even though Roo configures MessageService, it is not used by the Roo-generated Flex application.

The following listing shows the channels configured in the Roo-generated services-config.xml file:

```
services-config.xml
<channels>
    <channel-definition id="amf"
        class="mx.messaging.channels.AMFChannel">
```