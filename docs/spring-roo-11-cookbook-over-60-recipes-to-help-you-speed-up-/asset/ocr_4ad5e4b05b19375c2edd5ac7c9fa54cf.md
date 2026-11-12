> > ApplicationRequestFactory is managed by Roo; therefore, you should not modify it manually to add custom client-side service stub methods. Instead, add them to ScaffoldRequestFactory.

The interaction between entity proxy and server-side entity is achieved by configuring GWT's RequestFactoryServlet in the web.xml file of the GWT web application.

> > You don't need to configure RequestFactoryServlet in the web.xml file of the flightapp-gwt project because when you execute the gwt setup command, Roo configures GWT's RequestFactoryServlet in the web.xml file.

The following code shows the configuration of RequestFactoryServlet in the web.xml file of the flightapp-gwt application:

```
web.xml
<servlet>
  <servlet-name>requestFactory</servlet-name>
  <servlet-class>
    com.google.gwt.requestfactory.server.RequestFactoryServlet
  </servlet-class>
</servlet>
<servlet-mapping>
  <servlet-name>requestFactory</servlet-name>
  <url-pattern>/gwtRequest</url-pattern>
</servlet-mapping>
```

This configuration shows that by default Roo maps the RequestFactoryServlet to /gwtRequest URL, which you can change. If you change the URL mapping of RequestFactoryServlet, then you also need to change how ApplicationRequestFactory is created by ScaffoldModule—the Roo-generated GIN module we discussed earlier.

RequestFactory requires that the server-side entity must define a no-argument constructor, getId, getVersion, and find<Entity> methods. The getVersion should return the version, getId should return the unique ID associated with entity instance, and the find<Entity> method which accepts entity ID and returns the corresponding entity instance. So, if you create entities using the Roo entity command, then make sure that you don't set the name of the identifier field or version field to anything other than id and version, respectively. When you execute the Roo gwt setup command, and Roo finds that the names of ID and version fields is different from id and version, then it doesn't scaffold the GWT application.

Let's now look at some of the Roo-generated GWT activities and places.