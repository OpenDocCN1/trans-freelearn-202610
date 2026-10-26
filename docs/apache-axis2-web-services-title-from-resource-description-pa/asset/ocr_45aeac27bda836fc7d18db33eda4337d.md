Irrespective of the application server, you can get Axis2 to work from a custom repository by editing the web.xml file of the Axis2.war distribution. Once this is done, you do not need to worry about whether the application server will unpack the WAR or not, and if it is, where it is going to unpack and so on. You can even configure the application-server-based Axis2 to start up with a remote repository. First, let's look at how to configure Axis2.war to work with a local filesystem. Here, you need to add the following init parameters to the servlet section of the web.xml file:

```
<servlet>
    <servlet-name>AxisServlet</servlet-name>
    <display-name>Apache-Axis Servlet</display-name>
    <servlet-class>org.apache.axis2.transport.http.AxisServlet</servlet-class>
    <init-param>
        <param-name>axis2.xml.path</param-name>
        <param-value>path to custom axis2.xml (you need this only if you want to override the default axis2.xml)</param-value>
        <param-name>axis2.repository.path</param-name>
        <param-value>full path the custom repository</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
```

As mentioned earlier, you can configure the web.xml file to start Axis2 from a URL repository. In this case, you need to add the following two parameters:

```
<servlet>
    <servlet-name>AxisServlet</servlet-name>
    <display-name>Apache-Axis Servlet</display-name>
    <servlet-class>org.apache.axis2.transport.http.AxisServlet</servlet-class>
    <init-param>
        <param-name>axis2.xml.url</param-name>
        <param-value>http://localhot/myrepo/axis2.xml</param-value>
        <param-name>axis2.repository.url</param-name>
        <param-value>http://localhot/myrepo</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
```

As shown here, we have added two parameters called axis2.xml.url and axis2.repository.url. You can modify the values to suit your environment settings.