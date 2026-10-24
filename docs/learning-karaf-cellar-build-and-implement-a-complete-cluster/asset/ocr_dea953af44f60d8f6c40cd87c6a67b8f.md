```
<interface>10.3.16.*</interface>
<interface>10.3.10.4-18</interface>
<interface>192.168.1.3</interface>
</interfaces>
</network>
...
</hazelcast>
```

The configuration allows the usage of the * and - characters.

The * character indicates all the IP addresses on the netmask; in our example, 10.3.16.* means 10.3.16.1, 10.3.16.2, 10.3.16.3, and so on.

The - character indicates an IP address range; in our example, 10.3.10.4-18 means 10.3.10.4, 10.3.10.5, 10.3.10.6, and so on up to 10.3.10.18.

# SSL

Cellar allows you to use SSL socket communication between all the Cellar members.

With SSL, the communication channels between the nodes are secure.

In etc/hazelcast.xml, you just have to enable ssl and provide the keystore that contains the SSL certificate, as shown in the following code:

```
<hazelcast>
...
<network>
...
<ssl enabled="true">
<factory-class-name>
com.hazelcast.nio.ssl.BasicSSLContextFactory
</factory-class-name>
<properties>
<property name="keyStore">keyStore</property>
<property name="keyStorePassword">
keyStorePassword</property>
</properties>
</ssl>
</network>
...
</hazelcast>
```

You have to provide an SSLFactory class. Hazelcast provides a BasicSSLContextFactory class that you can use directly, or you can specify your own SSLFactory.