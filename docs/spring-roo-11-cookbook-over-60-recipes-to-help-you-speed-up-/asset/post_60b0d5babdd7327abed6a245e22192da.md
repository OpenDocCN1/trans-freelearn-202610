
15. The MyListener JMS message consumer asynchronously reads the JMS message from the myDestination queue and writes it to the standard output, as shown here:

```
JMS message received: DepartureDate: Tue Feb 01 00:00:00 IST 2011, ArrivalDate: Wed Feb 02 00:00:00 IST 2011, FlightDescription: Origin: NYC, Destination: INDIA, Price: 1200.0
```

## How it works...

The JMS add-on of Roo is responsible for processing JMS related commands, which are: jms setup, jms listener, and field jms template.

Spring simplifies integrating an enterprise application with a JMS provider. Spring's JmsTemplate class is a helper class, that enables the applications to send and receive JMS messages synchronously. The JmsTemplate class holds reference to the javax.jms.ConnectionFactory instance—used for creating connections with the JMS provider.

> The JMS provider in our example is the embedded (or in-memory) ActiveMQ. It is important to note that Roo only supports embedded ActiveMQ as the JMS provider. If you want to use any other JMS provider or standalone ActiveMQ, then you'll need to change the Roo-generated JMS provider configuration.

Let's look at various commands that we used in the flightapp-web application for setting up a JMS provider, and for sending and receiving JMS messages.

## Setting up a JMS provider

The jms setup command sets up a JMS provider for your enterprise application. The jms setup command accepts the following arguments:

- provider: A mandatory argument that specifies the JMS provider for which support needs to be added to the application. Roo defines only a single value that this argument can accept, that is, ACTIVEMQ_IN_MEMORY, which is meant for setting up embedded ActiveMQ as the JMS provider.

- destinationName: An optional argument that specifies the name of the JMS destination accessed by the application. If unspecified, the name of the destination is defaulted to myDestination.

- destinationType: An optional argument that identifies the type of the JMS destination specified via the destinationName argument. Roo defines only two possible values for this argument, which are QUEUE (if the JMS destination is of type queue) and TOPIC (if the JMS destination is of type topic). If unspecified, the destination type is defaulted to QUEUE.

The jms setup command creates an application context XML file: applicationContext-jms.xml in SRC_MAIN_RESOURCES/META-INF/spring directory. The applicationContext-jms.xml file configures embedded ActiveMQ broker, JmsTemplate, JMS ConnectionFactory, JMS destinations, and message listener containers.

The following listing shows the embedded ActiveMQ broker configuration in applicationContext-jms.xml:

```
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:amq="http://activemq.apache.org/schema/core" ...>
<amq:broker persistent="false" useJmx="true">
<amq:transportConnectors>
<amq:transportConnector uri="tcp://localhost:61616"/>
</amq:transportConnectors>
</amq:broker>
...
</beans>
```

The amq namespace refers to ActiveMQ schema, which allows configuring ActiveMQ in Spring's application context XML file. The <broker> element configures an embedded ActiveMQ broker whose name is localhost. If you want to specify a custom name for the broker, then you can do so by using brokerName attribute of <broker> element. The persistent attribute specifies whether the JMS messages received by the ActiveMQ broker are persisted into a data store or not. The value false instructs the broker not to persist messages. If you specify true as the value of the persistent attribute, ActiveMQ configurations KahaDB as the default data store for messages. The useJmx attribute specifies if broker's services are exposed via JMX. If the attribute value is true, then you can use JMX clients to invoke ActiveMQ broker's services such as start or stop broker, to add or remove topics and queue JMS destinations, and so on. The <transportConnectors> element defines the transport connectors on which ActiveMQ broker listens to a connection from clients. The <transportConnector> element in the given code listing specifies a tcp transport connector that listens on port 61616.

JMS ConnectionFactory is configured in the applicationContext-jms.xml file, as shown here:

```
<amq:connectionFactory brokerURL="vm://localhost"
id="jmsFactory"/>

<bean class="org.springframework.jms.connection.
CachingConnectionFactory" id="cachingConnectionFactory">
<property name="targetConnectionFactory">
<ref local="jmsFactory"/>
</property>
</bean>
```

