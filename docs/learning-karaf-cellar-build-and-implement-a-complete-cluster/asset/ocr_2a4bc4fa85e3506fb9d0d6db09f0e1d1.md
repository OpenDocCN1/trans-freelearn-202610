Now, we can deploy and start the service bundle with the following command:

```
karaf@root> osgi:install -s
mvn:org.apache.karaf.cellar.samples.dosgi.greeter/org.apache.karaf.cellar.samples.dosgi.greeter.service/2.3.1
```

We can see our Greeter service in the local service registry by running the following command:

```
karaf@root> ls 65

Apache Karaf :: Cellar :: Samples :: DOSGi Greeter ::
Service (65) provides:
------------------------------------------------------------
---
objectClass =
org.apache.karaf.cellar.samples.dosgi.greeter.api.Greeter
osgi.service.blueprint.compname = greeterImpl
service.exported.interfaces = *
service.id = 286
```

We can now see the `service.exported.interface` property that Cellar DOSGi is looking for.

Cellar DOSGi also provides the following `cluster:service-list` command to display the services registered on the cluster (in the distributed service registry):

```
karaf@root> cluster:service-list

Service Class                                      Provider
Node
org.apache.karaf.cellar.samples.dosgi.greeter.api.Greeter    node1:5701
```

We can see our Greeter service available in the cluster and provided by node1 of the cluster.

# # The client bundle

The client bundle provides a `dosgi-greeter:greet` shell command. The shell command is part of the client bundle, and you can find the command code on the GitHub source repository. The command calls the following `GreeterClient`:

```
package org.apache.karaf.cellar.samples.dosgi.greeter.client;

import org.apache.karaf.cellar.samples.dosgi.greeter.api.Greet;
```