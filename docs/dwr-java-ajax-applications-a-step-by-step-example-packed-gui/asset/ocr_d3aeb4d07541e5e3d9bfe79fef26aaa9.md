![](bbox=[0.191, 0.105, 0.81, 0.472])

# Configuring the Web Application

The context root is worth noting (visible in the screenshot above). We will need it when we test the application.

The other XML file, web.xml, is very important as we all know. This XML will hold the DWR servlet definition and other possible initialization parameters. The following code shows the full contents of the web.xml file that we will use:

```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="http://java.sun.com/xml/ns/javaee"
    xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
    id="WebApp_ID" version="2.5">
    <display-name>DWREasyAjax</display-name>
    <servlet>
        <display-name>DWR Servlet</display-name>
        <servlet-name>dwr-invoker</servlet-name>
        <servlet-class>
            org.directwebremoting.servlet.DwrServlet
```
```