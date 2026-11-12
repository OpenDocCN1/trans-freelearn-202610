# contextConfigLocation initialization parameter

The contextConfigLocation context initialization parameter identifies Spring's root web application context XML file(s), as shown here:

```
<context-param>
<param-name>contextConfigLocation</param-name>
<param-value>
classpath*:META-INF/spring/applicationContext*.xml
</param-value>
</context-param>
```

The META-INF/spring/applicationContext.xml file contains bean definitions that are shared by all servlets and filters defined in the flight-app web application. These bean definitions are available to application contexts loaded by DispatcherServlet. As we saw in the Creating a Roo project recipe of Chapter 1, Getting Started with Spring Roo and in Setting up a JPA provider for your project recipe of Chapter 2, Persisting Objects Using JPA an applicationContext.xml file contains bean definitions for configuring data sources, services, transactions, and so on, and needs to be shared across the web application.

If you create additional application context XML files, which contain bean definitions that you want to share across the web application, then you can add them to the value of the contextConfigLocation parameter using commas or space separated values, as shown here:

```
<param-value>
classpath*:META-INF/spring/applicationContext*.xml,
META-INF/spring/mycontext.xml
</param-value>
```

# ContextLoaderListener

The root web application context, identified by the contextConfigLocation context initialization parameter, is loaded by the ContextLoaderListener, which implements javax.servlet.ServletContextListener, as shown here:

```
<listener>
<listener-class>
org.springframework.web.context.ContextLoaderListener
</listener-class>
</listener>
```

# DispatcherServlet

The webmvc-config.xml file created by Roo identifies the web application context of the Roo-generated web application. The webmvc-config.xml file contains tile definitions, handler (or controller) definitions, handler mappings, view and exception resolution strategies, and so on. The DispatcherServlet of Spring is responsible for loading the webmvc-config.xml file and dispatching requests to appropriate handlers, resolving views, exceptions, and so on. In the web.xml of the flight-app, DispatcherServlet is configured to load webmvc-config.xml, as shown here: