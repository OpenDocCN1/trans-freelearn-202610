For instance, Karaf provides a `features:install <feature_name>` command to install a Karaf feature in the container. Additionally, Cellar provides `cluster:feature-install <group_name> <feature_name>`, which is similar to the `features:install` command. The `features:install` command will install the feature locally to the Karaf container, whereas the `cluster:feature-install` command will install the feature on the cluster in the given cluster group.

At the end of this chapter, you will find a table that introduces Cellar's `cluster:*` commands which correspond to the regular Karaf commands.

# Features

In order to deploy a feature to a given cluster group, we can use the `cluster:feature-install` command:

```
karaf@root> cluster::feature-install book eventadmin
```

The `cluster:feature-install` command accepts the following arguments:

+   - The cluster group's name (here, `book`)
+   - The feature's name (here, `eventadmin`)
+   - The feature's version (optional)

For instance, we installed the `eventadmin` feature on the `book` cluster group. As Cellar extends the regular Karaf commands, we will find the exact same functions prefixed to the cluster.

You can control the Karaf feature repositories (URLs) per cluster group. For instance, you can list the Karaf feature repositories that are registered and available on a cluster group with the following command:

```
karaf@root> cluster::feature-url-list book
mvn:org.apache.karaf.cellar/apache-karaf-cellar/2.3.1/xml/features
mvn:org.jclouds.karaf/jclouds-karaf/1.4.0/xml/features
mvn:org.apache.karaf.assemblies.features/standard/2.3.1/xml/features
mvn:org.apache.karaf.assemblies.features/enterprise/2.3.1/xml/features
```

You can add a feature's URL on a cluster group using `cluster:feature-url-add`.

For instance, you can add the Camel feature repository on the `book` cluster group with the following commands:

```
karaf@root> cluster::feature-url-add book mvn:org.apache.camel.karaf/
apache-camel/2.12.1/xml/features
karaf@root> cluster::feature-url-list book
```