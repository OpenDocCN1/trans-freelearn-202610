
The FlightDescriptionView.mxml shows a **New FlightDescription** button, and if clicked, it invokes the showForm method. The showForm method of FlightDescriptionView.mxml shows the form (represented by FlightDescriptionForm.mxml) for creating FlightDescription entity instances, as shown here:

![](img/b8c379a2ed921ead3aa2b01811dc29fa_283_0.png)

The following sequence diagram shows what happens behind the scenes when you click on the New FlightDescription button:

![](img/b8c379a2ed921ead3aa2b01811dc29fa_283_1.png)

This sequence diagram shows that the showForm method creates FlightDescriptionForm and FlightDescription objects. The FlightDescription object (which corresponds to FlightDescription JPA entity) acts as the form-backing object that we see in web applications. The showForm methods sets the FlightDescription object in the FlightDescriptionForm instance. Also, showForm adds an event listener for the FlightDescriptionEvent.CREATE event to FlightDescriptionForm.

The following code shows the showForm method:

```
private function showForm
(flightDescription:FlightDescription = null):void {
var form:FlightDescriptionForm =
PopUpManager.createPopUp(this, FlightDescriptionForm, true)
as FlightDescriptionForm;
form.flightDescription = flightDescription != null ?
flightDescription : new FlightDescription();

form.addEventListener(FlightDescriptionEvent.CREATE,
flightDescriptionView_flightDescriptionCreateEventHandler);
}
```

This code shows that, the addEventListener method accepts the type of event that FlightDescriptionForm object listens to, which is FlightDescriptionEvent.CREATE. The addEventListener also accepts the name of the handler method that is invoked when the event is received by the FlightDescriptionForm object. So, if the FlightDescriptionEvent.CREATE event is received by the FlightDescriptionForm object, it results in the invocation of the flightDescriptionView_flightDescriptionCreateEventHandler method. We'll come back to the handler method, but first let's look at how the FlightDescriptionEvent.CREATE event is generated.

The following sequence diagram shows that the FlightDescriptionEvent.CREATE event is generated when the user presses the Save button to create a FlightDescription JPA entity instance:

![](img/b8c379a2ed921ead3aa2b01811dc29fa_284_0.png)

This sequence diagram shows that when the **Save** button is clicked, it results in the invocation of the processSave method defined in FlightDescriptionForm.mxml. The processSave method validates the form data entered by the user using the mx.validators.Validator. If the data validation succeeds, form data is set in the FlightDescription ActionScript object. The processSave method now creates a FlightDescriptionEvent event of type FlightDescriptionEvent.CREATE and passes the FlightDescription ActionScript object as the payload of the event. Invoking the dispatchEvent method results in dispatching the newly created event to listeners.

So, after receiving the FlightDescriptionEvent.CREATE event, flightDescriptionView_flightDescriptionCreateEventHandler is invoked, as explained earlier. The following code shows the flightDescriptionView_flightDescriptionCreateEventHandler method, which invokes the FlightDescriptionService's create method to create an instance of the FlightDescription JPA entity:

```
protected function
flightDescriptionView_flightDescriptionCreateEventHandler
(event:FlightDescriptionEvent):void {
    flightDescriptionService.create(event.flightDescription);
}
...
```

The flightDescriptionService object in the previous code represents a mx.rpc.remoting.RemoteObject, which is used by Flex clients to access remoting destinations. RemoteObject is defined in FlightDescriptionView.mxml using the <RemoteObject> tag, as shown here:

```
<s:RemoteObject channelSet="{remotingChannels}"
    destination="flightDescriptionService"
    fault="flightDescriptionService_faultHandler(event)"
    id="flightDescriptionService">
...
</s:RemoteObject>
```

In this code, {remotingChannels} identifies the ChannelSet to use for communication with server-side Java objects. We saw in the previous recipe that remoting channels used by the flightapp_flex application are defined in the flightapp_flex_scaffold.mxml file using the <ChannelSet> tag. The destination attribute specifies the remoting destination that is accessed via RemoteObject.

