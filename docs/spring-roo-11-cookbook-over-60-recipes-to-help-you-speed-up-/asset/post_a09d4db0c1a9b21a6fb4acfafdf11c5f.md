
- 2. Include the Maven repository for Spring snapshot versions in your `pom.xml` file so that the Flex add-on can download Flex add-on dependencies that are not yet available in milestone or in the release Maven repository of Spring:

```xml
<repositories>
<repository>
<id>spring-maven-snapshot</id>
<name>Spring Maven Snapshot Repository</name>
<url>
http://maven.springframework.org/snapshot
</url>
</repository>
...
</repositories>
```

- 3. Configure the plugin repository for the Flexmojos Maven plugin (refer to the plugin documentation for more details: http://repository.sonatype.org/content/sites/flexmojos-site/3.8/plugin-info.html) in the `pom.xml` file, as shown here:

```xml
<pluginRepositories>
<pluginRepository>
<id>flexmojos-repository</id>
<url>
http://repository.sonatype.org/content/groups/flexgroup/
</url>
</pluginRepository>
...
</pluginRepositories>
```

- 4. Execute the `perform eclipse` command to update the project's classpath settings with the newly added dependencies in `pom.xml` file. It also adds Flex and ActionScript nature to the `flightapp_flex` Eclipse project.

## How it works...

The `flex setup` command is processed by the Flex add-on of Roo.

The `flex setup` command configures Spring BlazeDS integration and creates the necessary artifacts that are required for developing a Flex 4 application. The following table describes some of the important directories and files that were created by the `flex setup` command:

| Directory / File | Description |
| --- | --- |
| SRC_MAIN_WEBAPP/history | Contains history.css, history.js, and historyFrame.html files that are responsible for managing browser history. |
| SRC_MAIN_WEBAPP/WEB-INF/flex/services-config.xml | The services-config.xml is a BlazeDS configuration file that contains channels and corresponding endpoint configurations for the BlazeDS. |
| SRC_MAIN_WEBAPP/WEB-INF/spring/flex-config.xml | The flex-config.xml configures BlazeDS MessageBroker as a Spring-managed bean. |
| SRC_MAIN_WEBAPP/WEB-INF/spring/webmvc-config.xml | Spring's web application context XML, which is loaded by DispatcherServlet defined in the web.xml file. Additionally, the webmvc-config.xml file imports bean definitions from flex-config.xml. |
| ROOT/html-template | The html-template directory contains an index.template.html file that acts as an HTML template for embedding the Flex application. Roo generates a flightapp_flex_scaffold.html file (based on the index.template.html file) in the SRC_MAIN_WEBAPP directory for embedding our Flight App Flex application. So, to load our Flight App Flight application, we'll need to load the flightapp_flex_scaffold.html page in the web browser. |
| ROOT/src/main/flex/flightapp_flex_scaffold.mxml | The flightapp_flex_scaffold.mxml is the main application MXML file that defines the layout of the application and the initial user interface. |
| ROOT/src/main/flex/flightapp_flex_scaffold-config.xml | The flightapp_flex_scaffold-config.xml XML file overrides the default Flex compiler settings. In the Scaffolding Flex application from JPA entities recipe we'll see that this XML file contains fully-qualified ActionScript class names that correspond to the main views (MXML files) scaffolded by Roo for each JPA entity. |

Let's now look at the important configuration files created by Spring Roo.

The webmvc-config.xml file is Spring's web application context XML file, and is loaded by DispatcherServlet configured in the web.xml file of the flightapp_flex project. The webmvc-config.xml file imports bean definitions in the flex-config.xml application context XML file, as shown here:

```
webmvc-config.xml
<beans ...>
...
<import resource="flex-config.xml" />
</beans>
```

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
        <endpoint
        url="http://{server.name}:{server.port}/{context.root}/messagebroker/amf"
        class="flex.messaging.endpoints.AMFEndpoint"/>
    </channel-definition>
    ...
    <channel-definition id="longpolling-amf"
    class="mx.messaging.channels.AMFChannel">
    <endpoint url="http://{server.name}:{server.port}/{context.root}/messagebroker/amflongpolling"
    class="flex.messaging.endpoints.AMFEndpoint"/>
    <properties>
    <polling-enabled>true</polling-enabled>
    ...
    </properties>
    </channel-definition>
    ...
</channels>
```

This code listing shows channel definitions as created by Roo. A `<channel-definition>` element defines a channel. The id attribute is a unique identifier of a channel and is used by Flex clients to connect to an endpoint. The class attribute identifies the type of the channel. The enclosing `<endpoint>` element defines the endpoint corresponding to the channel. The url attribute of the `<endpoint>` element specifies the URL of the server and class attribute specifies the endpoint class. Flex components make use of channels to communicate with BlazeDS endpoints. For instance, the above code listing indicates that AMFChannel is used by Flex to communicate with the AMFEndpoint.

The {server.name} and {server.port} tokens used by the url attribute value are replaced at runtime based on the URL of the server from which the SWF file is downloaded. The {context.root} token is replaced with a value that is calculated at compile-time based on the `<contextRoot>` configuration option defined in the pom.xml file for the Flexmojos Maven plugin. Later in this recipe, we'll look at the Flexmojos Maven plugin configuration generated by Roo for the flightapp_flex project.

The incoming requests to DispatcherServlet are routed to the BlazeDS MessageBroker, because the `<mapping-pattern>` element value (that is, /messagebroker/*) in flex-config.xml maps to the url attribute value (http://.../messagebroker/..) of the `<endpoint>` elements in services-config.xml.

We saw earlier that the longpolling-amf channel is used as the default channel by MessageService for transporting messages. The `<polling-enabled>` property of the channel is set to true, which means that polling is enabled for the longpolling-amf channel.
