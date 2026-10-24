# Apache Karaf Cellar

In addition to the `cluster:bundle-*` commands, you can also use the Cellar BundleMBean over JMX. This MBean allows you to manipulate the bundles on the cluster with any kind of JMX client (such as JConsole), as shown in the following screenshot:

![](bbox=[0.175, 0.219, 0.825, 0.638])

# Karaf features

Apache Karaf Cellar is also able to sync Karaf features.

The Cellar features listener spreads all local actions to a feature on the cluster, just as for a bundle.

You also have the `cluster:feature-*` shell commands to manipulate the Karaf features on the cluster.