### There's more...

The Flex Add-on provides round-tripping support, that is, modifications to JPA entities are propagated to MXML and ActionScript files.

Flex Addon doesn't provide any support for controlling the methods that form a part of the Spring-managed remoting destinations. For instance, you can't control the methods that are part of the `FlightDescriptionService_Roo_Service.aj` file using `@RooFlexScaffold` annotation.

If you want that a method in the Spring-managed remoting destination is not accessible to Flex clients, then all you need to do is to either perform push-in refactoring or define the method in the corresponding Java class and add the `@RemotingExclude` annotation of Spring to the method.

Spring Roo makes use of JSR 303 annotations specified in the JPA entity to add Flex validators in the MXML files. For instance, if a JPA entity field specifies `@NotNull` JSR 303 annotation then Roo adds a Flex `StringValidator` or `NumberValidator` that checks that the field on the form is not blank. Note that Flex addon support for JSR 303 annotations is limited.

### See also

- Refer to the Getting started with Flex application development to see how you can set up Flex for your Roo project

## Getting started with Spring Web Flow

In Chapter 4, *Web Application Development with Spring Web MVC* we saw that Roo simplifies building Spring Web MVC applications. In this recipe, we'll look at how Roo sets up your Roo project to use Spring Web Flow—a framework that is built on top of Spring Web MVC.

Spring Web Flow allows modelling a web application as a set of flows, where each flow represents a finite state machine.

> The support for Spring Web Flow is broken in Spring Roo 1.1.3; therefore, you must use Spring Roo 1.1.4 or 1.1.5 to execute this recipe. This recipe has been developed using Spring Roo 1.1.5.

### Getting ready

Create a new directory `C:\roo-cookbook\ch05-webflow` in your system and copy the `ch05_webflow_app.roo` script that accompanies this book to the `ch05-webflow` directory.

Start the Roo shell from the ch05-webflow directory and execute the ch05_webflow_app.roo script using the script command. Executing the ch05_webflow_app.roo script creates a flightapp-webflow Eclipse project, sets up Hibernate as persistence provider, configures MySQL as the database for the application, and creates Flight, FlightDescription, Customer, and Address JPA entities. The Customer entity has a one-to-one relationship with the Address entity and Flight has a many-to-one relationship with the FlightDescription entity. If you are using a different database than MySQL or your connection settings are different than what is specified in the script, then modify the script accordingly.

### How to do it...

To set up the Spring Web Flow framework, follow the steps given here:

1. Execute the web flow Roo command to create a Customer flow, as shown here:

```
roo> web flow --flowName customer
Created SRC_MAIN_WEBAPP\WEB-INF\spring
Created SRC_MAIN_WEBAPP\WEB-INF\spring\webflow-config.xml
Created SRC_MAIN_WEBAPP\WEB-INF\spring\webmvc-config.xml
...
Created SRC_MAIN_WEBAPP\WEB-INF\views\customer
Created SRC_MAIN_WEBAPP\WEB-INF\views\customer\flow.xml
Created SRC_MAIN_WEBAPP\WEB-INF\views\customer\view-state-1.jspx
Created SRC_MAIN_WEBAPP\WEB-INF\views\customer\view-state-2.jspx
Created SRC_MAIN_WEBAPP\WEB-INF\views\customer\end-state.jspx
Created SRC_MAIN_WEBAPP\WEB-INF\views\menu.jspx
Created SRC_MAIN_WEBAPP\WEB-INF\views\customer\views.xml
...
Updated ROOT\pom.xml [...; added dependency org.springframework.webflow:spring-webflow:2.2.1.RELEASE]
```

2. Execute the perform eclipse command to update the project's classpath settings:

```
roo> perform eclipse
```

3. You can now import the flightapp_webflow project into your Eclipse IDE.

4. Exit the Roo shell and execute the tomcat:run goal of the Tomcat Maven plugin from the ch05-webflow directory to deploy the flightapp_webflow project to the embedded Tomcat container:

```
C:\roo-cookbook\ch05-webflow> mvn tomcat:run
```
