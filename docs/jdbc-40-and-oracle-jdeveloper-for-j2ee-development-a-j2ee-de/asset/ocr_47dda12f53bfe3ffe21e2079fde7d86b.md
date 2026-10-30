To use the MySQL data source, copy /docs/examples/jca/mysql-ds.xml to the /server/default/deploy directory. Modify the mysql-ds.xml configuration file by setting <driver-class/> to com.mysql.jdbc.Driver and <connection-url/> to jdbc:mysql://localhost:3306/test.

Finally, modify server/default/conf/login-config.xml with MySQL database settings and add the following <application-policy/> element to \server\ default\conf\login-config.xml:

```
<application-policy name = "MySqlDbRealm">
    <authentication>
        <login-module code = "org.jboss.resource.security.ConfiguredIdentityLoginModule" flag = "required">
            <module-option name ="principal"></module-option>
            <module-option name ="userName">root</module-option>
            <module-option name ="password"></module-option>
            <module-option name ="managedConnectionFactoryName">jboss.jca:service=LocalTxCM,name=MySqlDS</module-option>
        </login-module>
    </authentication>
</application-policy>
```

The above <application-policy/> element configures the JBoss server with the MySQL database. The following JBoss configuration with MySQL database is not required for the example application in this chapter. To use CMP entity EJBs with the MySQL data source some additional modifications are required. Set the <datasource> and <datasource-mapping> elements in the standardjbosscmp-jdbc.xml or jboss cmp-jdbc.xml file.

```
<jboss cmp-jdbc>
    <defaults>
        <datasource>java:/MySqlDS</datasource>
        <datasource-mapping>mySQL</datasource-mapping>
    </defaults>
</jboss cmp-jdbc>
```

We also have to set the <datasource> and <type-mapping> elements in the standardjaws.xml or jaws.xml file.

```
<jaws>
    <datasource>java:/MySqlDS</datasource>
    <type-mapping>mySQL</type-mapping>
</jaws>
```

Modifying the mysql-ds.xml, standardjaws.xml, standardjboss cmp-jdbc.xml, and login-config.xml files configures the JBoss 4.0 server to be used with a MySQL database.