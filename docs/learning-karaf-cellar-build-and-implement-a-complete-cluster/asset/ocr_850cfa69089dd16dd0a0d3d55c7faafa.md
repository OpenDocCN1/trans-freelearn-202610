# Features

For the features cluster event, the identifier is the name of the feature. You can get the feature names using the features:list command, which will give you the following output:

```
...
[uninstalled] [2.3.2          ] cellar-webconsole
    karaf-cell ar-2.3.2  Cellar plugin for Karaf WebConsole
[uninstalled] [1.0.1          ] transaction
    karaf-enterprise-2.3.1 OSGi Transaction Manager
[uninstalled] [1.0.1          ] jpa
    karaf-enterprise-2.3.1 OSGi Persistence Container
[uninstalled] [1.0.0          ] jndi
    karaf-enterprise-2.3.1 OSGi Service Registry JNDI access
[uninstalled] [1.0.0          ] application-without-isolation
    karaf-enterprise-2.3.1 Provide EBA archive support
```

By default, Cellar blacklists some features as follows:

```
default.features.whitelist.inbound = *
default.features.whitelist.outbound = *
default.features.blacklist.inbound =
config,management,hazelcast,cellar*
default.features.blacklist.outbound =
config,management,hazelcast,cellar*
```

The Hazelcast and Cellar features are installed by Cellar itself; so, there is no need to handle the cluster events for these features. That's why the default filter configuration allows all the cluster feature events except the config, management, hazelcast, and all the Cellar (cellar*) features.

On the other hand, the config and management features are provided by default by Karaf. As these are very low-level features, these are not required to handle the events for these features.