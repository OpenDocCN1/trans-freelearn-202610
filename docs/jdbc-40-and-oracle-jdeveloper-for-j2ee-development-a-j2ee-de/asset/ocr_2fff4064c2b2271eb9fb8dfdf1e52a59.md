# Configuring JDBC in Oracle JDeveloper

On clicking finish in the Finish window, a JSP gets added to the project. To the web.xml file, add a resource-ref element in the Managed Data Source.

![](bbox=[0.178, 0.191, 0.822, 0.522])

The res-ref-name specifies the JNDI name of the Managed Data Source. The resource-ref element is listed below:

```
<resource-ref>
  <res-ref-name>jdbc/MySQLConnectionDS</res-ref-name>
  <res-type>javax.sql.DataSource</res-type>
  <res-auth>Container</res-auth>
</resource-ref>
```

We also need to copy mysql-connector-java-5.1.3-rc-bin.jar to the C:\JDeveloper\j2ee\home\applib directory, which adds the MySQL Connector/J JDBC 4.0 driver to the runtime class path of the web application.