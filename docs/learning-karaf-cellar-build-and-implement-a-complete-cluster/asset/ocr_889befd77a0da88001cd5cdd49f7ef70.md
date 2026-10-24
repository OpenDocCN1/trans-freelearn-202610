For instance, the following configuration blocks (blacklist) the incoming cluster events (inbound) that contain features named my-feature or other-* in the default cluster group:

```
default.features.blacklist.inbound=my-feature,other-*
```

Cellar provides a default filtering configuration for the default cluster group. This configuration is used as a template when you create a new cluster group. You can update the filters' configuration live by directly modifying the configuration on the node or using the config:* and cluster:config-* commands.

For instance, to add a new filter to a cluster group, you can run the following commands:

```
karaf@root> config:edit org.apache.karaf.cellar.groups
```

```
karaf@root> config:propset mygroup.bundle.blacklist.outbound none
```

```
karaf@root> config:update
```

You can also use ConfigMBean to update the configuration remotely (using JConsole for instance).

The name of the cluster group is cluster_group, which you provided in the cluster:group-create command.

# Resources

In the previous chapter, we saw that we have an internal queue per resource and per cluster group.

A resource is a type of a cluster event. Cellar defines the following cluster events:

+   - bundle: This is a cluster event that corresponds to a bundle action (install, uninstall, start, stop, and so on)
+   - config: This is a cluster event that corresponds to a configuration action (create, delete, propset, propappend, propdel, and so on)
+   - features: This is a cluster event that corresponds to a feature action (addurl, delurl, install, uninstall, and so on)