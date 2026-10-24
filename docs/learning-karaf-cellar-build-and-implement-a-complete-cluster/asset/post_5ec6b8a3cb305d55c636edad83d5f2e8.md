
## The Filtering of Cluster Events

In the previous chapter, we saw how cluster events are created and transported using producers, consumers, and handlers. We saw that we have one internal queue managed per resource (bundle, feature, and configuration) and per cluster group. This allows us to group the nodes and target the provisioning of a resource on a specific group. So, it's the first kind of filtering at the node level.

In this chapter, we will cover the following topics:

- How to filter the cluster events in a given cluster group
- How to define the resources (bundles, features, configurations, and OBRs) to be filtered
- How to define the direction of the filter (coming in or going out from the nodes)

### The configuration of the filters

The filtering of cluster events is defined in the `etc/org.apache.karaf.cellar.groups.cfg` configuration file. In this configuration file, you can find the filters with the following format:

```
cluster_group.[bundle|config|features].[whitelist|blacklist].[inbound|outbound]=regex
```

For instance, the following configuration blocks (blacklist) the incoming cluster events (inbound) that contain features named my-feature or other-* in the default cluster group:

```
default.features.blacklist.inbound=my-feature,other-*
```

Cellar provides a default filtering configuration for the default cluster group. This configuration is used as a template when you create a new cluster group. You can update the filters' configuration live by directly modifying the configuration on the node or using the config:* and cluster:config-* commands.

For instance, to add a new filter to a cluster group, you can run the following commands:

```
karaf@root> config:edit org.apache.karaf.cellar.groups
karaf@root> config:propset mygroup.bundle.blacklist.outbound none
karaf@root> config:update
```

You can also use ConfigMBean to update the configuration remotely (using JConsole for instance).

The name of the cluster group is cluster_group, which you provided in the cluster:group-create command.

### Resources

In the previous chapter, we saw that we have an internal queue per resource and per cluster group.

A resource is a type of a cluster event. Cellar defines the following cluster events:

- bundle: This is a cluster event that corresponds to a bundle action (install, uninstall, start, stop, and so on)
- config: This is a cluster event that corresponds to a configuration action (create, delete, propset, propappend, propdel, and so on)
- features: This is a cluster event that corresponds to a feature action (addurl, delurl, install, uninstall, and so on)

Remember that each type of cluster event is managed by a specific handler.

In the filter definition, specify the resource just after `cluster_group`. This will allow you to filter the events of a specific type (`bundle`, `config`, and `features`).

### Blacklist and whitelist

In the filter definition, you can specify the `blacklist` or `whitelist` keyword after the resource. A blacklisted filter will block the corresponding cluster events. This means that the cluster events won't be delivered on the cluster. On the other hand, a whitelisted filter will allow the corresponding cluster events. The filter is built by evaluating the whitelist first and by overriding the blacklist later. With the combination of blacklist and whitelist filters, you can define fine-grained filtering.

### Inbound and outbound

Another property used to configure filters is the direction of the cluster event.
The two directions can be distinguished as follows:

- `inbound`: The `inbound` cluster events are the events that come into a node. These cluster events come from another node and enter other nodes.
- `outbound`: The `outbound` cluster events are the events that go out of a node. These cluster events are produced and sent from a node to other nodes.

Thanks to inbound and outbound filters, you can filter the same cluster event depending on its direction: incoming or outgoing.

### Regex and event identification

Finally, we have to specify the cluster event. The purpose is to allow (`whitelist`) or block (`blacklist`) specific cluster events. For instance, we want to block the following:

- The cluster event that contains a change in the feature named `my-feature`
- The cluster event that contains a change in the bundle named `my-bundle`

So, we have to identify and declare the cluster event. To identify a cluster event, we can use the following:

- The event identifier can be used; for instance, `org.apache.karaf.cellar.groups` identifies the configuration's cluster event (PID).
- A **regular expression (regex)** or glob based on the event identifier can be used. Declaring events using the fully qualified name can be tedious. The usage of a regex is much faster. For instance, `org.apache.karaf.cellar.*` identifies all the configuration cluster events containing PIDs.
- `*` selects all cluster events.
- The none keyword is a reserved keyword to not select any cluster event.

The identification depends on the resource of the cluster event filter.

#### Bundle

For a bundle cluster event, the identifier is the bundle's location. You can get the bundle's location using the `osgi:list -l` command, which will give you the following output:

```
...

[ 61] [Active      ] [Created  ] [  40]
mvn:org.apache.karaf.cellar/org.apache.karaf.cellar.features/2.3.2

[ 62] [Active      ] [Created  ] [  40]
mvn:org.apache.karaf.cellar/org.apache.karaf.cellar.management/2.3.2

[ 70] [Active      ] [Created  ] [  30]
mvn:org.apache.felix/org.apache.felix.eventadmin/1.3.2
```

For instance, if you want to block (filter) all the bundle cluster events, both inbound and outbound, for the eventadmin bundle in the `my` cluster group, you can specify the following filter in the `etc/org.apache.karaf.cellar.groups.cfg` configuration file:

```
my.bundle.blacklist.inbound=mvn:org.apache.felix/org.apache.felix.eventadmin/1.3.2

my.bundle.blacklist.outbound=mvn:org.apache.felix/org.apache.felix.eventadmin/1.3.2
```

This is where regex is very useful. We can simplify the filter as follows:

```
my.bundle.blacklist.inbound=*org.apache.felix.eventadmin*

my.bundle.blacklist.outbound=*org.apache.felix.eventadmin*
```

#### Configuration

For the configuration cluster event, the identifier is the configuration PID. You can get the configuration PID using the config:list command.

By default, Cellar blacklists its own configuration and avoids syncing the configurations that are only valid locally to a node. The following is how this configuration will look in the `etc/org.apache.karaf.cellar.groups.cfg` configuration file:

```
default.config.whitelist.inbound = *
default.config.whitelist.outbound = *
default.config.blacklist.inbound = org.apache.felix.fileinstall*,\
                            org.apache.karaf.cellar*,\
                            org.apache.karaf.management,\
                            org.apache.karaf.shell,\
                            org.ops4j.pax.logging,\
                            org.ops4j.pax.web
default.config.blacklist.outbound = org.apache.felix.fileinstall*,\
                            org.apache.karaf.cellar*,\
                            org.apache.karaf.management,\
                            org.apache.karaf.shell,\
                            org.ops4j.pax.logging,\
                            org.ops4j.pax.web
```

The following is an explanation of the configurations in the preceding code snippet:

- The org.apache.felix.fileinstall* configurations are excluded (inbound and outbound) as the polled directories are local to the node (so, it doesn't make sense to spread this configuration to the default cluster group)
- The org.apache.karaf.cellar* configurations are excluded as they are provided by Cellar itself
- The org.apache.karaf.management configuration is excluded as it contains the port number of the Karaf JMX MBean server (so it is local to a node)
- The org.apache.karaf.shell configuration is excluded as it contains the port number of the Karaf SSHd server (so it is local to a node)
- The org.ops4j.pax.logging configuration is excluded to be able to change the log level of a node without changing the log levels on the other nodes
- The org.ops4j.pax.web configuration is excluded as it contains the port number of the Karaf HTTP service (so it is local to a node)