The <connectionFactory> element of the amq namespace configures a JMS ConnectionFactory. JMS ConnectionFactory is typically configured in the application server and fetched by applications using JNDI. If ConnectionFactory is configured in the application server, then you can make use of the <jndi-lookup> element of Spring's jee schema to obtain it. The brokerURL attribute identifies the URL for connecting to ActiveMQ broker. The value of the brokerURL attribute is vm://localhost, which means that the VM (Virtual Machine) protocol is used by clients to access ActiveMQ broker named localhost. The vm protocol is used because the client (which is flightapp-web application in our case) and broker are located in the same JVM. The use of the VM protocol ensures improved performance because there is no network overhead involved between client and broker communication and the client directly invokes the methods of the broker.

Spring's CachingConnectionFactory class is a JMS ConnectionFactory adapter that caches instances of JMS Session, MessageConsumer, and MessageProducer for improved performance. The targetConnectionFactory property identifies the ConnectionFactory, which the CachingConnectionFactory instance uses to obtain the JMS Connection. The targetConnectionFactory property in the given configuration refers to the JMS ConnectionFactory created by the <connectionFactory> element of the amq namespace.

In the jms setup command, we also passed JMS destination name (myDestination) and type (QUEUE), which JMS add-on uses to configure a JMS queue named myDestination in the applicationContext-jms.xml file, as shown here:

```
<amq:queue id="myDestination" physicalName="myDestination"/>
```

Here, the <queue> element of amq namespace configures a JMS queue named myDestination. The physicalName attribute specifies the name of the JMS queue in ActiveMQ. JMS destinations are typically configured in the application server and fetched by applications using JNDI. If JMS destination is configured in the application server, then you can make use of <jndi-lookup> element of Spring's jee schema to fetch it.

Spring's JmsTemplate is also configured in applicationContext-jms.xml file, as shown here:

```
<bean class="org.springframework.jms.core.JmsTemplate"
 id="jmsTemplate">
 <property name="connectionFactory"
 ref="cachingConnectionFactory"/>
 <property name="defaultDestination" ref="myDestination"/>
 </bean>
```

The connectionFactory property refers to the CachingConnectionFactory instance. JmsTemplate defines send methods that accept the JMS Destination object or JMS destination name as the parameter. The defaultDestination property refers to the JMS destination that is used for sending or receiving messages when the send method used doesn't accept the JMS Destination object or JMS destination name as the parameter.

To allow the flightapp-web application to asynchronously receive JMS messages, the jms setup command configures a Spring's message listener container in the applicationContext-jms.xml file, as shown here:

```
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:jms="http://www.springframework.org/schema/jms" ...>
...
<jms:listener-container connection-factory="jmsFactory"
destination-type="queue" />
...
</beans>
```

A message listener container receives messages from the JMS provider and dispatches it to a message consumer implementation. The message listener container saves the effort for writing the code that you'll need to write for asynchronous message consumption, which includes registering with the JMS provider, managing transactions, and so on.

In the given configuration, the `<listener-container>` element of Spring's jms namespace creates a message listener container. The `connection-factory` attribute identifies the JMS ConnectionFactory that the container uses for creating connections with the JMS provider. The `destination-type` identifies the JMS destination type (queue, topic, or durable topic) from which the container receives messages. You can add `<listener>` elements (of Spring's jms namespace) inside `<listener-container>` to define the message consumers to which the listener container dispatches the messages for processing.

Let's now look at how we send JMS messages using the JmsTemplate class.

## Sending message using JMS Template

The field jms template command autowires the JmsTemplate instance into a class identified by the `class` argument. The fieldName argument identifies the name of the field with which JmsTemplate is added to the class.

The following code shows the FlightController.java file into which we added the JmsTemplate field using the jms template command:

```
import org.springframework.jms.core.JmsTemplate;
...
public class FlightController {

@Autowired
private transient JmsTemplate jmsTemplate;

public void sendMessage(Object messageObject) {
jmsTemplate.convertAndSend(messageObject);
}
}
```
